# Tools
[Electrostatics Simulation](http://www.falstad.com/emstatic/index.html)
# Electric Charge and Electric Field
## Coulomb's law
- 公式：
    两点电荷间静电力大小：$F = k\frac{|q_1 q_2|}{r^2}$（$k=9×10^9\ \text{N·m}^2/\text{C}^2$，r为电荷间距）
    
    矢量方向：同种电荷排斥，异种电荷吸引（需结合坐标系分解分量）
## Electirc Field
- 核心公式：
    1. 定义式（通用）：$E = \frac{F}{q_0}$（$q_0$ 为试探电荷，F为 $q_0$ 受的电场力）
    2. 点电荷电场：$E = k\frac{|q|}{r^2}$（q为场源电荷，r为到场点距离）
- 方向规则：
    - 正场源电荷：E背离电荷；负场源电荷：E指向电荷
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251011222126155.png)
### 带电线段
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012151114891.png)
$E_x = \frac{1}{4\pi\epsilon_0} \frac{Qx}{2a} \int_{-a}^{a} \frac{dy}{(x^2 + y^2)^{3/2}} = \frac{Q}{4\pi\epsilon_0} \frac{1}{x\sqrt{x^2 + a^2}}$
$E_y = -\frac{1}{4\pi\epsilon_0} \frac{Q}{2a} \int_{-a}^{a} \frac{y \, dy}{(x^2 + y^2)^{3/2}} = 0$
or, in vector form,
$\vec{E} = \frac{1}{4\pi\epsilon_0} \frac{Q}{x\sqrt{x^2 + a^2}} \hat{i} \tag{21.9}$

#### 无限长带电线段
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251013210125965.png)
- $E \cdot 2\pi r L = \frac{\lambda L}{\varepsilon_0} \implies E = \frac{\lambda}{2\pi\varepsilon_0 r}$
- 高斯面选择：同轴圆柱面（半径r，长度L），上下底面垂直于直线，侧面平行于直线；
- 通量分析：
    - 上下底面：电场 $vec{E}$ 沿径向（与底面法向量垂直），通量 $\Phi_{\text{底面}}=0$
    - 侧面：$\vec{E}$ 与侧面法向量同向（$\cos\theta=1$），E大小处处相等，通量 $\Phi_{\text{侧面}}=E \cdot 2\pi r L$
- 高斯定律应用：
    面内净电荷 $Q_{\text{enclosed}}=\lambda L$，代入得：
    $E \cdot 2\pi r L = \frac{\lambda L}{\varepsilon_0} \implies E = \frac{\lambda}{2\pi\varepsilon_0 r}$
### Ring of Charge
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012151740809.png)
$dE = \frac{1}{4\pi\epsilon_0} \frac{dQ}{x^2 + a^2}$
Using $\cos\alpha = x/r = x/(x^2 + a^2)^{1/2}$, the x-component $dE_x$ of this field is
$\begin{align*} dE_x &= dE\cos\alpha = \frac{1}{4\pi\epsilon_0} \frac{dQ}{x^2 + a^2} \frac{x}{\sqrt{x^2 + a^2}} \\ &= \frac{1}{4\pi\epsilon_0} \frac{x \, dQ}{(x^2 + a^2)^{3/2}} \end{align*}$
$\vec{E} = E_x \hat{i} = \frac{1}{4\pi\epsilon_0} \frac{Qx}{(x^2 + a^2)^{3/2}} \hat{i}$

### Arc of Charge
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012152348842.png)
$E_y = \int_{-L/2}^{L/2} dE_y = 0$
 $\begin{align*} dE_x &= k \, dq \, \cos\theta / r^2 \\ &= k \lambda \, ds \, \cos\theta / r^2 \end{align*}$
$s = r \theta, \, ds = r \, d\theta$
$E_x = k\lambda \int_{-L/2}^{L/2} \frac{r d\theta \cos\theta}{r^2} = \frac{k\lambda}{r} \int_{-\theta_0}^{\theta_0} d\theta \cos\theta= \frac{2k\lambda}{r} \sin\theta_0$

### 无限大均匀带电平面
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251013212121990.png)
- 高斯面选择：柱形高斯面（两个底面平行于平面，面积S；侧面垂直平面）；
- 通量分析：
    - 侧面：电场 $\vec{E}$ 垂直于侧面法向量，通量 $\Phi_{\text{侧面}}=0$
    - 两个底面：$\vec{E}$ 与底面法向量同向（平面两侧电场均垂直平面向外），每个底面通量ES，总通量 $\Phi_{\text{总}}=2ES$
- 高斯定律应用：
    面内净电荷 $Q_{\text{enclosed}}=\sigma S$，代入得：
    $2ES = \frac{\sigma S}{\varepsilon_0} \implies E = \frac{\sigma}{2\varepsilon_0}$
### 平行板电容器
- 原理：两板带等量异种电荷（$\pm Q$），电荷仅分布在 “相对表面”（外侧电荷抵消）；
- 电场叠加：单个平板电场 $E_0=\frac{\sigma}{2\varepsilon_0}$，两板间电场叠加（$E=E_0+E_0$），板外抵消（$E=E_0-E_0$）
- 公式：$E = \frac{\sigma}{\varepsilon_0}$（均匀电场，电容器的核心特性）

### Uniformly Charged Disk
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012152823582.png)
1. 步骤 1：回顾 “带电圆环的轴上电场”（基础结论）
对于一个半径为a、总电荷为Q的圆环，在其轴线（x轴）上距离圆心x处的电场，表达式为：$E_x = \frac{1}{4\pi\epsilon_0} \frac{Qx}{(x^2 + a^2)^{3/2}}$

2. 步骤 2：将 “带电圆盘” 分解为 “无数细圆环（微元环）”
把圆盘看成由无数个内半径r、外半径 $r+dr$ 的细圆环叠加而成。
- 微元环的电荷dQ
	设圆盘的**电荷面密度**为 $\sigma= \frac{Q}{\pi R^2}$。
	微元环的面积dA近似为 $2\pi r \cdot dr$ 
	微元环的电荷：$dQ = \sigma \cdot dA = \sigma \cdot 2\pi r \, dr$

- 微元环在P点的电场 $dE_x$
	微元环在轴上点P（距离圆盘中心x处）产生的x方向电场分量为：$dE_x = \frac{1}{4\pi\epsilon_0} \frac{dQ \cdot x}{(x^2 + r^2)^{3/2}}$
	将 $dQ = 2\pi\sigma r \, dr$ 代入上式，$dE_x = \frac{\sigma x}{2\epsilon_0} \cdot \frac{r \, dr}{(x^2 + r^2)^{3/2}}$

3. 步骤 3：对所有微元环积分，求总电场
	圆盘的细圆环从 “中心（$r=0$）” 到 “边缘（$r=R$）”，因此对 $dE_x$ 在 $r \in [0, R]$ 上积分，得到总电场 $E_x$：$E_x = \int_{0}^{R} dE_x = \int_{0}^{R} \frac{\sigma x}{2\epsilon_0} \cdot \frac{r \, dr}{(x^2 + r^2)^{3/2}}$

4. 积分计算（变量代换法）

令 $z = x^2 + r^2$，则 $dz = 2r \, dr$，即 $r \, dr = \frac{dz}{2}$。
- 当 $r=0$ 时，$z = x^2$；
- 当 $r=R$ 时，$z = x^2 + R^2$。
积分变为：$\int_{x^2}^{x^2 + R^2} \frac{\frac{dz}{2}}{z^{3/2}} = \frac{1}{2} \int_{x^2}^{x^2 + R^2} z^{-3/2} dz$
计算积分（幂函数积分公式：$\int z^n dz = \frac{z^{n+1}}{n+1} + C$，这里 $n = -3/2$）：$\int z^{-3/2} dz = \frac{z^{-1/2}}{-1/2} + C = -\frac{2}{\sqrt{z}} + C$
代入上下限：$\frac{1}{2} \left[ -\frac{2}{\sqrt{z}} \right]_{x^2}^{x^2 + R^2} = \frac{1}{2} \left( -\frac{2}{\sqrt{x^2 + R^2}} + \frac{2}{\sqrt{x^2}} \right) = -\frac{1}{\sqrt{x^2 + R^2}} + \frac{1}{x}$

5. 步骤 4：总电场的最终表达式
	将积分结果代回 $E_x$ 的表达式：$E_x = \frac{\sigma x}{2\epsilon_0} \left( -\frac{1}{\sqrt{x^2 + R^2}} + \frac{1}{x} \right)$

	展开并整理后：$E_x = \frac{\sigma}{2\epsilon_0} \left( 1 - \frac{x}{\sqrt{x^2 + R^2}} \right)= \frac{\sigma}{2\epsilon_0} \left[ 1 - \frac{1}{\sqrt{(R^2/x^2) + 1}} \right]$
#### 特殊情况
若圆盘 “无限大” $R \to \infty$，则 $\frac{x}{\sqrt{x^2 + R^2}} \to 0$，此时电场简化为：$E = \frac{\sigma}{2\epsilon_0}$ 这与 “无限大均匀带电平面的电场” 结论一致，验证了推导的合理性。
### 带电球壳
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251013205904858.png)
### 均匀带电球体
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251013205914264.png)


| 带电体类型                | 核心公式                                                                                                                                      | 特殊场景                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 有限长带电线段（垂直中垂线 / 轴线上） | $E = \frac{k\lambda L}{x\sqrt{x^2 + (L/2)^2}}$，其中 $\lambda = \frac{Q}{L}$（线电荷密度），L 为线段长度，x 为场点到线段中垂线的距离                                   | 无限长带电线段（$L \to \infty$）：$E = \frac{\lambda}{2\pi\varepsilon_0 x}$（结合高斯定律更简便） |
| 带电圆环（轴线上电场）          | $E = \frac{kQx}{(x^2 + a^2)^{3/2}}$，其中 Q 为圆环总电荷，a 为圆环半径，x 为场点到圆环平面的距离                                                                     | 圆环中心（$x = 0$）：$E = 0$；远场（$x \gg a$）：$E \approx \frac{kQ}{x^2}$（近似为点电荷电场）     |
| 均匀带电圆盘（轴线上电场）        | $E = \frac{\sigma}{2\varepsilon_0}\left(1 - \frac{x}{\sqrt{x^2 + R^2}}\right)$，其中 $\sigma = \frac{Q}{\pi R^2}$（面电荷密度），R 为圆盘半径，x 为场点到圆盘的距离 | 无限大带电圆盘（$R \gg x$）：$E = \frac{\sigma}{2\varepsilon_0}$（均匀电场，无距离衰减）           |
| 无限大带电平板电场            | $E = \frac{\sigma}{2\epsilon_0}$                                                                                                          |                                                                              |
| 平行板间电场               | $E = \frac{\sigma}{\epsilon_0}$                                                                                                           |                                                                              |

## Electric Dipole
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012170245051.png)
1. Moment
	- 大小：$p = qd$
	- 方向：从负电荷 $-q$ 指向正电荷 $+q$
2. Torque
	- 大小： $\tau = \tau_+ + \tau_- = (qE) \cdot d\sin\phi=pE\sin\phi \quad$
	- 矢量式：$\vec{\tau} = \vec{p}×\vec{E}$
		- 当 $\phi = 0^\circ$（$\vec{p}$ 与 $\vec{E}$ 平行），$\tau = 0$，电偶极子处于**稳定平衡**；
		- 当 $\phi = 180^\circ$（$\vec{p}$ 与 $\vec{E}$ 反平行），$\tau = 0$，但为**不稳定平衡**（微小扰动会使电偶极子转向平行方向）。
3. 电偶极子远场电场（轴线上）
	- 公式 $E = \frac{p}{2\pi\varepsilon_0 y^3}$（y为场点到偶极子中心的距离，方向与 $\vec{p}$ 一致）
	- 特点：电场与 $y^3$ 成反比（衰减比点电荷快）

# Gauss's Law
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251013195011883.png)
可用于求电场强度
选取原则：
	1. 高斯面必须经过求 E 的点
	2. 在求 E 部分高斯面上，场强和相对方向要处处相等（方便提出 E）
	3. 不求 E 的地方 $E\cdot S=0$
## Electirc Flux
- 正负判断：闭合面默认取 “外法向”（垂直表面向外）：
    - 电场线**穿出**闭合面（如正电荷外部）：通量为**正**
    - 电场线**穿入**闭合面（如负电荷外部）：通量为**负**
    - 无电荷 / 净电荷为零：穿入与穿出的电场线数量相等，总通量为零
- 关键结论：电通量与外部电荷无关（仅内部电荷影响总通量）

## Electrostatic
### 结论
1. 导体内部电场为 0（$E_{\text{内}}=0$）：
    若 $E_{\text{内}} \neq 0$，自由电子会在电场力作用下定向移动，与 “静电平衡” 矛盾；取内部任意高斯面，$\Phi_E=0$，故内部无净电荷（电荷仅分布在表面）。
    
2. 净电荷仅分布在导体表面：    
    内部无电荷，多余电荷（如 + Q）只能分布在表面；若导体有空腔，需进一步分析
    
3. 导体表面电场垂直于表面：
    若表面电场有切向分量，自由电子会沿表面移动，破坏平衡；==电场大小==为 $E_{\text{表面}}=\frac{\sigma}{\varepsilon_0}$（$\sigma$ 为表面电荷密度）。

### 电荷分布
- **空腔内无电荷**：空腔内表面无电荷，所有多余电荷分布在导体外表面（内部电场为 0，外部电场不影响内部）；
- **空腔内有电荷 + q**：在导体内部取高斯面（包围空腔），因 $E_{\text{内}}=0$，高斯面内净电荷必须为 0，故空腔内表面感应出 - q，外表面电荷为 “总电荷 $Q_{\text{总}} + q$”（内部电荷不影响外部）。

# Electric Potential

## 带电线段
|                                                                                                     |                                                                                                     |
| :-------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012151114891.png) | ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014154533838.png) |

## 带电圆环
- 模型：半径 a、总电荷 Q 的均匀带电圆环，求轴上距离环心 x 处的电势；
- 计算：每个电荷微元 dQ 到 x 点的距离均为 $r = \sqrt{x^2 + a^2}$（对称性），故：
    $V = \int \frac{k dQ}{\sqrt{x^2 + a^2}} = \frac{k Q}{\sqrt{x^2 + a^2}}$
- 特殊点：
    - 环心（x=0）：$V = \frac{k Q}{a}$；
    - 远场（x≫a）：$V \approx \frac{k Q}{x}$（近似为点电荷电势）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251012151740809.png)

## 带电球壳
- 外部（r>R）：电场与点电荷相同 $E = \frac{k Q}{r^2}$，电势 $V = \frac{k Q}{r}$
- 表面（r=R）：$V_{\text{表面}} = \frac{k Q}{R}$
- 内部（r<R）：E=0，电势 $V_{\text{内部}} = V_{\text{表面}} = \frac{k Q}{R}$（处处相等）

## 圆柱形电容器
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014162058304.png)

圆柱形电容器由内半径$r_a$、外半径$r_b$、长度L的同轴圆柱面组成，内圆柱带正电（线密度$+\lambda$），外圆柱带负电（线密度$-\lambda$）

两圆柱面间的电势差 $V_{ab} = V_a - V_b$（a为内圆柱，b为外圆柱），通过电场的线积分计算：$V_a - V_b = \int_{r_a}^{r_b} \vec{E} \cdot d\vec{l}$ 由于电场沿径向，积分路径与电场同方向，点积简化为 $E \, dr$。代入**无限长带电线段**电场公式 $E = \frac{\lambda}{2\pi\epsilon_0 r}$，得：$V_a - V_b = \int_{r_a}^{r_b} \frac{\lambda}{2\pi\epsilon_0 r} dr= \frac{\lambda}{2\pi\epsilon_0} \ln \frac{r_b}{r_a}$

- 圆柱形电容器的电容
电容的定义为$C = \frac{Q}{V_{ab}}$，其中：
- 总电荷$Q = \lambda L$（线密度$\lambda$，长度L，总电荷为$\lambda \times L$）；
- 电势差$V_{ab} = \frac{\lambda}{2\pi\epsilon_0} \ln \frac{r_b}{r_a}$（即$V_a - V_b$）。
代入电容定义式：$C = \frac{Q}{V_{ab}} = \frac{\lambda L}{\frac{\lambda}{2\pi\epsilon_0} \ln \frac{r_b}{r_a}}$约去$\lambda$后，化简得：$C = \frac{2\pi\epsilon_0 L}{\ln \frac{r_b}{r_a}}$
单位长度的电容（更能体现结构本身的电容特性）为：$\frac{C}{L} = \frac{2\pi\epsilon_0}{\ln \frac{r_b}{r_a}}$
### 为什么等效
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014162227144.png)
## Equipotential Surface
- 等势面
- Field lines and equipotential surfaces are always mutually ==perpendicular==.
## Electrostatic
### 结论
#### 1. 导体外表面的电场 “垂直于表面”

若导体外的电场有平行于表面的分量（切向分量$E_{\parallel}$），导体表面的自由电荷会在切向电场力作用下移动，破坏 “电荷静止” 的平衡状态。因此，只有电场垂直于表面时，电荷才不会沿表面移动，满足静电平衡

#### 2. 导体表面是 “等势面”

根据功的公式$W = \vec{F} \cdot \vec{s}$，垂直时电场力做功为 0。而电势差$U_{AB} = \frac{W_{AB}}{q}$，做功为 0 意味着表面上任意两点电势差为 0，因此导体表面是等势面

#### 3. 导体内部 “整个体积电势相同”（导体是等势体）

静电平衡时，导体内部电场 $\vec{E} = 0$。在导体内部任意两点间移动电荷，电场力做功 $W = \int \vec{E} \cdot d\vec{l} = 0$（因为 $\vec{E} = 0$）。由电势差定义 $U_{AB} = \frac{W_{AB}}{q}$，做功为 0 意味着内部任意两点电势差为 0，因此导体内部整个体积电势相同（是 “等势体”）

## Gradient
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014172014504.png)


### 例题
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014163440567.png)
- 电势相等的条件（导线连接的导体等势）
	导线是导体，最终两球会达到静电平衡—— 此时两球的**电势必须相等**（否则电荷会在导线中移动，直到电势相同）。

	对于孤立导体球，表面电势等效于 “球心点电荷的电势”，因此：

	- 球 B 的电势：$V_b = \frac{q_b}{4\pi \epsilon_0 b}$；
	- 球 A 的电势：$V_a = \frac{q_a}{4\pi \epsilon_0 a}$。

	由$V_a = V_b$，得：$\frac{q_a}{a} = \frac{q_b}{b}$

- 电荷分布的推导（结合总电荷守恒）

	联立 “$\frac{q_a}{a} = \frac{q_b}{b}$” 与 “$q_a + q_b = q$”，解得：$q_a = q \cdot \frac{a}{a + b}, \quad q_b = q \cdot \frac{b}{a + b}$即：**电荷与球的半径成正比**（半径大的球，分配的电荷更多）。

- 表面电场的计算（导体球表面的电场）

	导体球表面的电场，等效于 “球心点电荷的电场”，公式为$E = \frac{q_{\text{表面}}}{4\pi \epsilon_0 r^2}$（r为球的半径）。

	- 球 A 表面的电场：$E_a = \frac{q_a}{4\pi \epsilon_0 a^2} = \frac{q \cdot \frac{a}{a + b}}{4\pi \epsilon_0 a^2} = \frac{q}{4\pi \epsilon_0 (a + b) a}$
    
	- 球 B 表面的电场：$E_b = \frac{q_b}{4\pi \epsilon_0 b^2} = \frac{q \cdot \frac{b}{a + b}}{4\pi \epsilon_0 b^2} = \frac{q}{4\pi \epsilon_0 (a + b) b}$

- 关键结论：电场与半径的关系

	对比 $E_a$ 和 $E_b$ 的表达式，若 $b \gg a$（球 B 远大于球 A），则：$E_a \gg E_b$ 即：**半径越小的导体球，表面电场越强**（因为电场与半径成反比）。

# Capacitance and Dielectrics
## Capacitor
- 构成：Any two conductors separated by an insulator（绝缘体） form a capacitor
- 工作原理：外接电源时，两个导体分别带等量异种电荷（+Q 和 - Q），之间形成电场与电势差 V，电荷 Q 与电势差 V 成正比
## Capacitance  
- 定义：电容 C 是电容器储存电荷的能力 $C = \frac{Q}{V_{ab}}$
- 单位：法拉（F），1F = 1C/V（实际常用 μF=10⁻⁶F、nF=10⁻⁹F）
- 关键性质：C 仅由电容器的几何结构（形状、尺寸） 和极板间介质决定，与 Q、V 无关
### Parallel-plate Capacitor
- 推导步骤：
    1. 求电场（高斯定律）：极板电荷面密度 $σ=\frac{Q}{A}$，板间电场 $E=\frac{σ}{ε₀}$（均匀电场）
    2. 求电势差：$V=Ed=\frac{Qd}{ε₀A}$
    3. 求电容：代入 C=Q/V，得真空下电容：
        $C_0 = \frac{\varepsilon_0 A}{d}$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028172520880.png)
### Spherical Capacitor
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028172723911.png)
- 推导步骤
    1. 求电场（高斯定律）：取半径 r（a<r<b）的同心球面为高斯面，$E=\frac{kQ}{r²}$
    2. 求电势差：$V=∫（a→b）E・dr = kQ (1/a - 1/b)$
    3. 求电容：$C=\frac{Q}{V}$，得$C_0 = \frac{4\pi\varepsilon_0 ab}{b - a}$
- 特殊情况：若外球壳半径 $b→∞$，则 $C₀=4πε₀a$（孤立导体球的电容）
### Cylindrical Capacitor
[[Course/250/Lecture Notes#圆柱形电容器|Lecture Notes]] ![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028174429513.png)
- 结构：内圆柱（半径 a，$+λ$）与外圆柱壳（半径 b，$-λ$），长度为 L，壳间为真空。$\lambda$ 为电荷的线密度
- 推导步骤：
    1. 求电场（高斯定律）：取半径 r（a<r<b）的同轴圆柱面为高斯面，$E=λ/(2πε₀r)$
    2. 求电势差：$V=∫（a→b）E・dr = (λ/(2πε₀)) ln (b/a)$
    3. 求电容：总电荷 $Q=λL$，代入 $C=Q/V$，得单位长度电容 $\frac{C_0}{L} = \frac{2\pi\varepsilon_0}{\ln(b/a)}$

## Circuit Behavior
### Series
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028174928192.png)
- 定义：电容器首尾相连，电流路径唯一（如 $C₁$的负极接 $C₂$的正极）。
- 关键规律：
    1. 电荷相等：所有串联电容带电量相同$Q₁=Q₂=Q=Q_{eq}$，因电荷只能在导体间转移，无新电荷产生
    2. 电压相加：总电压等于各电容电压之和$V_{total} = V₁ + V₂ + ...$
    3. 等效电容：由 $C=Q/V，V₁=Q/C₁，V₂=Q/C₂$，代入总电压得：
        $\frac{1}{C_{eq}} = \frac{1}{C_1} + \frac{1}{C_2} + \dots$
- 结论：串联等效电容小于任意一个分电容（相当于增大极板间距，储电能力减弱）
### Parallel
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028174939630.png)
- 关键规律：
    1. 电压相等：所有并联电容的电压相同 $V₁=V₂=V=V_{total}$
    2. 电荷相加：总电荷等于各电容电荷之和 $Q_{total} = Q₁ + Q₂ + ...$
    3. 等效电容：由 $Q=CV，Q₁=C₁V，Q₂=C₂V$，代入总电荷得：$C_{eq} = C_1 + C_2 + \dots$
- 结论：并联等效电容大于任意一个分电容（相当于增大极板面积，储电能力增强）
### 例题
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251028175207598.png)

## Energy Stored in a Capacitor
电容器储存的能量来自于 E 而不是 Q
电容器储存的能量源于*充电过程中外界对电荷做功*，这部分功转化为电势能
- 充电某一时刻，电容器带电量为q，极板间电势差$v = \frac{q}{C}$
- 转移微小电荷dq时，外界需做功$dW = v \, dq = \frac{q}{C} dq$；
- 总功（即总电势能U）是从$q=0$到$q=Q$的积分：
    $U = W = \int_0^Q \frac{q}{C} dq = \frac{Q^2}{2C}$

### 能量公式的三种形式
结合$Q = CV$，电势能可改写为三种等价形式：$U = \frac{Q^2}{2C} = \frac{1}{2}CV^2 = \frac{1}{2}QV$
- 物理意义：电容器的储能能力由C和V共同决定，C越大、V越高，储能越多。

- 电容器的储能公式 $U = \frac{1}{2} \cdot \frac{1}{C} \cdot Q^2$，与弹簧弹性势能 $U = \frac{1}{2}kx^2$ 形式完全一致
### 能量密度

- 平行板电容器的体积$V_{\text{场}} = Ad$（A为极板面积，d为间距）；
- 总能量$U = \frac{1}{2}CV^2$，代入$C = \frac{\varepsilon_0 A}{d}$和$V = Ed$（E为电场强度），得能量密度（单位体积的能量）：
    
    $u = \frac{U}{Ad} = \frac{1}{2}\varepsilon_0 E^2$

## Dielectrics 电介质
电介质是 nonconducting material，几乎所有实际电容器都含电介质，其核心作用是增大电容、提高介电强度K
### Molecular Polarization
- 分子分类与极化机制
    1. 极性分子（如 H₂O）：无电场时分子偶极矩随机排列，总极化强度为 0；加电场后，偶极矩沿电场方向对齐，产生取向极化；
    2. 非极性分子（如 CO₂）：无电场时分子正负电荷中心重合；加电场后，电荷中心分离，产生位移极化；
- 结果：电介质*表面*诱导出 “束缚电荷”（与极板电荷异号），束缚电荷产生反向电场 Eᵢ，削弱原电场 E₀，最终板间电场 E=E₀ - Eᵢ（E<E₀）。

### Dielectric Constant K
- 定义：K 是 “有介质时电容与真空电容的比值”，K=C/C₀>1
- 核心公式（平行板电容）：有介质时，电容为：
    $C = K C_0 = \frac{K\varepsilon_0 A}{d}$
### Dielectric Breakdown
- 介电强度：电介质能承受的最大电场强度 $E_{max}$（超过则击穿，变为导体）
- 击穿原因：强电场使分子电离，产生自由电荷，电流急剧增大，电容器损坏

- 电介质的前提是 “不导电”（满足绝缘体的定义），额外增加了 “可极化” 的要求；
- 反之，有些绝缘体（如干燥的木材、普通橡胶）极化效应极弱（K≈1，接近真空），无法用于优化电容器性能，因此不算电介质。
### Gauss's law in dielectrics
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251029001344485.png)

设导体自由电荷产生的真空电场为$E_0$，束缚电荷产生的反向电场为$E_i$，则总电场$E = E_0 - E_i$

- 真空时（无电介质），由高斯定律得$E_0 = \frac{\sigma}{\varepsilon_0}$；
- 有电介质时，总电场会削弱（因$E_i$与$E_0$反向），$E = \frac{E_0}{K}$
- 自由电荷（导体上的电荷）：$Q_{\text{free}} = \sigma A$（A为高斯面的底面积）；
- 束缚电荷（电介质表面的电荷）：$Q_{\text{bound}} = -\sigma_i A$；
- 总电荷：$Q_{\text{encl}} = \sigma A - \sigma_i A$。

真空高斯定律为$\oint \vec{E} \cdot d\vec{A} = \frac{Q_{\text{encl}}}{\varepsilon_0}$。由于仅电介质中的*面*元有电场通量（导体内部$E=0$），因此：$E \cdot A = \frac{(\sigma - \sigma_i)A}{\varepsilon_0}$约去A后得：$E = \frac{\sigma - \sigma_i}{\varepsilon_0}$

由$E = \frac{E_0}{K} = \frac{\sigma}{K\varepsilon_0}$，代入上式：$\frac{\sigma}{K\varepsilon_0} = \frac{\sigma - \sigma_i}{\varepsilon_0}$整理得束缚电荷与自由电荷的关系：$\sigma - \sigma_i = \frac{\sigma}{K} \quad \text{或} \quad \sigma_i = \sigma\left(1 - \frac{1}{K}\right)$这说明：电介质的K越大，束缚电荷$\sigma_i$越接近自由电荷$\sigma$，电场削弱越明显

为了避免直接计算复杂的束缚电荷，引入电位移矢量 $\vec{D} = \varepsilon \vec{E} = K\varepsilon_0 \vec{E}$（$\varepsilon = K\varepsilon_0$ 为介电常数），此时高斯定律可简化为：

$\oint \vec{D} \cdot d\vec{A} = Q_{\text{encl-free}}$ 或等价地（代入 $\vec{D} = \varepsilon \vec{E}$）：$\oint \vec{E} \cdot d\vec{A} = \frac{Q_{\text{encl-free}}}{\varepsilon}$

电介质中的高斯定律只需考虑*自由电荷*（无需再处理束缚电荷），大大简化了电场计算
快速计算介质中的电场，如平行板电容 $D=σ=Q/A，E=D/(Kε₀)=σ/(Kε₀)$

# Current, Resistance, Electromotive Force
