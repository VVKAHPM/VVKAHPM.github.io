## 1. Basics
**绕任意轴的旋转公式**:

推导绕单位轴 $\mathbf{k}$ 旋转角度 $\theta$ 的旋转矩阵。

设：
* $\mathbf{k}$ 是单位旋转轴（$|\mathbf{k}|=1$）。
* $\theta$ 是旋转角度。
* $\mathbf{v}$ 是待旋转的向量。
* $\mathbf{v}'$ 是旋转后的向量。

### 1.1 向量分解

将向量 $\mathbf{v}$ 分解为平行于旋转轴 $\mathbf{k}$ 的分量 $\mathbf{v}_{\parallel}$ 和垂直于旋转轴 $\mathbf{k}$ 的分量 $\mathbf{v}_{\perp}$：
$$\mathbf{v} = \mathbf{v}_{\parallel} + \mathbf{v}_{\perp}$$

#### (a) 平行分量 $\mathbf{v}_{\parallel}$

平行分量在旋转过程中保持不变，通过点积求得：
$$\mathbf{v}_{\parallel} = (\mathbf{v} \cdot \mathbf{k}) \mathbf{k}$$
旋转后：$\mathbf{v}'_{\parallel} = \mathbf{v}_{\parallel}$

#### (b) 垂直分量 $\mathbf{v}_{\perp}$

垂直分量表示为：
$$\mathbf{v}_{\perp} = \mathbf{v} - \mathbf{v}_{\parallel} = \mathbf{v} - (\mathbf{v} \cdot \mathbf{k}) \mathbf{k}$$

### 1.2 旋转垂直分量 $\mathbf{v}_{\perp}$

垂直分量 $\mathbf{v}_{\perp}$ 在垂直于 $\mathbf{k}$ 的平面内旋转。我们引入一个辅助向量 $\mathbf{w}$，它垂直于 $\mathbf{v}_{\perp}$ 和 $\mathbf{k}$：
$$\mathbf{w} = \mathbf{k} \times \mathbf{v}$$

旋转后的垂直分量 $\mathbf{v}'_{\perp}$ 可以表示为 $\mathbf{v}_{\perp}$ 和 $\mathbf{w}$ 的线性组合：
$$\mathbf{v}'_{\perp} = (\cos\theta) \mathbf{v}_{\perp} + (\sin\theta) \mathbf{w}$$

### 1.3 合成旋转后的向量 $\mathbf{v}'$

将旋转后的两个分量相加：
$$\mathbf{v}' = \mathbf{v}'_{\parallel} + \mathbf{v}'_{\perp} = \mathbf{v}_{\parallel} + (\cos\theta) \mathbf{v}_{\perp} + (\sin\theta) \mathbf{w}$$

代入 $\mathbf{v}_{\parallel}$、$\mathbf{v}_{\perp}$ 和 $\mathbf{w}$ 的表达式：
$$\mathbf{v}' = (\mathbf{v} \cdot \mathbf{k}) \mathbf{k} + (\cos\theta) \left( \mathbf{v} - (\mathbf{v} \cdot \mathbf{k}) \mathbf{k} \right) + (\sin\theta) (\mathbf{k} \times \mathbf{v})$$ 整理得到罗德里格斯旋转公式的**向量形式**： $$\mathbf{v}' = \mathbf{v} \cos\theta + (\mathbf{k} \times \mathbf{v}) \sin\theta + \mathbf{k} (\mathbf{k} \cdot \mathbf{v}) (1 - \cos\theta)$$ 
为了得到 $\mathbf{v}' = \mathbf{R} \mathbf{v}$，我们需要将点积和叉积转换为矩阵乘法： 
(a) 点积项 $\mathbf{k} (\mathbf{k} \cdot \mathbf{v})$ 使用**外积**（Outer Product）矩阵 $\mathbf{K}_{\text{outer}} = \mathbf{k} \mathbf{k}^T$： $$\mathbf{k} (\mathbf{k} \cdot \mathbf{v}) = (\mathbf{k} \mathbf{k}^T) \mathbf{v}$$ $$\mathbf{K}_{\text{outer}} = \begin{pmatrix} k_x^2 & k_x k_y & k_x k_z \\ k_y k_x & k_y^2 & k_y k_z \\ k_z k_x & k_z k_y & k_z^2 \end{pmatrix}$$
(b) 叉积项 $\mathbf{k} \times \mathbf{v}$ 使用**反对称矩阵** $\mathbf{K}_{\times}$ 来表示叉积： $$\mathbf{k} \times \mathbf{v} = \mathbf{K}_{\times} \mathbf{v}$$ $$\mathbf{K}_{\times} = \begin{pmatrix} 0 & -k_z & k_y \\ k_z & 0 & -k_x \\ -k_y & k_x & 0 \end{pmatrix}$$
(c) 完整的旋转矩阵 $\mathbf{R}$ 将向量形式的公式转换为矩阵乘法，并利用单位矩阵 $\mathbf{I} \mathbf{v} = \mathbf{v}$： $$\mathbf{v}' = (\mathbf{I} \cos\theta) \mathbf{v} + (\mathbf{K}_{\times} \sin\theta) \mathbf{v} + (\mathbf{K}_{\text{outer}} (1 - \cos\theta)) \mathbf{v}$$ 最终的旋转矩阵 $\mathbf{R}$ 为： $$\mathbf{R} = \mathbf{I} \cos\theta + \mathbf{K}_{\times} \sin\theta + \mathbf{K}_{\text{outer}} (1 - \cos\theta)$$ 最终旋转矩阵 设 $c = \cos\theta$ 和 $s = \sin\theta$，将各项矩阵代入，得到 $3 \times 3$ 旋转矩阵 $\mathbf{R}$： $$\mathbf{R} = \begin{pmatrix} c + k_x^2(1-c) & k_x k_y(1-c) - k_z s & k_x k_z(1-c) + k_y s \\ k_y k_x(1-c) + k_z s & c + k_y^2(1-c) & k_y k_z(1-c) - k_x s \\ k_z k_x(1-c) - k_y s & k_z k_y(1-c) + k_x s & c + k_z^2(1-c) \end{pmatrix}$$

## 2. Registration
在几何重建中，我们往往会对同一个场景的不同区域分别采集点云数据，每个局部点云都需要通过点云配准过程融合进全局坐标系中，才能得到完整的最终结果。
这个过程就需要为点云之间找到一个合适的变换, 使得变换后的两组点云实现对齐.
![](Pasted%20image%2020251122212352.png)

**ICP: Iterative Closet Point**

![](Pasted%20image%2020251122212456.png)
![](Pasted%20image%2020251122212519.png)

### 2.1 ICP: PCA Initialize
PCA: 主成分分析, 找到一组数据中变化最大的方向 (可以理解为将数据投影到这个方向上最分散)

考虑一组点中心化后(每个点减去重心坐标)构成的矩阵 $P_{n\times 3}$ ,  
如果我们将数据投影到任意一个单位方向 $u$ 上, 投影后的点集 $\{y_i\}$ 的方差 $Var(y)$ 可计算为:

$$
Var(y)=\frac{1}{m}\sum_{i=1}^{m} y_i^2
$$
投影后的向量 $y=Pu$, 于是
$$
\begin{align}
Var(y)=Var(Pu)&=\frac{1}{m}(Pu)'(Pu)\\
&=u'(\frac{1}{m}P'P)u
\end{align}
$$
这是向量 $u$ 对应的二次型 $u'Cu$ 的值, 而它的最大值正是 $C$ 的最大特征值 (给定 $||u||=1$)

>[!note] u'Cu 最大值的推导
>由于 $C$ 是对称矩阵, 于是总是可以正交对角化, 正交变换改变模长, 故二次型变为 $u'\Sigma u$, 其中$\Sigma$ 是对角矩阵, 这样容易写出二次型的表达式为 $\lambda_1u_1^2+\lambda_2 u_2^2+\cdots+\lambda_n u_n^2$, 在 $||u||=1$ 的条件下显然让特征值最大的对应分量为 $1$ 有最大值.


构建协方差矩阵 $M=P'P$ , 找到 $M$ 特征值最大的方向(注意到 $M$ 是半正定矩阵, 特征值全为正数).

记两组点云为 $S,T$ , 中心分别为 $c_S,c_T$.
得到两组点云的主成分后, 找到一个旋转矩阵$R$使得两组点云的轴分别对齐.
最终:
$$
p_i'=Rp_i+t=c_T+R(p_i-c_S)
$$
可以看出 R 和 t 的最终结果.

PCA initialize 的目的就是先找到一系列合理匹配的点对.

### 2.2 ICP: SVD
![](Pasted%20image%2020251122215953.png)
![](Pasted%20image%2020251122220241.png)
![](Pasted%20image%2020251122220625.png)
我们要最大化 $\text{Trace} (\mathbf{\Sigma} \mathbf{M})$： $$\text{Trace} (\mathbf{\Sigma} \mathbf{M}) = \sum_{i} \sigma_i M_{ii}$$ 由于所有奇异值 $\sigma_i \geq 0$，且 $\mathbf{M}$ 是正交矩阵（$|M_{ii}| \leq 1$），该项在 $\mathbf{M}$ 为单位矩阵 $\mathbf{I}$ 时取得最大值 $\sum_{i} \sigma_i$。 $$\text{Maximum is achieved when } \mathbf{M} = \mathbf{I}$$
## 3. Surface Reconstruction
- Directly: Triangulation (Delaunay Triangulation)
- Indirectly: Implicit Surfaces + Marching Cubes (Poisson Surface Reconstruction)

### 3.1 德劳内三角剖分
三角剖分是指将点云转换为三角形网格的过程, 在给定点集上构造三角形网格没有唯一方法.
![](Pasted%20image%2020251123093350.png)

但是直觉上来说, 我们希望生成出来的三角形尽量"均匀", 而不是随意连接两个点. 德劳内三角剖分通过最大化三角剖分结果中最小的角, 使得三角形网格尽量均匀.

严格意义上来说，德劳内三角剖分并不是一种算法，而是指一类满足了特定条件的三角剖分。这个条件就是：生成结果中，任何三角形的外接圆内，都不包含任何点。可以证明，这样的三角剖分最大化了“所有三角形中最小的内角”
![](Pasted%20image%2020251123093529.png)

![](Pasted%20image%2020251123093745.png)


### 3.2 Poisson Surface Reconstruction

输入一些带法向的点 $\vec{V}$, 定义指示函数 $\chi_M$ , $M$ 内部的点具有 1 的函数值, 外部的点具有 0 的函数值, 那么该指数函数的梯度场 $\nabla \chi_M$ 就只在 $\partial M$ 上有非零梯度. 梯度方向与法向量正好反向.  因此 $\vec{V}$ 就可以看作是指示函数梯度场在边界的采样, 问题就转化为如何从梯度场恢复出原函数, 即从下式中估计 $\chi_M$:
$$
\nabla\chi_M=\vec{V}
$$

### 3.3 Marching Cubes
给定符号距离场函数, 重建顶点网格.
![](Pasted%20image%2020251123095121.png)
