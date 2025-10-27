
### 1.1 Recap: Ray Tracing

Forward: 从光源出发，追踪光线打到眼睛。（低效，可能很多光没有进入眼睛）
Backward: 从眼睛出发，追踪打到光源的光线。

- When ray hits a surface we perfrom shading:
	- **"shading" determines what color you observe (along the way)**

```cpp
#include <iostream>
using namespace std;
int main() {
	return 0;
}
```

```
for each pixel:
	ray = camera.getRay(pixel);
	c = scene.trace(ray, 0, +inf);
	image.set(pixel, c);

Scene::trace(ray, tMin, tMax):
	hit = surfaces.intersect(ray, tMin, tMax);
	if (hit)
		return hit.color();
	else 
		return backgroundColor;
```


**The reflection equation**

Reflected radiance is a (hemi)spherical integral of incident radiance from all directions

**BRDF(Bidirectional reflectance distribution function)**







