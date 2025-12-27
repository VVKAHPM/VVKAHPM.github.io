![](Pasted%20image%2020251216172601.png)

## SfM vs Calibration & Triangulation
- Calibration
	- Input: point pairs from 3D to 2D
	- Output: camera parameters
- Triangulation
	- Input: camera parameters, point pairs from 2D to 2D
	- Output: 3D points
- Fundamental Matrix
	- Input: 2D point pairs (at least 8)
	- Output: Fundamental Matrix
- SfM
	- Input: point pairs from 2D to 2D
	- Output: 3D points and camera parameters

## Problem formulation
**Input:** $m$ images of $n$ fixed 3D points (ignoring visibility) such that ($m$ 张图像, 每张图像有 $n$ 个点, 是要重建的三维坐标的像素坐标)

$$
x_{ij}\cong P_iX_j \quad (i=1,\dots,m;\ j=1,\dots,n)
$$

**Output:** estimate $m$ projectoin matrices $P_i$ and $n$ 3D points $X_j$ from the $mn$ correspondences $x_{ij}$

![](Pasted%20image%2020251216173712.png)

## The Ambiguity of SfM
![](Pasted%20image%2020251216174729.png)


即使我们对整个场景做了一个变换, 得到的点也不会改变.

![](Pasted%20image%2020251216175633.png)

平行性约束: 要求 $Q$ 不能把平行线变成不平行了
![](Pasted%20image%2020251216175620.png)

![](Pasted%20image%2020251216175637.png)
(可以看到, 图中的平行线仍然保持平行)

正交约束
![](Pasted%20image%2020251216175727.png)


## Affine SfM
![](Pasted%20image%2020251216180513.png)

当相机离物体很远, 或者焦距很大的时候, 近大远小的效果不是很明显, 因此可以近似认为是正交投影
![](Pasted%20image%2020251216180554.png)

![](Pasted%20image%2020251216180601.png)

在 非齐次坐标的情况下:

![](Pasted%20image%2020251216180906.png)

因此, $x_{ij}$ 可以写成 $x_{ij}=A_iX_j+t_i$

![](Pasted%20image%2020251216185407.png)

有 12 个自由度的仿射变换我们是无法确定的, 因此自由度为 8m+3n-12.

通过 centering, 我们可以去掉平移部分:
![](Pasted%20image%2020251216185621.png)
定义世界坐标系的原点为三维坐标点的中心, 我们就可以不用对三维坐标进行平均.

![](Pasted%20image%2020251216185656.png)

$$
\begin{align}
D&=MS \\
rank(D)&\leq rank(M) \leq 3\\
\end{align}
$$

利用 SVD 分解可以进行低秩近似并且做分解, 从而找到一组可行解.

![](Pasted%20image%2020251216190333.png)


利用之前仿射相机的约束, Ai 矩阵的第一行和第二行应该是标准正交的
![](Pasted%20image%2020251216191523.png)

## Dealing with Missing Data
在实际情况中, 很多点可能不会在某张照片出现
![](Pasted%20image%2020251216191820.png)

- Incremental bilinear refinement
![](Pasted%20image%2020251216192140.png)

- 首先找到一个稠密子团, 按照之前方法求出相机参数和三维坐标.
- 接下来我们看右边一列, 因为我们已经知道了相机参数还有对应点的坐标, 所以可以做 triangulation 求出三维坐标
- 接下来再看下面一行, 这些行的每个点我们都有原始的三维坐标和当前图像的像素坐标, 可以做 calibration 求出相机参数.
- 一直这样增量的做下去.

## Projective SfM
一般情况下, 如果对 $Q$ 没有任何约束:

![](Pasted%20image%2020251216192929.png)

![](Pasted%20image%2020251216193526.png)
![](Pasted%20image%2020251216193716.png)

![](Pasted%20image%2020251216193933.png)


## Incremental SfM
就是刚刚讲的思路, 最后做一次 bundle adjustment 优化一下整体结果.