## Panorama
![](Pasted%20image%2020251215190931.png)
![](Pasted%20image%2020251215190946.png)

## RANSAC
RANdom SAmple Consensus

### motivation
avoid outliers

### How to do
- 随机选择 s 个样本 (一般 s 为最小的可以确定一个模型的样本数量)
- 拟合模型
- 重复 N 次
- 选择具有最多 inliers 的模型.
- 使用所有的 inliers 重新拟合模型.

![](Pasted%20image%2020251215193801.png)

### Pros and Cons
Pros:

- Simple & general
- Applicable to many problems
- Often works well in practice

Cons:

- Has parameters to tune
- May fail for low inliers ratios
- Requires too many iterations