# Introduction
## Signal
### Analog signal 模拟信号
- 连续取值（无限可能值）
- may lose quality (hard to fix)
### Digital signal
Finite possible values
- Binary Digital Signal
> Each binary digit is a ==bit==

易于恢复，某个范围识别为 0，某个范围识别为 1 即可
### A/D Conversion
#### A2D
> Digitization
- 核心步骤：
    1. 采样（Sampling）：按固定时间间隔（采样率）采集模拟信号的幅值，将连续时间信号转换为离散时间信号
    2. 量化（Quantization）：将采样得到的幅值映射到有限的数字取值（如用 “00” 表示 0V、“01” 表示 1V 等）
- 关键参数：
    - **采样率（Sample Rate）**：单位时间内的采样次数，采样率越高，数字信号对模拟信号的时间维度还原越精准
    - **分辨率（Resolution）**：量化时可表示的幅值等级数量，由二进制位数决定（如 2 位二进制可表示 4 个等级，8 位可表示 256 个等级）。分辨率越高，数字信号对模拟信号的幅值还原越细腻
#### D2A
离散的数字信号转换回连续的模拟信号的过程

- Typical Digital System
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919195059502.png)

## 二进制
### 二进制转十进制
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920164108241.png)
### 十进制转二进制
#### 整数
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920164154274.png)
从下往上
#### 小数
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920164355999.png)
反复乘二，取整数部分，从上往下

### 二进制/六进制/八进制
二进制与十六进制可通过四位一组快速转化

|                                                                                                     | Example                                                                                             |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920164801940.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920165002238.png) |
### 负数
三种方式表示
#### Sign and magnitude
S&M
**MSB**(most significant bit) 代表符号
0 代表正，1 代表负
> 1100->-4
#### 1 's complement code
**Negation** 翻转所有位数
> 1100->0011->-3
#### 2's complement code
1. Negation
2. plus 1
> 1100->0011->0100->-4

计算机中signed numbers 以 2's complement 形式储存
#### Sign Extension
repeat sign bit w/o changing the value
- 1011 = ==1111==1011
- 0101 = ==0000==0101

### 计算
有**overflow** 需要加上carry，否则不加
检查方法：
1. ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920230942319.png)
2. 如果正+正出现负，或者负+负出现正，就有 overflow。两个符号相反的一定不overflow
> 16 进制里以 7/8 为界 <=7 是正的反之为负
> 8 进制里以 3/4 为界 <=3 是正的反之为负
# Basic Logic Gates
## 晶体管 (Transistor)
本质：电子开关 Electronic switch
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920231413268.png)
上图的 Voltage: Low 一般定义为 0（接地）High 一般为非 0

## CMOS 晶体管
全称: Complementary Metal-Oxide-Semiconductor
![fa160c3ab374e4f9869fae547a82a4ce.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/fa160c3ab374e4f9869fae547a82a4ce.jpg)
- **nMOS 晶体管**：当栅极（Gate）施加高电平（1）时，源极（Source）和漏极（Drain）之间形成导电通道，晶体管导通；低电平（0）时截止
- **pMOS 晶体管**：与 nMOS 相反，栅极施加低电平（0）时导通，高电平（1）时截止

## Truth Table
- 左侧列：列出所有输入变量的**全部可能组合**，若有 N 个输入，组合数量为 $2^N$（如 2 个输入有 4 种组合，3 个输入有 8 种组合）
- 右侧列：对应每种输入组合的输出结果

|                                                                                                     |                                                                                                         |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920232650100.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920232843378.png)<br> |
## Three Basic Logic Gates
从效率（功耗与速度）角度分析，nMOS 与 pMOS 必须采用 “串联 - 并联互补结构”（如 nMOS 串联、pMOS 并联，或反之）
![0c389e9ce6829a1ba3a087907c415177.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/0c389e9ce6829a1ba3a087907c415177.jpg)

> z: Connecting point between p&mMOS
### AND Logic
- 逻辑定义：仅当**所有输入均为 1**时，输出才为 1；否则输出为 0
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250920232407136.png)
![b70ec8fd5554899fb1f573333f65766a.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/b70ec8fd5554899fb1f573333f65766a.jpg)

### OR Logic
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250921003531885.png)

### NOT Logic
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250921003821707.png)

### 优先级
F = a AND NOT ( b OR NOT (c) )
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250921004101172.png) 

=  a·( b + c' )'
非运算（NOT） > 与运算（AND） > 或运算（OR）

## Other Gates
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923011925557.png)

==XOR 在两个输入时可用来对比是否相等==
- 在 CMOS 工艺中，与门（AND）是通过 “与非门（NAND）加非门（NOT）” 实现的
- 或门（OR）是通过 “或非门（NOR）加非门（NOT）” 实现的
- 因此，NAND和NOR在 CMOS 电路中更常被直接制造

# Boolean Algebra & Optimization
## Terminology
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923113041283.png)

注意：没有 Sum term，必须要像a+b 这种才是sum term。要是literal 的和
Sum-of-products 可以简写为SOP
Product of sum 可简写为 POS

## Basic Theorem
1. Theorem 1
- (a) $x + 0 = x$  
- (b) $x \cdot 0 = 0$

2. Theorem 2
- (a) $x + x' = 1$ 
- (b) $x \cdot x' = 0$ 

3. Theorem 3
- (a) $x + x = x$      
- (b) $x \cdot x = x$

4. Theorem 4
- (a) $x + 1 = 1$
- (b) $x \cdot 1 = x$

5. Involution
- $(x')' = x$

6. Commutative
- (a) $x + y = y + x$  
- (b) $xy = yx$

7. Associative
- (a) $x + (y + z) = (x + y) + z$
- (b) $x(yz) = (xy)z$

8. Distributive
- (a) $x(y + z) = xy + xz$ 
- (b) $x + yz = (x+y)(x+z)$ 
#proof 对右边 $(x + y)(x + z)$ 进行**展开**，利用逻辑运算的基本定律（如分配律、幂等律、吸收律）简化，看是否能得到左边 $x + yz$。
1. 展开右边： $(x + y)(x + z) = x·x + x·z + y·x + y·z$
    
2. 利用$x·x = x$简化： $= x + xz + xy + yz$
    
3. 利用$x + xz = x(1 + z) = x·1 = x$，因为 $1 + z = 1$，同理 $x + xy = x$： $= x + yz$

 9.Absorption
- (a) $x + xy = x$ 
- (b) $x(x + y) = x$
#proof $x\cdot{(x+y)}=x\cdot x+x\cdot y=1\cdot x+xy=x(1+y)=1\cdot x=x$

10. Theorem 5
- (a) $xy + xy' = x$      
#proof $x(y+y')$ 
- (b) $(x + y)(x + y') = x$
#proof 提取公因式 $x+$ : $x+yy'=x$

 11.🦋Theorem 6
- (a) $x + x'y = x + y$       
#proof $(x+x')(x+y)=1\cdot(x+y)=x+y$
- (b) $x(x' + y) = xy$

可类比 **+** 为 Union 并集。类比 **·** 为 Intersect 交集。

### DeMorgan's Law
- (a) $(x+y)'=x'y'$
- (b) $(xy)'=x'+y'$
> **Exercise**
> ![5ef65baa07f3c6b311ea2e16e0a9bd2b.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/5ef65baa07f3c6b311ea2e16e0a9bd2b.jpg)
### XOR Properties
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923153409435.png)
XOR 可视为比较两个数，相同取 0，不同取 1🐾
> $x\oplus y=x'y+xy'$

$x\odot y=x'y'+xy=(x\oplus y)'$ #XNOR
## Minterm & Maxterm
### 最小项（Minterm）
- 定义：**n 个变量的"AND运算（乘积）"项**，其中**每个变量恰好出现一次**（要么原形式 a，要么反形式 a'，但 a 和 a'不同时出现）
- 符号：用 $m_i$ 表示（i 是编号）
### 最大项（Maxterm）
- 定义：**n 个变量的"OR运算（和）"项**，其中**每个变量恰好出现一次**
- 符号：用 $M_i$ 表示
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923160154506.png)
> 最小项 / 最大项的编号 i，是 “变量取值组合” 的**十进制等效值**

### 写Minterm 形式
#### Method 1
根据 logic equation 依次代入，写出 truth table
从 truth table 里找出为 1 的 input 组合，写成 minterm 的形式
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250929202311496.png)
> sum of minterms

#### Method 2
步骤：乘上 A+A'，直到出现所有变量，之后写成 minterm 之和，再去掉重复的。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001221841693.png)

### Relationship
-  $m_{i}'=M_{i}$
> e.g.: $m_{0}=x'y'z'$; $m_{0}'=(x'y'z')'=x+y+z=M_{0}$
> if all the terms are indexed by 0-7,
> F= $\sum m(1.2.4.7)=\prod M(0.3.5.6)$

# Logic Optimization
## Criteria
How to define a "better" circuit?

| 指标        | 定义                         | 估算规则                                                                       |
| --------- | -------------------------- | -------------------------------------------------------------------------- |
| Delay     | 输入信号变化到输出稳定的时间，决定电路 “运行速度” | 每个 gate 延迟 = 1 gate-delay（忽略 inverters）寻找 ==critical path（经过 gate 数量最大的）== |
| Size/Area | 电路占用的物理资源，对应实现成本           | 每个门==输入==需 2 个晶体管                                                          |
| Power     | CMOS 电路，与开关频率、电压相关         | 动态功耗公式：$P = k \cdot C V^2 f$（k 为常数，C 为电容，V 为电压，f 为频率）                      |

## Boolean algebraic optimization
- （algebraic methods）
核心是利用布尔代数定律，减少项数和literals，从而减少 gate数量。

> e.g. **Two-level size optimization** using algebraic methods
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250930133314898.png)
> 原表达式 4 个项、12 个文字→化简后 2 个项、4 个文字；
> 对应晶体管从 24 个→8 个，延迟始终为 2 级（AND→OR）

> e.g. **Multi-level optimization**
> Sacrifices delay for smaller size
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250930133716632.png)

#效果 
- 主要优化目标：Size（减少输入数/晶体管数量）
- 如果表达式恰好化简成更少的层次，也可能顺带减小 Delay
## Two-level size optimization
- 形式：Sum of Products (AND-OR 或者 OR-AND) → 总是 **两级电路**
- Delay：**固定** 2 gate-delays
#效果 
- 目标：减少门输入数 → 优化 **Size**
- 常用工具：
    
    - 布尔代数（手工）
        
    - K-map：适合 ≤ 5–6 个变量
        
    - Quine–McCluskey：系统化算法

### K-map
#效果 得到最简 SOP
1. 形状：仅允许矩形或正方形（不允许 L 形）
2. 数量：每组包含 $2^N$ 个 “1”（N=0,1,2...，如 1、2、4、8 个）
3. 范围：尽量覆盖更多 “1”（减少项数），且所有 “1” 必须被覆盖
4. 边缘环绕：K-map 的左右边缘、上下边缘视为相邻
5. 5 个变量：把 E（第五个变量）控制不变，写两个 equation，在前面分别乘上 E 和 E'

![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001212057686.png)

> e.g.![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001212019178.png)

#### Prime Implicants (PI)
- Definition
		Implicant: a product term
		PI: a group that cannot be entirely contained by another implicant
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001214152276.png)

#### Essential PI
1. 根据“1”找。只能被一个 PI cover 的 1 使得这个 PI 成为 essential PI
2. 随后把这个 essential PI 写到 equation 里面
3. 剩下的没被 essential PI conver 的 1 用 non-essential PI 来覆盖（不需要用全部的 non-essential PI）
![4af6e4021d1c25c65dbba8ea4e77d87a.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/4af6e4021d1c25c65dbba8ea4e77d87a.jpg)

🐾补充：
此处先求 F'再 inverse 求 F，化简出来的表达式 input 更少（size 更小），delay 一样
> 题目写 simplest way 才需要这样考虑 F'
### Don't Care
- 定义：
某些输入组合 “不可能出现”（如十进制计数器的 10~15），其输出可视为 “任意值（0 或 1）”，标注为 X。
- 作用：
X 根据 “分组需求” 灵活视为 1 或 0。若视为 1 能合并更多 1，就当 1 用，从而减少项数。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250930104705837.png)
## Multi-level optimization
We don’t always need the speed of two level logic. Multiple levels may yield fewer gate
- 允许多于两级的逻辑，通过因式分解（提取公因式）减少文字数
#效果 
- 目标：进一步减小 **Size**
- 代价：**Delay 增加**
> 并不总是需要两级逻辑的速度——multi-level可能有更少的门
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001205053750.png)
## Critical Path
- 定义：电路里从输入到输出的最长延时路径
- 优化思路：
    - 如果目标是速度（减少 Delay），要缩短 critical path
    - 对非关键路径，可以multi-level，降低 Size
#效果 综合考虑 delay 与 size/power trade-off 的概念(==both==)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001210134889.png)

### Power Optimization
> 随着芯片上的晶体管数量越来越多，功耗的 “降低速度” 跟不上芯片 “尺寸缩小的速度”。这会导致一个直接问题：散热变得困难（芯片发热增多，冷却难度大幅提升）。

- CMOS 技术中的 “动态功耗”
> 在 CMOS 芯片里，导线从 0 电平切换到 1 电平的过程会消耗能量，这种功耗被称为 dynamic power。

动态功耗的计算公式为：$P = k \times C \times V^2 \times f$
- k：常数；
- C：导线的电容（电容越大，导线存储的电荷越多，切换时耗能越多）；
- V：电压（电压越高，电平切换的能量变化越大，耗电越多）；
- f：切换频率（单位时间内切换次数越多，总功耗越高）。

方法
 1. 降低电压：但会导致芯片运行变慢，且电压存在 “下限”（过低则芯片无法正常工作）
2. Use low-power gates
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251001222958316.png)

# Combination Circuit
## Definition
| 对比维度  | 组合逻辑电路（Combinational Circuit） | 时序逻辑电路（Sequential Circuit） |
| ----- | ----------------------------- | -------------------------- |
| 输出依赖  | 仅依赖「当前时刻的输入组合」，与历史状态无关        | 依赖「当前输入 + 电路历史状态」（有记忆）     |
| 电路结构  | 无反馈回路，无记忆单元（如触发器）             | 有反馈回路（输出回连输入），含记忆单元        |
| 功能复杂度 | 实现基础逻辑（如加法、选择、编码），分析简单        | 实现复杂功能（如计数、存储），分析难度高       |
| 典型例子  | Adder、MUX、Encoder/Decoder     | 计数器、寄存器、存储器                |
## 流程
1. Step 1：捕获功能（Capture the Function）
	- 目标：把 “实际需求” 转化为数字逻辑语言（真值表或逻辑方程）。
	- 选择原则：
	    - 输入变量少（如≤4 个）：用真值表（直观，不易错）
	    - 输入变量多（如≥5 个）：用逻辑方程
2. Step 2：转换为方程并化简（Convert to Equations）
	- 核心：从真值表 / 需求推导出逻辑方程，再用 “布尔代数” 或 “卡诺图（K-map）” 化简，减少逻辑门数量（优化 Size）和延迟（优化 Delay）。
3. Step 3：实现为逻辑电路（Implement as a Logic Circuit）
	- 目标：将化简后的逻辑方程，用 “与门、或门、异或门” 等基础门电路连接实现。
	- 关键技巧：可复用门电路（如多个输出共享一个与门），进一步优化电路规模。
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017011043509.png)

## Combinational Building Blocks
### Multiplexor (MUX)
- 核心功能：根据「选择信号」从 N 个数据输入中选 1 个输出。
- 标记：*2 to 1* or *2 by 1* or *2 x 1* mux
- 关键参数：
    - 数据输入数 N → 选择信号位数 k，满足 $k = \log_2 N$（如 2 输入→1 位选择位，4 输入→2 位选择位）
    - 扩展：可组成 “N 位 MUX”（如 4 位 2 选 1 MUX，用 4 个 2 选 1 MUX 共享 1 个选择位，同时处理 4 位数据
#### Internal Design
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017123715227.png)
- 还可有 Bus 代替 input（几个 bit 合成一个 bus） #bus
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017124316374.png)

#adder
### Half Adder
- 核心功能：实现「2 个 1 位二进制数的加法」
- 输入 / 输出：
    - 输入：加数 A、被加数 B；
    - 输出：本位和 Sum（A⊕B）、向高位进位 Carry（AB）；
- 电路：1 个异或门（算 Sum）+1 个与门（算 Carry）

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017124916901.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017124850458.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |

### Full Adder
- 核心功能：实现「3 个 1 位二进制数的加法」
- 化简：$\text{Sum} = \text{A'B'C} + \text{A'BC'} + \text{AB'C'} + \text{ABC}= \text{A'}(\text{B'C} + \text{BC'}) + \text{A}(\text{B'C'} + \text{BC})$ 
	令 $\text{D} = \text{B} \oplus \text{C}$，则式子变为：$\text{Sum} = \text{A'D} + \text{AD'}$
	而 $\text{A'D} + \text{AD'}$ 也是**异或运算**（$\text{A} \oplus \text{D}$）
    将 $\text{D} = \text{B} \oplus \text{C}$ 代回，最终得：$\text{Sum} = \text{A} \oplus \text{B} \oplus \text{C}$
    $\text{Carry} = \text{A'BC} + \text{AB'C} + \text{ABC'} + \text{ABC}= (\text{A} \oplus \text{B})\text{C} + \text{AB}$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017125705444.png)
化简后变成两个 Half Adder 和一个 OR gate

### Carry-Ripple Adder
- 核心功能：实现「多位二进制数加法」
- 原理：将多个全加器 “串联”，前一个全加器的 “进位输出” 作为后一个的 “进位输入”（进位像波浪一样传递，故称 “行波”）
- 特点：电路简单，但延迟由 “进位路径” 决定（4 位加法器需 4 个全加器延迟）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017131956805.png)

### Two's Complement Adder/Subtractor
XOR Gate 自带检查 overflow
#### Subtractor
别名：Lookahead Adder
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017143455603.png)
##### 4-Bit 2's Complement Subtractor
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017144216168.png)
> C0 补的是 1（根据 2's complement 规则）
#### Adder
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017144625824.png)

| 对比项   | Carry-ripple Adder | 补码加法器        | 补码减法器                 |
| ----- | ------------------ | ------------ | --------------------- |
| 运算对象  | 无符号数 / 简单二进制数      | 有符号数的补码      | 有符号数的补码（减法转加法）        |
| 减法支持  | 不直接支持（需额外逻辑）       | 不直接支持（只做加法）  | 支持（通过 “取反 + 加 1” 转加法） |
| 最低位输入 | 通常为 0              | 0            | 1                     |
| 取反逻辑  | 无                  | 无            | 对减数 B 每一位取反           |
| 溢出检测  | 一般不内置（需额外扩展）       | 内置（检测补码加法溢出） | 内置（同补码加法，因本质是加法）      |

### Arithmetic-Logic Unit (ALU)
- 定义：ALU是一种硬件组件，能根据控制输入，执行多种算术运算 (add, subtract, increment, etc.) 和多种逻辑运算 (AND, OR, etc.)
- 地位：Key component in computer，所有数据的算术 / 逻辑运算都由 ALU 完成。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017161554585.png)

### Encoder
- 核心功能：将 “多个独立输入信号” 编码为 “二进制代码”
- 应用：键盘编码（每个按键对应 1 个输入，编码为二进制码送 CPU）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017161824328.png)

### Decoder
- 核心功能：与编码器 “反向”，将 “二进制代码” 解码为 “唯一的输出信号”（如 3 位输入→8 个输出 *3 by 8 decoder*）
- 规则：1 组输入代码，仅 1 个输出有效
> one-hot: 输出只有一个 1, 与之相对 one-cold 输出只有一个 0
- 应用：数码管显示（二进制码解码为 “哪段灯管亮” 的信号）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017163746470.png)
> $e$ 是 enable signal
> 当是 1 的时候完成正常功能，是 0 的时候 block
- Minterm Generator: 输出的都是 minterm
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017164439314.png)

### Buffer
- Somewhat like a NOT gate without complementing the binary value 
- Amplify the driving capability of a signal 
	数字信号经过长导线传输会衰减，buffer 可以把信号恢复到原来的值，比如把 0.9 恢复到 1，但是衰减需要在一定范围内否则会被错误识别
- Insert delay 
- Protect input from output
	如果被电击了，buffer 会坏但是其他不会
- 需要供电
#### Tri-state Buffer
C 可能有三种值：0，1，Z
- Z 为 high impedence，没有电压，可能显示为 0
input 不能为 Z，但是 output 可以
floating input 带来 uncertainty，很危险

| Buffer                                                                                              | Tri-state Buffer                                                                                    |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017164959047.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017165016461.png) |
- 三种 Tri-state Buffers
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017170823464.png)
	第二种：Tri-state inverter 输出 invert
	第三种：Control signal invert

> e.g. 双向 I/O 口（如单片机的总线，既可以输入又可以输出，靠三态缓冲器切换）
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017165823707.png)

#### Application
![59903e4deaa416c1e3d5fa9617e68fb1.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/59903e4deaa416c1e3d5fa9617e68fb1.jpg)
- 在一个时刻只有一位 slave 能和 master 对话，通过 decoder 来控制
- 图上只经过一个 buffer，可以被 MUX 代替，但 MUX 特别大时延迟特别大

# Latches and Flip Flops
## Sequential Circuit
- Combinational circuit with *feedbacks*
- Decisive factor: 
	- Present inputs
	- Past input sequence 
	- Past outputs sequence
### Timing Concepts
- 输入 - 输出传播延迟（*input-output propagation delay*）：输入信号变化后，输出信号相应变化的延迟时间（由逻辑门、导线的延迟决定）。
- clock：为时序电路提供 “同步节拍”，让电路各部分在特定时间（如时钟上升沿 / 下降沿）更新状态（如触发器、寄存器的工作依赖时钟）。
- Other timing issues
> 如 “建立时间（Setup Time，输入需在时钟沿前保持稳定的时间）”“保持时间（Hold Time，输入需在时钟沿后保持稳定的时间）”等，这些是保证时序电路正确工作的约束

- Asynchronous: Control signal that **not** depend on clk signal
control signal 是 boss，控制变化
> * Latch, 给 FF 加的 Clear 等等 [[Course/270/Lecture Notes#Asynchronous Control Input|Lecture Notes]]

- Synchronous: Control signal that depend on clk signal
> * FF，给 FF 加的 Synchronous Clear 等等 [[Course/270/Lecture Notes#Synchronous Control Input]]
### Representation
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018002158050.png)

### Clock Signal
> A typical control input
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018145952493.png)
1. 时钟信号的作用
时钟是周期性脉冲序列（Periodic pulse train），用于同步时序电路的行为。

2. 时钟波形的关键元素
	- Rising edges：信号从低电平跳变到高电平的瞬间；
	- Falling edges：信号从高电平跳变到低电平的瞬间；
	- Duty cycle：高电平持续时间占整个周期的百分比（图中为 50%）

3. 时钟的核心参数
	- Clock period（时钟周期）：脉冲之间的时间间隔（即 “一次完整循环的时间”）
	- Clock cycle（时钟周期 / 循环）：与 “Clock period” 同义，指一次 “高电平→低电平→高电平” 的完整时间间隔
	- Clock frequency：周期的倒数
## Latch
### First Attempt
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017231229642.png)
> 尝试1：将输出连回输入

### SR Latch
-  结构：输入为 `S（Set）` 和 `R（Reset）`，输出为 `Q` 和 `Q'`（互补）。
- 工作逻辑（NOR 门实现，高电平有效）
    - `S=1，R=0` (Set)：Q=1，即使 S 变回 0，Q 仍保持 1（记忆）
    - `S=0，R=1` (Reset)：Q=0，即使 R 变回 0，Q 仍保持 0（记忆）
    - `S=0，R=0` (Hold)：Q 保持当前状态
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017232116906.png)
#### 禁用状态
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251017235530026.png)
同时按下 S、R 后，Q 会振荡。由于门的 delay 仍有差异，最后会停在 0 或 1

#### Alternative Implement of SR Latch
- 用NAND Gate 来实现
![a9634af1b9028614f0f8eca3d94c7f37.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/a9634af1b9028614f0f8eca3d94c7f37.jpg)
图中两处表明 S 和 R 的功能有反转
#### Gated SR Latch
- 问题：普通 SR 锁存器只要 S/R 变化就响应，容易受干扰（如输入毛刺）
- 改进：增加G（Gate Control），只有 G=1 时，S/R 才生效；G=0 时，无论 S/R 如何变化，Q 保持不变（锁存状态）。

| G   | S     | R   | $Q^+$ | 功能           |
| --- | ----- | --- | ----- | ------------ |
| 0   | X（任意） | X   | Q（保持） | Latch Locked |
| 1   | 0     | 0   | Q（保持） | Hold State   |
| 1   | 0     | 1   | 0     | Reset State  |
| 1   | 1     | 0   | 1     | Set State    |
| 1   | 1     | 1   | X（禁用） | Not Allowed  |
### Gated D Latch
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018143840821.png)
- 功能：Transparent Latch 当 G 是高电平的时候 D 被 copy 到 Q
	- 元件：D combine S and R into one input
		Inverter 防止 S 和 R 同时为 1
	- D=0: Reset 
	- D=1: Set 
	- G=0: Hold
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018145747604.png)

## Flip Flop
### D Flip Flop
> Rising-Edge Triggered D Flip Flop
- 功能：当 clk 出现 rising edge 的时候，Q copy D
- Properties: Edge Sensitive
- 被更广泛使用，因为 register 里是 D FF
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018152003456.png)

1. Master（主锁存器）：
    1. 当时钟CLK=0时，Master透明（G1=1），跟随输入D的值（P = D）。
    2. 当时钟CLK从0→1（上升沿），Master锁存（G1=0），保存上升沿瞬间的D值。
2. Slave（从锁存器）：
	1. 当时钟CLK=0时，Slave锁存（G2=0），保持之前状态（Q = Q2）。
	2. 当时钟CLK=1时，Slave透明（G2=1），将Master锁存的值（P）传递到输出Q（Q = P）

![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018151950510.png)

![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018152211228.png)
#### Application 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018160444506.png)
当一个时钟上升沿到来时：

1. D1读取Y，D2读取Q1，D3读取Q2，D4读取Q3。
    
2. 它们同时更新各自的输出。
    
3. 到下一个时钟上升沿到来之前，Q1, Q2, Q3, Q4 的值都保持不变。

#### Application 2
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018160559872.png)

### J-K Flip Flop
> Rising Edge-triggered J-K Flip Flop
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018160655333.png)
- 行为
    
    - `J=0, K=0`: 保持 (Hold)。`Q`的值不变
        
    - `J=0, K=1`: 复位 (Reset/Clear)。`Q` 强制变为 `0`
        
    - `J=1, K=0`: 置位 (Set)。`Q`强制变为 `1`。
        
    - `J=1, K=1`: 翻转 (Toggle)。`Q`的值翻转

## T Flip Flop
> Rising Edge-triggered T Flip Flop
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018161138550.png)
- 行为:
    - 如果 `T=1`（toggle），时钟沿到来后，输出 `Q` 会翻转
        
    - 如果 `T=0`（hold），时钟沿到来后，输出 `Q` 保持不变
        
- 主要用途:
    - 二进制计数器: 将T输入端固定为高电平（T=1），每个时钟周期`Q`都会翻转一次，实现了对时钟脉冲的计数。
        
    - 分频器: 一个T触发器（T=1）的输出频率是输入时钟频率的一半。

##  Asynchronous Control Input
Set 和 Clear 两个 control signal 执行功能时不用考虑 D 和 clk^1

|                                                                                                     |                                                                                                                                                    |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018165523776.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018165627313.png)<br>Set 和 Clear 上方的横杠以及 bubble 表示：当为 0 时，执行正常功能 |
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018170430575.png)

##  Synchronous Control Input
Sync_clear 需要在有 rising-edge 并且自己 active 的时候才能 clear

|                                                                                                         |                                                                                                         |
| :------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------ |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018170444428.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018170521188.png)<br> |

## 对比
| 对比维度  | 锁存器（Latch）                         | 触发器（Flip-Flop）                    |
| ----- | ---------------------------------- | --------------------------------- |
| 触发方式  | level sensitive                    | edge sensitive                    |
| 同步性   | *asynchronous* to the clock signal | *synchronous* to the clock signal |
| 抗干扰能力 | 弱（有效电平期间，输入毛刺会导致输出误变）              | 强（仅边沿采样，毛刺不影响）                    |
| 核心逻辑  | 透明模式（有效时输出跟随输入）                    | 边沿采样（仅瞬间响应，其他时间锁存）                |
| 结构复杂度 | 简单（1 级交叉耦合门，如 SR 锁存器）              | 复杂（2 级主从锁存器，如 D 触发器）              |
| 工程应用  | 异步电路（如总线缓冲、低功耗设计）                  | 同步电路（如 CPU 寄存器、计数器、状态机）           |
| 控制    | 除 clk 外还需要控制其他因为 asynchrnous       | 只需要控制 clk                         |
## Register
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018161742733.png)

> e.g.
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018161933559.png)
> - 需求：传感器输出 5 位温度数据，每小时记录一次，显示 “2 小时前、1 小时前、当前” 三个温度；
> - 方案：用 3 个 “5 位 register”（每个寄存器含 5 个 D FF），共享 每小时一次的 clk
    - 时钟触发时，当前温度存入 “当前寄存器”，原 “当前寄存器” 数据移到 “1 小时前寄存器”，原 “1 小时前” 数据移到 “2 小时前寄存器”；
    - 三个寄存器的输出分别接三个显示器，实现历史温度存储与显示。

### With  Synchronous Parallel Load
- 功能：
	当 load 等于 0，无论输入什么，Q 不变
	当 load 等于 1，Q 跟随 input 变化

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251018170829777.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022183655994.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |

# Verilog HDL
## Module & Port
Verilog 核心单元是 Module，数字电路功能均封装在 Module 内，每个 Module 包含三部分：
1. Port Declaration：定义模块的input, output，是模块与外部交互的接口
    - 例：半加器（Half Adder）模块 `Add_half` 的端口为 `sum`（输出，和）、`c_out`（输出，进位）、`a`（输入，加数 1）、`b`（输入，加数 2）
2. Internal Signal Declaration：用 `wire` 定义模块内部的连接信号（如 `c_out_bar`），`wire` 仅用于传递组合逻辑信号，不能存储数据（不是真正的电线）
3. Logic Implementation：通过 门实例化、assign 语句、always 块等方式描述电路逻辑，所有逻辑*并行*执行（顺序不影响结果）
### Half Adder (用 HDL)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022230037358.png)

### Full Adder
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023000334211.png)

```verilog
module Add_full (sum, c_out, a, b, c_in); // Parent Module
input  a, b, c_in; 
output sum, c_out;
wire   w1, w2, w3; // 内部wire：连接子模块的中间信号

// child module（两种方式）
Add_half M1 (w1, w2, a, b); // 按顺序关联（positional）：端口顺序与Add_half定义一致
Add_half M2 (.sum(sum), .c_out(w3), .a(w1), .b(c_in)); // 按名称关联（named）：顺序无关
or       (c_out, w2, w3); // primitive instantiation
endmodule
```

### Carry-Ripple Adder
#### Structural Model
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023001144496.png)
```verilog
// 4bit
module Add_rca_4 (sum, c_out, a, b, c_in);
output [3:0] sum; // 4-bit bus
output       c_out;
input  [3:0] a, b; //  4-bit bus
input        c_in;
wire         c_in2, c_in3, c_in4; //

Add_full M1 (sum[0], c_in2, a[0], b[0], c_in); // 最低位（bit0）
Add_full M2 (sum[1], c_in3, a[1], b[1], c_in2); // bit1，进位c_in2来自M1
Add_full M3 (sum[2], c_in4, a[2], b[2], c_in3); // bit2
Add_full M4 (sum[3], c_out, a[3], b[3], c_in4); // 最高位（bit3），进位c_out输出
endmodule

// 16bit
module Add_rca_16 (sum, c_out, a, b, c_in);
output [15:0] sum;
output        c_out;
input  [15:0] a, b;
input         c_in;
wire          c_in4, c_in8, c_in12;

Add_rca_4 M1 (sum[3:0],  c_in4,  a[3:0],  b[3:0],  c_in); // 第1个4位（bit3~0）
Add_rca_4 M2 (sum[7:4],  c_in8,  a[7:4],  b[7:4],  c_in4); // 第2个4位（bit7~4）
Add_rca_4 M3 (sum[11:8], c_in12, a[11:8], b[11:8], c_in8); // 第3个4位
Add_rca_4 M4 (sum[15:12],c_out,  a[15:12],b[15:12],c_in12); // 第4个4位
endmodule
```

> [3:0]的bus 代表 $s_{3}$ 是 MSB
> [0:3]的bus 则代表 $s_{0}$ 是 MSB
#### RTL
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023012347690.png)
### Comparator
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023001850033.png)
#### Structural Model
先画出 Gate-level Schematic，再直接用逻辑门（`and`、`or`、`not`、`xnor`）实例化，完全映射硬件电路
```verilog
module compare_2_str (A_lt_B, A_gt_B, A_eq_B, A0, A1, B0, B1);
input  A0, A1, B0, B1;
output A_lt_B, A_gt_B, A_eq_B;
wire   w1, w2, w3, w4, w5, w6, w7; // 可以implicitly declare（不写出来）

//Order does NOT matter
//All gates operate in parallel when inputs change
not  (w6, A1); 
not  (w7, A0); 
xnor (w4, A1, B1); 
xnor (w5, A0, B0); 
and  (A_eq_B, w4, w5); 
and (w1, w6, B1); 
and (w2, w6, w7, B0); 
and (w3, w7, B1, B0); 
or  (A_lt_B, w1, w2, w3); 
nor (A_gt_B, A_lt_B, A_eq_B); 
endmodule
```
- 适用场景：需精确控制硬件结构（如面积优化），但设计效率低

#### RTL Model
> Register Transfer Level

用 `assign` 语句直接描述布尔方程或数据操作，不关注具体门级结构
```verilog
// 方式1：直接写布尔方程
module compare_2 (A_lt_B, A_gt_B, A_eq_B, A1, A0, B1, B0);
input  A1, A0, B1, B0;
output A_lt_B, A_gt_B, A_eq_B;

// continuous assignment statements
// All concurrently excuted
assign A_lt_B = (~A1)&B1 | (~A1)&(~A0)&B0 | (~A0)&B1&B0;
assign A_eq_B = (~A1)&(~A0)&(~B1)&(~B0) | (~A1)&A0&(~B1)&B0 | A1&A0&B1&B0 | A1&(~A0)&B1&(~B0);
assign A_gt_B = A1&(~B1) | A0&(~B1)&(~B0) | A1&A0&(~B0);
endmodule

// 方式2：用拼接运算符（Concatenation）简化
module compare_2_logic (A_lt_B, A_gt_B, A_eq_B, A1, A0, B1, B0);
input  A1, A0, B1, B0;
output A_lt_B, A_gt_B, A_eq_B;

// {A1,A0}：将A1（高位）和A0（低位）拼接成2位总线🐾
assign A_lt_B = ({A1, A0} < {B1, B0});
assign A_gt_B = ({A1, A0} > {B1, B0});
assign A_eq_B = ({A1, A0} == {B1, B0});
endmodule
```
- 核心：`assign` 语句用于组合逻辑，信号类型为 `wire`；拼接运算符 `{}` 可将多个 1 位信号组合成总线，简化代码

#### Behavioral Model
用 `always` 块描述算法流程，不关注硬件结构和数据流动，仅定义 “输入→输出” 的逻辑关系，设计效率最高
```verilog
module compare_2_algo (A_lt_B, A_gt_B, A_eq_B, A, B);
input  [1:0] A, B; // 直接定义2位总线输入（A=[A1,A0], B=[B1,B0]）
output A_lt_B, A_gt_B, A_eq_B;
reg    A_lt_B, A_gt_B, A_eq_B; // always块内赋值的变量需定义为reg类型

// Sensitivity List
always @(A or B) //A 或 B 变化的时候就会执行下面的语句
begin 
  A_lt_B = 0; // 初始化（避免Unwanted Latch）
  A_gt_B = 0;
  A_eq_B = 0;
  
  if (A == B)      A_eq_B = 1; // A=B时，A_eq_B置1
  else if (A > B)  A_gt_B = 1; // A>B时，A_gt_B置1
  else             A_lt_B = 1; // 其余情况（A<B），A_lt_B置1
//begin和end间为procedural statement excuted within always
end
//执行完后到开头wait 等下一个condition
endmodule
```
- 关键：`always` 块内的变量需为 `reg` 类型（`reg` 仅表示 “可存储值”，不一定是寄存器）；Sensitive list 需包含所有触发信号，避免遗漏导致逻辑错误

Simple Circuit 一般用coding, Complicated Circuit 需要设计电路
Behavioral level coding style 是一种downside 的自上向下的设计 high level of abstraction
不会知道运行速度有多快

### 2-to-1 MUX
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023181411119.png)

### DFF
```verilog
// Synchronous Reset：仅在时钟上升沿生效
module D_ff_sync (q, data_in, clk, rst);
input  data_in, clk, rst;
output q;
reg    q;

// 敏感列表：仅时钟上升沿触发
always @(posedge clk) begin
  if (rst == 1'b1) q < = 1'b0; // 复位有效（rst=1），q置0（同步于时钟）
  else             q < = data_in; // 否则，q采样data_in
end
endmodule

// Asynchronous Reset：复位信号有效即生效，无需等时钟
module D_ff_async (q, data_in, clk, rst);
input  data_in, clk, rst;
output q;
reg    q;

// 时钟上升沿 或 复位上升沿
always @(posedge clk or posedge rst) begin
  if (rst == 1'b1) q <= 1'b0; // 复位有效，q直接置0（无需等时钟）
  else             q <= data_in;
end
endmodule
```
> 💜注意：Asynchrnous reset 需要用 posedge 来触发 (active high)
### Register
```verilog
module D_reg4 (Data_in, clock, reset, Data_out);
input  [3:0] Data_in; // 4位输入数据
input        clock, reset;
output [3:0] Data_out; // 4位输出数据
reg    [3:0] Data_out; // 多bit reg（[3:0]表示4位）

// 异步复位，时钟上升沿触发
always @(posedge reset or posedge clock) begin
  if (reset == 1'b1) Data_out <= 4'b0000; // 复位时，4位均置0
  else                Data_out <= Data_in; // 时钟边沿采样Data_in
end
endmodule
```

- 规则：`reg` 类型变量仅当 “赋值同步于时钟边沿” 时，才会被综合成 **Flip-Flop**；若仅用于组合逻辑（如 always 块内无时钟），则可能综合成 `wire` 或锁存器

### Parameterized Module
用 `parameter` 定义可配置参数（如`size`、延迟 `delay`），实例化时可重写参数，无需修改模块代码
```verilog
// Parameterized Module
module Param_Examp (y_out, a, b);
parameter size = 8, delay = 15; // 默认参数：8位宽度，15单位延迟
output [size-1:0] y_out; // 输出宽度由size决定（8位→[7:0]）
input  [size-1:0] a, b;

wire [size-1:0] #delay y_out = a ~^ b; // wire有一定延迟 模仿reality
endmodule

// 模块实例化：重写参数
module Param_Top;
reg [7:0] b1, c1; // 8位输入（默认参数）
reg [3:0] b2, c2; // 4位输入（重写参数）
wire [7:0] y1_out;
wire [3:0] y2_out;

// 实例1：使用默认参数（size=8, delay=15）
Param_Examp G1 (y1_out, b1, c1);

// 实例2：重写参数（size=4, delay=5），按参数定义顺序传递
Param_Examp #(4, 5) G2 (y2_out, b2, c2);
endmodule
```
#### Parameter Annotation
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251023185927122.png)

- 优势：同一模块可适配不同需求（如 8 位 / 4 位数据处理），减少代码冗余

## Testbench
Tester 输入到 UUT (Unit Under Test)里，形成 Testbench，testbench 既没有输入也没有输出
注意变量类型！🐾
```verilog
module Test_Bench;
  parameter half_period = 50;

  wire Q;
  reg  D, clock, reset;

// tester
  D_ff UUT (Q, D, clock, reset);

  initial begin // one shot from top
    #0    clock = 0; D = 0; reset = 1;
    #100  reset = 0;
    #20   D = 1; #150  D = 0; #10    D = 1;
    #20   D = 0; #20    D = 1; #100  D = 0;
  end // time is accumulated

  always #half_period clock = ~clock;

  initial #1000 $stop;
endmodule
```

- 核心：
    - `initial` 块：用于生成一次性激励（如复位、输入数据变化），仅执行一次
    - `always` 块：用于生成周期性信号（如时钟），循环执行
    - `#time`：表示延迟（单位由仿真工具设定，如 ns）
    - `$stop`：仿真结束命令
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251031191230428.png)
## Procedural Assignments
左侧必须要是 `reg` 类型
### Blocking Assignment
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251031185108805.png)
后面的 statement 会被前面的 block 住，有顺序
### Nonblocking Assignment
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251031185206700.png)
前面的语句不会block 后面的，同时进行

## Unwanted Latch
当 always 块中的条件分支（如 case、if-else）不完整时，Synthesis会生成Latch，导致逻辑错误
- 错误示例：case 无 default，未覆盖所有输入组合
- 解决方法：添加 default 分支，或在 always块开头初始化变量（如 y_out = 0），确保所有情况下变量都有赋值
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251031190609137.png)
如果不加 y=0; 写成
```Verilog
if ({a2,a1} == 2'b11) y=1; else
if ({a2,a1} == 2'b01) y=0; else
if ({a2,a1} == 2'b10) y=0; else
if ({a2,a1} == 2'b00) y=0;
```
这样还是会有 unwanted latch
variable 有 4 种取值：'0' = 0 ; '1' = 1; 'x' = unknown; 'z' = high impedence;
再加上 else y=0; 才不会有 unwanted latch

# Counter
Verilog: [[Verilog#Counter]]
1. 计数格式多样性
	计数器是一种数字电路，可以不同格式对数值进行计数
	- binary
	- decimal（通常为 BCD 编码）：将十进制数用二进制数编码（如9编码为 `1001`），适用于数码管显示等 “人类直观读数” 的场景
	- signed：通过补码等方式表示正负（如 8 位有符号数范围 `-128~127`），用于需要正负计数的场景（如温度传感器的正负温度计数）
	- unsigned：仅表示非负数（范围 `0~2ⁿ-1`），是最常见的默认格式
2. 硬件构成：n 位计数器由 n 个触发器组成
3. 计数范围：最多计$2^n - 1$个整数
4. 计数方向：Up或Down
5. 分类：异步计数器 vs 同步计数器

| 类别                                  | 时钟同步方式                  | 优点                      | 缺点                                   | 典型场景                             |
| ----------------------------------- | ----------------------- | ----------------------- | ------------------------------------ | -------------------------------- |
| Asynchrnous Counter(Ripple Counter) | 前级 FF的输出（Q）驱动后级 FF的 clk | 结构简单，无需复杂组合逻辑           | 存在 “纹波延迟”，触发器状态更新不同步，计数频率受限（高速场景易出错） | 低速、简单计数（如 LED 流水灯的节拍控制）          |
| Synchronous Counter                 | 所有 FF共享全局 clk，同一时钟沿同步更新 | 无纹波 delay，时序稳定，可工作在高频场景 | 组合逻辑稍复杂（需推导每个触发器的 “下一状态” 逻辑）         | 绝大多数数字系统（如 CPU 的程序计数器、FPGA 的定时器） |

## Synchronous Counter
 - All FFs are triggered by the same clock
 - May be implemented by different types of flip-flops (D, T or JK, D 比较常用)
### Control Signal
#### Load
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102195130143.png)
- 功能：
	通过 `load` 信号(Synchronous)，将外部数据 `Dat` 直接加载到计数器，定制计数起始值（如从 8 开始计数，而非 0）
- 实现原理：
	在 D 触发器的 D 输入前加**2 选 1 MUX**：
	- 当 `load=1` 时，MUX 选择 `Dat`，把 Dat 作为计数起点
	- 当 `load=0` 时，MUX 选择 “Q+1”（正常计数）
#### CE
> Count Enable
- 功能：
	通过 `CE` 信号控制 “是否计数”
	- `CE=1`：时钟边沿计数（Q=Q+1）
	- `CE=0`：保持当前值（Q=Q）
- 典型用途：
	- 等待外部应答信号（如其他模块准备好后再计数）Wait certain acknowledge signal coming from other device
	- 多计数器级联（用前级的 `CEO` 控制后级的 `CE`）Concatenate small counters into bigger ones
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102195543998.png)
#### Priority

| reset | load | CE    | Action on the rising clock edge |
| :---- | :--- | :---- | :------------------------------ |
| 1     | X    | X<br> | Clear (Qn&lt;=0)                |
| 0     | 1    | X     | Load(Qn<=Dn)                    |
| 0     | 0    | 1     | Count                           |
| 0     | 0    | 0     | Hold                            |
### CEO
加一个 *output*
如果 CEO 为 1，说明 count 到了最大的数，还在 count
$CEO=CE\cdot Q_{n-1}\cdot Q_{n-1}\dots Q_{0}$
### Binary Counter
用 characteristic table 来设计 combinational circuit 的部分
Current Value 是 FF 的输出 Q, Next Value 作为下一个要输入 FF 的 input
用 Kmap 等方式设计电路

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102183302656.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102183315962.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
### Alternative
- High level (Register Transfer Language-*RTL*) design w/ combinational and sequential building blocks
- 需要 N-input AND gate to detect terminal count (*TC*) TC=1 when terminal number is reached
- 如果是 down counter, TC 就用 NOR 来实现（全是 0 输出 1）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102210749159.png)
加上 control signal
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102210801858.png)
> CE 或者 ld 为 1 的时候允许 register load
> 如果 ld 为 0 说明要+1，此时也要 load
> 如果 ld 为 1 但是 CE 为 0，load 优先级大于 CE
### Customize Counting Sequence
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102200820641.png)
想要 count 6 个数，0-5，通过 AND 实现
### Clock Devider
1. 基本功能
	时钟分频器的核心作用是降低时钟信号的频率，让 “快时钟” 变成 “慢时钟”
	- 频率降低，周期变大：时钟的Frequency 和 Clock Cycle 成反比（周期 = 1 / 频率）。分频器通过延长周期来降低频率
	- 支持奇数 / 偶数倍分频
	- 应用场景：为低速设备（如 LCD 显示、按键检测）提供匹配的时钟，同时*降低功耗*（高频时钟会增加电路功耗，低速场景无需高频）
2. 设计规则
	- n 是计数器的位宽（即触发器的数量）
	    - 若 $N=5$，需满足 $2^{2} < 5 \le 2^3$，因此 $n=3$
	- 计数器需在 0 到 $N-1$ 之间循环计数。例如 $N=5$ 时，计数序列是 $0 \to 1 \to 2 \to 3 \to 4 \to 0$，一个循环包含N个输入时钟周期，从而实现N倍分频
> Duty cycle 会改变 

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102204619413.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102204524477.png)<br> |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
### Special Counters
#### Ring Counter
- 核心特征：
	- 始终只有 1 个 FF为 1（如 4 位：1000→0100→0010→0001→1000）；
	- 状态数 = FF数（4 位有 4 个有效状态，利用率低）
	- D 输入表达式（4 位）：$D₃=Q₀，D₂=Q₃，D₁=Q₂，D₀=Q₁$（循环移位）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102205223126.png)
##### Alternitive
这种计数器是 one-hot，可以用 decoder 来实现
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102205402465.png)
#### Johnson Counter
one bit by one bit 地输入 1111，再输入 0000
- 状态转换时仅 1 位变化（低功耗、低干扰）
- 状态数 = 2× 触发器数（4 位有 8 个有效状态）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102205557523.png)
#### BCD Counter
- 仅计 “0~9”（十进制），跳过 1010~1111（6 个无效状态）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102205627699.png)

> 2-bit
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102205728428.png)

$BCD_{0}$ 是个位， $BCD_{1}$ 是十位
当个位变成 9，CE 被 triger，十位加一
highlight 的 AND 是保证 89->90 的时候不会被清零
## Asynchronous Binary Counter
- 又叫 Ripple Counter
- 不是被 global clock 控制

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102191203703.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102191242156.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
### Problem
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102195034999.png)
> Timing issue

# Register & Shifter
- Subsystem
	内含 combinational & sequential circuit
	一个完整的数字系统（如 CPU、数字信号处理器）可拆解为两个相互协作的子系统
	- Datapath: 负责processes、store、route数据 (如 register)
	- Controller: generate signals to control datapath components based on environment event and state of a circut
## Example
- 需求：显示 4 个 8 位数据（温度 T、平均油耗 A、瞬时油耗 I、剩余里程 M）
- MUX方案：用 4 个 register，需要用 32 wires 来用于输出
	对于 CPU 来说 pin 和算力都有限
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102213510001.png)
- Above-Mirror Display 方案：输出 8 bit 的数据，2 bit 的选择信号，1 bit enable，只需要 11 wires 来用于输出
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102214253480.png)
### Problem
但是如果需要储存 16 组 32 bit 的数据，MUX 会变得很大
同时导致 too much fanout，线可能相互接触，造成 signal connect，noise，error
以及如果 drive too much devices，signal 可能会下降，不够 strong
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102215258923.png)

## Register
### Register File
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102221805942.png)
把多个 register 打包
- 基本参数：`M×N` 寄存器堆表示M 个 N bit register（如 4×32 表示 4 个 32 位寄存器）
- 端口组成：
	- Write: load sth into memory（像 parallel load）
	    - W_data: 要写入的数据
	    - W_addr: 要写入的地址
	    - W_en: 是否允许写入
	- Read: 不被 clock 控制
	    - R_data: 读出来的数据
	    - R_addr: 要读的地址
	    - R_en: 是否允许读
以 array 形式存在，类似 memory
- Write 和 Read 可以在同一个 clock cycle 里发生（作用于同一个 register）
- Read 的时候如果 Write 了新的数据，读到的东西会变

|       | 31  | 30  | ... | 1   | 0   |
| :---- | :-- | :-- | :-- | :-- | --- |
| reg 0 |     |     |     |     |     |
| reg 1 |     |     |     |     |     |
| reg 2 |     |     |     |     |     |
| reg 3 |     |     |     |     |     |
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102222833299.png)

### Shift Register
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251102223048361.png)
- n bit 需要 n clock cycle to update the data
- $Q_{0}$ 之后就 lost 了
- 可以 shift left or right
- 普通 register n-bit 共存于一个 rising edge
- 可以在 DFF 上加上 control input (e.g. Parallel load control input)

### Rotate Register
与普通 shift的区别：最低位的 `Q` 反馈到最高位的 `D` ，数据循环移位（无新数据输入）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251120225857973.png)

### Universal Shift Register
- Muti-functional, DFF 前加一个 MUX
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251120231037650.png)
#### Funtions
| Sh(Shift) | L(Load) | Action      |
| :-------- | :------ | :---------- |
| 0         | 0       | no change   |
| 0         | 1       | load        |
| 1         | X       | shift right |
- Shift 有更高的优先级
- 还有一种 input 组合，可以增加其他功能
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251120231341422.png)

## Example (Revisit)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251120231549221.png)
- 现在输出不需要 8 个 bit 了，只需要 1 个 bit
- 虽然 longer time 但是影响不大
## Shifter
- 注意：Shifter≠Shift Register
- Shifter是 Combinational *Datapath* component(无存储功能，即时移位), Shift Register是时序逻辑部件(有存储, clock触发)
- 基本功能：对输入数据进行 左移 / 右移 n 位，移位后的数据即时输出（无时钟依赖）
- 关键应用：
    - 左移 1 位 = 乘以 2（如`0011`→`0110`）
    - 右移 1 位 = 除以 2（如 `0110` → `0011`）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128214732927.png)
> 可通过 MUX 来进行功能的组合

### Example
1. 摄氏度→华氏度近似转换
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128215050434.png)
- 硬件实现：8 位 shifter（C 左移 1 位）+ 8 位 adder（加 32），大幅简化电路。

1. 温度平均值计算
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128215330396.png)
> 虽然 shift 进去了两个 0，但是只用写一个

### Chain Shifter
- 目标： 设计一个移位器，它不仅仅只能移位 1 位，而是可以根据指令向左移动 任意位
- 初级设计的缺陷
    - 如果在一个单独的步骤中完成所有可能的移位(0, 1, ...7), 硬件电路会变得非常复杂
    - 这需要大量的连线和巨大的MUX，因为每个输出位都需要能够接收来自任何输入位的信号。导致too many wires的问题
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128224900926.png)
这种设计的核心优势在于模块化和效率。
如果要移位 0-7 位：
- 暴力法需要一个巨大的 8 to1 MUX   
- 级联法只需要 3 个简单的 2 to1 MUX（分别决定是否移4、移2、移1）
- 这种结构更节省芯片面积，且易于扩展（例如要支持 32 位移位，只需增加移 16 和移 8 的层级即可）

# FSM
## Subsystem
subsystem 既包含 sequential circuit 又包含 combination circuit

| Subsystem  | 核心功能                                                                                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Datapath   | 负责数据的*存储、传输、加工*（如 register存储、ALU 运算、shifter操作）Datapath routes data to particular destination device according to control signals generated by controller |
| Controller | 以FSM 为核心，generates signals to control datapath based on environment event or state of a circuit                                                          |
## Definition
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128234541291.png)
> state transition 的时候有 delay
> clk^上面的^是 rising edge

### Clock Implicit
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128235305902.png)
- Synchronous FSM: 所有 condition 都有 clk posedge, 所以可以把 clk 省略, default condition
- Asynchronous FSM: 不常见
### Components
1. A set of states
	 Ex: {Off, On 1, On 2, On 3}
2. A set of inputs, a set of outpus
	 Ex: Inputs:{b}, Outpus:{x}
3. An initial state
	 Ex: "Off"
4. A set of transitions
	 Describes next states. Ex: Has 5 transitions
5. A set of actions (outputs)
	 Sets outputs while in states. Ex: x=0, x=1, and x=1

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129000330708.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129000341070.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
  ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129160405762.png)

### Common State Transition Property
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129175810066.png)
 1. 互斥性：同一状态的所有转换条件，任意时刻最多只有 1 个为真
2. 完备性：所有可能的输入组合，都有对应的转换路径（无 “无定义输入”）

### Standard Architecture
-  State register -- to store the present state 
- Combinational logic -- to compute outputs and next state 
- Known as controller
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130162014227.png)

### Step
1. 步骤 1：捕获 FSM 行为（画状态图）
	明确需求→定义状态→标注转换条件与输出，形成可视化的 state diagram
2. 步骤 2：确定架构参数 Capture the architecture
	- 计算状态寄存器位数：若有 `m` 个状态，需 `n=⌈log₂m⌉` 位寄存器（如 4 个状态需 2 位：`s1s0`）
	- 明确输入 / 输出信号：输入 `b`（1 位），输出 `x`（1 位），时钟 `clk`，复位 `reset`
3. 步骤 3：Encode the states
	将抽象状态映射为具体的二进制值
	- 二进制编码（文档示例）：Off=00，On1=01，On2=10，On3=11（2 位实现 4 个状态，节省寄存器资源）
	- One-hot码（工程常用）：Off=0001，On1=0010，On2=0100，On3=1000（用更多寄存器换更简单的组合逻辑，适合高频场景）
4. 步骤 4：创建 state table（current state→next state + output）
示例：

| 现态（状态名） | s1s0 | 输入 b | 次态 n1n0 | 输出 x |
| ------- | ---- | ---- | ------- | ---- |
| Off     | 00   | 0    | 00      | 0    |
| Off     | 00   | 1    | 01      | 0    |
| On1     | 01   | 0/1  | 10      | 1    |
| On2     | 10   | 0/1  | 11      | 1    |
| On3     | 11   | 0/1  | 00      | 1    |

5. 步骤 5：实现组合逻辑（推导布尔方程 + 硬件）
	从状态表提取 next state 和 output的 equation
## Example
### Output Special Patter
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251128235001609.png)
> 外面一个大箭头指进来是 initial state
### FSM with Input
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129155707767.png)

### Secure Car Key
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129163732449.png)
汽车钥匙需向车载电脑传输特定 ID（如 1101），仅当 ID 正确时允许启动发动机
ID holder in the car 是 shift register, shift in parallel out
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129164506537.png)
问题：如果 a 摁的比较久，会重复发送，repeat for several round

#### 完整版
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130171831526.png)

### Digital Clock
#### Attempt 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129164748529.png)
> 问题 1: 同时按三个按钮可以解锁

#### Attempt 2
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129164908706.png)
> 问题 2: arbg=1111，会有两种条件为 true

#### Attempt 3
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251129180054533.png)
> a 分为 aD'; a'; aD （r'+b+g 是 D）

### Synchronous Binary Counter
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130161136255.png)
- DFF 相当于一个 state register，输出的是 current state，进去的是 next state

### Push Button

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130163538744.png)     | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130163604121.png)     |
| :------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------ |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130163901508.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130164108767.png)<br> |
- x 只依赖 present state
### Button Press Synchronizer
无论按下按钮多久，只输出一个 clk cycle 的信号，且 rising edge 同步
- 比较：把 button 连在 counter 的 clk 还是 CE？
- A: 放在 CE，因为实际情况按钮会有震荡
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130165429129.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130165446807.png)
> C 状态是为了 check whether the button is released
> 2 bits 会有 more than enough 的 state

### Sequence Generator
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130171753559.png)

### Flight-Attendant Call Button Circuit
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130171917533.png)
> 按钮是 input, Q 是 present state，D 是 next state

# FSM Optimizations
## Mealy & Moore

| 维度    | Moore FSM              | Mealy FSM               |
| ----- | ---------------------- | ----------------------- |
| 输出依赖  | 仅依赖当前状态                | 依赖当前状态 + 当前输入           |
| 架构图   | 输出逻辑仅接 “当前状态”          | 输出逻辑接 “当前状态 + 输入”       |
| 状态图表示 | 输出标注在状态节点内（如 “S0/y=0”） | 输出标注在转换箭头上（如 “x=1/y=1”） |
| 输出稳定性 | 稳定（状态不变则输出不变，无毛刺）      | 可能有毛刺（输入变→输出变，无需等时钟）    |
| 状态数   | 通常更多（需为不同输出分配单独状态）     | 通常更少（同一状态可因输入输出不同值）     |
Mealy 和 Moore 区分的是 output
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130194335715.png)
### Moore

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130194529578.png) 之前看见的都是 Moore | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130194549387.png) |
| :---------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
### Mealy
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130195107578.png)<br>在 behaviour 中 In 有点类似于 asynchronous | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130195142634.png)<br> |
| :----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
### Example
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130195329056.png)
#### Mealy
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130195353903.png)
> A0 是 1st bit; A1 是 2nd bit; A2 是 3rd bit
> A3 回到 second bit

经过优化：

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130200308207.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130200332632.png) |
| :-------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130200357731.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130200412409.png) |
#### Moore
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130200743106.png)
> 等到 state transition 才会输出 1, slower response
> Mealy: 一收到第三个 input 就会 output 1

经过优化：

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130201349095.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130201404238.png) |
| :-------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130201416211.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130201504011.png) |
### Summary
1. Output
	- Mealy: depends on both inputs and presents
	- Moore: doesn't depend on inputs 
2. State Diagram
	-  Mealy: less states -> potentially less number of flip-flops
	- Moore: more states than Mealy -> possibly bigger circuit 
3. Speed of output response to the inputs
	- Mealy: quick, as soon as input changes
	- Moore: as long as one clock cycle delay 
4. TIMING ISSUE
	- Mealy: asynchronous, may cause serious problem
	- Moore: synchronous, more stable 
#### Standard Architecture
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130210632203.png)

#### Combination
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130210709599.png)
- Next state logic 没有区别
- 只是 output 有区别
## State Reduction
状态化简是 FSM 优化的第一步，核心是合并等价状态，减少状态数以降低寄存器数量
	核心概念：Equivalent States
		定义：若两个状态满足 “所有输入序列下，输出完全相同且次态完全对应”，则为等价状态 (Functionally same)
	Goal: Reduce number of states in FSM without changing behavior
		Fewer states potentially reduce *size* of state register
> e.g.![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130174329323.png)

### Method 1
把 state table 写成如下图，对比 next state 和 outputs
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130174848102.png)

### Method 2: Implication Tables
1. Construct a table of state pairs
	- 横向 / 纵向列出所有状态（排除对角线，因状态与自身必等价）

2. 标记 “输出不同” 的状态对（直接不等价）
	- 若两个状态对任意输入的输出不同，直接标记 “X”（非等价）；
	    例：Moore FSM 中，S0 输出 y=0，S1 输出 y=1→（S0,S1）标记 X
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130175653914.png)

3. Next State的状态对
- 列出*所有输入对应*的 next state i.e.输入是 0 列一行，输入是 1 列一行
- 若某状态对的次态对中存在 “已标记 X 的不等价状态对”，则该状态对也标记 X
- 重复此步骤至无新标记，剩余未标记的状态对即为等价状态
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130180109755.png)

4. 没有被打叉的格子 state 就相同合并
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130180238016.png)

#### Example 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130180849909.png)

#### Example 2
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130180928351.png)

#### Example 3: Mealy FSM
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130181331961.png)
> 需要分更多行讨论

### Complex FSM
- Automation needed
	- Table for large FSM too big for humans to work with
	- State reduction typically automated
## State Encoding
不同的 encoding 方式会带来 size 和 speed 的 trade off
### Gray Code
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130182924050.png)
像 Kmap 一样，每次只改变一位
### One-Hot Coding

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130183416991.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130183445407.png)<br> |
| :------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------ |
- 优点: Combinational Logic Simpler, Delay 小
- 缺点: Bigger State Register
- 可以提高 frequency，因为 clk 的 frequency 切换需要 combination circuit 完成处理

| 编码类型           | 原理                             | 优点                     | 缺点                | 适用场景                   |
| -------------- | ------------------------------ | ---------------------- | ----------------- | ---------------------- |
| 二进制编码          | n 个状态用 $log_{2}n$ 位二进制码        | 寄存器最少（如 5 个状态→3 位）     | 组合逻辑复杂（次态方程门输入多）  | 状态数多、资源紧张的低速场景         |
| Gray Code      | 相邻状态仅 1 位不同（如 00→01→11→10）     | 减少状态切换时的信号跳变，降低功耗      | 寄存器数量同二进制，组合逻辑略简单 | 对功耗敏感的场景（如低功耗传感器）      |
| One hot coding | n 个状态用 n 位，仅 1 位为 1（如 S0=0001） | 组合逻辑极简单（次态方程仅需选通门），速度快 | 寄存器最多（n 个状态→n 位）  | 高频场景（如 CPU 控制器、高速序列检测） |
## Unused State
FSM 状态编码后，若二进制码位数为 k，则存在 $2^k$ 个可能状态，其中部分是 unused state（如 5 个状态用 3 位编码，有 3 个无效状态：001,100,111）
必须处理未使用状态，否则上电后 FSM 可能进入无效状态，导致功能紊乱。
- 在 Kmap 中，x 被用来 make group bigger, term smaller
- 一旦确定了 PI, x 就被确定了是 0 还是 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130211215825.png)
图中 p2p1p0=100 的时候 n2=0 以此类推就能确定 unused state 的 next state
变成下图
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130211347578.png)
111 这个 unused state 需要 2 个 clk cycle 才能回到 normal operation
如果想要 111 更快回去可以人为指定 X
绝对不能有永远不能回去的 unused state，比如 001 的 next state 必须 avoid 001

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130211525277.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130211606541.png)<br> |
| :------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |

## FSM Reverse Engineering
当电路是 register 和 combinational circuit interconnect => FSM
1. 判断 FSM 类型：看输出逻辑的输入 —— 仅接状态→Moore；接状态 + 输入→Mealy
2. 确定状态数：状态寄存器位数 k→最大状态数$2^k$
3. 推导 next state：分析组合逻辑的输入（状态 + 输入）与输出（次态）的逻辑关系 写出 equation
4. 构建状态表：枚举所有 “输入 + 当前状态”，填入次态和输出
5. 绘制状态图：根据状态表，标注状态转换和输出，还原 FSM 行为

# RTL Design
level of abstract, 侧重于设计 system
## Component
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130214206540.png)
- Controllers: check the status of data path, need feedback and instruction from outside
- Controller 给 Datapath control signal, Datapath 给回去 feedback
- 两个都可能从 external environment get some feedback
- active edge of clock signal 是 state transition how fast 的决定因素 speed/pace
- 在下一个 rising edge 来之前，所有 components 要 settle down

## Steps
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130220858807.png)
> signal 不是 power，只能 carry information
- Step 1：捕获 HLSM（Capture High-Level State Machine）
	- 目标：用 HLSM 描述系统的 “输入、输出、状态动作与流转”，不涉及硬件细节
	- 关键输出：HLSM State Diagram，明确 “状态→动作→流转条件”
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130220928235.png)
> Data type 不只是 bits
> Statement 可以写一些计算

- Step 2：创建数据通路（Create Datapath）
	- 目标：将 HLSM 中的 “数据存储、运算” 映射为实际硬件组件，生成数据通路
	- 关键输出：Datapath 硬件图，包含组件、数据流向、控制信号接口
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130221030819.png)
> register 的 reset 一般是 aychronous 的，所以这里叫 clear (synchronous)
- Step 3：连接控制器与数据通路（Connect Controller to Datapath）
	- 目标：明确 Controller 与 Datapath 的交互接口 ——Controller 的输入来自 Datapath 的状态反馈，输出为 Datapath 的控制信号
	- 关键输出：系统顶层接口图，明确 “外部输入→Controller→Datapath→外部输出” 的信号流向
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130221414466.png)

| 方向            | 信号名称       | 来源 / 去向            | 功能说明             |
| ------------- | ---------- | ------------------ | ---------------- |
| Controller 输入 | `c`        | 外部（硬币传感器）          | 检测是否有硬币投入        |
| Controller 输入 | `tot_lt_s` | Datapath（比较器）      | 反馈 “累计金额是否不足”    |
| Controller 输出 | `d`        | 外部（出货机构）           | 控制是否出货           |
| Controller 输出 | `tot_clr`  | Datapath（`tot`寄存器） | 控制`tot`寄存器清零     |
| Controller 输出 | `tot_ld`   | Datapath（`tot`寄存器） | 控制`tot`寄存器加载加法结果 |
-  Step 4: Derive Controller's FSM
	- 目标：将 HLSM 的 “高层状态与动作” 转化为 Controller 的bit 级 FSM—— 用 FSM 处理 “控制信号”，替代 HLSM 中的 “数据操作”
	- 关键输出：Controller 的 FSM 状态图与状态表（State Table），明确 “当前状态 + 输入→下一状态 + 控制信号”
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130221544729.png)
> 需要是 signal bit

## Example
### Laser-Based Distance Measurer
- 功能：发射激光→检测反射→计算距离（`D = T × 3e8 m/s / 2`，`T`为激光往返时间）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251130224534495.png)

Step 1: Capture High-Level State Machine
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201200449650.png)
- 令 $f=3*10^8$，every clk cycle light travels 1m, number of clk cycle= distance (m)
- S3 可以加上 S'&(time is out) 到一个新状态，可能输出 error message，防止过了太久一直收不到反射回来的信号
- D 需要是整数，但若提高 clk frequency，可以变得更精确

Step2: Create a Datapath
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201201106922.png)

Step3: Connecting the Datapath to a Controller
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201201310932.png)

Step4: Deriving the Controller's FSM
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201201344317.png)
建议把所有 output bit 的状态都考虑上

关于 S0 和 S1 是否可以合并：
1. 合并后 D 很快被清零，显示的时间太短
2. 合并后 S0 combine 了很多 function，导致 circuit 变复杂，clock 必须要变慢

### Bus Interface
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201202713511.png)
- Peripheral
	1. 数据存储载体：每个 Peripheral 内置 1 个 32-bit 寄存器（Register），用于存储自身的专属数据（如传感器采集的数值、设备配置参数等）
	2. Faddr：每个 Peripheral 分配唯一的 4-bit 地址，用来 name peripheral，主处理器通过地址精准选择要访问的外设（如 Per0 地址 0000、Per1 地址 0001，直至 Per15 地址 1111）。所有 peripheral 都会收到 A (Adress)，但是对比之后是自己才会发送信号
	3. 总线通信节点：所有 Peripheral 共享Bus，通过Bus Interface与主处理器交互，避免主处理器与每个外设单独布线，简化硬件连接
	4. 可有不同 peripheral 控制不同 interaction

Step1: ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201203651095.png)
Step2:
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201203743546.png)
🐾发现 A 只是通过 controller 到了 datapath，应该作为 datapath 的 input
Step3&4:
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201203853172.png)

## Hierarchical Design
- Hierarchical design structure at different levels of abstraction
- Levels of abstraction: hiding the details in lower levels
- Hide details
- Reuse subsystems 

# Arithmetic Components
## ALU
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201204835022.png)
- 工程设计优化（避免资源浪费）
	- 早期方案缺陷：同时计算所有运算（加法、与、或等），再用 MUX 选择结果 —— 导致布线冗余、功耗浪费（多数运算结果无用）
	- 优化方向：通过控制信号直接触发目标运算，仅激活对应运算逻辑，关闭其他闲置模块，减少布线和功耗
- *Register* 很重要，需要等算完了再令 e=1，防止 LED 乱闪, take time to calculate correct results

#adder
## Adder
### Carry-Ripple Adder
![f5e2a6b0efa140ee1ae6be8aab8ca4f9.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/f5e2a6b0efa140ee1ae6be8aab8ca4f9.jpg)
- Crtical Path: 从最右侧的 input 到最左侧的 co 是 critical path。s 可以有 1/2 gate delay，如果选择的是 2-level，那么右侧输入到 s3 也是 critical path。
- 4 个 calculate in paraller, carry 还没被算出来的时候是 0
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201205025713.png)
- 进位延迟
- 例：4 位加法器 0111+0001（结果 01000），需等待进位从第 0 位（LSB）传递到第 3 位（MSB），4 个 FA 延迟后才输出正确结果
- 缺陷：位数越多，延迟越长（N 位加法器需 N 个 FA 延迟），无法满足高频系统需求
### Carry-Lookahead Adder
1. Attempt 1(不是 CLA)- 把所有输出都用 combination circuit, 无需等待 c 算好
	- Pros: Fast, 2 gate-level delays
	- Con: Large
2. Attempt 2 - 把 Carry 提前算好
	- 介于 Carry-ripple Adder 和 Attempt 1 之间的一种
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201212416355.png)
3. Attempt 3 - Using SPG block![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201212416355.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201213445499.png)
某一位产生进位 $c_{i+1}$ 只有两种可能，且这两种情况相互独立：
- 情况 1：本地生成进位：当前位 $A_i=1 且 B_i=1（G_i=1）$，无论低位是否有进位，$c_{i+1}$ 必然为 1=> G: Generate
- 情况 2：传播低位进位：当前位 $A_i≠B_i（P_i=1）$，且低位有进位 $（c_i=1）$，则 $c_{i+1}=1$（将 $c_i$ 传播上来）=> P: Propagate
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201214314949.png)
- 最上方一排是 direct input
- 计算 P&G : 1 delay
- 计算 Carry : 2 delay 在 PG 基础上，所以总共 3 delay
- 计算 Sum : $s_{0}=P_{0}\oplus cin$, $s_{1}=P_{1}\oplus c_{1}$, $s_{2}=P_{2}\oplus c_{2}$, $s_{3}=P_{3}\oplus c_{3}$, 在 Carry 上再加 1 delay，总计 4 delay
	- 注意 $s_{0}$ 很特殊，2delay，不是 critical path
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201220527776.png)

### Cascading Adders
1. Attempt 1:
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201220904670.png)
	3 delay for first cout, 其他 PG 在计算第一个 cout 的时候已经好了，之后 cout 只需要加 2 个 delay。总共是 9 个 delay for cout, 最后算 s15-s12 还需要加一个 delay

2. Adding two more outputs: P, G (group Propagate and group Generate)
![1545253bc340d84070d3a646c52a3a50.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/1545253bc340d84070d3a646c52a3a50.jpg)
- P 会比 G 快一些，G 需要 3
- PG 到算出 c3, c2, c1 需要 2
- 上方的 adder 需要等到 cin 才能开始，内部运算 carry 又需要 2
- s15-s13 再加一，8，s12 只要 6，等到 c3 算好就行
### CLA Adders
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201222211826.png)
- 最右侧的两个 c 应该是 provided directly
## Multiplier
都是 unsigned
### Array Style
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201222313604.png)
- 轮流将 multiplier 的每个 bit 与 multiplicant 做 AND，产生 partial product
- 优点：并行运算，速度快（仅需加法器阵列的延迟）
- 缺点：面积大（需大量加法器和与门），位数越多，硬件 8资源消耗越显著
### Sequential Style
- Don't compute all partial products simultaneously. Rather, compute one at a time (similar to by hand), maintain a running sum
- 优势：仅需 1 个加法器，硬件资源少（面积小）
- 缺点：串行执行，速度慢（需 N 个时钟周期完成 N 位乘法）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201223241839.png)
注意 shift 的行为

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201223414660.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251201223425020.png)<br> |
| :------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
