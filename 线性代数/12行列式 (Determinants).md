## 1. 灵魂三性质 (The Big Three)

Strang 认为，只要定义了这三条，其他的性质都是注定的。

1. 标准化 (Normalization)：单位矩阵的行列式为 1。
    
    $$\det(I) = 1$$
    
2. 符号交替 (Sign Reversal)：交换矩阵的任意两行，行列式变号。
    
    $$\det(P_{ij}A) = -\det(A)$$
    
3. **线性性 (Linearity)**：**仅针对某一行**（其他行保持不变时），满足加法和数乘。
    
    - 数乘：$\begin{vmatrix} t \cdot a & t \cdot b \\ c & d \end{vmatrix} = t \begin{vmatrix} a & b \\ c & d \end{vmatrix}$
        
    - 加法：$\begin{vmatrix} a+a' & b+b' \\ c & d \end{vmatrix} = \begin{vmatrix} a & b \\ c & d \end{vmatrix} + \begin{vmatrix} a' & b' \\ c & d \end{vmatrix}$
        

---

## 2. 性质推导 (3 推 7)

这就是你提到的“推导链条”。看这三条性质如何产生强大的计算法则。

### 性质 4：如果有两行相等，行列式为 0

- 推导：
    
    假设第 $i$ 行和第 $j$ 行相等。根据性质 2，交换这两行，行列式应该变号（$D = -D$）。
    
    在实数中，唯一满足 $x = -x$ 的数就是 0。
    
    $$\therefore D = 0$$
    

### 性质 5：消元操作不改变行列式值 (最重要！)

即：把第 $i$ 行的 $k$ 倍加到第 $j$ 行上，行列式不变。

- **推导**：利用**性质 3 (线性拆分)** 和 **性质 4**。
    
    $$ \begin{vmatrix} a & b \\ c-ka & d-kb \end{vmatrix} = \underbrace{\begin{vmatrix} a & b \\ c & d \end{vmatrix}}_{\text{原行列式}} - \underbrace{\begin{vmatrix} a & b \\ ka & kb \end{vmatrix}}_{\text{拆分出的部分}}$$
    
    看后半部分：提出 $k$ (性质3) $\to$ $k \begin{vmatrix} a & b \\ a & b \end{vmatrix}$。
    
    因为两行相等，由性质 4 可知其为 0。
    
    结论：消元法 ($A \to U$) 是计算行列式的核心手段！
    

### 性质 6：如果有一行全为 0，行列式为 0

- 推导：
    
    利用性质 3 (数乘)。
    
    $\begin{vmatrix} 0 & 0 \\ c & d \end{vmatrix} = \begin{vmatrix} 0 \times a & 0 \times b \\ c & d \end{vmatrix} = 0 \times \begin{vmatrix} a & b \\ c & d \end{vmatrix} = 0$。
    

### 性质 7：三角矩阵的行列式 = 对角线元素的乘积

这是计算 $n \times n$ 行列式的最终归宿。

- **推导**：
    
    1. 假设 $A$ 是上三角矩阵 $U$。
        
    2. 利用**性质 5 (消元)**，我们可以用对角线元素把上三角部分全部消成 0（只要对角线非零），变成对角矩阵 $D$。
        
    3. 利用**性质 3 (数乘)**，把对角线元素 $d_1, d_2, \dots, d_n$ 一个个提出来。
        
    4. 最后剩下单位矩阵 $I$。
        
    5. 利用性质 1，$\det(I) = 1$。
        
        $$\det(A) = d_1 \times d_2 \times \dots \times d_n \times 1$$
        

### 其他重要性质

- **性质 9**：$\det(AB) = \det(A)\det(B)$ （这是行列式最神奇的地方，体积具有乘法性）。
    
- **性质 10**：$\det(A^T) = \det(A)$ （这意味着所有对行的操作，对列也完全适用）。
    

---

## 3. 国内考试“必杀技” (Cheat Sheet)

Strang 的方法是化为 $U$ (性质 7)，这最通用。但在国内考研或期末考中，有一些特定形状的“套路题”，用以下技巧可以秒杀。

### 技巧 A：行（列）和相等模型

**特征**：每一行的元素加起来都等于同一个常数 $k$。

$$D_n = \begin{vmatrix} a & b & b \\ b & a & b \\ b & b & a \end{vmatrix}$$

**解法**：

1. 把所有列都加到第 1 列上。
    
2. 第 1 列就会变成全是 $k$。
    
3. 提出 $k$，第 1 列全变 1。
    
4. 用第 1 行的 1 把下面的全消成 0。
    

### 技巧 B：爪形/箭形行列式 (Arrowhead Matrix)

**特征**：只有第一行、第一列和主对角线有非零元素，像一个箭头。

$$D_n = \begin{vmatrix} x_1 & a_2 & a_3 & a_4 \\ b_2 & x_2 & 0 & 0 \\ b_3 & 0 & x_3 & 0 \\ b_4 & 0 & 0 & x_4 \end{vmatrix}$$

解法：

逆向消元。利用对角线上的 $x_2, x_3, x_4$ 把第一列的 $b_2, b_3, b_4$ 消成 0。

最后变成上三角，直接乘对角线。

### 技巧 C：三对角行列式 (Tridiagonal) —— 递推法

特征：除了主对角线和它上下两条副对角线，其余全是 0。

解法：

写出递推公式。通常展开第一行后，会发现子行列式还是同一种形状。

$$D_n = a D_{n-1} - b^2 D_{n-2}$$

（这种题目通常考数列通项公式）。

### 技巧 D：范德蒙德行列式 (Vandermonde)

**特征**：

$$\begin{vmatrix} 1 & 1 & 1 \\ x_1 & x_2 & x_3 \\ x_1^2 & x_2^2 & x_3^2 \end{vmatrix}$$

解法：直接背公式！

$$\prod_{1 \le j < i \le n} (x_i - x_j)$$

口诀：所有（后面减前面）的乘积。

### 技巧 E：分块矩阵 (Block Matrices)

**特征**：

$$\begin{vmatrix} A & B \\ 0 & D \end{vmatrix} \quad \text{或} \quad \begin{vmatrix} 0 & A \\ B & C \end{vmatrix}$$

**解法**：

- 主对角分块：$\det = \det(A)\det(D)$。
    
- 副对角分块（左下右上）：$\det = (-1)^{mn} \det(A)\det(B)$。**千万注意符号！**这里的 $m, n$ 是子块的维数。
    

### 技巧 F：加边法 (Adding a border)

特征：所有元素都是 $1$ 或者很接近，或者 $a_i b_j$ 这种秩 1 修正的形式。

解法：

构造一个 $n+1$ 阶行列式，通常加一行一列（比如加个 1 和 0），使其计算简化，结果保持不变。

---

## 4. 行列式的几何意义 (Geometric Meaning)

这是 Strang 极其强调的，也是理解“为什么行列式为 0 就是奇异”的关键。

- **2D**：$\det(\mathbf{u}, \mathbf{v})$ 是这两条向量构成的**平行四边形的面积**。
    
- **3D**：$\det(\mathbf{u}, \mathbf{v}, \mathbf{w})$ 是这三条向量构成的**平行六面体的体积**。
    
- **符号**：正负号代表了“手性”（左手系还是右手系）。如果 $\det = 0$，说明体积被压扁了（降维了），即向量线性相关。
    

---

