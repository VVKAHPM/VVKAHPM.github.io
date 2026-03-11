[]()## 0. From VAE to Flow Model
一个流模型与 VAE 的运转过程十分类似，在 VAE 中，我们首先从一个简单的先验分布 $\mathcal{N}(0,I)$ 中采样隐变量 $z$，之后利用神经网络得到 $p(x|z)=\mathcal{N}(\mu_{\theta}(z),\Sigma_{\theta}(z))$.

然而在训练的过程中，计算 $p_{\theta}(x)$ 需要对所有可能的 $z$ 进行积分，这是 intractable 的。如果我们能够让 $x$ 和 $z$ 之间建立起一个确定性的可逆映射 $x=f_{\theta}(z)$，那我们不是就可以直接利用 $p(z)$ 得到 $p(x)$ 了吗。（回忆一下随机变量的函数，$F(x\leq X)=F(z\leq f^{-1}_\theta (X))$, 两边对 $x$ 求导得 $p(x=X)=p_{z}(f^{-1}_\theta (X))\left|\det \frac{\partial f^{-1}}{\partial x}\right|$）

同时，为了让这个映射连续可微，我们必须保证 $x$ 和 $z$ 的维度是一样的。

## 1. A Flow of Transformations

**Normalizing:** 指的是变量变换时候用到的 Jacobian 行列式

**Flow:** 可逆变换可以组合在一起:

$$
z_m=f_{\theta_m}^{m}\circ\cdots\circ f_{\theta_1}^{1}(z_0) \triangleq f_{\theta}(z_0)
$$

## 2. Learning and Inference
Learning via **maximum likelihood** over dataset $\mathcal{D}$

$$
\max_{\theta} \log p_X(\mathcal{D};\theta)=\sum_{x\in\mathcal{D}}\log p_z(f_{\theta}^{-1}(x))+\log\Bigg|\det\bigg(\frac{\partial f_{\theta}^{-1}(x)}{\partial x}\bigg)\Bigg|
$$

Sampling via forward transformation $z \mapsto x$

$$
z\sim p_Z(z)\quad x = f_{\theta}(z)
$$

## 3. Triangular Jacobian
我们知道，如果 $x$ 的维度是 $n$ ，Jacobian 矩阵的维度将会是 $n\times n$, 计算它的行列式的复杂度将会是 $O(n^3)$ ，非常耗时

如果能让 Jacobian 矩阵是一个三角形，那么计算行列式将会非常简单，只需要将对角线元素乘起来就可以了。

![](Pasted%20image%2020260209194009.png)

### NICE - Additive coupling layers
**N**onlinear **I**ndependent **C**omponents **E**stimations

