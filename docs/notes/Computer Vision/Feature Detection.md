## Edge Detection
### Sobel
![](Pasted%20image%2020251215180418.png)

Remember to smooth the image before edge detection, otherwise the noise would affect the result.

Notice that:

$$
\frac{d}{dx}(f*g)=f*\frac{d}{dx}g
$$

so we can apply gaussian derivative kernel on the image.

### Image Gradient Magnitutde
$$
||\nabla f||=\sqrt{(\frac{\partial f}{\partial x})^2+(\frac{\partial f}{\partial y})^2}
$$

### Non-Maximum Suppresion
直接使用 gradient magnitude 产生的边缘过厚, 我们采用如下方式将其变细: 对于每个点, 只保留边的方向上梯度幅值最大的点.
![](Pasted%20image%2020251215181500.png)

### Double Threshold
\> 0.7 认为是边缘
< 0.3 认为不是边缘
中间认为是 弱边.

### Edge Tracking by Hysteresis
Weak edges that are connected to strong edges will be actual/real edges.

Weak edges that are not connected to strong edges will be removed

## Corners
### Harris Corner Detector
**核心观察:** 考虑一个窗口内的像素, 角点位置窗口在各个方向移动后像素值均有很大变化.

![](Pasted%20image%2020251215182021.png)

![](Pasted%20image%2020251215182041.png)

Use Taylor Series to simplify the calculation

![](Pasted%20image%2020251215182102.png)

M 的特征值反映了 E 在各个方向的变化幅度

![](Pasted%20image%2020251215182315.png)

### Properties of Corners
- equivariant with translation and rotation
- partially invariant to affine intensity changes
- Corners are not equivariant with scaling

![](Pasted%20image%2020251215184331.png)

how to make it equivariant with scaling? -- Gaussian Pyramids!

![](Pasted%20image%2020251215184504.png)

## Blobs
Blobs are regions in a digital image that differ in properties, such as brightness or color, compared to surrounding regions.

Blobs have fixed positions and sizes, can be localized, and are good interest points.

![](Pasted%20image%2020251215184807.png)