## 1. Discrete differential geometry
Compute approximations of the differential properties of this underlying surface directly from the mesh data.

### 1.1 Local Averaging Region
Key idea: 为了用离散来逼近连续, 我们要定义一个点的邻域, 这样我们可以假定某个微分量在邻域内变化很小, 看作一个定值, 这样将这个邻域的积分值除以面积就可以得到微分量.

常见的几种定义邻域方式:
![](Pasted%20image%2020251122203322.png)

### 1.2 Normal Vectors
![](Pasted%20image%2020251122203606.png)

### 1.3 Gradient
对于单个三角形, 给定三个顶点 $x_i,x_j,x_k$ 上的函数值 $f_i,f_j,f_k$, 三角形上任意一点的函数值 $f(x)$ 可根据 $x$ 的重心坐标插值得到.
$$
f(x)=\alpha f_i+\beta f_j + \gamma f_k \\
$$
$\alpha, \beta, \gamma$ 分别是所对三角形面积与整个三角形面积的比值.
![](Pasted%20image%2020251122204015.png)
那么 $f(x)$ 的梯度为:

$$
\nabla_x=\nabla_x\alpha f_i+\nabla_x\beta f_j+ \nabla_x\gamma f_k
$$
![](Pasted%20image%2020251122204401.png)
![](Pasted%20image%2020251122204417.png)

可见, 梯度在每个三角形内不变.

### 1.4 Laplace-Beltrami Operator
- Constant gradient on facet -> zero Laplace value
- Gradient on the vertex:
![](Pasted%20image%2020251122204929.png)

**motivation:**
$$
f''(x)=\lim_{\Delta x\to0} \frac{f(x+\Delta x)-f(x)+f(x-\Delta x)-f(x)}{2}
$$

**Contangent Formula**
![](Pasted%20image%2020251122205509.png)
![](Pasted%20image%2020251122205525.png)
**Uniform**
$$
\omega_{ij}=1 \ or \ \omega_{ij}=\frac{1}{n_i}
$$

![](Pasted%20image%2020251122205741.png)

## 2. Mesh Smoothing
![](Pasted%20image%2020251122205857.png)
![](Pasted%20image%2020251122210014.png)

## 3. Detail-Preserving Mesh Editing
Key idea: 通过使得新 mesh 的 Laplacian 与原来的 Laplacian 更接近保留细节
![](Pasted%20image%2020251122210610.png)

## 4. Level of Detail
- 对于近的物体使用更多的 Polygons 渲染
- 对于远的物体使用更少的 Polygons 渲染

## 5. Mesh Simplification
- Delete unnecessary vertices, edges and triangles
- 移除顶点
  ![](Pasted%20image%2020251122210817.png)
- 边塌缩
  ![](Pasted%20image%2020251122210830.png)

边塌缩算法是更为简单而常用的, 它通过选择出要被删除的边, 并将边的两个端点合并成一个点放在新的位置上.
为了选择要删除的边, 我们需要有一种误差度量, 最常用的是二次误差度量. 通过最小化新的顶点与之前对应的平面的 L2 距离，我们可以得到删除这条边会引入的二次度量误差.

细节参见 Lab.