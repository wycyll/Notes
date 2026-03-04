`nvidia-smi`
# cuda core fp32 gemm
## 指标
1. runtime
2. GFLOPS
3. peak GFLOPS
4. ncu 的 memory throughput
5. occupancy
6. 是否 memory-bound / compute-bound
## naive
```c++
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>
#include <cmath>
  
// FP32 GeMM
// Naive
// C = A * B
// A: M x K
// B: K x N
// C: M x N
// 一个 thread 负责计算 C 的一个元素 C[row][col]
__global__ void gemm_naive(const float *A, const float *B, float *C, int M, int N, int K) {

    // 根据 block 和 thread index 计算全局 row 和 col
    // y 轴对应行 (M)，x 轴对应列 (N)
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    int col = blockIdx.x * blockDim.x + threadIdx.x;

    // 边界检查，防止越界
    if (row < M && col < N) {
        float sum = 0.0f;
        // 计算点积: A 的第 row 行 与 B 的第 col 列
        for (int k = 0; k < K; ++k) {
            sum += A[row * K + k] * B[k * N + col];
        }
        // 写入结果到 Global Memory
        C[row * N + col] = sum;
    }
}  

// 初始化 Kernel
__global__ void init_matrix(float *data, int num_elements) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    if (tid < num_elements) {
        // 简单赋值，比如设为 1.0f 或者根据 idx 生成
        data[tid] = (float)(tid % 100) / 100.0f;
    }
}

int main() {
    int sizes[] = {128, 256, 512, 1024, 2048, 4096, 8192};
    int num_sizes = sizeof(sizes) / sizeof(sizes[0]);  

    for (int i = 0; i < num_sizes; i++) {
        int SIZE = sizes[i];
        int M = SIZE;
        int N = SIZE;
        int K = SIZE;

        size_t size_A = M * K * sizeof(float);
        size_t size_B = K * N * sizeof(float);
        size_t size_C = M * N * sizeof(float);
  
        float *d_A, *d_B, *d_C;
        cudaMalloc(&d_A, size_A);
        cudaMalloc(&d_B, size_B);
        cudaMalloc(&d_C, size_C);


        // 初始化数据
        int threadsPerBlockInit = 256;
        int blocksPerGridA = (M * K + threadsPerBlockInit - 1) / threadsPerBlockInit;
        int blocksPerGridB = (K * N + threadsPerBlockInit - 1) / threadsPerBlockInit;
        init_matrix<<<blocksPerGridA, threadsPerBlockInit>>>(d_A, M * K);
        init_matrix<<<blocksPerGridB, threadsPerBlockInit>>>(d_B, K * N);
        cudaDeviceSynchronize();

        // 配置 Kernel
        dim3 blockDim(32, 32);
        dim3 gridDim((N + blockDim.x - 1) / blockDim.x, (M + blockDim.y - 1) / blockDim.y);

        // 计时
        cudaEvent_t start, stop;

        cudaEventCreate(&start);

        cudaEventCreate(&stop);

        cudaEventRecord(start);
        gemm_naive<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
        cudaEventRecord(stop);
        cudaEventSynchronize(stop);
        float milliseconds = 0;
        cudaEventElapsedTime(&milliseconds, start, stop);
        double gflops = (2.0 * M * N * K) / (milliseconds * 1e6);
        printf("Size: %d x %d x %d | Time: %f ms | GFLOPS: %f\n", M, N, K, milliseconds, gflops);

        // 清理
        cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
        cudaEventDestroy(start);
        cudaEventDestroy(stop);
    }
    return 0;
}
```
## shared memory
```cpp
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>
#include <cmath>  
#define TILE_WIDTH 32

// FP32 GeMM
// Shared Memory Tiling
// 计算 C = A * B
// A: M x K
// B: K x N
// C: M x N
__global__ void gemm_shared(const float *A, const float *B, float *C, int M, int N, int K) {

    __shared__ float As[TILE_WIDTH][TILE_WIDTH];
    __shared__ float Bs[TILE_WIDTH][TILE_WIDTH]; 

    int bx = blockIdx.x;
    int by = blockIdx.y;
    int tx = threadIdx.x;
    int ty = threadIdx.y;
  

    // 确定当前线程负责计算的 C 矩阵元素的行号和列号
    int row = by * TILE_WIDTH + ty;
    int col = bx * TILE_WIDTH + tx; 
    float val = 0.0f;
 
    // 遍历计算 C 元素所需的 A 和 B 的所有子矩阵（分块/Tiles）
    // 阶段循环 (phase loop)，遍历每一个分块
    int numTiles = (K + TILE_WIDTH - 1) / TILE_WIDTH;
  
    for (int ph = 0; ph < numTiles; ++ph) {
        // 全局内存 -> Tiling A
        // 需要的 A 元素是 A[row][col_in_A_tile]
        // col_in_A_tile = ph * TILE_WIDTH + tx
        if (row < M && (ph * TILE_WIDTH + tx) < K) {
            As[ty][tx] = A[row * K + ph * TILE_WIDTH + tx];
        } else {
            As[ty][tx] = 0.0f;
        }

  
        if ((ph * TILE_WIDTH + ty) < K && col < N) {
            Bs[ty][tx] = B[(ph * TILE_WIDTH + ty) * N + col];
        } else {
            Bs[ty][tx] = 0.0f;
        }

        __syncthreads();

        for (int k = 0; k < TILE_WIDTH; ++k) {
           val += As[ty][k] * Bs[k][tx];
        }


        __syncthreads();
    } 

    if (row < M && col < N) {
        C[row * N + col] = val;
    }
}

  

// 初始化 Kernel

__global__ void init_matrix(float *data, int num_elements) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    if (tid < num_elements) {
        data[tid] = (float)(tid % 100) / 100.0f;
    }
}

  

int main() {
    int sizes[] = {128, 256, 512, 1024, 2048, 4096, 8192};
    int num_sizes = sizeof(sizes) / sizeof(sizes[0]);

  

    for (int i = 0; i < num_sizes; i++) {
        int SIZE = sizes[i];

        // 设置矩阵维度
        int M = SIZE;
        int N = SIZE;
        int K = SIZE;

        size_t size_A = M * K * sizeof(float);
        size_t size_B = K * N * sizeof(float);
        size_t size_C = M * N * sizeof(float);

  

        // Device 内存分配
        float *d_A, *d_B, *d_C;
        cudaMalloc(&d_A, size_A);
        cudaMalloc(&d_B, size_B);
        cudaMalloc(&d_C, size_C);

  

        // 在 GPU 上初始化数据
        int threadsPerBlockInit = 256;
        int blocksPerGridA = (M * K + threadsPerBlockInit - 1) / threadsPerBlockInit;
        int blocksPerGridB = (K * N + threadsPerBlockInit - 1) / threadsPerBlockInit;
        init_matrix<<<blocksPerGridA, threadsPerBlockInit>>>(d_A, M * K);
        init_matrix<<<blocksPerGridB, threadsPerBlockInit>>>(d_B, K * N);
        cudaDeviceSynchronize();

  

        // 配置 Kernel 启动参数

        // 每个 Block 32x32 = 1024 个线程

        // TILE_WIDTH 匹配 blockDim 的维度

        dim3 blockDim(TILE_WIDTH, TILE_WIDTH);

        dim3 gridDim((N + blockDim.x - 1) / blockDim.x, (M + blockDim.y - 1) / blockDim.y);

  

        // 启动 Kernel

        cudaEvent_t start, stop;
        cudaEventCreate(&start);
        cudaEventCreate(&stop);

        cudaEventRecord(start);

        gemm_shared<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
        cudaEventRecord(stop);
        cudaEventSynchronize(stop);
        float milliseconds = 0;
        cudaEventElapsedTime(&milliseconds, start, stop);
        double gflops = (2.0 * M * N * K) / (milliseconds * 1e6);
        printf("Size: %d x %d x %d | Time: %f ms | GFLOPS: %f\n", M, N, K, milliseconds, gflops);

        // 释放内存
        cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
        cudaEventDestroy(start);
        cudaEventDestroy(stop);
    }
    return 0;

}
```
## Opt1
```cpp
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>
#include <cmath>

#define TILE_WIDTH 32

// FP32 GeMM
// Shared Memory Tiling
// 计算 C = A * B
// A: M x K
// B: K x N
// C: M x N
__global__ void gemm_shared(const float *A, const float *B, float *C, int M, int N, int K) {

    __shared__ float As[TILE_WIDTH][TILE_WIDTH + 1];
    __shared__ float Bs[TILE_WIDTH][TILE_WIDTH + 1];

    int bx = blockIdx.x;
    int by = blockIdx.y;
    int tx = threadIdx.x;
    int ty = threadIdx.y;

    // 确定当前线程负责计算的 C 矩阵元素的行号和列号
    int row = by * TILE_WIDTH + ty;
    int col = bx * TILE_WIDTH + tx;

    float val = 0.0f;

    // 遍历计算 C 元素所需的 A 和 B 的所有子矩阵（分块/Tiles）
    // 阶段循环 (phase loop)，遍历每一个分块
    int numTiles = (K + TILE_WIDTH - 1) / TILE_WIDTH;

    for (int ph = 0; ph < numTiles; ++ph) {
        
        // 全局内存 -> Tiling A
        // 需要的 A 元素是 A[row][col_in_A_tile]
        // col_in_A_tile = ph * TILE_WIDTH + tx
        if (row < M && (ph * TILE_WIDTH + tx) < K) {
            As[ty][tx] = A[row * K + ph * TILE_WIDTH + tx];
        } else {
            As[ty][tx] = 0.0f;
        }

        // 全局内存 -> Tiling B
        // 需要的 B 元素是 B[row_in_B_tile][col]
        // row_in_B_tile = ph * TILE_WIDTH + ty
        if ((ph * TILE_WIDTH + ty) < K && col < N) {
            Bs[ty][tx] = B[(ph * TILE_WIDTH + ty) * N + col];
        } else {
            Bs[ty][tx] = 0.0f;
        }

        __syncthreads();

        for (int k = 0; k < TILE_WIDTH; ++k) {
            val += As[ty][k] * Bs[k][tx];
        }

        __syncthreads();
    }

    if (row < M && col < N) {
        C[row * N + col] = val;
    }
}

// 初始化 Kernel
__global__ void init_matrix(float *data, int num_elements) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    if (tid < num_elements) {
        data[tid] = (float)(tid % 100) / 100.0f;
    }
}

int main() {
    int sizes[] = {128, 256, 512, 1024, 2048, 4096, 8192};
    int num_sizes = sizeof(sizes) / sizeof(sizes[0]);
    
    printf("Size,AvgTime(ms),AvgGFLOPS,PeakGFLOPS\n");

    for (int i = 0; i < num_sizes; i++) {
        int SIZE = sizes[i];
        // 设置矩阵维度
        int M = SIZE;
        int N = SIZE;
        int K = SIZE;

        size_t size_A = M * K * sizeof(float);
        size_t size_B = K * N * sizeof(float);
        size_t size_C = M * N * sizeof(float);

        // Device 内存分配
        float *d_A, *d_B, *d_C;
        cudaMalloc(&d_A, size_A);
        cudaMalloc(&d_B, size_B);
        cudaMalloc(&d_C, size_C);

        // 在 GPU 上初始化数据
        int threadsPerBlockInit = 256;
        int blocksPerGridA = (M * K + threadsPerBlockInit - 1) / threadsPerBlockInit;
        int blocksPerGridB = (K * N + threadsPerBlockInit - 1) / threadsPerBlockInit;
        
        init_matrix<<<blocksPerGridA, threadsPerBlockInit>>>(d_A, M * K);
        init_matrix<<<blocksPerGridB, threadsPerBlockInit>>>(d_B, K * N);
        
        cudaDeviceSynchronize();

        // 配置 Kernel 启动参数
        // 每个 Block 32x32 = 1024 个线程
        // TILE_WIDTH 匹配 blockDim 的维度
        dim3 blockDim(TILE_WIDTH, TILE_WIDTH); 
        dim3 gridDim((N + blockDim.x - 1) / blockDim.x, (M + blockDim.y - 1) / blockDim.y);

        // 启动 Kernel
        cudaEvent_t start, stop;
        cudaEventCreate(&start);
        cudaEventCreate(&stop);

        // 预热
        gemm_shared<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
        cudaDeviceSynchronize();

        float total_milliseconds = 0.0f;
        float min_milliseconds = 1e9f;
        int num_repeats = 10;
        
        for (int r = 0; r < num_repeats; ++r) {
            cudaEventRecord(start);
            gemm_shared<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
            cudaEventRecord(stop);
            
            cudaEventSynchronize(stop);
            float current_milliseconds = 0;
            cudaEventElapsedTime(&current_milliseconds, start, stop);
            
            total_milliseconds += current_milliseconds;
            if (current_milliseconds < min_milliseconds) min_milliseconds = current_milliseconds;
        }

        float avg_milliseconds = total_milliseconds / num_repeats;
        double avg_gflops = (2.0 * M * N * K) / (avg_milliseconds * 1e6);
        double peak_gflops = (2.0 * M * N * K) / (min_milliseconds * 1e6);
        
        printf("%d, %f, %f, %f\n", SIZE, avg_milliseconds, avg_gflops, peak_gflops);

        // 释放内存
        cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
        cudaEventDestroy(start);
        cudaEventDestroy(stop);
    }

    return 0;
}

```
## Opt2
```cpp
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>
#include <cmath>

#define TILE_WIDTH 32
#define TY 4 // [Optimization] 每个线程计算的行数


// FP32 GeMM
// Shared Memory Tiling
// 计算 C = A * B
// A: M x K
// B: K x N
// C: M x N
__global__ void gemm_shared(const float *A, const float *B, float *C, int M, int N, int K) {

    // [Optimization] 使用寄存器缓存，减少 Shared Memory 访问
    // [Optimization] 向量化访存 (float4)
    // [Optimization] Padding to avoid bank conflicts (stride 36 floats = 144 bytes, 16-byte aligned)
    
    __shared__ float As[TILE_WIDTH][TILE_WIDTH + 4];
    __shared__ float Bs[TILE_WIDTH][TILE_WIDTH + 4];

    int bx = blockIdx.x;
    int by = blockIdx.y;
    int tx = threadIdx.x;
    int ty = threadIdx.y;
    
    // 线程线性索引，用于把数据加载进 Shared Memory
    int tid = ty * blockDim.x + tx;

    // 当前线程负责计算 C 矩阵的起始行和列
    // [Optimization] 线程粒度变粗: 负责 TY 行
    int row_start = by * TILE_WIDTH + ty * TY;
    int col = bx * TILE_WIDTH + tx;

    // 寄存器累加器
    float val[TY] = {0.0f};

    int numTiles = (K + TILE_WIDTH - 1) / TILE_WIDTH;

    for (int ph = 0; ph < numTiles; ++ph) {
        
        // [Optimization] Vectorized Load Using float4
        // 256 threads load 1024 floats -> each reads 1 float4
        // Calculate the indices within the tile (32x32)
        int load_r = tid / 8;        // 0..31
        int load_c = (tid % 8) * 4;  // 0,4,8...28

        // Load A -> As
        // A is M x K
        int A_r = by * TILE_WIDTH + load_r;
        int A_c = ph * TILE_WIDTH + load_c;
        
        // Assume aligned memory for simplicity in this optimization step
        if (A_r < M && A_c < K) {
            reinterpret_cast<float4*>(&As[load_r][load_c])[0] = 
                reinterpret_cast<const float4*>(&A[A_r * K + A_c])[0];
        } else {
             // OOB handling (simplified)
             reinterpret_cast<float4*>(&As[load_r][load_c])[0] = make_float4(0.0f, 0.0f, 0.0f, 0.0f);
        }

        // Load B -> Bs
        // B is K x N
        int B_r = ph * TILE_WIDTH + load_r;
        int B_c = bx * TILE_WIDTH + load_c;
        
        if (B_r < K && B_c < N) {
            reinterpret_cast<float4*>(&Bs[load_r][load_c])[0] = 
                reinterpret_cast<const float4*>(&B[B_r * N + B_c])[0];
        } else {
            reinterpret_cast<float4*>(&Bs[load_r][load_c])[0] = make_float4(0.0f, 0.0f, 0.0f, 0.0f);
        }

        __syncthreads();

        // Computation
        for (int k = 0; k < TILE_WIDTH; ++k) {
            // Cache B value in register (broadcast to all threads computing this column)
            float b_val = Bs[k][tx];
            
            // [Optimization] Compute TY elements
            #pragma unroll
            for (int i = 0; i < TY; ++i) {
                val[i] += As[ty * TY + i][k] * b_val;
            }
        }

        __syncthreads();
    }

    // Store results
    #pragma unroll
    for (int i = 0; i < TY; ++i) {
        int r = row_start + i;
        if (r < M && col < N) {
            C[r * N + col] = val[i];
        }
    }
}

// 初始化 Kernel
__global__ void init_matrix(float *data, int num_elements) {
    int tid = blockIdx.x * blockDim.x + threadIdx.x;
    if (tid < num_elements) {
        data[tid] = (float)(tid % 100) / 100.0f;
    }
}

int main() {
    int sizes[] = {128, 256, 512, 1024, 2048, 4096, 8192};
    int num_sizes = sizeof(sizes) / sizeof(sizes[0]);
    
    printf("Size,AvgTime(ms),AvgGFLOPS,PeakGFLOPS\n");

    for (int i = 0; i < num_sizes; i++) {
        int SIZE = sizes[i];
        // 设置矩阵维度
        int M = SIZE;
        int N = SIZE;
        int K = SIZE;

        size_t size_A = M * K * sizeof(float);
        size_t size_B = K * N * sizeof(float);
        size_t size_C = M * N * sizeof(float);

        // Device 内存分配
        float *d_A, *d_B, *d_C;
        cudaError_t err_malloc;
        err_malloc = cudaMalloc(&d_A, size_A);
        if(err_malloc != cudaSuccess) printf("Malloc A failed: %s\n", cudaGetErrorString(err_malloc));
        err_malloc = cudaMalloc(&d_B, size_B);
        if(err_malloc != cudaSuccess) printf("Malloc B failed: %s\n", cudaGetErrorString(err_malloc));
        err_malloc = cudaMalloc(&d_C, size_C);
        if(err_malloc != cudaSuccess) printf("Malloc C failed: %s\n", cudaGetErrorString(err_malloc));

        // 在 GPU 上初始化数据
        int threadsPerBlockInit = 256;
        int blocksPerGridA = (M * K + threadsPerBlockInit - 1) / threadsPerBlockInit;
        int blocksPerGridB = (K * N + threadsPerBlockInit - 1) / threadsPerBlockInit;
        
        init_matrix<<<blocksPerGridA, threadsPerBlockInit>>>(d_A, M * K);
        init_matrix<<<blocksPerGridB, threadsPerBlockInit>>>(d_B, K * N);
        
        cudaDeviceSynchronize();

        // Configure Kernel Launch Parameters
        // Block: 32x8 (256 threads)
        dim3 blockDim(TILE_WIDTH, TILE_WIDTH / TY); 
        // Grid: 
        dim3 gridDim((N + blockDim.x - 1) / blockDim.x, (M + blockDim.y * TY - 1) / (blockDim.y * TY));

        // Launch Kernel
        cudaEvent_t start, stop;
        cudaEventCreate(&start);
        cudaEventCreate(&stop);

        // 预热
        gemm_shared<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
        cudaError_t err = cudaGetLastError();
        if (err != cudaSuccess) {
            printf("CUDA Error after warmup: %s\n", cudaGetErrorString(err));
        }
        cudaDeviceSynchronize();

        float total_milliseconds = 0.0f;
        float min_milliseconds = 1e9f;
        int num_repeats = 10;
        
        for (int r = 0; r < num_repeats; ++r) {
            cudaEventRecord(start);
            gemm_shared<<<gridDim, blockDim>>>(d_A, d_B, d_C, M, N, K);
            cudaEventRecord(stop);
            
            cudaEventSynchronize(stop);
            float current_milliseconds = 0;
            cudaEventElapsedTime(&current_milliseconds, start, stop);
            
            total_milliseconds += current_milliseconds;
            if (current_milliseconds < min_milliseconds) min_milliseconds = current_milliseconds;
        }

        float avg_milliseconds = total_milliseconds / num_repeats;
        double avg_gflops = (2.0 * M * N * K) / (avg_milliseconds * 1e6);
        double peak_gflops = (2.0 * M * N * K) / (min_milliseconds * 1e6);

        printf("%d, %f, %f, %f\n", SIZE, avg_milliseconds, avg_gflops, peak_gflops);

        // 释放内存
        cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
        cudaEventDestroy(start);
        cudaEventDestroy(stop);
    }

    return 0;
}

```
### Padding
#### 1. 基础版 (Scalar Shared) 为什么没有冲突？
在 gemm_shared 中，每个线程读取 1 个 float：

- Block 维度 `(32, 32)`。
- Warp 内的 32 个线程（假设 `ty=0`），`tx` 从 0 到 31。
- 写入地址：`As[0][tx]`。
- 映射到 Bank：`Bank = tx % 32`。
- 结果：Warp 内 32 个线程分别对应 32 个不同的 Bank (0~31)。完美无冲突。

#### 2. Opt2 (Vectorized) 为什么产生了冲突？
在 gemm_opt2 中，为了提升带宽，我们用了 `float4`（一次搬运 4 个 float, 128位）：

- Block 维度 `(32, 8)` (256线程)。
- 一个 Warp (32线程) 一次性搬运 32×4=128个 float。
- `TILE_WIDTH=32`，所以一行只有 32 个 float。这意味着一个 Warp 现在要同时填满 4 行 (128/32=4)。

让我们看看 Warp 内线程的写入行为（无 Padding 时 `As[32][32]`）：

- 线程 0~7：填满 第0行 (As[0][...])。
    - 线程0 写 `As[0][0..3]` -> 占用 Bank 0,1,2,3
- 线程 8~15：填满 第1行 (As[1][...])。
    - 线程8 写 `As[1][0..3]`。因为没有 Padding，第1行首地址紧挨着第0行尾地址。`32%32=0`。第1行也是从 Bank 0 开始的！
    - 线程8 写 Bank 0,1,2,3。
- 冲突发生：在同一个时钟周期内，Warp 内的 线程0 和 线程8 都要往 Bank 0~3 写数据。
- 同理，线程16、线程24 也会加入混战。
- 结果：产生 4路 Bank Conflict (4-way conflict)，导致写入 Shared Memory 的速度变慢。

#### 3. Padding 为什么能解决？
我们把宽度改为 `As[32][32+4]` (也就是 36 个 float)：
- 第0行 起始于 Bank 0。
    - 线程0 写 Bank 0,1,2,3。
- 第1行 起始于哪里？
    - 偏移量是 36。`36 % 32 = 4`。
    - 第1行起始于 Bank 4。
    - 线程8 写 Bank 4,5,6,7。
- 结论：线程0 (Bank 0-3) 和 线程8 (Bank 4-7) 错开了！

所以，Padding 在这里是为了配合 `float4` 引起的多行并发写入 而必须加的补丁。
# Tensor Core GeMM



