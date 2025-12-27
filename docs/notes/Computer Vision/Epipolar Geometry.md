![](Pasted%20image%2020251215204650.png)
![](Pasted%20image%2020251215204659.png)
![](Pasted%20image%2020251215204718.png)
![](Pasted%20image%2020251215204724.png)
![](Pasted%20image%2020251215204740.png)
![](Pasted%20image%2020251215204753.png)

## Three Configuration
- Convergine Cameras: 基线与成像平面不平行, 极点有限, 极点有可能在成像平面上可见也有可能不可见.
  ![](Pasted%20image%2020251215204953.png)
- Parrallel to Image Plane: 成像平面和基线平行, 极点在无穷远处, 极线平行于基线.
  ![](Pasted%20image%2020251215205647.png)
- Motion Perpendicular to the image plane: 基线和成像平面垂直, 成像平面平行, 极点即相机平面主点
  ![](Pasted%20image%2020251215205755.png)

## Essential Matrix & Fundamental Matrix
![](Pasted%20image%2020251215205926.png)
![](Pasted%20image%2020251215210128.png)

>[!warning] 
>即使一组点对满足了对极约束, 并不意味着它们就是相同的 3D 点在各个成像平面的投影.

### Calibrated Case
> Suppose camera intrinsic parameters are known, and the world coordinate system is set to that of the first camera.

The Projection matrices are:

$$
K[I|O] ,K'[R|t]
$$

Therefore, we can multiply the projection matrices and the image points by inverse calibration matrices to get **normalized image coordinates**

$$
x=K^{-1}x_{pixel} \cong [I|O]X 
$$

$$
x'=K'^{-1}x_{pixel}' \cong [R|t]X
$$

![](Pasted%20image%2020251215210812.png)

注意到叉乘可以用矩阵表示:

$$
t_{\times}=\begin{bmatrix}
0 & -t_3 & t_2 \\
t_3 & 0 & -t_1 \\
-t_2 & t_1 & 0
\end{bmatrix}
$$

于是条件可以进行转化:

![](Pasted%20image%2020251215212042.png)

E 矩阵就是 $t_{\times}R$

注意到一个三维向量就代表了三维平面上的一条直线 $ax+by+c=0$, $Ex$ 就是 $x$ 在另一个平面对应的极线, $E^T x'$ 就是 $x'$ 在另一个平面对应的极线.

![](Pasted%20image%2020251215212829.png)

E 的秩为 2, 有 5 个自由度. (3旋转+3平移-scaling ambiguity)

### Uncalibrated Case
If $K,K'$ is unknown, then the constraint is:

$$
x_{pixel}'^TFx_{pixel}=0
$$

where

$$
F=K'^{-T}EK^{-1}
$$

which is called the fundamental matrix.

![](Pasted%20image%2020251215223558.png)

![](Pasted%20image%2020251215223832.png)


### Rank-2 Constraint
利用 Eight-Point Algorithm 求出的 f 只满足了 $||f||=1$, 我们还需要满足 $\text{rank}(f)=2$

低秩近似: SVD 分解, 将最后一个奇异值置为 0.

### Normalized Eight-Point Algorithm
注意到 x, x' 是像素坐标, 乘起来有 $10^6$ 量级, 而八点算法的 $U$ 矩阵中最小的只有 1.

![](Pasted%20image%2020251215224648.png)

因此, 在每张照片中我们都将其中心平移至原点并且 scale 使得均方距离为 2pixels. 假设 normalize 变换矩阵分别为 $T,T'$

则 

$$
\begin{align}
x_{normal} &=Tx_{pixel}\\
x'_{normal} &=T'x'_{pixel}\\
x'^T_{normal}F_{normal}&x_{normal}=0\\
x'_{pixel}T'^TF_{normal}&Tx_{pixel}=0

\end{align}
$$

比较可知

$$
F=T'^TF_{normal}T
$$

### Nonlinear method
![](Pasted%20image%2020251215225223.png)


## Applications of the Essential Matrix

#### Decompose R,t from an Essential Matrix

令 $\hat{t}$ 代表 $t$ 方向上的单位向量, $[\hat{t}]_{\times}$ 作用在一个向量上会使得它 $t$ 方向上的分量置为 0, 正交平面上的向量旋转 $90\degree$, 因此可以写成:

$$
[\hat{t}]_{\times}=SZR_{90\degree}S^T=
\begin{bmatrix}
s_0 & s_1 & \hat{t}
\end{bmatrix}
\begin{bmatrix}
1 &  & \\
& 1 & \\
& & 0
\end{bmatrix}
\begin{bmatrix}
0 & -1 & \\
1& 0 & \\
& & 1
\end{bmatrix}
\begin{bmatrix}
s_0^T \\
s_1^T\\
\hat{t}^T
\end{bmatrix}
$$

其中 $s_0,s_1,\hat{t}$ 为标准正交基.

##### Recall 基变换
假设原基为 $[e_0, e_1, e_2]$ , 新基为 $[f_0,f_1,f_2]$ 

新基在原来的基向量的坐标为 $x_0,x_1,x_2$, 即:

$$
[f_0,f_1,f_2]=[e_0,e_1,e_2][x_0|x_1|x_2]
$$

假设原来某个向量 $a$ 在原来基向量下的坐标是 $\boldsymbol{x}$, 在新基下的坐标是 $\boldsymbol{y}$

$$
\begin{align}
a&=[e_0,e_1,e_2]\boldsymbol{x}\\
 &=[f_0,f_1,f_2][x_0|x_1|x_2]^{-1}\boldsymbol{x}\\
 &=[f_0,f_1,f_2]\boldsymbol{y}
\end{align}
$$

因此 

$$
\boldsymbol{y}=[x_0|x_1|x_2]^{-1}\boldsymbol{x}
$$


由于 $Z$ 就是 $[\hat{t}]_{\times}$ 的奇异值矩阵 (不难注意到该矩阵的两个奇异值都是 1, 可以用叉乘的几何意义想象.)

对 $E$ 进行奇异值分解

$$
E=SZR_{90\degree}S^TR=U\Sigma V^T
$$

由于 $Z=\Sigma$, 比较可知 $S=U, R=UR_{90\degree}^TV^T$ 

由于符号等问题, 我们需要比较一下 4 组解, 看哪一组更合理.

![](Pasted%20image%2020251216135111.png)
![](Pasted%20image%2020251216135130.png)

正负号: 两种情况 (++, --)

$s_0,s_1$ 交换: 两种情况 ($[s_0,s_1],[s_1,s_0]$, 导致旋转 $90, -90$)

一共四组解