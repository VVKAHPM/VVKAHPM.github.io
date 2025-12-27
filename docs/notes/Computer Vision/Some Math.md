## Convolution
(实际上, 计算机视觉中一部分的 convolution 指的其实是 correlation, 也即不是翻转再相乘.)

### Definition

离散形式:

$$
(f*g)[n]=\sum_{m=-\infty}^\infty f[m]g[n-m]
$$
连续形式:

$$
(f*g)(t)=\int_{-\infty}^{\infty}f(\tau)g(t-\tau)d\tau
$$
二维离散卷积:

$$
(I*K)[i,j]=\sum_m\sum_nI[m,n]\cdot K[i-m,j-n]
$$

二维互相关 (correlation):

$$
(I*K)[i,j]=\sum_m\sum_nI[i+m,j+n]\cdot K[m,n]
$$

### Properties

交换律, 结合律, 分配律, 微分性质, 平移不变性质.