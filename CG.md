
---

- Computer Graphics: the use of computers to <ins>synthesize</ins>（合成）and <ins>manipulate</ins>（操作）visual information.

  - <ins>可视化</ins>是一种操作视觉信息的方法：将其他类型的信息（如 空间信息）变化为视觉信息。

- 好的画面：看画面是不是<ins>足够亮</ins>。因为它体现了渲染中一个关键技术<ins>全局光照</ins>的好坏。

- ❓ 在图形学中如何表述卡通风格？

- 渲染（rendering）：光线在不同几何体之间弹跳最终进入“人眼”的过程。

- Rasterization（光栅化）：把三维空间中的几何形体显示在屏幕上。

- 实时（real-time）：每秒钟可以生成30幅画面。如果达不到这个标准，则称之为离线（offline）。

- 计算机视觉：一切需要猜测/推断/推理的内容（需 分析 理解意义，如 图像识别），基本都属于计算机视觉。

- 计算机图形学（特指 rendering）是在研究如何把表示三维空间中模型的信息 变成 一幅二维的image；而反过来，研究如何从一张图中分析出其各个内容信息 则是计算机视觉。

- 计算机图形学也研究如何描述三维形体（modelling）及其材质（如何与光线作用），如何做仿真（simulation）。

- 计算机视觉也研究图像处理（图像到图像，主要基于深度学习）。

- Use an IDE! (<ins>Visual Studio</ins> for Windows, <ins>Visual Studio Code</ins> for cross platform, <ins>NOT</ins> recommend for C++: CLion, Eclipse, Sublime Text, Vi/Vim, Emacs)

---

- 计算机图形学依赖于：

  - Math: Linear Algebra, Calculus, Statistics.
  - Physics: Optics, Mechanics.
  - Others: Signal Processing, Numerical Analysis.

- Vector does not hold information about its absolute starting position.

- Cartesian Coordinates 即 直角坐标系。

- 在图形学中，向量默认为 $n \times 1$ 矩阵（即 列向量）。

- Dot (Scalar) Product:

  - $\vec a \dot \vec b = \left\lVert \veca \right\rVert \left\lVert \vecb \right\rVert cos\theta$
  - $\left\lVert \vec a \right\rVert \left\lVert \vec b \right\rVert \cos\theta$

---
