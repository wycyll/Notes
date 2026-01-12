# Loss
## Image Classification
*   定义：输入图像（像素矩阵），输出一个预定义集合中的标签（如 "猫"）。
*   核心困难：语义鸿沟 (Semantic Gap)：人类看到的是物体，计算机看到的是 $[0, 255]$ 的三维张量（高度 x 宽度 x 3 通道）。
*   具体挑战：视角变化、光照强度、背景遮挡、形变、类内差异（如各种品种的猫）
## Nearest Neighbor, NN
*   原理：训练阶段记忆所有图片；预测阶段寻找训练集中最像的那张。
*   距离函数 (Distance Metric)：
    *   L1 距离 (曼哈顿距离)：$d_1(I_1, I_2) = \sum_p |I_1^p - I_2^p|$
    *   L2 距离 (欧氏距离)：$d_2(I_1, I_2) = \sqrt{\sum_p (I_1^p - I_2^p)^2}$
*   K-最近邻 (KNN)：不仅看最近的一张，而是看最近的 K 张并投票。K 越大，分类边界越平滑，对噪声抗干扰能力越强。
*   评价：预测极慢 $O(N)$，且像素级别的距离比较无法反映真实的物体语义

## 超参数与数据划分
*   超参数 (Hyperparameters)：如 K 的取值、距离函数的选择。它们不是学习出来的，必须手动预设。
*   数据划分（极其重要）：
    *   训练集 (Train)：用于学习参数。
    *   验证集 (Validation)：用于调节超参数，选出表现最好的那一组。
    *   测试集 (Test)：绝对不能参与调参。仅在最后运行一次，评价真实表现。
*   交叉验证 (Cross-Validation)：将训练集分成多份，轮流做验证。适用于小数据集。

## Linear Classifier
*   数学形式：$f(x, W) = Wx + b$
    *   $x$：输入图像（展平为一维向量，如 3072 维）
    *   $W$：权重矩阵 (Weights)，代表模型的知识
    *   $b$：偏置项 (Bias)，不随输入变化的偏好
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110104819911.png)

*   三个理解视角：
    1.  代数视角：对像素值进行加权求和。
    2.  视觉视角（模板匹配）：$W$ 的每一行相当于该类别的一个模板。预测就是看当前图像跟哪个模板最像。
    3.  几何视角：在超高维空间中，线性分类器是用直线/平面（超平面）将不同类别的点切分开。
*   局限性：无法解决非线性问题（如环形分布、多模态分布、XOR 问题）
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110105949972.png)


## Loss Function
*   多类 SVM 损失 (Hinge Loss)：
    *   公式：$L_i = \sum_{j \neq y_i} \max(0, s_j - s_{y_i} + \text{margin})$
    *   核心：正确类别的分数要比错误类别的高出一个“安全间距”
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110110359544.png)

![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110110342741.png)
*   Softmax 分类器 (多项逻辑回归)：
    *   流程：原始分数 -> 指数化 (exp) -> 归一化 (Sum to 1) -> 得到概率。
    *   损失：$L_i = -\log(P(\text{正确类别}))$
    *   目标：让正确类别的概率无限接近 1
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110110309575.png)
- 对比
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110110434465.png)

# Regularization
核心思想：我们不希望模型在训练集上表现得太完美，因为这会导致过拟合 (Overfitting)——即模型只记住了训练集的噪声，而在新数据上表现很差。
1.  完整损失函数公式：$$L(W) = \underbrace{\frac{1}{N} \sum_{i=1}^N L_i(f(x_i, W), y_i)}_{\text{Data Loss (数据损失)}} + \underbrace{\lambda R(W)}_{\text{Regularization (正则化)}}$$
    *   Data Loss：希望模型预测与标签匹配
    *   Regularization：希望模型尽量简单，防止模型复杂到去拟合噪声
    *   $\lambda$ (超参数)：调节两者之间的平衡
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110112904285.png)
2.  奥卡姆剃刀原理 (Occam's Razor)：在所有能解释数据的假设中，最简单的那个通常是最好的
3.  常见的正则化类型：
    *   L2 正则化 (Weight Decay)：$R(W) = \sum_k \sum_l W_{k,l}^2$。这是最常用的。
        *   直观理解：L2 喜欢“分散”权重。如果输入向量是 $[1,1,1,1]$，它更喜欢权重 $[0.25, 0.25, 0.25, 0.25]$ 而不是 $[1, 0, 0, 0]$。因为它希望利用所有的输入特征，而不是过度依赖某一个，从而提高鲁棒性。
    *   L1 正则化：$R(W) = \sum_k \sum_l |W_{k,l}|$。它喜欢让权重变得稀疏（很多权重变成 0）
    *   Elastic Net：L1 和 L2 的结合。 $R(W) = \sum_k \sum_l \beta W_{k,l}^2 + |W_{k,l}|$ 
    *   更高级的正则化：Dropout（随机失活）、Batch Normalization（批归一化）、Stochastic depth（随机深度）
# Optimization
核心目标：找到一组 $W$，使得全损失 $L$ 达到最小。
1.  可视化理解：想象损失函数是一个崎岖的地形，你站在山上（当前的 $W$），目标是走到地势最低的山谷
2.  策略 #1 ：Random Search完全靠运气乱试
    *   结论：非常糟糕，准确率极低，不可取。
3.  策略 #2 ：利用梯度 (Gradient)：
    *   梯度定义：在每一个维度上计算偏导数组成的向量。它指向函数上升最快的方向
    *   梯度下降 (Gradient Descent)：我们沿着梯度的负方向走，因为那是下降最快的方向
4.  梯度的两种计算方式 (重要比对)：
    *   数值梯度 (Numerical Gradient)：通过公式 $\frac{f(x+h)-f(x)}{h}$ 近似计算
        *   优点：易于编写，不容易出错
        *   缺点：计算极慢（需要对每个参数加 $h$ 算一遍损耗），且结果不精确
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110120612853.png)
	- 解析梯度 (Analytic Gradient)：利用微积分推导导数公式
		- 优点：计算极快，结果精确
		- 缺点：推导容易出错
	- 实践方案：使用解析梯度进行训练，但用数值梯度来验证你的数学公式是否写对（这叫 Gradient Check）
## 各种优化算法 (Optimizers)
### SGD (随机梯度下降)：
每次不使用全量数据（计算量太大），而是随机抽取一小部分图片（Minibatch，如 32/64/128 张）来估算梯度。
	SGD 的问题：
		- 如果在某个维度地势陡峭而在另一个维度平缓，SGD 会疯狂抖动，进展缓慢。
		- 容易陷入局部最小值或鞍点 (Saddle Point)（梯度为 0 的地方）
		- gradients come from minibatches so they can be noisy
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110121014045.png)
### SGD + Momentum (动量法)：
*   引入“速度”的概念。球下山时不只看当前的坡度，还会利用惯性
*   作用：帮助跳出局部最小值，平滑抖动，加速下山
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110121118203.png)
$$ \begin{cases} v_{t+1} = \rho \cdot v_t - \alpha \cdot \nabla f(x_t) \quad \text{（第一步：更新动量）} \\ x_{t+1} = x_t + v_{t+1} \quad \text{（第二步：更新参数）} \end{cases} $$
1. 抑制左右抖动（陡峭方向）
	累积后：
	- 正负抵消，速度被“平均掉”，抖动大幅减小
2. 加速前进（平缓方向）
    - 每一步梯度方向一致,虽然很小，但一直同向
	- 把这些小梯度累积起来，速度越来越快
### Nesterov Momentum：
- 一种改进的动量法，先按照惯性走一段，在“未来位置”计算梯度，再做修正，更不容易过冲
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110122122002.png)$$ \begin{cases} \tilde{x}_t = x_t + \rho \cdot v_t \quad \text{（第一步：预判下一步位置）} \\ v_{t+1} = \rho \cdot v_t - \alpha \cdot \nabla f(\tilde{x}_t) \quad \text{（第二步：用预判位置的梯度更新动量）} \\ x_{t+1} = x_t + v_{t+1} \quad \text{（第三步：更新参数）} \end{cases} $$
实际代码中，为了减少计算量，会把公式变形为“参数先临时更新到预判位置，再调整”，但逻辑和原始公式完全一致： 
$$ \begin{cases} x_{\text{temp}} = x_t + \rho \cdot v_t \quad \text{（临时预判位置，同 $\tilde{x}_t$）} \\ v_{t+1} = \rho \cdot v_t - \alpha \cdot \nabla f(x_{\text{temp}}) \quad \text{（用临时位置算梯度）} \\ x_{t+1} = x_{\text{temp}} - \alpha \cdot \nabla f(x_{\text{temp}}) \quad \text{（简化后的参数更新）} \end{cases} $$
### RMSProp
- 自适应学习率：给每个参数不同的学习率。坡度大的参数步子迈小点（防止抖动），坡度小的迈大点（加速）
```Python
grad_squared = 0
while True:
    dx = compute_gradient (x)
    grad_squared = decay_rate * grad_squared + (1 - decay_rate) * dx * dx  //记录“每个维度，最近一段时间内，梯度大小的平均水平”
    x -= learning_rate * dx / (np.sqrt (grad_squared) + 1e-7) 
    //自动归一化每个维度的步长
```
Progress along steep  directions is damped. Progress along flat directions is accelerated. 
#### AdaGrad
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110130407576.png)
$$\begin{cases} G_t = G_{t-1} + (\nabla f(x_t))^2 \quad \text{（累积梯度的平方和）} \\ x_{t+1} = x_t - \alpha \cdot \frac{\nabla f(x_t)}{\sqrt{G_t} + \epsilon} \quad \text{（参数更新）} \end{cases}$$ 
- 缺陷
	步长会逐渐衰减到0
		因为 $G_t$ 是“梯度平方的累积和”（$G_t = G_{t-1} + (\nabla f(x_t))^2$），训练步数越多，$G_t$ 会越来越大
		分母 $\sqrt{G_t}$ 会随着训练持续增大 → 实际学习率 $\frac{\alpha}{\sqrt{G_t}}$ 会逐渐趋近于0
		最终结果：训练后期参数更新步长几乎为0，模型可能“提前停滞”，无法收敛到最优解
### Adam
*   Momentum + RMSProp 的结合体。它既有动量的惯性，又能自适应调节步长，并加入了偏置修正 (Bias Correction) 解决刚开始训练时的偏差

|名字|记什么|作用|
|---|---|---|
|first_moment (m)|梯度的滑动平均|方向（Momentum）|
|second_moment (v)|梯度平方的滑动平均|尺度（RMSProp）|
```Python
first_moment = 0      # m_t
second_moment = 0     # v_t

for t = 1, 2, 3, ...
    dx = compute_gradient(x)
    ...
    x -= update
```
- Momentum 部分
```Python
first_moment = beta1 * first_moment + (1 - beta1) * dx
```
- RMSProp / AdaGrad 部分
```Python
second_moment = beta2 * second_moment + (1 - beta2) * dx * dx
```
- Bias correction
```Python
first_unbias  = first_moment  / (1 - beta1  t)
second_unbias = second_moment / (1 - beta2  t)
```
早期的 m、v 会被严重“压小”
$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}$, $\quad\hat{v}_t = \frac{v_t}{1 - \beta_2^t}$
- 前几步：强行“放大” m 和 v, 后期：$\beta^t \to 0$，修正项 → 1，自动消失。
- Bias correction 只在训练早期重要
#### AdamW
- 标准 Adam 是在计算梯度（`dx = compute_gradient(x)`）的时候，把 L2 正则化的梯度（即 `λ*x`，λ 是正则化系数）加到原始梯度里
- Adam 的自适应学习率会 “缩放梯度”，导致 L2 正则化的 “惩罚力度” 被干扰
- AdamW 的解决思路是把 “权重衰减” 从 “梯度计算” 中分离，放到 “参数更新” 的最后一步：
- 具体逻辑：先正常计算模型loss的梯度（`dx` 里没有 L2 项），然后计算 Adam 的动量、自适应学习率，最后在更新参数时，额外加上权重衰减项，即 `learning_rate * λ * x`

| 算法             | 更新形式                                       |
| -------------- | ------------------------------------------ |
| SGD            | $x -= \alpha g$                            |
| SGD + Momentum | $x -= \alpha v$                            |
| RMSProp        | $x -= \alpha \frac{g}{\sqrt{E[g^2]}}$      |
| Adam           | $x -= \alpha \frac{\hat m}{\sqrt{\hat v}}$ |
### Learning Rate
1.  学习率 (Hyperparameter #1 )：是模型中最重要的超参数。
    *   太大：损失会爆炸（爆炸式上升）或剧烈震荡。
    *   太小：训练太慢，容易卡在半山腰。
2.  学习率衰减 (LR Decay)：
    *   训练初期步子大，后期步子小。
    *   策略：阶梯衰减 (Step)、余弦衰减 (Cosine)、线性衰减
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110125725104.png)
$\alpha_{0}:$ Initial learning rate   $\alpha_{t}:$ Learning rate at epoch t    $T:$ Total number of epoch
3.  预热 (Linear Warmup)：
    *   训练前几千次迭代，学习率从 0 慢慢升到设定值，防止刚开始梯度过大把模型震碎
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110125852665.png)


### 高阶优化
1.  二阶优化 (Second-Order Optimization)：
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110130724767.png)
    *   利用海森矩阵 (Hessian Matrix) 考虑地形的曲率
    *   牛顿法 (Newton's Method)：虽然理论上能一步到位，但计算 $N \times N$ 矩阵的逆在深度学习中是不可能的（$N$ 是百万级的参数量）
    *   L-BFGS：一种节省内存的二阶近似方法，适用于小规模、确定性数据。
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20260110130818774.png)

2. 实际：
	- Adam (W)：多数场景的默认优选
		- 特点：适用性广，即使使用固定学习率，也能达到不错的训练效果
		- 优势：无需过多调整超参数，训练效率高（适合快速验证模型、小数据集或时间有限的场景）

	- SGD+Momentum：潜力更高但需更多调参
		- 特点：理论上能比 Adam 取得更好的最终模型性能（泛化能力更强）
		- 代价：需要额外调整学习率（LR）和学习率调度策略（如衰减方式），调参成本更高（适合追求最优性能、有充足调参时间的场景）

	- 全批次更新场景：可考虑二阶及更高阶优化
		- 前提：若训练时能负担 “全批次更新”（即每次用整个数据集计算梯度，通常仅适用于小数据集）
		- 选择：可以尝试二阶及更高阶优化方法（这类方法在全批次下计算成本可控，且能利用更精准的损失近似提升效率）

# Deep Learning
