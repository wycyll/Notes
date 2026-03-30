# Chap 1
## 1. Signal Classification
### 1.1 By Domain Dimension (Number of Inputs)

| Type       | Definition                                        | Example                              |
| :--------- | :------------------------------------------------ | :----------------------------------- |
| 1D Signal  | Function of one variable (most common: time $t$). | Audio sound $s(t)$, ECG voltage.     |
| M-D Signal | Function of multiple variables.                   | 3D volumetric data, video $I(x,y,t)$ |

### 1.2 By Range Dimension (Output Type)

| Type                | Definition                                | Example                                |
| :------------------ | :---------------------------------------- | :------------------------------------- |
| Scalar Signal       | Output is a single number (real/complex). | Grayscale image intensity.             |
| Multichannel Signal | Output is a vector.                       | RGB color image (3 channels: R, G, B). |

### 1.3 By Time Characteristics

| Type                 | Definition                             | Example                                  |
| :------------------- | :------------------------------------- | :--------------------------------------- |
| Continuous-Time (CT) | Defined for all $t∈R.$                 | Sine wave, exponential decay.            |
| Discrete-Time (DT)   | Defined only at discrete points $n∈Z$. | Stock prices daily, monthly temperature. |

### 1.4 By Amplitude Characteristics

| Type              | Definition                                   | Example                      |
| :---------------- | :------------------------------------------- | :--------------------------- |
| Continuous-Valued | Amplitude can take any value in an interval. | 0V to 5V analog voltage.     |
| Discrete-Valued   | Amplitude takes only finite/discrete values. | Digital counts (0, 1, 2...). |

### 1.5 By Determinism

| Type          | Definition                                    | Example                       |
| :------------ | :-------------------------------------------- | :---------------------------- |
| Deterministic | Exact mathematical formula predicts behavior. | $sin(2πft)$.                  |
| Random        | Unpredictable, described by probability.      | White noise, background hiss. |
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260325195655170.png)

## 2. Transformations of CT Signals
### 2.1 Time Transformations
Modify the time axis of x(t):
1. Time reversal (folding): $y(t)=x(−t)$ → reverse playback (mirror around $t=0$).
2. Time scaling: $y(t)=x(at)$
    - $a>1$: Compress the signal (fast playback).
    - $0<a<1$: Stretch the signal (slow motion).
3. Time shifting: $y(t)=x(t−t_{0}​)$
    - $t_{0}>0$: Delay (shift right); $t_{0}​<0$: Advance (shift left).
    - _Physical systems can only delay signals_.
4. General time transformation: 两种形式
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260325200511910.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260325200522642.png)

### 2.2 Amplitude Transformations
- Amplitude reversal: $y(t)=−x(t)$
- Amplitude scaling: $y(t)=a⋅x(t)$
- Amplitude shifting: $y(t)=x(t)+b$
### 2.3 Calculus Operations

- Differentiator: $y(t)=\frac{d}{d​t}x(t)$ (derivative undefined at sharp corners, e.g., $x(t)=e^{-2|t|}$ at $t=0$).
- Integrator: $y(t) = \int_{-\infty}^t x(\tau) d\tau$→ running integral (output is a _function_, not a single number—different from calculus area calculation).

### 2.4 Two-Signal Operations
- Addition: $y(t) = x_1(t) + x_2(t)$.
- Multiplication: $y(t) = x_1(t) \cdot x_2(t)$ (core of Amplitude Modulation (AM)).
## 3. Signal Characteristics

### 3.1 Periodicity
- Periodic: Exists $T > 0$ such that $x(t+T) = x(t)$ for all $t$. $T$ is the fundamental period.
- Theorem: The sum of two periodic signals is periodic if and only if the ratio of their periods is rational ($T_1/T_2 \in \mathbb{Q}$). If irrational, the sum is aperiodic.
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326000301087.png)
### 3.2 Even & Odd Symmetry
- Even: $x(-t) = x(t)$ (symmetric about y-axis, e.g., $\cos(t), e^{-t^2}$).
- Odd: $x(-t) = -x(t)$ (symmetric about origin, $x(0)=0$, e.g., $\sin(t), t$).
- Decomposition: Any signal can be uniquely split into even + odd parts: 
- $x(t) = \underbrace{\frac{1}{2}[x(t) + x(-t)]}_{x_e(t)} + \underbrace{\frac{1}{2}[x(t) - x(-t)]}_{x_o(t)}$
### 3.3 Average, Energy & Power
These quantify the "strength" of a signal over time.

| Metric        | Formula                                                         | Definition                      |
| :------------ | :-------------------------------------------------------------- | :------------------------------ |
| Average Value | $A = \lim_{T\to\infty} \frac{1}{2T} \int_{-T}^{T} x(t)dt$       | DC component (DC offset).       |
| Energy ($E$)  | $E = \int_{-\infty}^{\infty}\|x(t)\|^2 dt$                      | Total "strength" of the signal. |
| Power ($P$)   | $P = \lim_{T\to\infty} \frac{1}{2T} \int_{-T}^{T}\|x(t)\|^2 dt$ | Average energy per unit time.   |
#### Signal Classification
- Energy Signal: $E < \infty$ (e.g., $e^{-t}u(t)$). Implies $P = 0$.
- Power Signal: $P < \infty$ (e.g., $\sin(t)$). Implies $E = \infty$.
- Neither: $E = \infty, P = \infty$ (e.g., $x(t) = t^2$, non-physical).
## 4. Exponential Signals
These are fundamental because they are solutions to Linear Constant-Coefficient Differential Equations (LCCDEs) (e.g., RC/RL circuits).
### General Form
$x(t) = ce^{at}$

| Parameter              | Behavior                                                                                               |
| :--------------------- | :----------------------------------------------------------------------------------------------------- |
| $a > 0$                | Growing exponential (unstable).                                                                        |
| $a < 0$                | Decaying exponential (stable, e.g., capacitor voltage).                                                |
| $a = j\omega$          | Complex Exponential ($e^{j\omega t}$). Uses Euler's formula: $e^{j\theta} = \cos\theta + j\sin\theta$. |
| $a = \sigma + j\omega$ | Damped Sinusoid ($e^{\sigma t}\cos(\omega t)$) (decaying oscillation).                                 |

## 5. Singularity Functions
These are idealized mathematical constructs used to model physical phenomena (switches, impulses).
### 5.1 Unit Step Function $u(t)$
$u(t) = \begin{cases} 1, & t > 0 \\ 0, & t < 0 \end{cases}$
- Model: Ideal switch (closes at $t=0$).
- Use: Simplifies piecewise functions (e.g., $x(t) = e^{-t}\sin(5t)u(t-2)$).
### 5.2 Rectangle Function $rect(t)$
$rect(t) = \begin{cases} 1, & -1/2 < t < 1/2 \\ 0, & \text{otherwise} \end{cases}$
- Relation: $rect(t) = u(t+1/2) - u(t-1/2)$.
- Use: Defining a finite-duration pulse.
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326004244254.png)

三角：
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326004901146.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326004832673.png)
### 5.3 Unit Impulse Function $\delta(t)$ (Dirac Delta)
An ideal pulse with zero width, infinite height, and unit area. It is defined by its properties, not standard calculus.
#### Key Properties
1. Sifting Property: $\int_{-\infty}^{\infty} x(t)\delta(t-t_0)dt = x(t_0)$ (extracts the value at $t_0$).
2. Sampling Property: $x(t)\delta(t-t_0) = x(t_0)\delta(t-t_0)$.
3. Scaling: $\delta(at) = \frac{1}{|a|}\delta(t)$ (compression stretches the area).
4. Relation to Step: $\delta(t) = \frac{d}{dt}u(t)$ and $u(t) = \int_{-\infty}^t \delta(\tau)d\tau$.
5. Algebraic: $t\delta(t) = 0$.
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326011351048.png)
#### Practical impulse function
$\boldsymbol{\delta_\Delta(t)}$ 是一个分段矩形脉冲，对任意 $\Delta > 0$，定义为：
$$\delta_\Delta(t) \triangleq 
\begin{cases}
\dfrac{1}{\Delta}, & 0 < t < \Delta, \\
0, & \text{otherwise}.
\end{cases}$$
- 仅在区间 $(0, \Delta)$ 内有非零值，值为 $\frac{1}{\Delta}$
- 其余时间点取值为 0
##### 1. 面积恒为 1（Unity Area）
这个脉冲的宽度是 $\Delta$（从 $t=0$ 到 $t=\Delta$），高度是 $\frac{1}{\Delta}$，因此：
$\text{面积} = \text{宽度} \times \text{高度} = \Delta \times \frac{1}{\Delta} = 1$
这是冲激函数最本质的特征：总强度（积分）始终为 1，无论 $\Delta$ 取多小。
##### 2. 极限行为 ($\boldsymbol{\Delta \to 0}$）
当 $\Delta$ 趋近于 0 时：
- 宽度：$\Delta \to 0$，脉冲变得越来越窄，最终只在 $t=0$ 这一点附近有非零值
- 高度：$\frac{1}{\Delta} \to \infty$，脉冲变得越来越高，趋近于无穷大
- 面积：始终保持为 1，这保证了极限下能得到理想狄拉克 $δ$ 函数 $\delta(t)$。

理想狄拉克δ函数 $\delta(t)$ 是一个广义函数：它在 $t \neq 0$ 时为 0，在 $t=0$ 处“无穷大”，且积分面积为 1。现实中无法直接实现这样的信号，所以用 $\delta_\Delta(t)$ 作为近似模型——$\Delta$ 越小，这个矩形脉冲就越接近理想冲激的行为。

从广义函数的角度，理想δ函数正是 $\delta_\Delta(t)$ 在 $\Delta \to 0$ 时的极限：
$\delta(t) = \lim_{\Delta \to 0} \delta_\Delta(t)$
这个极限不是普通函数的逐点极限，而是在积分作用下的极限：对任意光滑测试函数 $\phi(t)$，
$\int_{-\infty}^{\infty} \phi(t) \delta_\Delta(t) dt = \int_{0}^{\Delta} \phi(t) \cdot \frac{1}{\Delta} dt \xrightarrow{\Delta \to 0} \phi(0)$
这正好匹配了δ函数的筛分性质（提取 $t=0$ 处的值）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260326012705969.png)
> e.g. p250
> 注意 rect 和 u (t) 的转换方式不同
## 6. Continuous-Time (CT) Systems
A system maps $x(t) \to y(t)$ via an operator $\mathcal{T}$: $y(t) = \mathcal{T}[x(t)]$.
### 6.1 System Representation
- Input-Output Equation: Mathematical rule (e.g., $y(t) = \int_{-\infty}^t x(\tau)d\tau$).
- Block Diagrams: Visual representation using adders, integrators, differentiators.
- Interconnections:
  - Series (Cascade): $y(t) = \mathcal{T}_2[\mathcal{T}_1[x(t)]]$.
  - Parallel: $y(t) = \mathcal{T}_1[x(t)] + \mathcal{T}_2[x(t)]$.
### 6.2 Six Core System Properties
Properties are split into Amplitude and Time categories.
#### A. Amplitude Properties
1. Linearity
   - Definition: Satisfies superposition:
     $\mathcal{T}[a_1x_1 + a_2x_2] = a_1\mathcal{T}[x_1] + a_2\mathcal{T}[x_2]$
   - Implication: Zero input leads to zero output.
   - Examples: Integrator, amplifier. Non-examples: $y=x^2$ (non-linear), $y=2x+3$ (affine, not linear).
2. BIBO Stability (Bounded Input, Bounded Output)
   - Definition: If $|x(t)| \le M_x < \infty$, then $|y(t)| \le M_y < \infty$.
   - Examples: Stable (amplifier, moving average). Unstable (integrator: input $u(t)$ leads to output $tu(t)$, which grows without bound).
3. Invertibility
   - Definition: Each output corresponds to exactly one input (unique inverse system $\mathcal{T}^{-1}$).
   - Examples: Invertible (amplifier $y=2x$). Non-invertible (rectifier $y=|x|$ loses sign information).

#### B. Time Properties
4. Causality
   - Definition: Output $y(t)$ depends only on present and past inputs ($\tau \le t$), not future inputs.
   - Examples: Causal (integrator). Non-causal (symmetric moving average: $y(t) = \frac{1}{2T}\int_{t-T}^{t+T}x(\tau)d\tau$).
5. Memory (Memoryless vs. Dynamic)
   - Memoryless: $y(t)$ depends only on $x(t)$ (e.g., $y=2x$). All memoryless systems are causal.
   - Dynamic (With Memory): $y(t)$ depends on past/future inputs (e.g., integrator, delay).
6. Time-Invariance
   - Definition: A time shift in the input results in an identical time shift in the output. If $x(t) \to y(t)$, then $x(t-t_0) \to y(t-t_0)$.
   - Test Method:
     1. Compute $y(t)$ for $x(t)$.
     2. Compute $y(t-t_0)$ (shift the output).
     3. Compute $y_d(t)$ using $x_d(t) = x(t-t_0)$ as input.
     4. If $y(t-t_0) = y_d(t)$, the system is time-invariant.
   - Examples: Invariant (integrator). Variant (modulator $y(t) = \cos(\pi t)x(t)$, time-reversal $y=3x(-t)$, $y (t)=x (at)$). p369
   - A time-varying gain results in a time-varying system while systems with constant gains are time-invariant (e.g., $y(t) = 2x(t)$).
#### Time Properties
4. Causality: Output y(t) depends only on present/past inputs (no future inputs).
    - Causal: Integrator; Non-causal: Symmetric moving average.
5. Memory:
    - Memoryless (static): y(t) depends only on x(t) (e.g., y=2x(t)).
    - Dynamic (with memory): Depends on past inputs (e.g., integrator).
    - _All memoryless systems are causal_.
6. Time-Invariance: Input shift → output shift (x(t−t0​)→y(t−t0​)).
    - Time-invariant: Integrator, amplifier.
    - Time-varying: Modulator y(t)=cos(πt)x(t), time reversal y=3x(−t).

# Chap 2
## 1. Introduction to LTI Systems
### 1.1 What is an LTI System?
A system is LTI if it satisfies two fundamental properties:
#### (1) Linearity
A system is linear if it obeys superposition:
For any inputs $x_1(t), x_2(t)$ and constants $a_1, a_2$:
$\mathcal{T}\left[a_1x_1(t)+a_2x_2(t)\right] = a_1y_1(t)+a_2y_2(t)$
This combines:
- Additivity: $\mathcal{T}[x_1+x_2] = y_1+y_2$
- Homogeneity (scaling): $\mathcal{T}[ax] = ay$

#### (2) Time-Invariance
A system is time-invariant if delaying the input only delays the output (no shape change):
If $x(t) \to y(t)$, then $x(t-t_0) \to y(t-t_0)$.

### 1.2 LTI Core Result
Every LTI system is fully described by its impulse response $h(t)$, and input-output is given by convolution:
$y(t) = x(t) * h(t) = \int_{-\infty}^{\infty}x(\tau)h(t-\tau)d\tau$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260330184134992.png)

## 2. Analysis Technique for Linear Systems
The universal strategy for linear systems is:
1. Decompose the complex input $x(t)$ into simple elementary signals (e.g., impulses $\delta(t)$, complex exponentials $e^{j\omega t}$).
2. Compute the response of the system to each elementary signal.
3. Apply superposition: Sum the weighted responses to get the total output $y(t)$.
This works *only* for linear systems.
## 3. Impulse Response $h(t)$
### 3.1 Definition
The impulse response $h(t)$ of an LTI system is:
The output of the system when the input is a unit impulse $\delta(t)$.
$\delta(t) \xrightarrow{\mathcal{T}} h(t)$
$h(t)$ contains *all information* about the LTI system.

### 3.2 Examples
#### Example 1: Moving Average System
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260330185337646.png)
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260330185349624.png)
#### Example 2: Toy Car System
Input = applied force; Output = velocity.
After an impulse hit (instant force), velocity decays due to friction:
$h(t) = ae^{-t/b}u(t)$
- $a$: Related to the car's mass
- $b$: Related to friction

---

## 5. Impulse Representation of CT Signals
Any CT signal $x(t)$ can be written as an integral (weighted sum) of impulses:
$x(t) = \int_{-\infty}^{\infty}x(\tau)\delta(t-\tau)d\tau$
This comes from the sifting property of $\delta(t)$:
$\int_{-\infty}^{\infty}x(t)\delta(t-t_0)dt = x(t_0)$
This decomposition is the foundation of convolution.

---

## 6. Convolution Integral for LTI Systems
### 6.1 Derivation
1. Decompose $x(t)$ into impulses: $x(t) = \int x(\tau)\delta(t-\tau)d\tau$
2. By linearity: System acts on each impulse
3. By time-invariance: $\delta(t-\tau) \to h(t-\tau)$
4. Result: Convolution integral
$y(t) = \int_{-\infty}^{\infty}x(\tau)h(t-\tau)d\tau = x(t)*h(t)$

### 6.2 Graphical Convolution Steps
1. Fold: $h(\tau) \to h(-\tau)$
2. Shift: $h(-\tau) \to h(t-\tau)$
3. Multiply: $x(\tau) \cdot h(t-\tau)$
4. Integrate: Compute the area of the product

### 6.3 Examples
- Input: Unit step $u(t)$; Output: Step response $s(t)$
- Input: Rectangular pulse; Output: Piecewise exponential signal

---

## 7. Properties of Convolution
Convolution obeys 3 key algebraic rules:
1. Commutative: $x*h = h*x$
2. Associative: $(x*h_1)*h_2 = x*(h_1*h_2)$ (for cascaded systems)
3. Distributive: $x*(h_1+h_2) = x*h_1 + x*h_2$ (for parallel systems)

Special convolution with impulses:
- $x(t)*\delta(t) = x(t)$
- $x(t)*\delta(t-t_0) = x(t-t_0)$

---

## 8. LTI System Properties (from $h(t)$)
All LTI system characteristics are determined only by $h(t)$.

### 8.1 Causality
A causal system's output depends only on past/present inputs (no future inputs).
Rule: $h(t) = 0$ for all $t<0$
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260330211810168.png)
### 8.2 Memory
- Memoryless: Output depends *only* on current input.
  Rule: $h(t) = a\delta(t)$ 除 $x=0$ 外 $h (t)=0$
- Dynamic (with memory): All other systems
  - FIR (Finite Impulse Response): $h(t)$ non-zero over finite time 某些地方有值
  - IIR (Infinite Impulse Response): $h(t)$ non-zero forever 一直有值
### 8.3 BIBO Stability
A system is stable if bounded input → bounded output.
Rule: $h(t)$ is absolutely integrable
$\int_{-\infty}^{\infty}|h(t)|dt < \infty$
### 8.4 Invertibility
A system is invertible if we can recover $x(t)$ from $y(t)$.
Rule: There exists an inverse system $h_i(t)$ such that
$h(t)*h_i(t) = \delta(t)$
## 9. Step Response $s(t)$
### 9.1 Definition
The step response $s(t)$ is the output to a unit step input $u(t)$:
$u(t) \xrightarrow{\mathcal{T}} s(t)$
### 9.2 Relation to $h(t)$
- Step response = integral of impulse response:
  $s(t) = \int_{-\infty}^{t}h(\tau)d\tau$
- Impulse response = derivative of step response:
  $h(t) = \frac{d}{dt}s(t)$
This is useful for measuring $h(t)$ in experiments (step signals are easier to generate than impulses).
## 10. CT Systems Described by Differential Equations
Real-world LTI systems (RLC circuits, mechanical systems) are modeled by:
Linear Constant-Coefficient Differential Equations (LCCDE):
$\sum_{k=0}^N a_k \frac{d^ky}{dt^k} = \sum_{k=0}^M b_k \frac{d^kx}{dt^k}$

### 10.1 Solution of LCCDE
The total solution has two parts:
1. Homogeneous solution $y_h(t)$ (natural response): Determined by the system itself (solves $\sum a_k y^{(k)}=0$)
2. Particular solution $y_p(t)$ (forced response): Determined by the input $x(t)$
$y(t) = y_h(t) + y_p(t)$
### 10.2 Initial Rest Condition
For causal LTI systems:
If $x(t)=0$ for $t\le t_0$, then $y(t)=0$ for $t\le t_0$.

---

## 11. Chapter Summary
1. LTI systems are fully described by their impulse response $h(t)$.
2. Input-output relationship: Convolution integral $y(t)=x(t)*h(t)$.
3. All system properties (causal, stable, etc.) are judged by $h(t)$.
4. Physical LTI systems are modeled by linear constant-coefficient differential equations.
5. Impulse response and step response are related by differentiation/integration.

---
Do you want me to translate the key formulas and definitions into a one-page cheat sheet for exam review?当前文件内容过长，豆包只阅读了前 41%。