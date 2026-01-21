# Week 1
## 1. CUDA 编程模型（SIMT）

- CUDA 采用 **SIMT（Single Instruction, Multiple Threads）** 模型
    
- 一个 kernel 表示一段程序代码，会被大量线程同时执行
    
- 所有线程执行相同指令流，但处理不同数据
    
- 并行性来源于线程数量，而非显式 for 循环
    

---

## 2. CUDA 执行层级结构

### 2.1 Thread / Block / Grid

- **Thread**
    
    - 最小执行单元
        
    - 每个 thread 通常负责一个数据元素
        
- **Block**
    
    - 一组 thread 的集合
        
    - block 内线程可以：
        
        - 使用 shared memory
            
        - 通过 `__syncthreads()` 同步
            
- **Grid**
    
    - 所有 block 的集合
        
    - 不同 block 之间不能通信或同步
        

---

### 2.2 线程索引计算（1D）

标准全局索引计算方式：

```
i = blockIdx.x * blockDim.x + threadIdx.x
```

含义：

- `threadIdx.x`：thread 在 block 内的局部编号
    
- `blockIdx.x`：block 在 grid 内的编号
    
- `blockDim.x`：每个 block 的 thread 数
    
- `i`：该 thread 对应的全局数据索引
    

---

## 3. Kernel 启动与执行语义

### 3.1 Kernel 启动

- kernel 通过三重尖括号语法启动：
    

```
kernel<<<numBlocks, blockSize>>>(...)
```

- 启动的是 `numBlocks × blockSize` 个线程
    

---

### 3.2 异步执行特性

- kernel launch 是 **异步** 的：
    
    - CPU 发起 kernel 后立即返回
        
    - GPU 在后台执行 kernel
        
- 若 CPU 需要使用 GPU 计算结果，必须显式同步：
    

```
cudaDeviceSynchronize()
```

否则可能发生未完成计算就读取结果的问题（正确性错误）。

---

## 4. Global Memory

### 4.1 定义与特性

- Global memory 是 GPU 的主存（显存）
    
- 特点：
    
    - 容量大（GB 级）
        
    - 所有线程可访问
        
    - 延迟高，但带宽大
        
- 通过 `cudaMalloc` / `cudaMallocManaged` 分配的数组默认位于 global memory
    

---

### 4.2 Global Memory 的访问粒度

- Global memory 访问不是以单 thread 为单位，而是以 **warp（32 threads）** 为单位
    
- 一个 warp 内线程的访问会被硬件合并（coalescing）
    

---

## 5. Coalesced Global Memory Access

### 5.1 定义

- 当同一个 warp 中的线程访问 **连续内存地址** 时：
    
    - GPU 可将访问合并为少量内存事务
        
    - 称为 **coalesced access**
        

---

### 5.2 示例

理想（coalesced）访问模式：

```
thread 0  → x[0]
thread 1  → x[1]
thread 2  → x[2]
...
```

非理想（non-coalesced）访问模式：

```
thread 0  → x[0]
thread 1  → x[1000]
thread 2  → x[2000]
...
```

---

### 5.3 性能影响

- coalesced 访问可最大化带宽利用率
    
- non-coalesced 访问会产生大量内存事务，显著降低性能
    

---

## 6. Elementwise Kernel（以 ReLU 为例）

### 6.1 ReLU 定义

- 数学形式：`ReLU(x) = max(0, x)`
    
- 属于 elementwise 操作：
    
    - 各元素之间相互独立
        
    - 非常适合并行
        

---

### 6.2 CUDA ReLU Kernel 的内存访问模式

每个 thread：

1. 从 global memory 读取一个元素 `x[i]`
    
2. 在寄存器中执行计算
    
3. 将结果写回 global memory `y[i]`
    

特点：

- 每个 thread：1 次 global load + 1 次 global store
    
- 访问地址连续 → coalesced
    
- 计算量极小 → **memory-bound kernel**
    

---

## 7. Block Size 的基本选择原则（经验层面）

- warp 大小为 32
    
- blockSize 通常选为 32 的倍数，避免部分 warp 浪费
    
- 常用默认值：128 / 256
    
    - 提供足够 warp 数量以隐藏内存延迟
        
    - 不易触发资源限制
        
- 最终 blockSize 需通过 profiling 验证
    

---

## 8. Profiling 工具

### 8.1 ncu（Nsight Compute）

- Kernel-level profiler
    
- 用于分析单个 kernel 的执行情况
    
- 基本用法：
    

```
ncu ./program
```

---

### 8.2 nsys（Nsight Systems）

- System-level profiler
    
- 用于分析整个程序的时间线：
    
    - CPU/GPU 执行关系
        
    - kernel launch 与同步行为
        
- 基本用法：
    

```
nsys profile ./program
```

## 9. Code

```cpp
#include <cstdio>

#include <cuda_runtime.h>

  
__global__ void vecAdd(const float* a, const float* b, float* c, int n) {

    // 全局线程索引：每个线程对应一个元素

    int i = blockIdx.x * blockDim.x + threadIdx.x;

    if (i < n) c[i] = a[i] + b[i];

}

  
static void ck(cudaError_t e, const char* msg) {

    if (e != cudaSuccess) {

        std::fprintf(stderr, "CUDA error (%s): %s\n", msg, cudaGetErrorString(e));

        std::exit(1);

    }

}

  
int main() {

    const int n = 1 << 20;          // 1,048,576 个元素

    const size_t bytes = n * sizeof(float);

  

    float *a = nullptr, *b = nullptr, *c = nullptr;

  
    // Unified Memory：CPU/GPU 共用指针（入门最省事）

    ck(cudaMallocManaged(&a, bytes), "cudaMallocManaged(a)");

    ck(cudaMallocManaged(&b, bytes), "cudaMallocManaged(b)");

    ck(cudaMallocManaged(&c, bytes), "cudaMallocManaged(c)");

  
    // CPU 初始化

    for (int i = 0; i < n; i++) {

        a[i] = 1.0f;

        b[i] = 2.0f;

        c[i] = 0.0f;

    }

  

    // 配置：每个 block 256 线程，block 数量覆盖 n

    int blockSize = 256;

    int numBlocks = (n + blockSize - 1) / blockSize;

  

    // 启动 kernel（异步）

    vecAdd<<<numBlocks, blockSize>>>(a, b, c, n);

  

    // 检查 launch + 等待完成

    ck(cudaGetLastError(), "kernel launch");

    ck(cudaDeviceSynchronize(), "cudaDeviceSynchronize");

  

    // 验证（抽查前 10 个 + 全量检查可选）

    bool ok = true;

    for (int i = 0; i < 10; i++) {

        if (c[i] != 3.0f) ok = false;

        std::printf("c[%d]=%.1f\n", i, c[i]);

    }

    std::printf(ok ? "PASS (first 10)\n" : "FAIL\n");

  

    cudaFree(a);

    cudaFree(b);

    cudaFree(c);

    return ok ? 0 : 1;

}
```
# Week 2
## Code
```cpp
__global__ void reduce_sum(float* input, float* output) {
    __shared__ float sdata[256];

    int tid = threadIdx.x;
    int i = blockIdx.x * blockDim.x + tid;

    // 1. load global → shared
    sdata[tid] = input[i];
    __syncthreads();

    // 2. 在 shared memory 里做 reduction
    for (int stride = blockDim.x / 2; stride > 0; stride >>= 1) {
        if (tid < stride) {
            sdata[tid] += sdata[tid + stride];
        }
        __syncthreads();
    }

    // 3. 写回结果
    if (tid == 0) {
        output[blockIdx.x] = sdata[0];
    }
}

```