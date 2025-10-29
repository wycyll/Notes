# Basic concept
电流方向（1 D）- 正：顺时针负：逆时针
![image.png|497x219](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250916111251930.png)
P: 进入正极的电流乘电压
==各个电子元件功率之和为 0==
> ![44450ae37cece6fe69c1d5b7e0505396.jpg|402x301](https://raw.githubusercontent.com/wycyll/obsidian-images/master/44450ae37cece6fe69c1d5b7e0505396.jpg)
## Circuit Elements
###  Passive Elements: 
- ==cannot generate== electric energy
> resistors, capacitors, inductors
### Active elements: 
- devices capable of ==generating== electric energy
> generators, batteries, operational amplifiers
#### voltage or current sources
- most important active sources
##### Independent sources
电压/电流完全独立于其他电路元件
- **Independent voltage source**
![image.png|535x249](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250916173419560.png)
>$v$不变，$i$可以是任何值
- **Independent current sources**
![image.png|226x263](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250916173625757.png)
 >$i$不变，$v$可以是任何值
##### Dependent sources
the source quantity is controlled by another voltage or current
![image.png|567x311](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250916175019505.png)
> 看菱形上的标识，左边是电压，右边是电流
- A voltage-controlled voltage source (VCVS)
- A current-controlled voltage source (CCVS)
- A voltage-controlled current source (VCCS)
- A current-controlled current source (CCCS)
---
# Basic Laws
## Ohm 's Law
### 电阻 Resistance
计算公式：
		$R=\frac{ρl}{A}$
		-$\rho$==resistivity==：材料的**电阻率**（固有属性，单位$\Omega \cdot m$，如铜的电阻率远小于橡胶）；
		-$l$：电阻元件的长度（越长，电阻越大）；
		-$A$：横截面面积（越粗，电阻越小）
- Resistance: 电阻，一种 physical property，用符号$R$来表示
- Resistor: 电子元件
- 永远 **Consume power**
#### Resistors with extreme value
| 类型                | 电阻特性           | 电压           | 电流           | 示例                                                                                                          |
| :---------------- | :------------- | :----------- | :----------- | :---------------------------------------------------------------------------------------------------------- |
| Short Circuit<br> |$R \to 0$     |$V = 0$     | 可任意大（由外电路决定） | <br>![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919142911712.png)<br> |
| Open Circuit      |$R \to \infty$| 可任意大（由外电路决定） |$i = 0$     | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919142614099.png)         |
#### 分类
- **按结构分**：
    - 固定电阻：阻值不可调（如普通碳膜电阻）；
    - 可变电阻：阻值可调节，典型代表是 “电位器”==potentiometer==（用于音响音量调节，通过滑动触点改变接入电路的电阻值）。
- **按伏安特性分**（核心区别：是否满足欧姆定律）：
    - 线性电阻 ==linear resistor==：严格服从$V=IR$，阻值R为常数，伏安（$i-V$）特性是**过原点的直线**（如普通电阻）；
    - 非线性电阻 ==nonlinear resistor==：不服从欧姆定律，阻值随电流 / 电压变化，伏安特性是曲线（如二极管，正向导通时电阻小，反向截止时电阻极大）。
### 电导 Conductance
- **公式**：$G = \frac{1}{R} = \frac{i}{V}$
- **单位**：西门子（S），或姆欧（$\mho$），1S = 1A/V（与电阻单位互为倒数）。
> 高电阻并非 “无用”e.g. old style light bulb
>  	EE->hear->light
## Nodes, Branches, and Loops
### Branch
> 单个二端元件（如电阻、电压源、电流源），是电路的 “基本单元”。
### Nodes
> 两条或以上 Branch的连接点==（导线连接的点视为同一节点）==

|                                                                                                     |                                                                                                         |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919153100844.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919153129361.png)<br> |
### Loop
> 电路中任意闭合的路径。

- Independent loop: the loop contains at least one branch which is not a part of any other independent loop.
> 至少包含一条其他独立回路没有的 branch
> 有很多种划分方式
- Mesh: a loop that does not enclose any other loops. (i.e., smallest loop)
判断例子：[Slides](Course/215/Slides/ECE2150J_C03.pdf#page=52)

Max number of independent loop = number of mesh ^1
### 三者联系
==b (branches) =$l$ (independent loop) + n (nodes) - 1==
$l$指 independent loop 的最大值 [[Course/215/Lecture Notes#^1]]

## Kirchhoff's Laws
### Kirchhoff's current law(KCL)
• Based on the **law of conservation of charge**
流入节点的电流之和 = 流出节点的电流之和。 i.e. $\sum_{n=1}^N i_n = 0$
- 闭合边界的 KCL
区域内总流入电流 = 总流出电流（无需考虑内部节点的电流分布）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919184859417.png)

### Kirchhoff's voltage law (KVL)
- Based on the principle of **conservation of energy**
> 电场力对电荷做功的代数和为零（电荷绕闭合回路一周，能量无净增益或损失），反映为电压的代数和为零。

回路中电压降之和 = 电压升之和 i.e.$\sum_{m=1}^M v_m = 0$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919193153520.png)
- 串联电压源合并
多个电压源串联时，总电压为各电压源的代数和（与总绕行方向一致的为正，相反为负）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250919193551335.png)

## Series Resistors

$R_{eq}=R_{1}​+R_{2}​+⋯+R_{N}​=\sum_{n=1}^{N}R_n ​$

- 由串联等效电阻得总电流：$i = \frac{v}{R_{eq}} = \frac{v}{R_1 + R_2 + \dots + R_N}$；
- 任意一个电阻$R_n$的电压$v_n = iR_n$，代入电流表达式：$v_n = \frac{R_n}{R_1 + R_2 + \dots + R_N} \cdot v = \frac{R_n}{\sum_{k=1}^N R_k} \cdot v$
## Parallel Resistors

$\frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2} + \dots + \frac{1}{R_N} = \sum_{n=1}^N \frac{1}{R_n}$
因电导$G = \frac{1}{R}$，代入上式得：$G_{eq} = G_1 + G_2 + \dots + G_N = \sum_{n=1}^N G_n$

- 各支路电流：$i_1 = \frac{v}{R_1}$，$i_2 = \frac{v}{R_2}$，代入电压表达式：$i_1 = \frac{R_2}{R_1 + R_2} \cdot i, \quad i_2 = \frac{R_1}{R_1 + R_2} \cdot i$
- 多电阻情况$i_n = \frac{\frac{1}{R_n}}{\sum_{k=1}^N \frac{1}{R_k}} \cdot i​=\frac{G_{1}​+G_{2}​+⋯+G_{n}}{G_{n}}​​​⋅i$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923163832401.png)

## Wye-Delta Transformations
有些电路很难分析，不是并联也不是串联，需要进行 transformation
### Delta to wye
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923172348309.png)
### Wye to delta
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923172418319.png)

# Methods of analysis
## Nodal Analysis
1. 选参考节点：设最下方为地（0V），非参考节点为$v_1$、$v_2$。
2. 对非参考节点列 KCL
3. 解矩阵方程
![4042e72a3f09bcbf2b6084cfe6715b83.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/4042e72a3f09bcbf2b6084cfe6715b83.jpg)
### Cramer's Rule
用于解矩阵方程
![fc25d22ae34f87fe35dc609cbf589099.jpg](https://raw.githubusercontent.com/wycyll/obsidian-images/master/fc25d22ae34f87fe35dc609cbf589099.jpg)

### Inspection Method
当电路仅由**独立电流源**和**线性电阻**组成，且有$N$个“非参考节点”（即不接地的节点）时，节点电压方程可直接写成**矩阵形式**： 
$\begin{bmatrix} G_{11} & G_{12} & \dots & G_{1N} \\ G_{21} & G_{22} & \dots & G_{2N} \\ \vdots & \vdots & \vdots & \vdots \\ G_{N1} & G_{N2} & \dots & G_{NN} \end{bmatrix} \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_N \end{bmatrix} = \begin{bmatrix} i_1 \\ i_2 \\ \vdots \\ i_N \end{bmatrix}$
其中各符号的物理意义： 
- 对角线元素$G_{kk}$：节点$k$连接的所有电阻的“电导之和”（电导$G = \frac{1}{R}$，故为正值）； 
- 非对角线元素$G_{kj} (k \neq j)$：两节点间公共电阻的“电导的负值”（故为负值）； 
- 节点电压向量$\mathbf{v}$：各非参考节点相对于“参考节点（地）”的电压； 
- 电流源向量$\mathbf{i}$：流入各非参考节点的独立电流源的代数和（流入为正，流出为负）。 
> ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250923192817889.png)
习题：[[Course/215/Exercise#Supernode]]

### Supernode
- 问题核心：电压源电流未知，无法单独对两个节点列 KCL；
- 解决方案：将电压源及其两端节点 “合并为超节点”，对超节点整体列 KCL，同时补充 “电压源的约束方程”（节点电压差 = 电压源电压）。
e.g. p 42
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251005222820788.png)

|                                                                                                     |                                                                                                     |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251005222904510.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251005222923459.png) |

## Mesh Analysis
### 前提
- 平面电路 vs 非平面电路：
    - 平面电路：可在平面内绘制，支路无交叉；
    - 非平面电路：无法避免支路交叉，网孔法不适用。

|                                                                                                         |                                 |
| :------------------------------------------------------------------------------------------------------ | :------------------------------ |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006173811171.png)<br> | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006173821938.png)<br> |
### 步骤
- 步骤 1：定义网孔电流（通常设为顺时针）
- 步骤 2：对每个网孔列 KVL 方程（用网孔电流表电压）
> KVL 的核心：沿网孔绕行一周，电压升的总和 = 电压降的总和（或 “所有电压的代数和 = 0”，顺时针绕行时 “电压升为正，电压降为负”）。
- 步骤 3：解方程组
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006174100649.png)

### Inspection Method
对仅含**独立电压源**和**线性电阻**的平面电路，可直接观察列矩阵方程，规则：

- 电阻矩阵（左侧）：
    - 对角线元素$R_{kk}$（自电阻）：网孔 k 内所有电阻之和（正，因网孔电流流过所有电阻产生压降）；
    - 非对角线元素$R_{kj}$（互电阻）：网孔 k 与 j 间公共电阻的负值（负，因公共电阻的压降对两个网孔方向相反）。
- 右侧电压向量：沿网孔顺时针绕行的独立电压源代数和（电压升为正，电压降为负）。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006174135831.png)
> 习题：[[Course/215/Exercise#Supermesh]]

### Supermesh
电流源的电压未知（无法直接用欧姆定律表示），分两种情况处理：

情况 1：电流源仅在一个网孔内
- 直接已知网孔电流：若电流源电流为 I，则该网孔电流$i = ±I$（符号取决于电流源方向与网孔电流方向是否一致），减少一个方程。
e.g. [Slides](ECE2150J_C03.pdf#page=91)

情况 2：电流源在两个网孔之间 —— 超网孔（Supermesh）
- 问题核心：电流源电压未知，无法单独对两个网孔列 KVL；
- 解决方案：将电流源及其串联元件 “剔除”，合并两个网孔为超网孔，对超网孔列 KVL，补充 “电流源的约束方程”（网孔电流差 = 电流源电流）。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006174842577.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251006174914617.png)

| 对比维度    | 节点分析法（Nodal）      | 网孔分析法（Mesh）           |
| ------- | ----------------- | --------------------- |
| 核心定律    | 基尔霍夫电流定律（KCL）     | 基尔霍夫电压定律（KVL）         |
| 适用电路结构  | 多并联元件、多电流源        | 多串联元件、多电压源            |
| 变量数量    | 非参考节点数（n-1），节点少则优 | 网孔数（m），网孔少则优          |
| 特殊处理    | 电压源→超节点           | 电流源→超网孔               |
| 目标变量偏好  | 需节点电压（如分压、功率计算）   | 需支路 / 网孔电流（如电流控制的受控源） |
| 观察法适用场景 | 仅独立电流源 + 线性电阻     | 仅独立电压源 + 线性电阻         |

# Circuit Theorems
## Lineartity
### Homogeniety
- 定义：若电路的输入乘以常数k，则输出也乘以相同常数k
- 数学表达：若输入$x \to$输出y，则$k x \to k y$。
- 电路实例：
    - 欧姆定律$V=IR$：电流i变为k i，电压V变为$k IR = k V$，满足齐次性
    - 功率$P=I^2R$：电流i变为k i，功率P变为$k^2 I^2R = k^2 P$，不满足齐次性（输出缩放比例与输入不同），因此功率是 “非线性量”，后续定理（如叠加）不适用
### Additivity
- 定义：若输入$x_1 \to$输出$y_1$，输入$x_2 \to$输出$y_2$，则输入$x_1+x_2 \to$输出$y_1+y_2$
- 非线性反例：$y=ax^2$，若$x_1 \to y_1=ax_1^2$，$x_2 \to y_2=ax_2^2$，则$x_1+x_2 \to a(x_1+x_2)^2 = ax_1^2 + 2ax_1x_2 + ax_2^2 \neq y_1+y_2$，不满足可加性

## Superposition
- 基于线性电路的Additivity：若电路有多个独立源，the *total* response is the *sum* of the *individual responses*
### 应用步骤
1. 独立源 “单独作用” 的处理
每次只让一个独立源工作，其余独立源需关闭
- 电压源关闭：用*短路*代替（输出 0V，等效为导线）
- 电流源关闭：用*开路*代替（输出 0A，等效为断开）

2. 受控源的处理
Dependent Source需保留，不能关闭
因为受控源的输出由电路中的其他变量（如电流i、电压v）决定，不是 “独立存在” 的

3. 功率的特殊说明
叠加定理**不适用于功率计算**
因为功率是电压 / 电流的二次函数，不满足线性关系（总功率≠各独立源单独作用时的功率之和）。需先叠加电压 / 电流，再用最终的电压、电流计算功率

> 习题：[[Course/215/Exercise#Superposition]]

## Source Transformation
- 等效条件
	- 电压源→电流源：$V_s$串联电阻R → 电流源$I_s = \frac{V_s}{R}$并联电阻R
    
	- 电流源→电压源：$I_s$并联电阻R → 电压源$V_s = I_s R$串联电阻R
    
	- 方向规则：电流源的方向从电压源的 “负极指向正极”（保证端口电流方向一致，避免等效错误）
    
	- **证明**：对比两种电路的 i-v 关系（端口电压v与电流i的关系）
    
	    - 电压源串联R：$v = i R + V_s$；
	    - 电流源并联R：$v = i R + I_s R$；
	        令两式相等，得$V_s = I_s R$，即等效条件
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251020005240096.png)
> 注意方向

习题：[[Course/215/Exercise#Source Transformation|Exercise]]
## Thevenin's Theorem
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251020105954192.png)
任何线性二端电路（左），都能等效为一个电压源$V_{\text{Th}}$串联电阻$R_{\text{Th}}$的简单电路（右）

## 证明
假设线性电路内部有多个独立源（如$v_{s1}、v_{s2}$电压源，$i_{s1}、i_{s2}$电流源），外部接电流源 i。根据 superposition，端口电压 v 可分解为两部分：$v = \underbrace{A_0 i}_{\text{外部电流源 } i \text{ 的贡献}} + \underbrace{A_1 v_{s1} + A_2 v_{s2} + A_3 i_{s1} + A_4 i_{s2}}_{\text{内部所有独立源的贡献}}$
将 “内部独立源的总贡献” 记为$B_0$，则简化为：$v = A_0 i + B_0$

1. 端口 “开路”（$i = 0$）
此时端口电压等于开路电压$v_{\text{oc}}$。代入$v = A_0 i + B_0$，得：$v_{\text{oc}} = B_0$
2. 内部所有独立源 “关闭”（电压源短路、电流源开路）
此时内部源无贡献（$B_0 = 0$），电路仅由 “等效电阻” 构成。代入$v = A_0 i + B_0$，得：$v = A_0 i$→ 说明$A_0$是内部源关闭后的等效电阻$R_{\text{eq}}$（即$A_0 = \frac{v}{i} = R_{\text{eq}}$）。
3. 结合戴维南定理的定义：
-$B_0 = v_{\text{oc}} = V_{\text{Th}}$（$V_{\text{Th}}$是戴维南电压，即开路电压）；
-$A_0 = R_{\text{eq}} = R_{\text{Th}}$（$R_{\text{Th}}$是戴维南电阻，即内部源关闭后的等效电阻）。
因此，端口电压的表达式$v = A_0 i + B_0$可改写为：$v = R_{\text{Th}} i + V_{\text{Th}}$

## 求解
-$R_{Th}$的两种求解方法
	1. 情况 1：都是 independent source
		直接计算独立源关断后的输入电阻（电阻串并联组合）。
	2. 情况 2：有 dependent source
		独立源关断，受控源保留
		- 端口 a-b 加测试电压$v_o$，测量流入端口的电流$i_o$，则$R_{Th} = \frac{v_o}{i_o}$；
		- 或加测试电流$i_o$，测量端口电压$v_o$，则$R_{Th} = \frac{v_o}{i_o}$。 

-$V_{th}$求解
	直接通过电压分析计算 (如 nodal analysis, mesh analysis)
	terminal 的两端（a 和 b）视作电压相同（接地）

习题：[[Course/215/Exercise#Thevenin's Theorem|Exercise]]
> - 特殊情况：负电阻负电阻$R_{Th}$
> 	当电路含受控源时，$R_{Th}$可能为负
> 	- 物理意义：负电阻不代表实际存在负电阻元件，而是等效模型 —— 表示电路是 “有源负载”，向外提供功率

## Norton's Law
$R_{N}$的寻找方法同$R_{th}$
-$I_{N}$求解
1. 短路端口：将原电路的端子 a 和 b 用导线直接短接；
2. 计算短路电流：电路分析，计算此时短路导线中从 a 到 b 的电流，该电流即为$i_{SC}$；
3. 等效关系：根据诺顿定理的等效性，$I_N = i_{SC}$
4. 方向：和$I_{SC}$完全一样
> 或等效$I_{N}=\frac{V_{th}}{R_{th}}$

习题：[[Course/215/Exercise#Norton's Law|Exercise]]

## Max Power Transfer
## Efficiency
- 应用场景：电力系统（发电、输电、配电）。这类系统需传输大量电能，核心诉求是 “减少传输损耗”，让更多电能被负载有效利用。
- 核心定义：效率是 “负载获得的有用功率” 与 “电源提供的总功率” 的比值，公式为：$\text{Efficiency} = \frac{\text{Useful Power Output（负载功率）}}{\text{Total Power Input（电源总功率）}}$
## Power
- 应用场景：通信、电子系统（如射频电路、信号放大）。这类场景不优先考虑损耗，核心诉求是 “让负载获得的功率绝对值最大”（如最大化信号强度）
- 目标：让负载吸收的功率数值最大，即使电源自身损耗较大也可接受

## 推导
- 电路电流：$I = \frac{V_S}{R_L + R_S}$；
- 负载功率：$P_L = I^2 R_L = \left( \frac{V_S}{R_L + R_S} \right)^2 R_L$；
- 电源总功率：$P_{\text{total}} = I^2 (R_L + R_S) = \frac{V_S^2}{R_L + R_S}$；
- 效率：$\eta = \frac{P_L}{P_{\text{total}}} = \frac{R_L}{R_L + R_S}$。

对负载功率$P_L = \left ( \frac{V_S}{R_L + R_S} \right)^2 R_L$关于$R_L$求导，令导数为 0，可得：
当$R_L = R_S$时，负载功率$P_L$最大
此时$P_{L,\text{max}} = \frac{V_S^2}{4 R_S}$
此时效率仅有 50%

| Circuit types     | I. 电力系统（Power utility systems）                                            | II. 通信与电子系统（Communication & Electronic systems）                      |
| ----------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| 功率规模（Power scale） | 传输大量电能（如发电等场景）<br>（Large quantities of electric power (generation, etc.)） | 传输少量电能<br>（Small amount of power is being transferred）               |
| Primary concern   | 电能传输的效率<br>（Efficiency of the power transfer）                             | 尽可能将更多功率传输到负载<br>（Transmit as much of power as possible to the load） |

# Operational Amplifiers
简称 Op Amp，是一种高增益、差分输入、单端输出的线性集成电路，可作为 voltage-controlled voltage source，结合外部反馈元件（如电阻、电容）能实现数学运算（加减、积分、微分）或信号放大
$v_o = A v_d = A (v_2 - v_1)$
## Power
Amp 是active element，需要被 1-2 个电源供电
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251021224842840.png)
- 若为单电源供电：仅接 “正电源$V^+$” 和 GND，输出电压范围通常在$0 \sim V^+$之间（具体受运放类型影响）
- 若为双电源供电：同时接 “正电源$V^+$” 和 “负电源$V^-$”（通常对称，如$\pm 5\,\text{V}$、$\pm 12\,\text{V}$），输出电压范围可覆盖$V^- \sim V^+$（如$-5\,\text{V} \sim +5\,\text{V}$）

在电路图中电源本身会被省略，但电流不可省略
KCL:$i_o = i_1 + i_2 + i_+ + i_-$
> -$i_1$、$i_2$：流入反相端、同相端的输入电流
> -$i_+$、$i_-$：正、负电源提供的电流（为运放内部电路和输出负载供电）

![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251021230020761.png)

## Ideal Op Amp
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251021230404552.png)

| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251021230953967.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251021231012846.png) |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |

### 特点
- Infinite open-loop gain,$A=\infty$
- Infinite input resistance,$R_{i}=\infty$
- Zero output resistance,$R_{0}=0$

1.$i_{1}=i_{2}=0$
	$R_{i}$无穷大->open circuit->no current flows
	By KVL:$V_{2}+R_{i}i+V_{1}=0$
	$R_{i}$无穷大->i=0->$i_{1}=i_{2}=0$
	- 不代表$i_{0}=0$还有$i_{+},i_{-}$
2.$v_{d}=v_{2}-v_{1}=0$
	$v_{0}=Av_{d}<\infty$, A is infinite->$v_{d}$=0
	- 不代表$v_{2}=v_{1}=0$
## Inverting Amplifier (Inverter)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022125257352.png)
- 结构：输入信号$v_i$接反相端（-）；$R_f$连接输出与反相端；同相端（+）接地
- 特性：输出与输入极性相反（反相放大）
- 若不记得 A，可用反相端的 kcl 推导
- 只依赖外部电路

## Noninverting Amplifier
- 结构：输入信号$v_i$接同相端（+）；$R_f$与$R_1$组成分压网络，将部分输出反馈至反相端（-）
- 特性：输出与输入极性相同（同相放大），增益始终$\geq 1$
- 只依赖外部电路
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022130603757.png)

### Voltage follower circuit
- 在 Noninverting Amplifier 的基础上，$R_{f}=0$or$R_{1}=\infty$ot both. A=1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022150450284.png)
- 优势：在信号源内阻较高时，输出可提供大电流，同时从输入几乎不汲取电流
- 功能：输出电压与输入电压相等（电压增益为 1），但功率增益显著更大，可实现电压跟随的同时增强带载能力
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022150929925.png)

#### Negative Feedback
1. 定义：负反馈的核心是 “反向抵消”—— 当某个事件发生时，会触发一个动作来抵消该事件的原始影响，从而使系统趋于稳定。
2. 过程：
	- 若输出$V_{\text{out}}$下降 → 反相端$V_-$也会下降相同幅度；
	- 此时，同相端与反相端的电压差$(V_+ - V_-)$会增大；
	- 负反馈会主动调整输出，最终使$V_+ \cong V_-$

#### Buffer
1. 第一级（First stage）的输出通过电压跟随器，再驱动第二级（Second stage）。

2. 其核心优势源于电压跟随器的两个关键特性：
	- 极高输入阻抗（very high input impedance）：从第一级汲取的电流几乎为 0，不会影响第一级的输出电压（避免minterstage loading）
	- 极低输出阻抗（very low output impedance）：能为第二级提供足够的驱动电流，保证第二级电路正常工作。

3. 实际意义：最小化级间交互
	电压跟随器作为缓冲器，最终实现两个效果：
	- 隔离电路：让 “第一级” 和 “第二级” 各自独立工作，互不干扰
	- 消除级间负载：避免因后级电路的低阻抗 “拉低” 前级的输出电压，确保信号无损失地传输
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022152230857.png)

## Summing Amp (Adder)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022152612890.png)

## Difference Amp (Subtractor)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022155746617.png)
### Common mode & difference mode
在双输入电路中，输入信号 $v_1$、$v_2$ 可分解为两部分：
$v_d = v_2 - v_1$ 
$v_c = \dfrac{v_1 + v_2}{2}$
- 差模信号$v_d$：两个输入的“差值”，是我们想放大的有用信号。
    
- 共模信号$v_c$：两个输入共同拥有的部分，通常是噪声或干扰。
---
想象两个麦克风同时录音：
- 背景噪声（风声、电源干扰） → 共模信号
    
- 左右声源差别（一个人靠近左边说话） → 差模信号
    
差分放大器要放大差别、忽略相同，即放大语音信号、抑制噪声。

---
为了消除共模信号，当 $v_{1}=v_{2}$ 的时候，需要 $v_{0}=0$
把 $v_{1}=v_{2}$ 代入，令 $v_{0}=0$，得出 $\frac{R_{1}}{R_{2}}=\frac{R_{3}}{R_{4}}$，化简得出 $v_{0}=\frac{R_{2}}{R_{1}}(v_{2}-v_{1})$
若进一步令所有电阻相等，$v_{0}=v_{2}-v_{1}$
## Cascaded Op Amp Circuits
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251022172752838.png)
$A = \frac{v_o}{v_1} = \frac{v_2}{v_1} \cdot \frac{v_3}{v_2} \cdot \frac{v_o}{v_3} = A_1 \cdot A_2 \cdot A_3$
- 灵活实现高增益：通过多个 “低增益级” 的级联，可轻松实现很高的总增益
- 避免级间负载：运放的 “高输入阻抗” 和 “低输出阻抗” 特性，使得前级的输出不会被后级的输入拉低
习题：[[Course/215/Exercise#Cascaded Op Amp|Exercise]]

## Applications
### DAC
#### 核心功能
DAC 的作用是将数字信号（二进制 0/1）转换为模拟电压。例如，4 位 DAC 的数字输入范围是`0000`到`1111`，对应输出是连续的模拟电压值。
#### 原理
- 输入与电阻：数字输入$V_1 \sim V_4$ 为 0V（逻辑 0）或 1V（逻辑 1），其中$V_1$ 是MSB，$V_4$ 是LSB。每个输入通过不同电阻（$R_1, R_2, R_3, R_4$）连接到运放反相端，反馈电阻$R_f$ 形成求和回路
- 输出公式推导：
    
   $-V_o = \frac{R_f}{R_1}V_1 + \frac{R_f}{R_2}V_2 + \frac{R_f}{R_3}V_3 + \frac{R_f}{R_4}V_4$
    
    若电阻满足二进制权重关系（如$R_1:R_2:R_3:R_4 = 1:2:4:8$，$R_f$ 与$R_1$ 匹配），则权重系数为$1, 0.5, 0.25, 0.125$，对应二进制位的$2^3, 2^2, 2^1, 2^0$ 权重，最终输出可表示为：
    
   $-V_o = k(2^3V_1 + 2^2V_2 + 2^1V_3 + 2^0V_4)$
#### Resolution
- 输出示例：以$R_1=10\,\text{kΩ}, R_2=20\,\text{kΩ}, R_3=40\,\text{kΩ}, R_4=80\,\text{kΩ}, R_f=10\,\text{kΩ}$ 为例：
    - 输入`0000`：$-V_o = 0$ →$V_o = 0\,\text{V}$
    - 输入`0001`：$-V_o = 0.125\,\text{V}$ →$V_o = -0.125\,\text{V}$
    - 输入`1111`：$-V_o = 1+0.5+0.25+0.125 = 1.875\,\text{V}$ →$V_o = -1.875\,\text{V}$
- 分辨率定义：DAC 能分辨的最小模拟输出电压（即 LSB 对应的电压）。公式为：$\text{Resolution} = \frac{V_{\text{OFS}}}{2^n - 1}$其中$V_{\text{OFS}}$ 是满量程输出电压（所有位为 1 时的输出），n 是 DAC 的位数
    - 上例中$n=4$，$V_{\text{OFS}}=1.875\,\text{V}$，则分辨率$= \frac{1.875}{15} = 0.125\,\text{V}$，与 LSB 的权重一致