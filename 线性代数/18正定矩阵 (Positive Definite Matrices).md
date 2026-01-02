## 1. 核心概念：能量与极值

为什么我们关心正定矩阵？因为它们与**“最小值”**直接相关。

- 二次型 (Quadratic Form)：
    
    对于一个对称矩阵 $A$，函数 $f(\mathbf{x}) = \mathbf{x}^T A \mathbf{x}$ 称为二次型。
    
    - 在 $\mathbb{R}^2$ 中：$f(x, y) = ax^2 + 2bxy + cy^2$。
        
- 正定性 (Positive Definiteness)：
    
    如果对于任意非零向量 $\mathbf{x} \neq \mathbf{0}$，都有：
    
    $$ \mathbf{x}^T A \mathbf{x} > 0$$
    
    则称 $A$ 是**正定矩阵**。
    
- 几何意义：
    
    这意味着函数图像是一个碗形 (Bowl-shaped)，就像抛物面 $z = x^2 + y^2$。它有且仅有一个全局最低点（原点）。
    

---

## 2. 判别正定性的 5 个测试 (The 5 Tests)

Gilbert Strang 总结了判断一个 $n \times n$ **对称**矩阵是否正定的 5 种等价方法。这在考试和工程中极其实用。

### 测试 1：特征值测试 (Eigenvalue Test)

- **条件**：所有的特征值 $\lambda_i > 0$。
    
- **直观**：在主轴方向上，曲面都是向上弯曲的。
    
- **应用**：理论分析最常用。
    

### 测试 2：能量测试 (Energy Test / Definition)

- **条件**：对于任意 $\mathbf{x} \neq \mathbf{0}$，$\mathbf{x}^T A \mathbf{x} > 0$。
    
- **直观**：系统的总能量永远为正，不会坍塌。
    

### 测试 3：主元测试 (Pivot Test)

- **条件**：高斯消元法（不换行）得到的所有主元 (Pivots) $d_i > 0$。
    
- **应用**：计算机数值计算最快的方法（不需要算特征值）。
    

### 测试 4：行列式测试 (Determinant Test)

- **条件**：所有**顺序主子式 (Leading Principal Minors)** 的行列式都大于 0。
    
    - $D_1 = a_{11} > 0$
        
    - $D_2 = \det \begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix} > 0$
        
    - ...
        
    - $D_n = \det(A) > 0$
        
- **注意**：光看 $\det(A)>0$ 是不够的！（比如 $\begin{bmatrix} -1 & 0 \\ 0 & -1 \end{bmatrix}$ 行列式为 1，但它是负定的）。
    

### 测试 5：Cholesky 分解 (Cholesky Decomposition)

- **条件**：存在一个上三角矩阵 $R$（对角线元素非零），使得 $A = R^T R$。
    
- **意义**：这相当于给矩阵“开平方”。$A = LDL^T$ 的变体。
    

---

## 3. 几何直观：切片分析

让我们通过二元函数 $f(x, y) = ax^2 + 2bxy + cy^2$ 来看看矩阵 $A = \begin{bmatrix} a & b \\ b & c \end{bmatrix}$ 的正定性。

1. **正定 (Positive Definite)**：
    
    - **形状**：碗形 (Bowl)。
        
    - **特征**：$\lambda_1 > 0, \lambda_2 > 0$。
        
    - **水平集**：椭圆 (Ellipses)。
        
2. **不定 (Indefinite)**：
    
    - **形状**：马鞍面 (Saddle Point)。在一个方向上升，另一个方向下降。
        
    - **特征**：$\lambda_1 > 0, \lambda_2 < 0$。
        
    - **判定**：$\det(A) < 0$。
        
3. **负定 (Negative Definite)**：
    
    - **形状**：山峰 (Peak)。
        
    - **特征**：所有 $\lambda_i < 0$。
        
    - **判定**：$-A$ 是正定的。
        

---

## 4. 极小值问题 (Minimizing a Function)

这是正定矩阵在微积分中的最大应用。

考虑二次函数（包含一次项）：

$$P(\mathbf{x}) = \frac{1}{2} \mathbf{x}^T A \mathbf{x} - \mathbf{b}^T \mathbf{x}$$

- **导数**：$\nabla P = A\mathbf{x} - \mathbf{b}$。
    
- **驻点**：当 $A\mathbf{x} = \mathbf{b}$ 时，导数为 0。
    
- 二阶充分条件：
    
    这个驻点是极小值点的充要条件是：二阶导数矩阵（海森矩阵 Hessian Matrix） $A$ 是正定矩阵。
    

> **结论**：解线性方程组 $A\mathbf{x}=\mathbf{b}$，本质上就是在寻找能量函数 $P(\mathbf{x})$ 的最低点（如果 $A$ 正定）。

---

## 5. 半正定矩阵 (Positive Semidefinite, PSD)

这是边界情况，比如平底的碗。

- **定义**：$\mathbf{x}^T A \mathbf{x} \ge 0$。
    
- **判定**：
    
    - 所有特征值 $\lambda_i \ge 0$（允许有 0）。
        
    - 所有主子式 $\ge 0$（不仅是左上角的顺序主子式，是所有的）。
        
    - $A$ 是奇异的（Singular）。
        

### 最重要的 PSD 例子：$A^T A$

对于**任意**实矩阵 $A$（哪怕是长方形的 $m \times n$）：

1. $S = A^T A$ 总是**对称的**。
    
2. $S = A^T A$ 总是**半正定的**。
    
    - 证明：$\mathbf{x}^T (A^T A) \mathbf{x} = (A\mathbf{x})^T (A\mathbf{x}) = ||A\mathbf{x}||^2 \ge 0$。
        
3. **何时正定？**
    
    - 当 $A$ 的**列线性无关**（Rank = $n$）时，$A^T A$ 是**正定**的。
        
    - 这是最小二乘法 $(A^T A)\hat{x} = A^T b$ 能够有唯一解的数学保证。
        

---

**小结**：

- **正定** = 正能量 = 只有正特征值 = 碗形 = 极小值。
    
- **$A^T A$** 是连接任意矩阵和正定矩阵的桥梁。
    
