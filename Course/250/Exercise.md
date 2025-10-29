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
    

内球壳总电荷为 $+q$，即  
$$  
Q_a + Q_b = +q.  
$$  
外球壳总电荷为 $-q$，即  
$$  
Q_c + Q_d = -q.  
$$

---

### 空腔内 ($r<a$) 无电荷

导体内部电场必须为零。  
若空腔内没有电荷，则不需要感应电荷来抵消电场，因此  
$$  
Q_a = 0.  
$$

于是由内壳总电荷为 $+q$：  
$$  
Q_b = +q.  
$$

---

### 内外壳之间 ($b<r<c$)

在该区域内，包围的总电荷为 $+q$，因此外壳的内表面会被感应出 $-q$：  
$$  
Q_c = -q.  
$$  
为保持外壳总电荷为 $-q$：  
$$  
Q_d = 0.  
$$

---

因此四个表面的电荷分布为：  
$$  
Q_a = 0,\qquad Q_b = +q,\qquad Q_c = -q,\qquad Q_d = 0.  
$$

---

## (b) 各区域的电场

利用球对称性（或高斯定律结果）：  
$$  
E(r) = \frac{1}{4\pi\varepsilon_0}\frac{Q_{\text{enc}}}{r^2},  
$$  
其中 $Q_{\text{enc}}$ 为半径 $r$ 内所包围的净电荷。

---

### 区域划分

1. **区域 I：$r < a$**  
    $$Q_{\text{enc}}=0 \Rightarrow E=0.$$
    
2. **区域 II：$a < r < b$**  
    导体内部电场为零：  
    $$E=0.$$
    
3. **区域 III：$b < r < c$**  
    包含内壳电荷 $+q$：  
    $$  
    E(r)=\frac{1}{4\pi\varepsilon_0}\frac{q}{r^2},\quad \text{方向向外。}  
    $$
    
4. **区域 IV：$c < r < d$**  
    导体内部电场为零：  
    $$E=0.$$
    
5. **区域 V：$r > d$**  
    所包围的净电荷为 $+q - q = 0$：  
    $$E=0.$$
    

---

因此：  
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251014111353797.png)

---

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
