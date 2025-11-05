# 无限大带电平板
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014104914797.png)

|面号|位置|
|---|---|---|
|1|左板外侧|向左的外面|
|2|左板内侧|朝向空隙|
|3|右板内侧|朝向空隙|
|4|右板外侧|向右的外面|

设各面的面电荷密度分别为：
$$  
\sigma_1, \ \sigma_2, \ \sigma_3, \ \sigma_4  
$$
已知每个导体的总电荷为：
$$  
\sigma_1 + \sigma_2 = \frac{3Q}{A}, \qquad  
\sigma_3 + \sigma_4 = \frac{Q}{A}.  
\tag{1}  
$$
导体内部电场必须为零。利用无限大带电面在两侧产生的电场  
（每侧大小为 $\sigma/(2\varepsilon_0)$，方向远离正电荷），  
对左右导体内部写出电场平衡条件。

---

- 左导体内部（在面 1 与面 2 之间）

取向右为正方向。

- 面 1 在左侧 → 右侧场为 $+\sigma_1/(2\varepsilon_0)$
    
- 面 2 在右侧 → 左侧场为 $-\sigma_2/(2\varepsilon_0)$
    
- 面 3、4 都在右侧 → 对左侧点各贡献 $-\sigma_3/(2\varepsilon_0)$、$-\sigma_4/(2\varepsilon_0)$
    
导体内总场为 0：

$$  
\sigma_1 - \sigma_2 - \sigma_3 - \sigma_4 = 0.  
\tag{2}  
$$
---
- 右导体内部（在面 3 与面 4 之间）

- 面 1、2、3 在左侧 → 各产生 $+\sigma_1/(2\varepsilon_0)$、$+\sigma_2/(2\varepsilon_0)$、$+\sigma_3/(2\varepsilon_0)$
    
- 面 4 在右侧 → 产生 $-\sigma_4/(2\varepsilon_0)$
    

导体内电场为 0：
$$  
\sigma_1 + \sigma_2 + \sigma_3 - \sigma_4 = 0.  
\tag{3}  
$$
---

从 (2) 得：
$$  
\sigma_1 = \sigma_2 + \sigma_3 + \sigma_4.  
$$
代入 (3)：
$$  
(\sigma_2 + \sigma_3 + \sigma_4) + \sigma_2 + \sigma_3 - \sigma_4 = 0  
$$
化简得：

$$  
2\sigma_2 + 2\sigma_3 = 0  
\quad\Rightarrow\quad  
\sigma_2 = -\sigma_3.  
\tag{4}  
$$

再代回 (1)：

$$  
\sigma_1 + \sigma_2 = \frac{3Q}{A}, \qquad  
\sigma_3 + \sigma_4 = \frac{Q}{A}.  
$$

由 (4) 知 $\sigma_3 = -\sigma_2$，于是：

$$  
-\sigma_2 + \sigma_4 = \frac{Q}{A} \quad\Rightarrow\quad  
\sigma_4 = \sigma_2 + \frac{Q}{A}.  
\tag{5}  
$$

再把 (4)、(5) 代入 (2)：

$$  
\sigma_1 = \sigma_2 + \sigma_3 + \sigma_4 = \sigma_2 - \sigma_2 + \sigma_4 = \sigma_4.  
$$

于是：

$$  
\sigma_1 = \sigma_4.  
\tag{6}  
$$

代入 (1) 的第一式：

$$  
\sigma_1 + \sigma_2 = \frac{3Q}{A}.  
\tag{7}  
$$

同时由 (6)、(5) 可得：

$$  
\sigma_1 = \sigma_2 + \frac{Q}{A}.  
\tag{8}  
$$

将 (8) 代入 (7)：

$$  
\sigma_2 + \frac{Q}{A} + \sigma_2 = \frac{3Q}{A}  
\quad\Rightarrow\quad  
2\sigma_2 = \frac{2Q}{A}  
\quad\Rightarrow\quad  
\sigma_2 = \frac{Q}{A}.  
$$

于是：

$$  
\sigma_3 = -\frac{Q}{A}, \quad  
\sigma_1 = \sigma_4 = \frac{2Q}{A}.  
$$

---

 (3) 三个区域的电场

取向右为正方向。各区域的电场为所有面电场叠加。
- 区域 1（左外）

$$  
E_1 = -\frac{\sigma_1 + \sigma_2 + \sigma_3 + \sigma_4}{2\varepsilon_0}  
= -\frac{2Q/A + Q/A - Q/A + 2Q/A}{2\varepsilon_0}  
= -\frac{2Q}{\varepsilon_0 A}.  
$$

方向：向左。

---

- 区域 2（两板之间）

$$  
E_2 = \frac{\sigma_1 + \sigma_2 - \sigma_3 - \sigma_4}{2\varepsilon_0}  
= \frac{2Q/A + Q/A - (-Q/A) - 2Q/A}{2\varepsilon_0}  
= \frac{Q}{\varepsilon_0 A}.  
$$

方向：向右（从左板指向右板）。

---
- 区域 3（右外）

$$  
E_3 = +\frac{\sigma_1 + \sigma_2 + \sigma_3 + \sigma_4}{2\varepsilon_0}  
= +\frac{2Q}{\varepsilon_0 A}.   
$$
**物理解读：**  
内侧两面形成一对等量异号电荷，产生均匀场  
$$E_2 = \frac{Q}{\varepsilon_0 A}$$  
外侧两面各自带多余正电荷，产生向外的电场  
$$E_1 = E_3 = \frac{2Q}{\varepsilon_0 A}.$$
# 带电球体

### 子题 A：空心绝缘球（总电荷Q，内半径r，外半径$R=2r$）

#### 子题 B：实心绝缘球（总电荷Q，半径R）

问题：Find the electric potential V everywhere (inside/outside)，$V(\infty)=0$.
#### 解题步骤（以空心球为例，实心球类似）

##### 第一步：用高斯定理求各区域电场E

#### 1. 区域 1：空腔内部（$r' < r$）

- 高斯面：半径$r' < r$的球面，内部电荷$Q_{\text{内}} = 0$；
- 高斯定理：$E_1 \cdot 4\pi r'^2 = \frac{0}{\varepsilon_0} \implies E_1 = 0$。

#### 2. 区域 2：球壳内部（$r < r' < R$）

- 电荷体密度：$\rho = \frac{Q}{\frac{4}{3}\pi(R^3 - r^3)} = \frac{3Q}{4\pi(8r^3 - r^3)} = \frac{3Q}{28\pi r^3}$（因$R=2r$）
- 高斯面内电荷：$Q_{\text{内}} = \rho \cdot \frac{4}{3}\pi(r'^3 - r^3) = Q \cdot \frac{r'^3 - r^3}{R^3 - r^3}$；
- 高斯定理：$E_2 \cdot 4\pi r'^2 = \frac{Q_{\text{内}}}{\varepsilon_0} \implies E_2 = \frac{Q(r'^3 - r^3)}{4\pi\varepsilon_0 r'^2 (R^3 - r^3)}$（径向向外）

#### 3. 区域 3：球壳外部（$r' > R$）

- 高斯面内电荷$Q_{\text{内}} = Q$；
- 高斯定理：$E_3 \cdot 4\pi r'^2 = \frac{Q}{\varepsilon_0} \implies E_3 = \frac{Q}{4\pi\varepsilon_0 r'^2}$（与点电荷电场一致）

##### 第二步：积分求各区域电势V

#### 1. 区域 3：外部（$r' > R$）

$V_3 = \int_{r'}^{\infty} E_3 dr' = \int_{r'}^{\infty} \frac{Q}{4\pi\varepsilon_0 r'^2} dr' = \frac{Q}{4\pi\varepsilon_0 r'}$

#### 2. 区域 2：球壳内部（$r < r' < R$）

需分两段积分（$r' \to R$用$E_2$，$R \to \infty$用$V_3(R)$）：$V_2 = \int_{r'}^{R} E_2 dr' + V_3(R)$代入$E_2$和$V_3(R) = \frac{Q}{4\pi\varepsilon_0 R}$，积分得：$V_2 = \frac{Q}{8\pi\varepsilon_0 (R^3 - r^3)} \left( 3R^2 - r'^2 - \frac{2r^3}{r'} \right)$（代入$R=2r$可简化为具体表达式）

#### 3. 区域 1：空腔内部（$r' < r$）

$V_1 = \int_{r'}^{r} E_1 dr' + V_2(r) = 0 + V_2(r) = \frac{Q}{8\pi\varepsilon_0 (R^3 - r^3)} \left( 3R^2 - 3r^2 \right) = \frac{3Q(R^2 - r^2)}{8\pi\varepsilon_0 (R^3 - r^3)}$

#### 实心球电势（简化）

- 外部（$r' > R$）：$V_3 = \frac{Q}{4\pi\varepsilon_0 r'}$；
- 内部（$r' < R$）：$E_2 = \frac{Q r'}{4\pi\varepsilon_0 R^3}$，积分得$V_2 = \frac{Q}{8\pi\varepsilon_0 R} \left( 3 - \frac{r'^2}{R^2} \right)$。

#### 答案总结（空心球，$R=2r$）

- 空腔内部（$r' < r$）：$V_1 = \frac{3Q(4r^2 - r^2)}{8\pi\varepsilon_0 (8r^3 - r^3)} = \frac{9Q}{56\pi\varepsilon_0 r}$；
- 球壳内部（$r < r' < 2r$）：$V_2 = \frac{Q}{8\pi\varepsilon_0 \cdot 7r^3} \left( 12r^2 - r'^2 - \frac{2r^3}{r'} \right)$；
- 外部（$r' > 2r$）：$V_3 = \frac{Q}{4\pi\varepsilon_0 r'}$

# Nonuniform
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251105105935220.png)
1. 选取电荷元dQ
    - 取y处的微元dy，$dQ = |y| dy$（因$y \in [-a, +a]$，$|y|$对称，可计算$y>0$部分再乘 2）；
    - dQ到P点的距离：$r = \sqrt{x^2 + y^2}$。
2. 计算dQ在P点的电场$d\vec{E}$
    - 大小：$dE = \frac{1}{4\pi\varepsilon_0} \cdot \frac{dQ}{r^2} = \frac{|y| dy}{4\pi\varepsilon_0 (x^2 + y^2)}$；
    - 方向：沿r向外（正电荷），与 x 轴夹角$\theta$，故 x 分量$dE_x = dE \cos\theta = dE \cdot \frac{x}{r}$，y 分量$dE_y = dE \sin\theta$（对称抵消）。
3. 积分求总电场（x 方向，$y>0$部分乘 2）$E = 2 \int_{y=0}^{a} dE_x = 2 \cdot \frac{x}{4\pi\varepsilon_0} \int_{0}^{a} \frac{y dy}{(x^2 + y^2)^{3/2}}$
    
4. 计算积分（换元法）令$u = x^2 + y^2$，$du = 2y dy \implies y dy = \frac{du}{2}$，积分限$u \in [x^2, x^2 + a^2]$：$\int_{0}^{a} \frac{y dy}{(x^2 + y^2)^{3/2}} = \frac{1}{2} \int_{x^2}^{x^2 + a^2} u^{-3/2} du = \frac{1}{2} \left( \frac{2}{\sqrt{x^2}} - \frac{2}{\sqrt{x^2 + a^2}} \right) = \frac{1}{x} - \frac{1}{\sqrt{x^2 + a^2}}$
    
5. **总电场**$E = \frac{x}{2\pi\varepsilon_0} \left( \frac{1}{x} - \frac{1}{\sqrt{x^2 + a^2}} \right) = \frac{1}{2\pi\varepsilon_0} \left( 1 - \frac{x}{\sqrt{x^2 + a^2}} \right)$
P点电场大小为$\boxed{E = \frac{1}{2\pi\varepsilon_0} \left( 1 - \frac{x}{\sqrt{x^2 + a^2}} \right)}$，方向沿 x 轴正方向（若$x>0$）
# 静电平衡
### 已知条件
- 内球壳导体：内半径 a，外半径 b，总电荷 +q
- 外球壳导体：内半径 c，外半径 d，总电荷 −q
- 两球壳同心，满足 a<b<c<d;

要求：  
(a) 求各个表面上的电荷分布；  
(b) 求各区域的电场；  
(c) 求两壳之间的电容。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014110905919.png)
## (a) 各表面的电荷分布

四个表面分别记作：
- 内球壳内表面：$r=a$，电荷 $Q_a$
- 内球壳外表面：$r=b$，电荷 $Q_b$
- 外球壳内表面：$r=c$，电荷 $Q_c$
- 外球壳外表面：$r=d$，电荷 $Q_d$
内球壳总电荷为 $+q$，即  $Q_a + Q_b = +q$
外球壳总电荷为 $-q$，即 $Q_c + Q_d = -q$
### 空腔内 ($r<a$) 无电荷

导体内部电场必须为零。  
若空腔内没有电荷，则不需要感应电荷来抵消电场，因此  $Q_a = 0$
于是由内壳总电荷为 $+q$：  $Q_b = +q$
### 内外壳之间 ($b<r<c$)
在该区域内，包围的总电荷为 $+q$，因此外壳的内表面会被感应出 $-q$： $Q_c = -q$  
为保持外壳总电荷为 $-q$：  $Q_d = 0$

因此四个表面的电荷分布为：  
$Q_a = 0,\qquad Q_b = +q,\qquad Q_c = -q,\qquad Q_d = 0$
## (b) 各区域的电场
利用球对称性（或高斯定律结果）：  
$$  
E(r) = \frac{1}{4\pi\varepsilon_0}\frac{Q_{\text{enc}}}{r^2},  
$$
其中 $Q_{\text{enc}}$ 为半径 $r$ 内所包围的净电荷。
### 区域划分
1. **区域 I：$r < a$** , $Q_{\text{enc}}=0 \Rightarrow E=0$
2. **区域 II：$a < r < b$**  
    导体内部电场为零：  $E=0.$
3. **区域 III：$b < r < c$**  
    包含内壳电荷 $+q$：  $E(r)=\frac{1}{4\pi\varepsilon_0}\frac{q}{r^2},\quad \text{方向向外。}$
4. **区域 IV：$c < r < d$**  
    导体内部电场为零：  $E=0.$
5. **区域 V：$r > d$**  
    所包围的净电荷为 $+q - q = 0$：  $E=0.$
因此：  
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014111353797.png)
## (c) 电容

电势差定义为  
$$  
V = \int_b^c E(r),dr  
= \frac{q}{4\pi\varepsilon_0}\int_b^c \frac{dr}{r^2}  
= \frac{q}{4\pi\varepsilon_0}\Big(\frac{1}{b}-\frac{1}{c}\Big).  
$$

于是电容为  
$$  
C = \frac{q}{V}  
= 4\pi\varepsilon_0 \frac{bc}{c-b}.  
$$
# 同位素
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251103203722928.png)
1. 离子化与加速

铀原子核（带电量 $q = +Ze$，$Z=92$）通过 “离子源” 电离为正离子，进入 “加速电场”（电压 U）：由动能定理：$qU = \frac{1}{2}mv^2 \implies v = \sqrt{\frac{2qU}{m}}$（m 为离子质量，v 为加速后速度）。

2. 速度选择器（选速）

速度选择器由 “垂直的电场 E 和磁场 $B_1$” 组成：

- 电场力：$F_E = qE$（沿 E 方向）；
- 洛伦兹力：$F_B = qvB_1$（与 E 方向相反）；
- 仅当 $F_E = F_B$ 时，离子直线通过选择器：$qE = qvB_1 \implies v = \frac{E}{B_1}$（与质量无关，所有离子获得相同速度 v）。

3. 偏转磁场分离

离子进入 “垂直于速度的均匀磁场 $B_2$”，做匀速圆周运动：

- 洛伦兹力提供向心力：$qvB_2 = \frac{mv^2}{R} \implies R = \frac{mv}{qB_2}$；
- 因 $v = \frac{E}{B_1}$ 相同，$R \propto m$：$m_{238} > m_{235} \implies R_{238} > R_{235}$；

4. 探测与分离

不同半径的圆周运动使铀 - 235 和铀 - 238 的离子打在探测器的不同位置，实现分离。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251103220616535.png)

# 安培力
## EX 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251103224227949.png)
$I = \frac{\varepsilon}{R} = \boxed{\frac{BLv}{R}}$
- 电流元 $d\vec{l}$ 的方向为半圆环的切线方向，设其与水平方向夹角为 $\theta$（$\theta \in [-\frac{\pi}{2}, \frac{\pi}{2}]$），则 $d\vec{l} = \frac{L}{2} d\theta (-\sin\theta \hat{x} + \cos\theta \hat{y})$。
- 磁场 $\vec{B} = -B\hat{z}$，因此叉乘 $d\vec{l} \times \vec{B} = \frac{L}{2} B d\theta (-\cos\theta \hat{x} - \sin\theta \hat{y})$。
- x 方向分量：$F_x = I \cdot \frac{BL}{2} \int_{-\frac{\pi}{2}}^{\frac{\pi}{2}} (-\cos\theta) d\theta$
    积分得 $\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}} \cos\theta d\theta = 2$，因此：$F_x = I \cdot \frac{BL}{2} \cdot (-2) = -IBL$
- y 方向分量：$F_y = I \cdot \frac{BL}{2} \int_{-\frac{\pi}{2}}^{\frac{\pi}{2}} (-\sin\theta) d\theta$
    积分得 $\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}} \sin\theta d\theta = 0$，因此 $F_y = 0$
代入 $I = \frac{BLv}{R}$，得总安培力：$F = |F_x| = \frac{BLv}{R} \cdot BL = \boxed{\frac{B^2 L^2 v}{R}}$

## EX 2
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251103232131352.png)
安培力微元为 $d\vec{F} = I d\vec{l} \times \vec{B}$，总力是所有电流元受力的矢量积分：$\vec{F} = \int I d\vec{l} \times \vec{B}$
- 段 (1)：直线段垂直于磁场平面，$d\vec{l}$ 与 $\vec{B}$ 平行（$\sin\phi = 0$），故 $\vec{F}_1 = 0$。
- 段 (3)：直线段平行于 x- 轴，$d\vec{l} = -L\hat{i}$，$\vec{B} = B\hat{k}$，则 $d\vec{F}_3 = I(-L\hat{i}) \times (B\hat{k}) = ILB\hat{j}$，总力 $\vec{F}_3 = ILB\hat{j}$
- 段 (2)：半圆环（半径 R），取电流元 $d\vec{l} = R d\theta (-\sin\theta\hat{i} + \cos\theta\hat{j})$，则 $d\vec{F}_2 = I d\vec{l} \times \vec{B} = IRB d\theta (-\cos\theta\hat{i} - \sin\theta\hat{j})$
    - x- 分量积分：$F_{2x} = IRB \int_0^\pi \cos\theta d\theta = 0$（对称性抵消）
    - y- 分量积分：$F_{2y} = IRB \int_0^\pi \sin\theta d\theta = 2IRB$，故 $\vec{F}_2 = 2IRB\hat{j}$
矢量叠加得：$\vec{F} = \vec{F}_1 + \vec{F}_2 + \vec{F}_3 = 0 + 2IRB\hat{j} + ILB\hat{j} = IB(2R + L)\hat{j}$
## EX 3
长直电流 I 产生的磁场为 $B = \frac{\mu_0 I}{2\pi r}$，方向垂直纸面向里（右手定则）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251103233502498.png)

水平电流段沿 x- 轴从 $x=b$ 到 $x=c$，电流为 I，电流元 $d\vec{l} = dx\hat{i}$。安培力微元 $d\vec{F} = I d\vec{l} \times \vec{B} = I \cdot dx\hat{i} \times \left( -\frac{\mu_0 I}{2\pi x}\hat{z} \right) = \frac{\mu_0 I^2}{2\pi x} dx\hat{y}$
总力积分：$F = \int_{b}^{c} \frac{\mu_0 I^2}{2\pi x} dx = \frac{\mu_0 I^2}{2\pi} \ln\frac{c}{b}$，方向沿 y- 轴正方向

