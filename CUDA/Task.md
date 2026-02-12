`nvidia-smi`
# cuda core fp32 gemm
## 指标
1. runtime
2. GFLOPS
3. ncu 的 memory throughput
4. occupancy
5. 是否 memory-bound / compute-bound
---
### 你要回答的问题

- 为什么小矩阵差距不明显？
    
- 为什么大矩阵差距变大？
    
- 什么时候开始 compute-bound？
    
- CUDA Core FP32 的性能上限是多少？
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
### 性能
```cpp
Size: 128 x 128 x 128 | Time: 0.037472 ms | GFLOPS: 111.931687
Size: 256 x 256 x 256 | Time: 0.038528 ms | GFLOPS: 870.910320
Size: 512 x 512 x 512 | Time: 0.178816 ms | GFLOPS: 1501.182487
Size: 1024 x 1024 x 1024 | Time: 1.119872 ms | GFLOPS: 1917.615315
Size: 2048 x 2048 x 2048 | Time: 8.523008 ms | GFLOPS: 2015.704841
Size: 4096 x 4096 x 4096 | Time: 74.865852 ms | GFLOPS: 1835.802962
Size: 8192 x 8192 x 8192 | Time: 435.304382 ms | GFLOPS: 2525.845529
```

## 疑问
cpu 验证有必要吗？

# Tensor Core GeMM



