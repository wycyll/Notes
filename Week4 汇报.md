
# 第1页：标题页


本周我的 CUDA 学习阶段主要聚焦在 **从 CUDA Core 优化到 Tensor Core 硬件加速**。  
整个优化路线围绕 **矩阵乘法（GEMM）** 展开，通过逐步优化 kernel 的方式，观察 GPU 性能提升的来源。

---

# 第2页：时间进度

这一页介绍本周的整体进度。

在 **Week 3** 我已经完成了 Shared Memory 的基础优化，实现了 tiled matrix multiplication。

本周 **Week 4 的重点** 是继续深入两个层次：

第一是 **寄存器级优化（Register-level optimization）**  
第二是 **硬件单元级优化（Tensor Core）**

同时在 kernel 层面，我实现了一个逐步演进的 pipeline：

Naive  
→ Shared Memory  
→ CUDA Opt1  
→ CUDA Opt2  
→ Tensor Basic  
→ Tensor Opt


---

# 第4页：Shared Memory Bank Conflict

知识点回顾

GPU 的 Shared Memory 由多个 **memory bank** 组成。

当多个线程访问 **同一个 bank 的不同地址** 时，就会产生 **Bank Conflict**。

Bank Conflict 会导致：

一次访问被拆成多次串行访问

从而显著降低带宽。

例如：

32 个线程同时访问同一 bank  
就会发生 **32-way bank conflict**。

因此，在设计 shared memory 布局时，需要尽量避免多个线程落在同一个 bank 上。

---

# 第5页：Padding 与 Vectorized Access

解决 Bank Conflict 的常见方法之一是 **Padding**。

例如原来的 shared memory：

As[32][32]

可以改为：

As[32][33]

通过增加 stride，可以改变 bank 映射方式，使不同线程访问不同 bank。

第二个优化是 **Vectorized Access**。

通过 `float4` 实现：

一次加载 128-bit 数据。

优势：

减少指令数量  
提高显存带宽利用率

实现方式通常是：

reinterpret_cast<float4*>

这样每条 load 指令就能加载 4 个 float。

---

# 第6页：Thread Tiling 与 Register Reuse

接下来进入 **线程级优化**。

核心思想是：

**让一个线程计算多个输出元素。**

例如：

原本

1 thread → 1 output

优化后

1 thread → 4 outputs

这样做有两个好处：

第一，提高计算密度（Arithmetic Intensity）

第二，提高寄存器复用率。

例如 B 矩阵中的一个元素：

读取一次

可以参与多个 FMA 运算。

这样就减少了 Shared Memory 访问次数。

---

# 第7页：Loop Unrolling

接下来是一个常见的 kernel 优化技术。

 **Loop Unrolling**。

通过：

 `#pragma unroll `

展开循环。

这样可以减少：

分支判断开销  
指令调度开销。

---

# 第8页：Occupancy 与 Latency Hiding

GPU 的执行机制是 **warp scheduling**。

当一个 warp 在等待内存访问时：

GPU 可以立刻调度另一个 warp。

这种机制叫：

**Latency Hiding**

因此，GPU 的高性能依赖：

大量并发 warp。

Occupancy 指的是：

一个 SM 上能同时运行的 active warp 数量。

更高的 occupancy 通常可以：

更好地隐藏 memory latency。

还有一个kernel 优化技术。是 **Double Buffering**。

基本思想是：

在计算当前 tile 时  
提前加载下一 tile。

这样就可以实现：

计算与内存访问重叠。

从而提升整体吞吐量。

---

# 第9页：Warp-Level Optimization 与 Tensor Core

接下来进入 **Warp-level optimization**。

在这一层，32 个线程会协作完成一个计算任务。

两个重要技术：

Warp Shuffle  
Warp Matrix Operations

Warp Shuffle 可以让线程之间直接交换寄存器数据。

Warp Matrix Operations 则是：

Tensor Core 的底层指令。

Tensor Core 是专门用于矩阵乘法的硬件单元。

执行的核心操作是：

D = A × B + C

与普通 CUDA Core 相比，

Tensor Core 的吞吐量可以提升一个数量级。

---

# 第10页：Tensor Core 原理

Tensor Core 通过 **Warp 级协作**完成矩阵乘法。

32 个线程共同计算一个 tile。

同时支持：

混合精度计算

通常是：

FP16 输入  
FP32 累加

这样既保证了速度，又保持一定精度。

---

# 第11页：WMMA 接口

CUDA 提供了 **WMMA API** 来调用 Tensor Core。

核心概念是 **Fragment**。

一个 fragment 表示矩阵 tile 的一部分。

整个计算流程是：

Load  
MMA  
Store

Load：

把矩阵加载到 fragment

MMA：

执行矩阵乘加

Store：

把结果写回 global memory。

---

# 第12页：实现 Pipeline

这一页总结整个实现流程。

从最初的：

Naive  
Shared Memory

逐步演进到：

CUDA Opt1  
CUDA Opt2

再到：

Tensor Basic  
Tensor Opt

每一步都对应不同层级的 GPU 优化。

---

# 第13页：CUDA Opt1 性能分析

在 Opt1 中，我尝试了 **Padding**。

但实验结果发现：

Padding 反而导致性能下降。

原因是：

当前 kernel 的访问模式：

As[ty][k]  
Bs[k][tx]

本身就没有 Bank Conflict。

因此 Padding 并没有解决问题，

反而增加了：

地址计算开销  
shared memory 使用量。

---

# 第14页：CUDA Opt2（线程粒度优化）

Opt2 的主要优化是 **Thread Tiling**。

原来的 block：

32 × 32  
1024 threads

现在改为：

32 × 8  
256 threads

但每个线程计算：

4 个输出元素。

例如：

row_start = by_32 + ty_4

每个线程写：

C[row_start + 0][col]  
C[row_start + 1][col]  
C[row_start + 2][col]  
C[row_start + 3][col]

这样可以：

提高计算密度  
提高 ILP

---

# 第15页：Vectorized Memory Access

Opt2 中还引入了 **向量化访存**。

通过 `float4`：

一次加载 4 个 float。

优势：

减少 load 指令  
提高 DRAM 带宽利用率。

---

# 第16页：Register Tiling

另一个关键优化是 **Register Tiling**。

每个线程维护：

val[4]

四个累加器。

B 矩阵的一个元素：

只读取一次

但可以用于：

4 次 FMA 计算。

这样显著减少了：

Shared Memory 访问压力。

---

# 第17页：Tensor Core Basic

接下来进入 Tensor Core 版本。

Tensor Basic 的目标是：

跑通最小 WMMA pipeline。

也就是：

Load fragment  
MMA  
Store

验证 Tensor Core 计算流程。

---

# 第18页：Tensor Core Opt

在 Tensor Core 优化版本中，

主要做了两个优化。

第一：

Block-level Tiling

原来：

1 block = 1 warp

现在：

1 block = 16 warps

每个 block 可以计算：

64 × 64 的 tile。

这样可以提升 SM 利用率。

---

# 第19页：Instruction Level Parallelism

第二个优化是：

提高 **Instruction Level Parallelism**

当 A 和 B 的 tile 被加载到 shared memory 后，

多个 warp 可以重复使用这些数据。

这样可以：

减少 global memory traffic  
提高数据 reuse。

---

# 第20页：Attention 与 Transformer

最后介绍应用场景。

Transformer 的核心计算也是矩阵运算。

例如：

Q = XWq  
K = XWk  
V = XWv

然后计算：

QK^T

得到 attention score。

再经过 softmax，

最后计算：

Attention(Q,K,V)

因此：

Transformer 的核心计算

本质就是 **大规模矩阵乘法**。

这也是为什么：

Tensor Core 对 AI 计算非常重要。
