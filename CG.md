# Xiaoyang Liu's Notes on Computer Graphics

## Menu<a name="menu"></a>

- [Overview](#overview)
- [Linear Algebra](#LA)
- [Transformation](#transformation)

---

# Overview<a name="overview"></a>

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

# Linear Algebra<a name="LA"></a>

- 计算机图形学依赖于：

  - Math: Linear Algebra, Calculus, Statistics.
  - Physics: Optics, Mechanics.
  - Others: Signal Processing, Numerical Analysis.

- Vector does not hold information about its absolute starting position.

- Cartesian Coordinates 即 直角坐标系。

- 在图形学中，向量默认为 $n \times 1$ 矩阵（即 列向量，即 矩阵作用在向量上时默认为左乘）。

## Dot (Scalar) Product:

- Operating on vectors, resulting in scalar.

- 点乘可以快速得到两个向量的夹角。

- $\vec a \cdot \vec b = \left\lVert \vec a \right\rVert \left\lVert \vec b \right\rVert \cos\theta$
    
  And thus, for unit vectors: $\cos\theta = \hat a \cdot \hat b$
    
- $\cos\theta = \frac{\vec a \cdot \vec b}{\left\lVert \vec a \right\rVert \left\lVert \vec b \right\rVert}$

- 若将向量定义为列向量且看作 $n \times 1$ 的矩阵，则点乘的矩阵表示如下：$$\vec a \cdot \vec b = {\vec a}^{T} \vec b$$

- 点乘满足：

  - $\vec a \cdot \vec b = \vec b \cdot \vec a$
  - $\vec a \cdot (\vec b + \vec c) = \vec a \cdot \vec b + \vec a \cdot \vec c$
  - $(k \vec a) \cdot \vec b = \vec a \cdot (k \vec b) = k (\vec a \cdot \vec b)$ where $k$ is a scalar.

- In <ins>Cartesian</ins> coordinates, dot product satisfies:

$$
\vec a \cdot \vec b =
\begin{pmatrix}
x_a \\
y_a \\
z_a
\end{pmatrix}
\cdot
\begin{pmatrix}
x_b \\
y_b \\
z_b \\
\end{pmatrix}
= x_a x_b + y_a y_b + z_a z_b
$$

- 点乘可以帮助我们任意地对一个向量做（相对于另一个向量的）垂直与平行的分解：
  
  Let ${\vec b_{\bot a}}$ be the projection (vector) of $\vec b$ on $\vec a$. Then:
  
  ${\vec b_{\bot a}} = k \hat a$ where we can see that $k = \left\lVert {\vec b_{\bot a}} \right\rVert = \left\lVert \vec b \right\rVert \cos \theta = \hat a \cdot \vec b$
  
  另一方向分量为 $\vec b - \vec b_{\bot a}$

- 点乘的结果还可以告诉我们两个向量的方向是否基本一致（点乘结果 $ > 0$），是否基本相反（点乘结果 $ < 0$），或相互垂直（点乘结果 $=0$）。
  
- 点乘的结果还可以告诉我们两个单位向量的方向有多接近（即 它们之间夹角有多大）：方向越接近点乘结果越接近 $1$，方向越相反点乘结果越接近 $-1$。

## Cross (Vector) Product

- Cross product (denoted by the operator $\times$), when taking in two vectors in 3D, say, $a$ and $b$ (note that this order matters), returns another 3D vector, say $c$, that is <ins>orthogonal</ins> to both $a$ and $b$, with the direction of $c$ determined by right-hand rule starting from $a$ towards $b$.

- The magnitude of a cross product is as follows:
  
  $$\left\lVert \vec a \times \vec b \right\rVert = \left\lVert \vec a \right\rVert \left\lVert \vec b \right\rVert \sin \theta$$
  
  where $\theta$ is between $0^o$ and $180^o$.

- Properties of cross product:

  $$\vec a \times \vec b = - \vec b \times \vec a$$

  $$\vec a \times \vec a = \vec 0$$

  (note that $\vec 0$ is the zero vector i.e. tuple of zeros, not the scalar $0$)

  $$\vec a \times (\vec b + \vec c) = \vec a \times \vec b + \vec a \times \vec c$$

  $$k(\vec a \times \vec b) = (k \vec a) \times \vec b = \vec a \times (k \vec b)$$

- 如果在一个 Cartesian Coordinates 里 $\vec x \times \vec y = \vec z$, 那么我们就说这个坐标系是一个<ins>右手坐标系</ins>。 反之若$\vec x \times \vec y = - \vec z$，则该坐标系为<ins>左手坐标系</ins>。

- Algebraically: (其中 $A^* $ 称作 dual matrix)

$$
\vec a \times \vec b =
\begin{pmatrix}
y_a z_b - y_b z_a \\
z_a x_b - x_a z_b \\
x_a y_b - y_a x_b
\end{pmatrix}
= A^* b =
\begin{pmatrix}
0 & -z_a & y_a \\
z_a & 0 & -x_a \\
-y_a & x_a & 0
\end{pmatrix}
\begin{pmatrix}
x_b \\
y_b \\
z_b
\end{pmatrix}
$$

- 叉乘在图形学中的作用：

  - 判断一个向量是在另一个向量的左侧还是右侧：
    
    给定两个向量 $a$ 和 $b$，在由 $a$ 和 $b$ 构成的平面中，若 $a$ 逆时针旋转小于 $180^o$ 可到达 $b$ 的方向，则称 $b$ 在 $a$ 的左侧；若 $a$ 顺时针旋转小于 $180^o$ 可到达 $b$ 的方向，则称 $b$ 在 $a$ 的右侧。
    
    此时若以 $a$ 和 $b$ 构成的平面为 $xy$ 平面建立右手坐标系，则当$b$ 在 $a$ 的左侧时 $\vec a \times \vec b \gt 0 $，当$b$ 在 $a$ 的右侧时 $\vec a \times \vec b \lt 0 $。
  
  - 判断一个点是否在一三角形内部：
    
    设3D直角坐标系内有一三角形 $ABC$ 及一点 $P$。可见，若 $\vec{AB} \times \vec{AP}$, $\vec{BC} \times \vec{BP}$, $\vec{CA} \times \vec{CP}$ 三者符号相同，则 $P$ 点在三角形 $ABC$ 内部，反之则在其外部。值得注意的是，若 $P$ 点在三角形上（即 以上三者之一为 $0$ ），则可自行定义 $P$ 点是在三角形 $ABC$ 内部还是外部。

## Matrices

- The rule for matrices addition and multiplication of a matrix by a scalar: element by element.

- Properties of matrices multiplication:

  - $AB$ and $BA$ are different in general.
  - $(AB)C=A(BC)$
  - $A(B+C)=AB+AC$
  - $(A+B)C=AC+BC$
  - $AA^{-1}=A^{-1}A=I$
  - $(AB)^{-1}=B^{-1}A^{-1}$
  - $(AB)^{T}=B^{T}A^{T}$

---

# Transformation<a name="transformation"></a>

### Scale:

$$\vec v = S_{a,b} \vec {v_0}$$

$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
a & 0 \\
0 & b
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix}
$$

### Reflection on y-axis:

$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
-1 & 0 \\
0 & 1
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix}
$$

### Shear（切变）on the line $y=1$ with $(0,1) \to (a,1)$:

$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
1 & a \\
0 & 1
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix}
$$

- Rotation:

  Define: by default, we rotate <ins>counter-clockwise w.r.t the origin</ins>.

  $$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
R_{\theta}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix} =
\begin{pmatrix}
\cos{\theta} & - \sin{\theta} \\
\sin{\theta} & \cos{\theta}
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix}
$$
  
  其中$R_{\theta}$可通过特殊点$(1,0)$及$(0,1)$的变换中得出。

<br>

- A <ins>linear transformation</ins> of $n$-dimensional vector $\iff$ A $n \times n$ matrix.

- A translation can be represented as follows:

$$x=x_0+t_x$$

$$y=y_0+t_y$$

Since there is no matrix which can produce such transformation, <ins>translation is not linear transformation</ins>.

---
