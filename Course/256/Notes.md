# Syllabus
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20250925164753578.png)
#### 1. ODE 与 PDE 的本质区别

| 类型         | 核心特征                | 独立变量数量        | 例子（后续展开）                                                                   |
| ---------- | ------------------- | ------------- | -------------------------------------------------------------------------- |
| ODE（常微分方程） | 仅含**常导数**（对单一自变量求导） | 1 个（如 $t,x$）  | $\frac{d y}{d x}=x y$                                                      |
| PDE（偏微分方程） | 含**偏导数**（对多个自变量求导）  | ≥2 个（如 $t,x$） | $\frac{\partial u}{\partial t}=\alpha^2 \frac{\partial^2 u}{\partial x^2}$ |


#### 2. ODE 的细分分类

##### （1）显式 ODE 与隐式 ODE

- **显式 ODE (explicit)**：能解出最高阶导数，形式为：$\frac{d^n y}{d t^n} = g\left(t, y, \frac{d y}{d t}, ..., \frac{d^{n-1} y}{d t^{n-1}}\right)$
- 例子：$\frac{d y}{d x}=x y$
- **隐式 ODE**：无法解出最高阶导数，形式为：$G\left(t, y, \frac{d y}{d t}, ..., \frac{d^n y}{d t^n}\right) = 0$
- 例子：$e^{(y')^2}=\sin y$（一阶隐式，$y'$ 含在指数中）、$\frac{d^2 y}{d x^2} +1 - y^2=0$（二阶隐式，即 $G(x,y,y'')=y'' +1 -y^2=0$）。

##### （2）线性 ODE 与非线性 ODE

- **线性 ODE 的标准形式** $a_n(t) \frac{d^n y}{d t^n} + a_{n-1}(t) \frac{d^{n-1} y}{d t^{n-1}} + ... + a_1(t) \frac{d y}{d t} + a_0(t) y = F(t)$ 核心定义：最高阶导数、各阶导数及因变量y均为**一次项**，系数 $a_n(t),...,a_0(t)$ 仅依赖自变量t（或为常数），“零阶导数（zeroth ader danvatie）” 即y本身。

- **分类补充**：
    - 若系数 $a_n(t),...,a_0(t)$ 为常数→**常系数线性 ODE** constant coefficient；
    - 若右边 $F(t)=0$ →**齐次线性 ODE (homogeneous)**
    - 若 $F(t)≠0$ →**非齐次线性 ODE**。
- **非线性 ODE**：不满足线性定义（含高阶导数的乘积、幂次、三角函数等）
- 例子：
    - $\frac{d y}{d x} \cdot y +3 =x^2$（含 $y \cdot \frac{d y}{d x}$ 的乘积项）；
    - $\frac{d^2 y}{d t^2} + \sin y=0$（含 $\sin y$ 的非线性项）。

##### （3）自治 ODE 与非自治 ODE
- **自治 ODE (autonomous)**：显式形式中，g不依赖于**自变量**（如t），即：$\frac{d^n y}{d t^n} = h\left(y, \frac{d y}{d t}, ..., \frac{d^{n-1} y}{d t^{n-1}}\right)$
- 例子：$y'' + 2 =0$（不依赖t）、$\frac{d y}{d t}=y^2$（不依赖t）。

- **非自治 ODE**：g依赖于自变量，
- 例子：$y'' +5 = \sin t$（依赖t）、$\frac{d y}{d t}=t y$（依赖t）。
- 物理意义：自治 ODE 对应 “时间平移不变” 的系统（如无外力的单摆运动），非自治 ODE 对应受时变外力的系统（如受周期力的单摆）。