![](Pasted%20image%2020251216195615.png)![](Pasted%20image%2020251216195711.png)
![](Pasted%20image%2020251216195716.png)


## Visual Hull based MVS

视觉凸包. 通过把每个相机的轮廓线画出来求交得到物体的一个轮廓, 填充轮廓内部.

![](Pasted%20image%2020251216200321.png)

通过先不让人出现拍一张照片, 再让人出现拍一张照片, 然后相减, 我们就可以得到轮廓线
![](Pasted%20image%2020251216200440.png)

## Plane-Sweep Stereo
先以一张图像为参考, 投射到 $[Zmin, Zmax]$ 的深度 (可以均匀划分), 得到不同的深度的三维坐标. 将坐标重投影到其他源图像 $I_i$ 上, 得到像素颜色值, 计算一下这些颜色值的方差, 选择使得方差最小的深度.

## Depth Map based MVS
每次通过相邻两个相机构建深度图 [Two-View Stereo](Two-View%20Stereo.md), 最后 merge 起来

注意不要进行平滑处理, 因为我们有很多相机, 直接平滑处理可能会因为很多点没拍到而导致结果很大误差.

![](Pasted%20image%2020251216201559.png)

## Patched-based MVS
![](Pasted%20image%2020251216202353.png)

不只是单像素的看, 也不是整体(Visual Hall, 轮廓线求交)的看, 而是分成一个个 patch.
![](Pasted%20image%2020251216203119.png)
![](Pasted%20image%2020251216203450.png)

![](Pasted%20image%2020251216203635.png)
![](Pasted%20image%2020251216203642.png)

通过调整 $n(p),c(p)$ 使得 $N(p)$ 尽可能的大. 因为这是连续函数, 所以可以求导.

例如, 先通过 triangulation 确定一下 $c(p)$ 的初始值, 然后初始化 $n(p)$ 为跟第一张照片平行, 然后根据这个初始值去优化.

$V(p)$ 的更新: 先选出 $N(i,\cdot,p)$ 总和最大的作为 reference, 然后从这个出发移除掉相似度小于某个阈值的图像. 

问题: 可能由于重复纹理导致错误匹配, 实践中要求:
![](Pasted%20image%2020251216205955.png)

## Pipeline
先对图像进行划分, 划分成一个个 Patch (例如 9x9 像素块), 每个 Patch 做 Feature Detection

之后从一个图像开始, 做 Feature Matching, 通过多张图片利用上面所述的方法构建三维世界中的 Patch. 

这样得到的 patch 比较稀疏, 接下来做 patch expansion.

如果一个 patch 旁边有未形成的 patch, 我们就用它来给邻居进行初始化.

最后进行过滤.

![](Pasted%20image%2020251216214037.png)

