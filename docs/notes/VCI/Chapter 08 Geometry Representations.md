## 1. How to encode geometry on a computer
显示表达:
- 点云
- Polygon mesh
- Subdivision surface
- ...
隐式表达:
- Level set
- Algebraic surface

## 2. Triangle mesh
- 保存三角形的每个顶点 $P_i=(x,y,z)$
- 用点的索引构成的三元组表示一个三角形 $(i,j,k)$
- 更一般地也可以用 vertex table, edge table, polygon-surface table 构建
  ![](Pasted%20image%2020251122193753.png)
- 还可以用 List of triangles 的方法, 每个三角形保存三个顶点的坐标.
  ![](Pasted%20image%2020251122193834.png)

**Comparison:**
用 List of triangles 表达
- 简单 
- 包含太多冗余信息(重复点)

用 List of vertices + List of indexed triangles
- 空间花费小
- 复杂

如何找相邻三角形?
根据 Triangle Edge list, 假设每个元素 (vi, vj, t) 表示三角形 t 的一条连接 vi, vj 的边. 将边排序后, 如果相邻则会有重复的 vi, vj. 例如有 (vi, vj, t1), (vi, vj, t2), t1 与 t2 就是相邻三角形

## 3. Half-edge data structure
 ![](Pasted%20image%2020251122194335.png)
 利用半边结构, 可以很容易处理一些信息:
 ![](Pasted%20image%2020251122194502.png)
 ![](Pasted%20image%2020251122194507.png)

## 4. Pros and Cons of polygon surface
- Good: Simple, fast in rendering and processing
- Bad: Need much more surfaces to present smooth curved surfaces

## 5. Subdivision surface
Key idea: 利用不断细分网格逼近连续的曲面.
![](Pasted%20image%2020251122194748.png)

### 5.1 Catmull-Clark vertex update rules
1. Add new face point, new edge point
	![](Pasted%20image%2020251122195236.png)
2. Update Vertex point (n: number of neighbor edges)
	![](Pasted%20image%2020251122195307.png)
3. Connect points to form new edges.
4. Store new faces (1 face -> 4 faces)

### 5.2 Loop subdivision
A common subdivision rule for **triangle** meshes.
![](Pasted%20image%2020251122200042.png)

## 6. Mesh parameterization
A function that puts input surface in one-to-one correspondence with a 2D domain.

**弹簧系统:**
将每条边看作一个弹簧, 劲度系数为 $D_{ij}$, 自变量为纹理坐标 $t_i=(u_i,v_i)$, 则弹簧能量
$$
E=\frac{1}{2}\sum_{i=1}^{n}\sum_{j\in neighbor(i)} \frac{1}{2}D_{ij}||t_i-t_j||^2
$$

当 $E$ 达到最小时:
$$
\frac{\partial{E}}{\partial{t_i}}=\sum_{j\in neighbor(i)} D_{ij}(t_i-t_j)=0 \\
$$
$$
t_i=\sum \lambda_{ij}t_j,\lambda_{ij}=\frac{D_{ij}}{\sum_{k\in neighbor(i)} D_{ik}}
$$

根据这个可以列出方程 $At=0$, 在边界处加上一些约束避免出现平凡解. 例如可以制定边界的 parameter 坐标.

一般 $\lambda_{ij}$ 的选取有几种情况:
- average: $\lambda_{ij}=\frac{1}{n_i}, n_i 为 i 邻居数量$
- 调和坐标系数: $\lambda_{ij}=(\cot \gamma_{ij}+\cot \gamma_{ji})/2$
- 均值坐标系数: $\lambda_{ij}=(\tan \frac{\beta_{ji}}{2} + \tan \frac{\alpha_{ij}}{2}) / r_{ij}, r_{ij} 为边的长度$
![](Pasted%20image%2020251122202128.png)

