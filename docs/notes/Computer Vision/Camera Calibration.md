## Problem
- Given $n$ points with known 3D coordinates $X_i$ and known image projections $x_i$, estimate the camera parameters.
![](Pasted%20image%2020251215201902.png)

![](Pasted%20image%2020251215202008.png)


## How to get parameters
通过最小化 $||Ap||$ 我们得到了一个近似 $K[R|t]$ 的矩阵. 如何从中得到 $K,R,t$ ?

Key observation: $K$ is a upper triangular matrix, $R$ is a orthonormal matrix

We can perform $\text{RQ}$ decomposition to get $K,R,t$

![](Pasted%20image%2020251215202416.png)


## Triangulation
得到了两张图的对应点后, 如何重建三维坐标:

直接通过连线相交的方式, 由于噪音的存在, 不一定可以相交于同一点.

![](Pasted%20image%2020251215202606.png)

![](Pasted%20image%2020251215202612.png)

![](Pasted%20image%2020251215202634.png)
![](Pasted%20image%2020251215202753.png)

