## 8.1 假设检验问题
![](Pasted%20image%2020251205141411.png)

**第一类错误和第二类错误:**

| 真实情况     | 样本情况                     | 判断       | 正确性       |
| -------- | ------------------------ | -------- | --------- |
| $H_0$ 为真 | $x \in \mathcal{W}$      | 否定 $H_0$ | 错误, 第一类错误 |
| $H_0$ 为假 | $x \in \mathcal{W}$      | 否定 $H_0$ | 正确        |
| $H_0$ 为真 | $x \not \in \mathcal{W}$ | 接受 $H_0$ | 正确        |
| $H_0$ 为假 | $x \not \in \mathcal{W}$ | 接受 $H_0$ | 错误, 第二类错误 |
一般来说, 不可能使犯两类错误的概率一致地任意小, 所以我们首先希望犯第一类错误的概率尽可能小.

**功效函数:**

设 $(\Theta_0, \Theta_1)$ 为总体模型 $X \sim F_{\theta}(\theta \in \Theta)$ 的一个假设检验问题, $\boldsymbol{X}=(X_1,\cdots,X_n)$ 为总体的一个样本, $\mathcal{W}$ 为该问题的一个否定域, $\alpha \in (0,1)$ 是一个常数.

(1) 称定义在 $\Theta$ 上的函数 $\beta_{\mathcal{W}}(\theta) \triangleq P_{\theta}(\mathbf{X}\in \mathcal{W})$  为 $\mathcal{W}$ 的功效函数

(2) 若 $\mathcal{W}$ 满足条件

$$
\sup_{\theta\in\Theta_0}\beta_{\mathcal{W}}(\theta)\leq\alpha
$$

则称 $\mathcal{W}$ 为假设检验问题 $(\Theta_0, \Theta_1)$ 的一个 **显著性水平** (简称水平) 为 $\alpha$ 的否定域.

(人话: 如果参数在 $\Theta_0$ 内那么抽样得到的样本在否定域的概率小于等于 $\alpha$, 也就是说被否定的概率小于等于 $\alpha$)

这样我们就控制了犯第一类错误的概率, 接下来我们可以在显著性水平为 $\alpha$ 的否定域中寻找最优的否定域, 也就是要让犯第二类错误的概率也尽可能小.

设 $(\Theta_0, \Theta_1)$ 为总体模型 $X \sim F_{\theta}(\theta \in \Theta)$ 的一个假设检验问题, $\boldsymbol{X}=(X_1,\cdots,X_n)$ 为总体的一个样本, $\mathcal{W}$ 为该问题的一个否定域, 称 $\mathcal{W}$ 为该假设检验问题的水平为 $\alpha$ 的 **一致最大功效否定域(UMP否定域)**, 如果:

(1) $\mathcal{W}$ 是水平为 $\alpha$ 的否定域.

(2) 对任何其他水平为 $\alpha$ 的否定域 $\widetilde{\mathcal{W}}$, 均有:

$$
\beta_{\mathcal{W}}(\theta)\geq\beta_{\widetilde{\mathcal{W}}}(\theta) \quad (\forall \theta\in\Theta_1)
$$

(人话: $\mathcal{W}$ 满足犯第一类错误的概率小于等于 $\alpha$, 而且犯第二类错误的概率是最小的, 也就是条件里面的 $H_0$ 不为真的时候否定概率最大)

由于 UMP 否定域的要求太苛刻, 不得已而求其次, 提出了无偏性的要求

设 $(\Theta_0, \Theta_1)$ 为总体模型 $X \sim F_{\theta}(\theta \in \Theta)$ 的一个假设检验问题, $\mathbf{X}=(X_1,\cdots,X_n)$ 为总体的一个样本, $\mathcal{W}$ 为该问题的一个水平为 $\alpha$ 的否定域, 称 $\mathcal{W}$ 为该假设检验问题的水平为 $\alpha$ 的 **无偏否定域**, 如果对任何 $\theta_0\in\Theta_0$ 和 $\theta_1\in\Theta_1$ 均有:

$$
\beta_{\mathcal{W}}(\theta_0)\leq \alpha \leq\beta_{{\mathcal{W}}}(\theta_1) 
$$

有了无偏性的条件我们就可以排除一些很奇怪的否定域, 它们在参数属于 $\Theta_1$ 的时候有的功效函数很小, 很容易接受 $H_0$ 导致犯第二类错误, 有的功效函数又很大, 导致我们找不到一致最大功效否定域. 排除了这类否定域后我们就可以定义最优无偏否定域(UMPU否定域).

设 $(\Theta_0, \Theta_1)$ 为总体模型 $X \sim F_{\theta}(\theta \in \Theta)$ 的一个假设检验问题, $\boldsymbol{X}=(X_1,\cdots,X_n)$ 为总体的一个样本, $\mathcal{W}$ 为该问题的一个否定域, 称 $\mathcal{W}$ 为该假设检验问题的水平为 $\alpha$ 的 **最优无偏否定域(UMPU否定域)**, 如果:

(1) $\mathcal{W}$ 是水平为 $\alpha$ 的无偏否定域.

(2) 对任何其他水平为 $\alpha$ 的无偏否定域 $\widetilde{\mathcal{W}}$, 均有:

$$
\beta_{\mathcal{W}}(\theta)\geq\beta_{\widetilde{\mathcal{W}}}(\theta) \quad (\forall \theta\in\Theta_1)
$$

在选择零假设的时候, 我们要权衡哪一类错误带来的损失更大, 选择合适的零假设. 

## 8.2 N-P 引理和似然比检验

先来考虑最简单的情况, 设 $X\sim F_{\theta}$, $\theta\in\Theta=\{\theta_0, \theta_1\}$ , 即参数空间只有两个点, 假设检验问题为:

$$
H_0:\theta=\theta_0 \leftrightarrow H_1:\theta=\theta_1
$$

这类检验问题称为 **简单假设检验问题**, 当 $\Theta_0, \Theta_1$ 有一个不是单点集的时候就是复杂假设检验问题. 对于简单假设检验问题, 有如下的 N-P 引理.

(N-P 引理) 设 $X$ 的一个样本为 $\boldsymbol{X}=(X_1,\cdots,X_n)$, 假设 $X$ 的分布密度为 $f(\boldsymbol{x},\theta),\theta=\theta_0或\theta_1$, 记 $L(\boldsymbol{x},\theta)=\prod_{i=1}^n f(x_i,\theta)$,  其中 $\boldsymbol{x}=(x_1,\cdots,x_n)$, 若对于给定的 $\alpha\in(0,1)$, $n$ 维空间的集合

$$
\mathcal{W}=\{\boldsymbol{x}:L(\boldsymbol{x},\theta_1)>\lambda_0L(\boldsymbol{x},\theta_0)\} \quad (\lambda_0\geq0为常数)
$$

满足

$$
\beta_\mathcal{W}(\theta_0)\triangleq\int_{\mathcal{W}}L(\boldsymbol{x},\theta_0)\mathrm{d}\boldsymbol{x}=\alpha
$$

则 $\mathcal{W}$ 就是水平为 $\alpha$ 的 UMP 否定域.

(人话: 如果有一个满足似然比大于某个常数的集合, 而且参数为 $\theta_0$ 的时候样本落在这个集合的概率是 $\alpha$, 那么它就是 UMP 否定域)

证明: 首先说明 $\mathcal{W}$ 是一个水平为 $\alpha$ 的否定域(显然). 然后说明任取一个水平为 $\alpha$ 的否定域 $\widetilde{\mathcal{W}}$ 有 $\beta_{\mathcal{W}}(\theta_1) \geq \beta_{\widetilde{\mathcal{W}}}(\theta_1)$ 

$$
\begin{align*}
\beta_{\tilde{W}}(\theta_1) - \beta_{W}(\theta_1) &= P_{\theta_1}(\mathbf{X} \in \tilde{W}) - P_{\theta_1}(\mathbf{X} \in W) \\
&= \int \cdots \int_{\tilde{W}} L(\mathbf{x}, \theta_1) d\mathbf{x} - \int \cdots \int_{W} L(\mathbf{x}, \theta_1) d\mathbf{x} \\
&= \int \cdots \int_{\tilde{W} \setminus W} L(\mathbf{x}, \theta_1) d\mathbf{x} - \int \cdots \int_{W \setminus \tilde{W}} L(\mathbf{x}, \theta_1) d\mathbf{x} \\
&\ge \lambda_0 \left[ \int \cdots \int_{\tilde{W} \setminus W} L(\mathbf{x}, \theta_0) d\mathbf{x} - \int \cdots \int_{W \setminus \tilde{W}} L(\mathbf{x}, \theta_0) d\mathbf{x} \right] \\
&= \lambda_0 \left[ \int \cdots \int_{\tilde{W}} L(\mathbf{x}, \theta_0) d\mathbf{x} - \int \cdots \int_{W} L(\mathbf{x}, \theta_0) d\mathbf{x} \right] \\
&= \lambda_0 [\alpha - \beta_{\tilde{W}}(\theta_0)] \ge 0
\end{align*}
$$

证明的关键是利用了集合 $\mathcal{W}$ 的性质, 在集合内的 $\boldsymbol{x}$ 满足似然比大于常数, 不在集合里的就是小于该常数, 刚好一个在正号一个在负号可以同时利用这个不等式. 最后一步利用 $\widetilde{\mathcal{W}}$ 是水平为 $\alpha$ 否定域的性质.

N-P 引力的核心是利用似然比 $\lambda(\boldsymbol{x})\triangleq L(\boldsymbol{x},\theta_1)/L(\boldsymbol{x},\theta_0)$ 构造否定域, 因此给出的否定域又称似然比否定域, 检验法叫做似然比检验. 一般来说先写出否定域的形式, 然后在参数为 $\theta_0$ 的条件下再根据水平 $\alpha$ 求出常数 $\lambda_0$.

附带说明一下：作为假设检验的解，否定域 $W$ 是一样本点 $\mathbf{x}$ 的集合。通常经过简化以后，否定域具有 $\{\mathbf{x}: T(\mathbf{x}) > c\}$ 或 $\{\mathbf{x}: T(\mathbf{x}) < c_1\} \cup \{\mathbf{x}: T(\mathbf{x}) > c_2\}$ 的形式，它是通过统计量 $T(\mathbf{x})$ 构造得到的。此时，统计量 $T(\mathbf{x})$ 就称为检验统计量。

在求否定域的时候, 可利用否定域内容不变的原则对否定域的形式作变换, 使得否定域中待定常数的确定变得十分容易.

根据 N-P 引理求出来的否定域具有无偏性, 可以验证 $\beta_{\mathcal{W}}(\theta_1)\geq\alpha$, 验证过程基本上还是利用 $\mathcal{W}$ 的性质将积分化为在 $\mathcal{W}$ 上的和在它的余集上的. 然后利用不等式放缩.

## 8.3 单参数模型的检验

接下来考虑稍微复杂一点的假设检验问题, 设假设检验问题为

$$
H_0:\theta\in\Theta_0 \leftrightarrow H_1:\theta=\theta_1 \tag{3.1}
$$

现在 $\Theta_0$ 不再是单点集了, 但是, 如果满足以下条件, 我们可以找出水平为 $\alpha$ 的 UMP 否定域.

(定理3.1) 设 $\theta_0\in\Theta_0$ 是一个参数点, $\mathcal{W}$ 是简单假设检验问题

$$
H_0:\theta\in\theta_0 \leftrightarrow H_1:\theta=\theta_1 \tag{3.2}
$$

的水平为 $\alpha$ 的 UMP 否定域, 若 $\mathcal{W}$ 还满足条件:

$$
\beta_{\mathcal{W}}(\theta)\leq\alpha(\theta\in\Theta_0) \tag{3.3}
$$

那么它也是上述 (3.1) 假设检验问题的水平为 $\alpha$ 的 UMP 否定域.

证明:
- 首先, 根据 (3.3) $\mathcal{W}$ 是 (3.1) 的一个水平为 $\alpha$ 的否定域. 
- 如果 (3.1) 有一个水平为 $\alpha$ 的否定域 $\widetilde{\mathcal{W}}$, 它也是检验问题 (3.2) 的水平为 $\alpha$ 的否定域. (3.2是取了一个单点,它犯错的概率肯定小于 $\alpha$) 
- 由于 $\mathcal{W}$ 是 (3.2) 的 UMP 否定域, 所以肯定有   
  $$
	 \beta_{\mathcal{W}}(\theta_1)\geq\beta_{\widetilde{\mathcal{W}}}(\theta_1)
   $$
 - 所以 $\mathcal{W}$ 也是 (3.1) 的 UMP 否定域

这个定理巧妙地将 UMP 的验证从 $\Theta_0$ 转移到了 $\theta_0$ 应用这个定理的困难是 $\theta_0$ 很难找, (3.3) 的条件也很难验证.

(3.1) 是复杂假设检验问题的特例, 下面是更一般的问题:

$$
H_0:\theta\in\Theta_0\leftrightarrow H_1:\theta\in\Theta_1 \tag{3.4}
$$

这里两个集合都不再是单点集了. 对于这样的问题, 有如下定理:

(定理3.2) 若对于 $\Theta_1$ 的每一个单点 $\theta_1$ , 假设检验问题 (3.1) 存在水平为 $\alpha$ 的 UMP 否定域 $\mathcal{W}$ , 而且它不依赖于 $\theta_1\in\Theta_1$(对于所有 $\theta_1$ 都是这个), 则 $\mathcal{W}$ 也是 (3.4) 的水平为 $\alpha$ 的 UMP 否定域.

验证过程和上面的类似, 都是利用取定单点把 UMP 的验证过程转移到单点上.

下面指出对于一类单参数指数族分布, 有可能利用上述定理找到 UMP 否定域.

如果分布密度可以分解成如下形式:

$$f(\mathbf{x}, \theta) = S(\theta)h(\mathbf{x})\exp\{C(\theta)T(\mathbf{x})\} \quad (\theta \in \Theta), \quad (3.6)$$

其中 $C(\theta)$ 为 $\theta$ 的严格增函数, 那么就称 $\{f(x,\theta),\theta\in\Theta\}$ 构成**单参数指数族**.

显然, 如果 $\theta$ 越大, 相应的 $L(x,\theta)$ 也就越大.

现在考虑单参数指数族分布中如下的假设检验问题：

$$H_0: \theta \le \theta_0 \leftrightarrow H_1: \theta > \theta_0 \tag{3.9}$$

(称此假设检验问题为**单边假设检验问题**。同样，假设检验问题 $H_0: \theta \ge \theta_0 \leftrightarrow H_1: \theta < \theta_0$ 也称为**单边假设检验问题**)

相应地，称假设检验问题

$$H_0: \theta = \theta_0 \leftrightarrow H_1: \theta \ne \theta_0$$

为**双边假设检验问题**。利用定理 3.1、定理 3.2 和定理 3.3，可得到假设检验问题 (3.9) 的水平为 $\alpha$ 的 **UMP 否定域**。

设 $\mathbf{X}$ 的分布族 $\{f(\mathbf{x}, \theta), \theta \in \Theta\}$ 为单参数指数族，又设 $\mathbf{X} = (X_1, \dots, X_n)$ 为来自 $\mathbf{X}$ 的一个样本。记

$$\mathcal{W} = \left\{\mathbf{x}: \sum_{i=1}^n T(x_i) > c \right\},$$

其中 $c$ 为任一常数。只要 $\alpha = P_{\theta_0}(\mathbf{X} \in \mathcal{W}) \ne 0$，则 $\mathcal{W}$ 就是假设检验问题 (3.9) 的水平为 $\alpha$ 的 **UMP 否定域**。

核心就是利用 $\beta_{\mathcal{W}}$ 的非减性质, 只要 $\theta_0$ 的时候犯错概率等于 $\alpha$, 那么参数小于等于 $\theta_0$ 的时候犯错概率肯定小于等于 $\alpha$. 所以它是 (3.9) 的一个水平为 $\alpha$ 的否定域.

对于任意 $\theta_0<\theta_1$, 考虑简单假设检验问题 $(\theta_0,\theta_1)$, 似然比否定域根据指数族分布的性质必然可以化为  $\{\boldsymbol{x}:\sum T(x_i)>c\}$ 的形式, $\mathcal{W}$ 正是它的 UMP 否定域. 而且与 $\theta_1$ 的选取无关. 由定理 3.2 知道它也是 (3.9) 的 UMP 否定域.

>[!note] 如何理解似然比否定域必然可以化为统计量>c的形式
>只要满足了 $P_{\theta_0}(\boldsymbol{x}\in\mathcal{W})=\alpha$, 对于具体的问题因为似然函比函数关于 $\theta$ 是单调的, 对于简单检验问题$(\theta_0,\theta_1)$ 我们总能通过代换得到似然比大于某个常数的形式, 这个常数与具体的 $\theta_0, \theta_1$ 有关.