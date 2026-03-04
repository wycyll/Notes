![ad8263ed3e0be75bdf98bc11d3c69c17.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/ad8263ed3e0be75bdf98bc11d3c69c17.jpg)
# Week 1
## 1. CUDA 编程模型（SIMT）

- CUDA 采用 SIMT（Single Instruction, Multiple Threads） 模型
    
- 一个 kernel 表示一段程序代码，会被大量线程同时执行
    
- 所有线程执行相同指令流，但处理不同数据
    
- 并行性来源于线程数量，而非显式 for 循环
    

---

## 2. CUDA 执行层级结构

### 2.1 Thread / Block / Grid

- Thread
    
    - 最小执行单元
        
    - 每个 thread 通常负责一个数据元素
        
- Block
    
    - 一组 thread 的集合
        
    - block 内线程可以：
        
        - 使用 shared memory
            
        - 通过 `__syncthreads()` 同步
            
- Grid
    
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

- kernel launch 是 异步 的：
    
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

- Global memory 访问不是以单 thread 为单位，而是以 warp（32 threads） 为单位
    
- 一个 warp 内线程的访问会被硬件合并（coalescing）
    

---

## 5. Coalesced Global Memory Access

### 5.1 定义

- 当同一个 warp 中的线程访问 连续内存地址 时：
    
    - GPU 可将访问合并为少量内存事务
        
    - 称为 coalesced access
        

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
    
- 计算量极小 → memory-bound kernel
    

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
## 1. Code
```cpp
__global__ void reduce_sum(float* input, float* output) {
    __shared__ float sdata[256];
    //给每个block分配一块shared memory数组sdata，长度256

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
## 2. CUDA Memory Hierarchy

### 2.1 Global Memory 的局限性

- Global memory：
    
    - 延迟高
        
    - 带宽大
        
    - 适合一次性、连续（coalesced）访问
        
- 对于 elementwise kernel（如 ReLU）：
    
    - 每个元素只访问一次
        
    - 性能主要受带宽限制
        
- 对于存在数据复用的计算：
    
    - 重复从 global memory 读取会成为主要瓶颈
        

---

### 2.2 Shared Memory 的引入

- Shared memory 是：
    
    - 位于 GPU 片上的高速内存
        
    - block 内 thread 共享
        
    - 容量小（KB 级），速度快
        
- 生命周期：
    
    - 与 block 绑定
        
    - block 执行结束后自动释放
        
- Shared memory 不是自动缓存，需要程序员显式管理
    
---

## 3. Shared Memory 的作用范围与语义

- Shared memory 的可见性：
    
    - 仅限于同一个 block 内的 thread
        
- 不同 block 之间：
    
    - 不能共享 shared memory
        
    - 不能进行同步
        
- 设计原因：
    
    - 只有 block 内 thread 被保证可同时驻留与同步
        
    - shared memory 的作用域与硬件调度模型一致
        

---

## 4. Shared Memory 的基本使用方式

### 4.1 声明方式

```C++
__shared__ float sdata[BLOCK_SIZE];
```

含义：

- 为每个 block 分配一份 shared memory 数组
    
- block 内所有 thread 可读写
    
---

### 4.2 标准使用流程
绝大多数 shared memory kernel 遵循以下结构：
1. 从 global memory 加载数据到 shared memory
    
2. 使用 `__syncthreads()` 进行 block 内同步
    
3. 基于 shared memory 进行计算
    
4. 必要时将结果写回 global memory
    
---

## 5. 线程同步机制：`__syncthreads()`

- `__syncthreads()` 是 block 级同步原语
    
- 语义：
    
    - block 内所有 thread 都到达该语句后，才能继续执行
        
- 使用场景：
    
    - 当 thread 需要读取其他 thread 写入的 shared memory 数据时
        
- 缺失同步的后果：
    
    - 读取未完成写入的数据
        
    - 结果不确定（未定义行为）
        

---

## 6. 例子：Reduction 问题

### 6.1 Reduction 的计算特点

- 目标：将多个输入元素归约为一个输出（如求和）
    
- 特点：
    
    - 存在跨 thread 的数据依赖
        
    - 无法用纯 elementwise 并行解决
        

---

### 6.2 Reduction 的并行基本思想

- 将问题拆分为：
    
    - 每个 block 计算一个部分结果
        
- 每个 block 内：
    
    - 使用 shared memory 存储中间结果
        
    - 通过多轮并行 reduction 将数据规模逐步减半
        
- Reduction 的并行复杂度：
    
    - 需要 `log₂(blockDim.x)` 轮
        

---

## 7. Stride-based Reduction 的核心逻辑

- Reduction 采用逐轮 stride 缩减的方式：
    
    - 第一轮：stride = blockDim.x / 2
        
    - 每一轮 stride 减半
        
- 每一轮中：
    
    - 只有 `tid < stride` 的 thread 参与计算
        
    - 数据规模在 shared memory 中逐步压缩
        
- 设计原因：
    
    - 每个 thread 一次只能合并两个元素
        
    - 一轮并行操作最多只能将数据量减半
        

---

## 8. Reduction Kernel 的结构性认知

- Kernel 内始终存在固定数量的 thread
    
- `tid` 在 kernel 生命周期内保持不变
    
- 不同轮次中：
    
    - 通过条件判断决定哪些 thread 参与计算
        
- 最终结果被压缩至 shared memory 的第一个元素（如 `sdata[0]`）

# Week 3
## 1. Coalesced Global Memory Access

### 1.1 概念

GPU 访问 global memory 时是 warp（32 threads）一起访问。

如果 warp 中线程访问的地址是：

- 连续地址
    
- 对齐地址
    

GPU 就可以把这些访问 合并成更少的 memory transaction。

#### 理想访问模式

```
thread0 → A[0]
thread1 → A[1]
thread2 → A[2]
...
thread31 → A[31]
```

这是 coalesced access。

#### 非理想访问

```
thread0 → A[0]
thread1 → A[1000]
thread2 → A[2000]
```

会产生大量 memory transaction。

---

### 1.2 为什么重要

GEMM 的性能通常 受 memory bandwidth 限制。

如果 global memory 不 coalesced：

- DRAM transaction 增加
    
- 带宽利用率降低
    
- kernel 性能严重下降
    

---

### 1.3 实现方式

常见做法：

让
```
threadIdx.x
```

对应 连续列元素

例如：
```
B[row][tx]
```

这样 warp 访问的是连续地址。

---

## 2. Shared Memory Bank Conflict

### 2.1 概念

Shared memory 被分成多个 bank。

可以理解为：

> 多条并行访问通道。

如果 warp 内多个线程访问：

- 不同地址
    
- 但这些地址落在 同一个 bank
就会发生：*bank conflict*。访问会被串行化。
---

### 2.2 举例
如果访问 stride = 32：
```
thread0 → A[0]
thread1 → A[32]
thread2 → A[64]
...
```
所有访问可能落入同一个 bank。
结果：
```
32-way bank conflict
```
---
### 2.3 影响
shared memory 本来非常快。
但如果 bank conflict 严重：shared memory latency 会大幅增加

---
## 3. Shared Memory Padding

### 3.1 思想

改变 shared memory 的 stride，避免访问模式产生 bank conflict。

---

### 3.2 实现方式

原本：

```
__shared__ float As[32][32];
```

改成：

```
__shared__ float As[32][33];
```

或者

```
__shared__ float As[32][36];
```

---

### 3.3 为什么有效

padding 改变了行跨度 stride
从32 * 4 bytes变成33 * 4 bytes，避免线程访问落入同一 bank。

---
## 4. Vectorized Memory Access

### 4.1 概念

一次读取多个元素，而不是逐个读取。

常见类型：

```
float2
float4
```

例如：
> float4表示4 个 float (16 bytes)一次读取

---

### 4.2 优点

减少：
- load/store 指令数量
    
- memory transaction 数量

提高：
- memory bandwidth 利用率
    
---

### 4.3 GEMM 中的典型做法

让每个线程读取4 个连续 float而不是4 次 scalar load

例如：
```
reinterpret_cast<float4*>(...)
```
---

## 5. Thread Tiling / Register Blocking

### 5.1 概念

让一个线程计算 多个输出元素。
原始方式：
```
1 thread → 1 output
```
优化后：
```
1 thread → 4 outputs
```
---
### 5.2 实现方式

使用寄存器数组：

```
float val[4];
```

每个线程计算：

```
C[row+0][col]
C[row+1][col]
C[row+2][col]
C[row+3][col]
```

---
### 5.3 优点

提高Arithmetic Intensity即计算量 / memory访问量减少memory traffic

---

## 6. Register Reuse

### 6.1 思想

把频繁使用的数据缓存到 寄存器。

例如：

```cpp
float b_val = Bs[k][tx];
```

然后重复使用：

```
val0 += A * b_val
val1 += A * b_val
val2 += A * b_val
val3 += A * b_val
```

---

### 6.2 优点

减少shared memory load 提高计算效率

---

## 7. Loop Unrolling

## 7.1 概念

把循环展开，减少循环控制开销。
例如：
```
#pragma unroll
```
原本:
```
for(i=0;i<4;i++)
```
编译器展开为：
```
val0 += ...
val1 += ...
val2 += ...
val3 += ...
```
---
### 优点
减少：

- branch
    
- loop overhead
    
提高instruction parallelism

---

# Week 4：高级 Kernel 优化

## 1. Occupancy

## 1.1 概念

GPU 一个 SM 可以同时运行多个 block。

Occupancy 指：

```
实际活跃线程数
/
硬件最大线程数
```

---

### 1.2 为什么重要

高 occupancy 可以：

```
隐藏 memory latency
```

当一个 warp 等待 memory 时：

GPU 可以调度另一个 warp。

---

### 1.3 影响因素

- register 使用量
    
- shared memory 使用量
    
- block size
    

通常需要用工具分析：

```
ncu
nsight
```

---

## 2. Latency Hiding

GPU 的核心思想：

```
当一个 warp 等待 memory
→ 调度另一个 warp
```

如果 warp 数量太少：

```
GPU 会空闲等待
```

所以需要：

```
更多 active warps
```

---

## 3. Double Buffering

## 3.1 概念

让*数据加载*和*计算*重叠执行。
普通流程：
```
load tile
compute
load tile
compute
```
Double buffering：
```
load next tile
while computing current tile
```
---
### 3.2 实现
使用两组 shared memory：
```
As0 / As1
Bs0 / Bs1
```
实现：
```
load + compute overlap
```
---

## 4. Warp-Level Optimization
GPU 调度单位是：
```
warp (32 threads)
```
一些优化直接在 warp 内进行：
- warp shuffle
- warp reduction
- warp matrix operations
可以减少：shared memory 使用
---
## 5. Tensor Core

Tensor Core 是 NVIDIA 专门为矩阵乘法设计的硬件单元。
一次可以执行：
```
matrix multiply accumulate
```
例如：
```
16 × 16 × 16
```
---
### 支持的数据类型
- FP16
- BF16
- TF32
- INT8
---
### 性能
Tensor Core 的矩阵乘法性能：远高于 CUDA Core

---
## CUDA GEMM 完整优化路线

```
Naive GEMM
↓
Shared Memory Tiling
↓
Coalesced Global Memory Access
↓
Avoid Bank Conflict
↓
Vectorized Memory Load
↓
Thread Tiling / Register Blocking
↓
Instruction Level Parallelism
↓
Occupancy Optimization
↓
Double Buffering
↓
Tensor Core
```
