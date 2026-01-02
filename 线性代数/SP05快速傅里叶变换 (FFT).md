## 1. 复数单位根 (Complex Roots of Unity)

FFT 的基石不是实数，而是位于复平面单位圆上的点。

- **定义**：满足方程 $z^n = 1$ 的 $n$ 个复数解。
    
- **通解**：它们均匀分布在单位圆上，将圆 $n$ 等分。
    
    $$ w_k = e^{i \frac{2\pi k}{n}} = \cos\left(\frac{2\pi k}{n}\right) + i \sin\left(\frac{2\pi k}{n}\right), \quad k=0, \dots, n-1$$
    
- 主单位根 (Primitive Root) $w_n$：
    
    通常记 $w_n = e^{-i \frac{2\pi}{n}}$（注意：在定义 DFT 矩阵时，指数通常取负号；在物理学中常取正号，此处遵循 Strang/MATLAB 的惯例）。
    
    - **关键性质**：
        
        1. $w_n^n = 1$ (绕一圈回到起点)。
            
        2. $w_n^{n/2} = -1$ (转半圈)。
            
        3. **正交性**：$1 + w + w^2 + \dots + w^{n-1} = 0$。
            

---

## 2. 傅里叶矩阵 (The Fourier Matrix, $F_n$)

离散傅里叶变换 (DFT) 可以写成矩阵向量乘法：$\mathbf{y} = F_n \mathbf{x}$。

- 矩阵定义：
    
    $F_n$ 是一个 $n \times n$ 的矩阵，其中的元素为 $(F_n)_{jk} = w_n^{jk}$（$j, k$ 从 0 到 $n-1$）。
    
- 以 $n=4$ 为例：
    
    令 $w = i$ (或者 $-i$)。
    
    $$ F_4 = \begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & w & w^2 & w^3 \\ 1 & w^2 & w^4 & w^6 \\ 1 & w^3 & w^6 & w^9 \end{bmatrix}$$
    
    注意 $w^4=1$，所以指数可以对 4 取模，矩阵变得非常有规律。
    
- 正交性 (Orthogonality)：
    
    $F_n$ 的列是相互正交的（在复数意义下，$\bar{\mathbf{c}}_i^T \mathbf{c}_j = 0$）。
    
    但是列的长度是 $\sqrt{n}$，不是 1。
    
- 逆矩阵 (Inverse Matrix)：
    
    求逆极其简单，不需要高斯消元！
    
    $$ F_n^{-1} = \frac{1}{n} \bar{F}_n$$
    
    （即：把 $w$ 换成 $w^{-1}$ 或 $\bar{w}$，然后除以 $n$）。
    

---

## 3. FFT 的核心思想：分治法 (Divide and Conquer)

如果直接计算 $F_n \mathbf{x}$，我们需要做 $n^2$ 次乘法。当 $n=1024$ 时，大约是一百万次。

FFT 的发现是：$F_n$ 可以由两个更小的 $F_{n/2}$ 组成。

### 3.1 步骤

1. 将输入向量 $\mathbf{x}$ 分为**偶数项** ($x_0, x_2, \dots$) 和 **奇数项** ($x_1, x_3, \dots$)。
    
2. 这就是重排矩阵（置换矩阵 $P$）的作用。
    

### 3.2 矩阵分解公式 (极重要)

$$F_n = \begin{bmatrix} I_{n/2} & D_{n/2} \\ I_{n/2} & -D_{n/2} \end{bmatrix} \begin{bmatrix} F_{n/2} & 0 \\ 0 & F_{n/2} \end{bmatrix} P$$

- **$P$**：奇偶置换矩阵。
    
- **$F_{n/2}$**：规模减半的傅里叶矩阵。
    
- **$D_{n/2}$**：对角修正矩阵，元素为 $[1, w, w^2, \dots, w^{n/2-1}]$。
    
    - 这里利用了性质 $w_{n/2} = w_n^2$。
        

### 3.3 复杂度分析

- $T(n) = 2T(n/2) + O(n)$
    
    - $2T(n/2)$：两个小规模的 FFT。
        
    - $O(n)$：最后乘以对角阵 $D$ 并进行加减法的开销。
        
- 解这个递归方程得到：**$T(n) \approx \frac{1}{2} n \log_2 n$**。
    
- **对比**：对于 $n=1024$，
    
    - $n^2 \approx 1,000,000$。
        
    - $n \log n \approx 10,000$。
        
    - **快了 100 倍！**
        

---

## 4. 蝶形运算 (Butterfly Operations)

将上述矩阵分解展开成代数公式，就得到了著名的“蝶形”流图。

对于第 $k$ 个输出（前一半）和第 $k + n/2$ 个输出（后一半）：

$$\begin{cases} y_k &= E_k + w^k O_k \\ y_{k + n/2} &= E_k - w^k O_k \end{cases}$$

- $E_k$：偶数部分的 DFT 结果。
    
- $O_k$：奇数部分的 DFT 结果。
    
- $w^k$：**旋转因子 (Twiddle Factor)**。
    

图解意义：

之所以叫“蝶形”，是因为在信号流图中，两个输入 ($E_k, O_k$) 交叉产生两个输出，形状像一只蝴蝶。

---

## 5. 循环卷积与对角化 (Circular Convolution)

FFT 不仅仅是为了算频谱，它还是解卷积的利器。

- 卷积定理：
    
    时域中的循环卷积 $\mathbf{c} = \mathbf{a} * \mathbf{b}$ $\iff$ 频域中的点乘 $\hat{\mathbf{c}} = \hat{\mathbf{a}} \cdot \hat{\mathbf{b}}$。
    
    $$ F(\mathbf{a} * \mathbf{b}) = (F\mathbf{a}) \cdot (F\mathbf{b})$$
    
- 矩阵视角：
    
    循环卷积对应循环矩阵 (Circulant Matrix, $C$)。
    
    $$ C = F^{-1} \Lambda F$$
    
    - 这意味着：**傅里叶矩阵 $F$ 是所有循环矩阵的特征向量矩阵！**
        
    - 任何循环矩阵都能被 FFT 对角化。
        

---

小结：

FFT 将矩阵乘法分解为稀疏矩阵的乘积序列。

1. **$w_n$** 是正交的灵魂。
    
2. **$F_n$** 是变换的载体。
    
3. **蝶形运算** 是递归的实现。
    
