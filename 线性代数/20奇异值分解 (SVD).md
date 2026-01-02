## 1. 核心公式 (The Decomposition)

任何 $m \times n$ 的矩阵 $A$，都可以分解为三个特殊矩阵的乘积：

$$A = U \Sigma V^T$$

- **$U$ ($m \times m$)**：**左奇异向量矩阵**。它是一个**正交矩阵**。
    
- **$\Sigma$ ($m \times n$)**：**奇异值矩阵**。它是一个**对角矩阵**（只有对角线 $\sigma_i$ 非零，其余全为 0）。
    
- **$V^T$ ($n \times n$)**：**右奇异向量矩阵**的转置。它也是一个**正交矩阵**。
    

### 几何意义

矩阵 $A$ 将向量 $\mathbf{x}$ 从 $\mathbb{R}^n$ 映射到 $\mathbb{R}^m$ 的过程 $A\mathbf{x}$，被 SVD 分解为三个简单的动作：

1. **旋转 ($V^T$)**：在输入空间 $\mathbb{R}^n$ 中旋转坐标轴。
    
2. **拉伸 ($\Sigma$)**：沿着坐标轴方向拉伸或压缩（拉伸量为 $\sigma_i$）。
    
3. **旋转 ($U$)**：在输出空间 $\mathbb{R}^m$ 中再次旋转。
    

> **一句话总结**：SVD 告诉我们，任何线性变换，在选对了一组正交基（$V$）后，本质上都只是**拉伸**。

---

## 2. 怎么求 $U, \Sigma, V$？ (The Calculation)

我们要利用之前学的“半正定矩阵” $A^T A$ 和 $A A^T$。

### 第一步：求 $V$ 和 $\Sigma$

我们要找一组正交向量 $v$，使得 $Av$ 也是正交的。

$$A^T A = (V \Sigma^T U^T) (U \Sigma V^T) = V (\Sigma^T \Sigma) V^T = V \Sigma^2 V^T$$

- 这说明：**$V$ 是 $A^T A$ 的特征向量矩阵**。
    
- **$\Sigma^2$ 是 $A^T A$ 的特征值矩阵**。
    
    - 即奇异值 $\sigma_i = \sqrt{\lambda_i(A^T A)}$。
        
    - 因为 $A^T A$ 半正定，所以 $\lambda_i \ge 0$，开根号没问题。通常按 $\sigma_1 \ge \sigma_2 \ge \dots$ 排序。
        

### 第二步：求 $U$

同理：

$$A A^T = (U \Sigma V^T) (V \Sigma^T U^T) = U (\Sigma \Sigma^T) U^T = U \Sigma^2 U^T$$

- 这说明：**$U$ 是 $A A^T$ 的特征向量矩阵**。
    

### 更简单的求 $U$ 方法

如果 $\sigma_i \neq 0$，我们可以直接利用关系式 $A\mathbf{v}_i = \sigma_i \mathbf{u}_i$：

$$\mathbf{u}_i = \frac{A \mathbf{v}_i}{\sigma_i}$$

这样算出来的 $\mathbf{u}_i$ 自动是单位长度且正交的，而且符号不会搞错。

---

## 3. SVD 与四个基本子空间 (The Big Picture)

这是 SVD 最美妙的地方，它完美展示了四个子空间的正交基。

设 $A$ 的秩为 $r$（即有 $r$ 个非零奇异值）。

### 在 $V$ 中（$\mathbb{R}^n$ 输入空间）

- **前 $r$ 列** $\mathbf{v}_1, \dots, \mathbf{v}_r$：构成了 **行空间 $C(A^T)$** 的标准正交基。
    
- **后 $n-r$ 列** $\mathbf{v}_{r+1}, \dots, \mathbf{v}_n$：构成了 **零空间 $N(A)$** 的标准正交基（对应 $\sigma=0$）。
    

### 在 $U$ 中（$\mathbb{R}^m$ 输出空间）

- **前 $r$ 列** $\mathbf{u}_1, \dots, \mathbf{u}_r$：构成了 **列空间 $C(A)$** 的标准正交基。
    
- **后 $m-r$ 列** $\mathbf{u}_{r+1}, \dots, \mathbf{u}_m$：构成了 **左零空间 $N(A^T)$** 的标准正交基。
    

> **总结**：SVD 为每个子空间都找到了一组完美的正交基。

---

## 4. 应用：图像压缩与低秩近似 (Low Rank Approximation)

如果我们把 $A$ 写成 $r$ 个秩 1 矩阵的和：

$$A = \sigma_1 \mathbf{u}_1 \mathbf{v}_1^T + \sigma_2 \mathbf{u}_2 \mathbf{v}_2^T + \dots + \sigma_r \mathbf{u}_r \mathbf{v}_r^T$$

由于 $\sigma_1 \ge \sigma_2 \ge \dots$，前面的项包含了矩阵的主要信息。

Eckart-Young 定理：

如果我们只想用秩为 $k$ 的矩阵 $A_k$ 来近似 $A$，最好的办法就是截断 SVD，只保留前 $k$ 项：

$$A_k = \sum_{i=1}^k \sigma_i \mathbf{u}_i \mathbf{v}_i^T$$

这是所有秩 $k$ 矩阵中误差（范数）最小的。

应用实例：

一张 $1000 \times 1000$ 的黑白照片就是一个矩阵。

- 如果保留所有信息，需要存 $10^6$ 个像素。
    
- 如果做 SVD，发现前 20 个奇异值很大，后面的几乎为 0。
    
- 我们只存前 20 个 $\sigma, \mathbf{u}, \mathbf{v}$。
    
- 数据量 $\approx 20 \times (1000 + 1000 + 1) \approx 40,000$。
    
- **压缩比 25:1**，而且肉眼几乎看不出区别。
    

---

## 5. 伪逆 (Moore-Penrose Pseudoinverse, $A^+$)

当 $A$ 不可逆（不是方阵，或者有 0 特征值）时，怎么解 $A\mathbf{x}=\mathbf{b}$ 的“最优解”？

我们需要构造一个“伪逆” $A^+$。

利用 SVD，$A = U \Sigma V^T$，那么 $A$ 的逆这就应该是“$V \Sigma^{-1} U^T$”。

但是 $\Sigma$ 里有 0，不能求逆。

办法：只对非零的 $\sigma_i$ 求倒数 $1/\sigma_i$，零还是保留为 0。记为 $\Sigma^+$。

$$A^+ = V \Sigma^+ U^T$$

**最小二乘解**的最简形式就是：$\mathbf{x}^+ = A^+ \mathbf{b}$。

---
