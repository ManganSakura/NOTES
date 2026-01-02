## 1. 核心概念：透过现象看本质

- **线性变换 $T$**：一种从输入空间 $V$ 到输出空间 $W$ 的映射。
    
- **线性条件**：
    
    1. $T(\mathbf{v} + \mathbf{w}) = T(\mathbf{v}) + T(\mathbf{w})$ （加法保持）
        
    2. $T(c\mathbf{v}) = cT(\mathbf{v})$ （数乘保持）
        
    
    > **一句话总结**：原点必须保持不动，直线变换后还是直线（或点），平行的网格变换后依然是平行的网格。
    
- 矩阵即坐标：
    
    一旦我们选定了基 (Basis)，任何线性变换都可以表示为一个矩阵 $A$。
    
    $$ \text{Input } \mathbf{x} \xrightarrow{\text{Transformation } T} \text{Output } T(\mathbf{x})$$
    
    对应于：
    
    $$ \text{Coordinates } \mathbf{c} \xrightarrow{\text{Matrix } A} \text{New Coordinates } A\mathbf{c}$$
    

---

## 2. 几何变换 (Geometric Transformations)

以二维空间 $\mathbb{R}^2$ 为例，假设基为标准基 $\begin{bmatrix} 1 \\ 0 \end{bmatrix}, \begin{bmatrix} 0 \\ 1 \end{bmatrix}$。

### 2.1 旋转 (Rotation)

将向量绕原点逆时针旋转 $\theta$ 角。

- **矩阵 $Q_{\theta}$**：
    
    $$ Q = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$
    
- **性质**：
    
    - **正交矩阵**：$Q^T = Q^{-1}$（旋转回去 = 转置）。
        
    - **保持长度**：旋转不改变向量长度。
        
    - **特征值**：$\lambda = e^{i\theta}, e^{-i\theta}$（通常是复数，除非旋转 $180^\circ$）。
        

### 2.2 投影 (Projection)

将向量“压扁”到一条穿过原点的直线（或子空间）上。

- 矩阵 $P$：
    
    如果投影到 $x$ 轴：
    
    $$ P = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$$
    
- **性质**：
    
    - **幂等性**：$P^2 = P$（投两次和投一次一样）。
        
    - **对称性**：$P^T = P$。
        
    - **特征值**：只有 $1$（在直线上的向量）和 $0$（垂直于直线的向量）。
        

### 2.3 反射 (Reflection)

将向量关于某条直线（镜面）对称过去。

- 矩阵 $H$：
    
    如果关于 $x$ 轴反射（$y$ 变 $-y$）：
    
    $$ H = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$$
    
- **与投影的关系**：
    
    $$ H = 2P - I$$
    
    （反射 = 投影过去两倍距离 - 原位置）
    
- **性质**：
    
    - $H^2 = I$（反射两次回到原点）。
        
    - **特征值**：$1$（在镜面上的向量）和 $-1$（垂直于镜面的向量）。
        

---

## 3. 微积分的矩阵表示 (Calculus via Matrices)

这是 Gilbert Strang 最喜欢的例子。我们将**多项式**看作向量，将**求导**和**积分**看作线性变换。

设定空间：三次多项式空间 $P_3$。

基 (Basis)：$1, x, x^2, x^3$。任何多项式 $a + bx + cx^2 + dx^3$ 对应向量 $\mathbf{v} = [a, b, c, d]^T$。

### 3.1 微分矩阵 (Differentiation Matrix, $D$)

操作：求导 $\frac{d}{dx}$。

- $1 \to 0$
    
- $x \to 1$
    
- $x^2 \to 2x$
    
- $x^3 \to 3x^2$
    

**构造矩阵**（列向量是基变换后的结果）：

$$A_{\text{diff}} = \begin{bmatrix} 0 & 1 & 0 & 0 \\ 0 & 0 & 2 & 0 \\ 0 & 0 & 0 & 3 \\ 0 & 0 & 0 & 0 \end{bmatrix}$$

- **性质**：
    
    - 这是一个**幂零矩阵 (Nilpotent)**。$A^4 = 0$（三次多项式求导4次就没了）。
        
    - 特征值全为 0。
        
    - **奇异矩阵**：不可逆（因为常数的导数是0，信息丢失了，不知道原常数是多少）。
        

### 3.2 积分矩阵 (Integration Matrix, $S$)

操作：从 0 到 $x$ 积分 $\int_0^x$。

- $1 \to x$
    
- $x \to \frac{1}{2}x^2$
    
- $x^2 \to \frac{1}{3}x^3$
    
- $x^3 \to \frac{1}{4}x^4$（需要扩展输出空间到 $P_4$，或者截断）
    

为了能乘起来，我们假设都在 $P_3$ 空间（忽略最高次项溢出），矩阵形式为：

$$A_{\text{int}} = \begin{bmatrix} 0 & 0 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 1/2 & 0 & 0 \\ 0 & 0 & 1/3 & 0 \end{bmatrix}$$

### 3.3 微积分基本定理的矩阵版

- **先积分再求导**：
    
    $$ A_{\text{diff}} \cdot A_{\text{int}} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 0 \end{bmatrix} \approx I$$
    
    （在对应的子空间内是单位矩阵）
    
- **先求导再积分**：
    
    $$ A_{\text{int}} \cdot A_{\text{diff}} \neq I$$
    
    （因为常数项求导丢失了，积不回来了。第一行第一列是 0）。
    

---

## 4. 基变换 (Change of Basis) —— 核心难点

同一个线性变换，如果选了不同的基，矩阵 $A$ 就会改变。

如果我们有一组“好基”（特征向量），矩阵就会变成对角矩阵 $\Lambda$。

关系公式：

$$A = M \Lambda M^{-1}$$

- $M$：基变换矩阵（特征向量矩阵）。
    
- $\Lambda$：新基下的变换矩阵（对角阵）。
    

---

