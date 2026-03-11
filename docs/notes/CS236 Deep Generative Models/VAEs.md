
## 0. Background
在现实生活中，我们不仅想让机器“识别”数据（判别模型, discriminative models），更想让机器“创造”数据（生成模型, generative models）。当一个模型学会生成**有意义**的数据的时候，我们倾向于认为它学习到了数据的**内在逻辑**。

- **目标**：学习真实世界数据 $x$ 的概率分布 $p_{X}(x)$。对于离散数据，它是概率；对于连续数据，它是概率密度。
- **功能**：我们希望一个生成模型可以做到这样几件事:
	- 给定输入 $x$，输出概率 $p_{X}(x)$. 这可以帮助我们进行 outliers 的分析，例如应用在对抗样本的分析。
	- 根据分布 $p_{X}$ 随机进行采样，生成数据 $x$。例如现在非常流行的文生图模型。


## 1. Latent Variables
真实数据 $x$（如像素）是极度高维且冗余的。我们假设数据是由某些低维、不可见的**隐变量 (Latent Variables) $z$** 驱动生成的。

> [!abstract] 隐变量的物理直觉
> - **数据 $x$**：一张微笑的脸。
> - **隐变量 $z$**：[嘴部弧度, 眼睛弯曲度, 性别...]。
> - **生成逻辑**：$z \xrightarrow{p(x|z)} x$。
> - **意义**：如果我们能掌握 $z$ 的分布，就能通过控制 $z$ 来有规律地生成 $x$。

## 2. Mixture of Gaussians
**motivation:** Combine simple models into a more complex and expressive one.

我们选择 $\mathcal{N}(0,I)$ 作为 $z$ 的先验, 并且假设 $p(x|z;\theta)\sim \mathcal{N}(\mu_{\theta}(z), \Sigma_{\theta}(z))$, 这里的 $\mu_{\theta}(z), \sigma_{\theta}(z)$ 均为神经网络的计算结果。这样 $p(x)$ 就可以写为:

$$
p(x) = \int p(x|z;\theta)p(z)dz
$$

可以证明这个模型可以逼近几乎所有分布。

## 3. Maximum Likelihood Estimation
在生成模型的训练中，我们不可能知道真实的 $p(x)$ 分布如何，我们只能通过采样得到的数据集来学习。

根据最大似然估计的思想，假设我们有数据集 $\mathcal{D}=\{x^{(1)},\cdots,x^{(M)}\}$, 最大似然估计学习的目标就是最大化概率:

$$
\prod_{x\in\mathcal{D}}p(x;\theta)=\prod_{x\in\mathcal{D}}\int p(x|z;\theta)p(z)dz
$$

实际情况中我们更常用对数似然估计，因为这样数值稳定性较高，也即最大化

$$
\sum_{x\in\mathcal{D}}\log p(x;\theta)
$$

为了优化，我们必须计算出 $p(x;\theta)$，这需要计算积分 $\int p(x|z;\theta)p(z)dz$

一个比较 naive 的想法是直接利用 Monte Carlo 的思想通过采样几个 $z$ 来逼近积分，但是在高维度的数据下这样的估计方差是很大的，因为大部分的 $z$ 对应的 $p(x|z)$ 是很小的。为了方便和后面的方法比较，我们可以先来量化一下这个方差：

我们的目标是估计 $I=\mathbb{E}_{z \sim p(z)} \left[ f(z)\right]=\int f(z)p(z)dz$

(在 VAE 中，$f(z)=p(x|z;\theta)$，根据神经网络的输出计算得到) 

假设我们从 $p(z)$ 中随机采样 $N$ 个点 $z_1,\cdots,z_N$ , 估计为 

$$
\hat{I}_{NMC}=\frac{1}{N}\sum_{i=1}^Nf(z_i)
$$

首先这个估计是无偏的:

$$
\mathbb{E}[\hat{I}_{NMC}]=\frac{1}{N}\sum_{i=1}^N \mathbb{E}_{z_i\sim p(z)}[f(z_i)]=\mathbb{E}_{z\sim p(z)}[f(z)]
$$

方差为:

$$
\begin{align}
Var(\hat{I}_{NMC})&=\frac{1}{N}Var_{z\sim p(z)}(f(z))\\
				&=\frac{1}{N}(\mathbb{E}[f^2(z)]-(\mathbb{E}[f(z)])^2) \\
				&=\frac{1}{N}(\int f^2(z)p(z)dz-(\int f(z)p(z)dz)^2)
\end{align}
$$


为了解决 Naive Monte Carlo 的方差问题，我们引入了 **Importance Sampling**。我认为这是理解 VAE 的一个核心点。

**核心思想**：既然只有少部分 $z$ 能生成有意义的 $x$，那我们就引入一个提议分布 $q(z)$，让它专门在“重要”的区域采样。
$$p(x) = \int q(z) \frac{p(x, z)}{q(z)} dz = \mathbb{E}_{z \sim q(z)} \left[ \frac{p(x, z)}{q(z)} \right]$$

我们按照 $q(z)$ 同样取样 $N$ 个点 $z_1,\cdots,z_N$，估计为：

$$
\hat{I}_{IS}=\frac{1}{N}\sum_{i=1}^N \frac{f(z_i)p(z_i)}{q(z_i)}
$$

可以看到 naive Monte Carlo 就是 $q(z)=p(z)$ 的特例

不难证明这个估计也是无偏的，其方差为：

$$
\begin{align}
Var(\hat{I}_{NMC})&=\frac{1}{N}Var_{z\sim q(z)}(\frac{f(z)p(z)}{q(z)})\\
				&=\frac{1}{N}(\int \frac{f^2(z)p^2(z)}{q^2(z)}q(z)dz-(\int \frac{f(z)p(z)}{q(z)}q(z)dz)^2)\\
				&=\frac{1}{N}(\int\frac{f^2(z)p^2(z)}{q(z)}dz-(\int f(z)p(z)dz)^2)
\end{align}
$$

这里的 $(\int f(z)p(z)dz)^2$ 与前面的是相同的，我们只需要关注减号前面那一项。

可以证明，在约束 $\int q(z)dz=1$ 的条件下，当 $q(z)=p(z|x)$ 时，$\int \frac{f^2(z)p^2(z)}{q(z)}$ 有最小值，此时 $Var(\hat{I}_{NMC})=0$. 

有了这个 insight, 我们就可以理解 VAE 的 encoder 和 decoder 分别在做什么了，encoder 在学习 $q(z)=p(z|x)$ 将 $x$ 编码为隐变量 ，decoder 学习 $p(x|z)$ 用以生成数据。 

## 4. Evidence Lower BOund (ELBO)

前面提到，在训练的时候我们一般采用 Maximum log-likelihood. 但是在引入 propose distribution $q$ 之后，直接取 log 会导致一个问题：

$$
\begin{align}
\log p_{\theta}(x)&=\log\left(\sum_{z\in\mathcal{Z}}p_{\theta}(x,z)\right)\\
&=\log\left(\sum_{z\in\mathcal{Z}}\frac{q(z)}{q(z)}p_{\theta}(x,z)\right)\\
&=\log\left(\mathbb{E}_{z\sim q(z)}\left[\frac{p_{\theta}(x,z)}{q(z)}\right]\right)
\end{align}
$$

但是在采样的时候我们实际上是先把概率对数化再相加以保证数值稳定，所以我们估计的期望是：

$$
\mathbb{E}_{z\sim q(z)}\left[\log\left(\frac{p_{\theta}(x,z)}{q(z)}\right)\right]
$$

由于 $\log$ 是凹函数，根据 Jensen Inequality 有:

$$
\log\left(\mathbb{E}_{z\sim q(z)}\left[\frac{p_{\theta}(x,z)}{q(z)}\right]\right)\geq\mathbb{E}_{z\sim q(z)}\left[\log\left(\frac{p_{\theta}(x,z)}{q(z)}\right)\right]
$$

因此我们实际上估计的是对数似然的一个下界，称作 **E**vidence **L**ower **BO**und (**ELBO**) （可以证明，当 $q(z)=p(z|x)$ 时，等号成立。），虽然我们估计的只是一个下界，但是只要让下界足够大我们的对数似然也应该足够大。

让我们重新整理一下这个不等式：

$$
\begin{aligned}
\log p_{\theta}(\mathbf{x}) & \geq \sum_{\mathbf{z}} q(\mathbf{z}) \log \left(\frac{p_\theta(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})}\right) \\
& = \underbrace{\sum_{\mathbf{z}} q(\mathbf{z}) \log p_\theta(\mathbf{x}, \mathbf{z})}_{\text {Loglikelihood as if fully observed }} - \underbrace{\sum_{\mathbf{z}} q(\mathbf{z}) \log q(\mathbf{z})}_{\text {Entropy } H(q) \text { of } q} \\
& = \sum_{\mathbf{z}} q(\mathbf{z}) \log p_\theta(\mathbf{x}, \mathbf{z})+H(q)
\end{aligned}
$$

事实上，不等式左右两边之差正是 $q(z)$ 与 $p(z|x)$ 的 $KL$ 散度：

$$
\begin{align}
D_{KL}\left(q(z)||p(z|x)\right)&=\sum_{z}q(z)\log\left(\frac{q(z)}{p(z|x)}\right)\\
&=\sum_zq(z)\log q(z)-\sum_z q(z)\log p(z|x)\\
&=-H(q)-\sum_zq(z)\log  p_{\theta}(x,z)+\sum_zq(z) \log p(x;\theta)\\
&=\log p_{\theta}(x)-\sum_z q(z)\log p_{\theta}(x,z)-H(q)
\end{align}
$$

这也解释了为什么 $q(z)=p(z|x)$ 的时候，等号成立。

## 5. Variational Inference
事实上，我们不可能知道 $p(z|x)$，因此需要通过神经网络来学习 $q_{\phi}(z|x)$ 近似它。

讲到这里，我们可以来看看 VAE 中的 **variational（变分）** 指的究竟是什么。

变分与泛函有着紧密的联系，泛函指的是**函数的函数**，不同于一般的函数将数作为输入，泛函将函数作为输入，得到一个数。变分可以简单理解为**泛函对函数的微分**。变分法是数学分析中的一个分支，它通过研究函数与泛函的微小变分，来求解泛函的极大值与极小值。因此 VAE 中我们其实在训练一个 encoder 逼近 $p_{\theta}(z|x)$， 变分指的是从分布族中找一个最优的分布 $q_{\phi}(z)$, 使得它尽量靠近 $p_{\theta}(z|x)$。(这里 $\phi$ 指的是变分参数)

我们怎么衡量靠近呢？当然是使用 $KL$ 散度。因此我们的目标就是使得 $D_{KL}(q_{\phi}(z|x),p_{\theta}(z|x))$ 最小化。观察我们上面的 ELBO, 你会发现：

$$
D_{KL}(q_{\phi}(z|x),p_{\theta}(z|x))=\log p_{\theta}(x)-\underbrace{\mathcal{L}(x;\theta,\phi)}_{\text {ELBO}}
$$
在后面 decoder 的 $\theta$ 固定的情况下，最大化 ELBO 其实就是最小化 KL 散度。

| **特性**   | **普通神经网络 (如 MLP, CNN)**       | **变分自编码器 (VAE)**                         |
| -------- | ----------------------------- | ---------------------------------------- |
| **目标**   | 寻找一个**最优的点**（Weights/Biases）。 | 寻找一个**最优的分布**（Mean/Variance）。            |
| **本质**   | **函数拟合**：学习 $y = f(x)$ 的映射关系。 | **密度估计**：学习数据的底层概率生成机制。                  |
| **中间变量** | 隐藏层神经元输出的是**具体的数值**。          | 隐藏层输出的是**分布的参数**（均值 $\mu$ 和方差 $\sigma$）。 |

总结来说，从数学的角度看，VAE 在做两件事：

- encoder 阶段：固定 $\theta$, 优化 $\phi$
	- $\log p(x;\theta)$: 常数
	- 最大化 ELBO
	- 最小化 KL
- decoder 阶段：固定 $\phi$, 优化 $\theta$
	- 提高 $\log p(x;\theta)$ （无法直接得到，积分维度灾难，还是通过提高 ELBO 下界）
	- Maximum Log-Likelihood Estimation

## 6. Learning

根据上面的推导，VAE 的目标就是最大化 ELBO 

$$
\mathcal{L}(x;\theta,\phi)=\mathbb{E}_{z\sim q(z;\phi)}[\log p(x,z;\theta)-\log q(z;\phi)]
$$

对于参数 $\theta$, 只要可以通过 $q(z;\phi)$ 采样，我们就可以用 Monte Carlo 的思想来计算。

对于参数 $\theta$ 的梯度是好计算的：

$$
\nabla_{\theta}\mathbb{E}_{z\sim q(z;\phi)}[\log p(x,z;\theta)-\log q(z;\phi)]=\mathbb{E}_{z\sim q(z;\phi)}[\nabla_{\theta}\log p(x,z;\theta)]
$$

但是对于参数 $\phi$, 因为我们求期望时候的概率分布就与 $\phi$ 有关，因此它的梯度不像 $\theta$ 那么简单可以计算。

接下来介绍一个简单但不易泛化的方法，对于一些简单的分布 $q$ 如高斯分布这么做是有效的。

### Reparameterization

What we want to take gradient with respect to $\phi$ :
$$
\mathbb{E}_{q(z;\phi)}[r(z)]=\int q(z;\phi)r(z)dz
$$

Suppose $q(z;\phi)=\mathcal{N}(\mu,\sigma ^2I)$ is Gaussian with parameters $\phi=(\mu,\sigma)$, These are equivalent ways of sampling:

- Sample $z\sim q(z;\phi)$
- Sample $\epsilon\sim\mathcal{N}(0,I), z=\mu+\sigma\epsilon=g(\epsilon;\phi)$.

$$
\begin{align}
\mathbb{E}_{q(z;\phi)}[r(z)]&=\int q(z;\phi)r(z)dz\\
&=\int\mathcal{N}(\epsilon)r(\mu+\sigma\epsilon)d\epsilon\\
&=\mathbb{E}_{\epsilon}[r(g(\epsilon;\phi))]
\end{align}
$$

$$
\nabla_{\phi}\mathbb{E}_{q(z;\phi)}[r(z)]=\nabla_{\phi}\mathbb{E}_{\epsilon}[r(g(\epsilon;\phi))]=\mathbb{E}_{\epsilon}[\nabla_{\phi}r(g(\epsilon;\phi))]
$$

我们可以用 Monte Carlo 方法从 $\mathcal{N}(0,I)$ 中采样 $\epsilon$ 来近似期望。