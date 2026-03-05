
| 特性   | CUDA Core      | Tensor Core      |
| ---- | -------------- | ---------------- |
| 类型   | 标量 ALU         | 矩阵乘法单元           |
| 计算方式 | 标量/向量          | 小矩阵              |
| 精度   | FP32/FP64      | FP16/TF32/INT8 等 |
| 使用方式 | 普通 CUDA kernel | WMMA / cuBLAS    |
| 速度   | 普通             | 非常快              |

## 1. Tensor Core 是什么

- Tensor Core 是 GPU 中 **专门用于矩阵乘法的硬件单元**
    
- 与普通 CUDA Core 的 Fused Multiply-Add (FMA) 不同，它不是执行单个乘加，而是执行 **矩阵乘法累加（Matrix Multiply Accumulate, MMA）**
    

计算形式为：

D = A × B + C

其中

- A、B 是输入矩阵
    
- C 是累加矩阵
    
- D 是结果矩阵
    

Tensor Core 一次执行的是 **一个小矩阵块的乘法**，而不是单个元素计算。

---

## 2. Tensor Core 和 CUDA Core 的区别

普通 CUDA Core：

- 每条指令执行  
    1 次乘法 + 1 次加法
    
- 计算粒度是 **单个元素**
    

Tensor Core：

- 每条指令执行 **整个矩阵块乘法**
    
- 本质是 **矩阵级 FMA**
    

简单理解：

CUDA Core  
→ 计算 a × b + c

Tensor Core  
→ 计算 A × B + C（矩阵）

因此吞吐量可以提升很多倍。

---

## 3. Tensor Core 一次计算的矩阵大小

不同架构略有不同，但常见模式是：

16 × 16 × 16

表示：

A 为 16 × 16  
B 为 16 × 16  
结果 C 为 16 × 16

即：

C(16×16) = A(16×16) × B(16×16) + C(16×16)

这一操作包含：

256 个输出元素  
每个元素 16 次 FMA

总共约 **4096 次 FMA**

但 Tensor Core **一条指令完成**。

---

## 4. Tensor Core 的执行单位

Tensor Core **不是线程级执行**，而是 **warp 级执行**。

- 1 warp = 32 threads
    
- warp 内所有线程协作完成一次矩阵运算
    

也就是说：

普通 CUDA Core

- thread → 一个计算元素
    

Tensor Core

- warp → 一个矩阵 tile
    

---

## 5. Tensor Core 的数据类型

Tensor Core 主要支持低精度计算。

常见组合：

- FP16 × FP16 → FP32 accumulate
    
- BF16 × BF16 → FP32 accumulate
    
- TF32 × TF32 → FP32 accumulate
    
- INT8 × INT8 → INT32 accumulate
    

最常见情况：

FP16 输入  
FP32 累加

这样可以同时获得：

- 高吞吐量
    
- 较好的数值稳定性
    

---

## 6. Tensor Core 编程接口（WMMA）

CUDA 提供一个高级接口：

WMMA  
Warp Matrix Multiply Accumulate

WMMA 的核心概念是 **fragment**

fragment 表示 warp 持有的一块矩阵数据。

常见 fragment 类型：

- matrix_a
    
- matrix_b
    
- accumulator
    

计算流程一般是：

1. load matrix fragment
    
2. 执行 mma 运算
    
3. store 结果
    

也就是：

load → mma → store

---

## 7. Tensor Core Kernel 的基本结构

Tensor Core 的 GEMM kernel 结构和普通 GEMM 类似：

1. 从 global memory 读取数据
    
2. 放入 shared memory tile
    
3. 从 shared memory 加载 fragment
    
4. 调用 Tensor Core mma 指令
    
5. 将结果写回
    

流程结构：

global memory  
→ shared memory tile  
→ register fragment  
→ tensor core mma  
→ register  
→ global memory

区别只是：

普通 GEMM 使用

- FMA
    

Tensor Core GEMM 使用

- MMA
    

---

## 8. Tensor Core 与 Tile 结构

在 Tensor Core GEMM 中通常存在三层 tile：

1）Block tile

- 一个 block 计算的大矩阵块
    

2）Warp tile

- 一个 warp 计算的矩阵块
    

3）Tensor Core tile

- Tensor Core 一次计算的小矩阵
    

典型结构：

matrix  
→ block tile  
→ warp tile  
→ tensor core tile

例如：

block 计算 128×128  
warp 计算 64×64  
tensor core 计算 16×16

---

## 9. Tensor Core 为什么速度快

主要原因有三点：

1）矩阵级指令  
一条指令执行大量 FMA

2）专用硬件单元  
Tensor Core 专门为矩阵运算设计

3）低精度计算  
FP16 / BF16 运算吞吐量更高

因此理论性能通常是 CUDA Core 的 **10 倍以上**。

---

## 10. Tensor Core 使用条件

要使用 Tensor Core，通常需要满足：

- 数据类型支持（FP16 / BF16 / TF32 等）
    
- 矩阵尺寸满足 tile 要求
    
- 数据对齐
    
- warp 协作执行
    

如果这些条件不满足，GPU 会退回普通 CUDA Core 计算。

---

## 11. Tensor Core 与普通 GEMM 的关系

普通 GEMM：

- thread 计算单个元素
    
- 使用 FMA
    

Tensor Core GEMM：

- warp 计算矩阵 tile
    
- 使用 MMA
    

但整体算法结构仍然相同：

- tiling
    
- shared memory
    
- register blocking
    
- memory coalescing
    

只是 **计算核心从 CUDA Core 变成 Tensor Core**。

---

## 12. Tensor Core 在深度学习中的作用

Tensor Core 主要用于：

- 深度学习训练
    
- 深度学习推理
    
- 大规模矩阵乘法
    
- Transformer / CNN / attention
    

因为这些计算本质都是 **GEMM 或 GEMM 变形**