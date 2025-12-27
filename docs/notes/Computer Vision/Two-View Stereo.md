## Basic Stereo Matching Algorithm
- Correspondence + Triangulation
- Use epipolar line to find correspondence and pick the best match

### Simplest case
epipolar lines = corresponding scanlines
![](Pasted%20image%2020251216161603.png)

对于这种情况, 显然的 epipolar 约束就是 v=v'

![](Pasted%20image%2020251216161621.png)

### General case
如果图像平面不平行, 我们就需要利用 fundamental matrix 将其变换到平行于基线的情况. (Stereo Rectification)

![](Pasted%20image%2020251216161707.png)


## Depth from Disparity
![](Pasted%20image%2020251216161753.png)
![](Pasted%20image%2020251216161818.png)

### error analysis
对于同一个真实深度为 $Z$ 的物体进行分析

假设真实的 disparity 为 $d_0$, 测量得到的 disparity 为 $d_1$

$$
\begin{align}
d_0 &= \frac{fB}{Z}\\
\Delta z&=\left|\frac{fB}{d_0}-\frac{fB}{d_1} \right| \\
&=fB\left|\frac{\Delta d}{d_0d_1}\right| \\
&\approx \frac{fB}{d_0^2}|\Delta d| \\
&=\frac{Z^2}{fB}|\Delta d|
\end{align}
$$

可以看到 $B$ 越大误差越小.

但是 $B$ 越大两个相机离得越远, 我们图像重叠部分就可能比较少, 找匹配点也比较困难.

![](Pasted%20image%2020251216162835.png)


## Local Stereo Matching
![](Pasted%20image%2020251216163103.png)

### Similarity Metrics
![](Pasted%20image%2020251216163118.png)

![](Pasted%20image%2020251216163208.png)

但是 Local Stereo matching 无法处理非常相似的 window.
![](Pasted%20image%2020251216163256.png)


## Global Stereo Matching

### Non-Local Constraints:

- Uniqueness
  ![](Pasted%20image%2020251216163744.png)
  左图中一个点对应右图两个点
- Ordering
  ![](Pasted%20image%2020251216163804.png)
  出现顺序不一样
- Smoothness
  ![](Pasted%20image%2020251216163839.png)
  相邻的点可能由于一个在前一个在后导致视差剧烈跳变.

### Disparity Space Image
![](Pasted%20image%2020251216163920.png)

通过定义匹配代价, 我们就可以利用动态规划算法进行匹配.


### Stereo Matching via Global Optimization
![](Pasted%20image%2020251216164142.png)![](Pasted%20image%2020251216164415.png)

Speed up -> Downsample, estimate, then upsample


## Other methods
- Deep Learning
- Structured Light
  利用投影仪向待测物体表面投射一个已知的 **光模式**, 由于物体深度变化, 图案会发生形变或位移. 相机从一个固定视角捕获这个形变后的团. 算法需要确定相机图像上的每个像素点，对应于投影仪投射的哪个原始光束, 一旦确定了对应关系就可以进行 Triangulation.
- Laser Scanning