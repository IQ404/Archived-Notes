# Xiaoyang Liu's Notes on Computer Graphics

## Menu<a name="menu"></a>

- [Overview](#overview)
- [Linear Algebra](#la)
- [Transformation](#transformation)
- [Rasterization](#rasterization)
- [Shading](#shading)
- [Geometry]()
- [Ray Tracing](#raytracing)

- [Animation](#animation)

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

# Linear Algebra<a name="la"></a>

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

- <ins>Note</ins>: OpenGL uses left hand coords.

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

## 2D Transformations

📜 Scale:

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

📜 Reflection on y-axis:

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

📜 Shear（切变/错切）on the line $y=1$ with $(0,1) \to (a,1)$:

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

📜 Rotation:

Define: by default, we rotate <ins>counter-clockwise w.r.t the origin</ins>.

$$\begin{pmatrix}
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

值得注意：

$$R_{-\theta}=R_{\theta}^{-1}=
\begin{pmatrix}
\cos{\theta} & \sin{\theta} \\
-\sin{\theta} & \cos{\theta}
\end{pmatrix}=
R_{\theta}^{T}
$$

i.e. 旋转矩阵是一种正交矩阵。

<br>

📜 A <ins>linear transformation</ins> of $n$-dimensional vector $\iff$ A $n \times n$ matrix.

📜 A translation can be represented as follows:

  $$x=x_0+t_x$$

  $$y=y_0+t_y$$

We can see that: there is no matrix which can produce such transformation $\iff$ <ins>translation is not linear transformation</ins>.
  
We can rather represent the translation as follows:
  
$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
I_{2\times 2}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix} +
\begin{pmatrix}
t_x \\
t_y
\end{pmatrix}
$$

📜 仿射变换（Affine Transformation/Map）：

A transformation that accounts for linear map and translation.

$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix} +
\begin{pmatrix}
t_x \\
t_y
\end{pmatrix}
$$

<ins>Note</ins>: following the particular definition of affine map above, affine map will <ins>first do linear mapping, then do translation</ins>.

📜 Homogeneous Coordinates（齐次坐标）

<ins>Note</ins>: All the conclusions here in 2D are still true in 3D.

In <ins>2D</ins>, define:

- point: $(x,y,1)^T$
- vector: $(x,y,0)^T$

Now, we can represent translation of 2D <ins>point</ins> under homogenous coordinates by a single matrix (left-)multiplication:

$$
\begin{pmatrix}
x \\
y \\
w
\end{pmatrix} =
\begin{pmatrix}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0 \\
1
\end{pmatrix} =
\begin{pmatrix}
x_0+t_x \\
y_0+t_y \\
1
\end{pmatrix}
$$

On the other hand, vector 具有<ins>平移不变性</ins>：

$$
\begin{pmatrix}
x \\
y \\
w
\end{pmatrix} =
\begin{pmatrix}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0 \\
0
\end{pmatrix} =
\begin{pmatrix}
x_0 \\
y_0 \\
0
\end{pmatrix}
$$

Further Define:

- Any 2D point (i.e. $w\neq 0$) in homogenous coordinates $(x,y,w)^T$ represent the 2D point $(x/w,y/w,1)^T$.

Then, we can see that, in homogenous coordinates with $w=0$ or <ins>reduced</ins> to $1$:

- vector $+$ vector $=$ vector
- point $-$ point $=$ vector
- point $+$ vector $=$ point
- point $+$ point $=$ mid-point

<br>

Any affine map of the form (i.e. first do linear map, then do translation):

$$
\begin{pmatrix}
x \\
y
\end{pmatrix} =
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0
\end{pmatrix} +
\begin{pmatrix}
t_x \\
t_y
\end{pmatrix}
$$

can be written in homogenous coordinates:

$$
\begin{pmatrix}
x \\
y \\
w
\end{pmatrix} =
\begin{pmatrix}
a & b & t_x \\
c & d & t_y \\
0 & 0 & 1
\end{pmatrix}
\begin{pmatrix}
x_0 \\
y_0 \\
w
\end{pmatrix}
$$

<ins>Note</ins>: the last raw (0 0 1) is only valid when representing affine map.

For examples:

- Scale:

$$
S(s_x,s_y)=
\begin{pmatrix}
s_x & 0 & 0 \\
0 & s_y & 0 \\
0 & 0 & 1
\end{pmatrix}
$$

- Rotation:

$$
R(\theta)=
\begin{pmatrix}
\cos{\theta} & -\sin{\theta} & 0 \\
\sin{\theta} & \cos{\theta} & 0 \\
0 & 0 & 1
\end{pmatrix}
$$

- Translation:

$$
T(t_x,t_y)=
\begin{pmatrix}
1 & 0 & t_x \\
0 & 1 & t_y \\
0 & 0 & 1
\end{pmatrix}
$$

📜 The transformation that rotates $\theta$ around a given point $c$:  $$T(\vec c) \cdot R(\theta) \cdot T(-\vec c)$$

## 3D Transformations

📜 Scale:

$$
S(s_x,s_y,s_z)=
\begin{pmatrix}
s_x & 0 & 0 & 0 \\
0 & s_y & 0 & 0 \\
0 & 0 & s_z & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

📜 Translation:

$$
T(t_x,t_y,t_z)=
\begin{pmatrix}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

📜 Rotation around $x$-, $y$- or $z$-axis (anti-clockwise w.r.t the axis pointing outwards, 右手系):

$$
R_x(\theta)=
\begin{pmatrix}
1 & 0 & 0 & 0 \\
0 & \cos \theta & -\sin \theta & 0 \\
0 & \sin \theta & \cos \theta & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

$$
R_y(\theta)=
\begin{pmatrix}
\cos \theta & 0 & \sin \theta & 0 \\
0 & 1 & 0 & 0 \\
-\sin \theta & 0 & \cos \theta & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

$$
R_z(\theta)=
\begin{pmatrix}
\cos \theta & -\sin \theta & 0 & 0 \\
\sin \theta & \cos \theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

❓ If Pitch and Yaw can represent Roll, why would we need Roll?

📜 Rodrigues' Rotation Formula

$$
R(\vec{n}, \theta)=
\cos{(\theta)}I+(1-\cos{(\theta)})\vec{n}\vec{n}^T+\sin{(\theta)}
\begin{pmatrix}
0 & -n_z & n_y \\
n_z & 0 & -n_x \\
-n_y & n_x & 0
\end{pmatrix}
$$

where $R$ is a $3\times 3$ matrix, $I$ is the $3\times 3$ identity matrix, $\vec{n}$ is the $3\times 1$ vector <ins>starting from the origin</ins> representing the rotation axis and $\theta$ is the amount of angle we rotate (anti-clockwise).

<ins>Note</ins> that the rightmost matrix is the matrix representation of $\vec{n} \times$.

📜 Quaternion（四元数）

主要为求旋转与旋转之间的插值而引入。四元数与矩阵存在一一对应的转化。需用其解决具体问题时可自行查找相关资料。

## View / Camera transformation

📜 To define a camera, we need:

- its position: $\vec{e}$
- its look-at (gaze) direction: $\hat{g}$
- its up direction: $\hat{t}$

📜 We always transform al the things in the world so that the camera is at the origin, up at $y$-axis and looking at $-z$-axis.

To do this, we have:

$$
M_{\text{view}}=R_{\text{view}}T_{\text{view}}
$$

where $T_{view}$ is a translation from a world position $e$ to the origin:

$$
T_{\text{view}}=
\begin{pmatrix}
1 & 0 & 0 & -x_e \\
0 & 1 & 0 & -y_e \\
0 & 0 & 1 & -z_e \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

where $e$ is the original position of the camera (when it is not at the origin).

$R_{\text{view}}$ is the rotation that aligns the original orientation of the camera with the $x$-, $y$-, $z$-axis. To write such a matrix, we can first write down the matrix which rotates $\hat{x}$, $\hat{y}$, $\hat{z}$ to the original orientation of the camera (which is easier to write):

$$
R^{'}=
\begin{pmatrix}
x_{\hat{g}\times\hat{t}} & x_{\hat{t}} & x_{- \hat{g}} & 0 \\
y_{\hat{g}\times\hat{t}} & y_{\hat{t}} & y_{- \hat{g}} & 0 \\
z_{\hat{g}\times\hat{t}} & z_{\hat{t}} & z_{- \hat{g}} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

Now, note the statements:

- All rotation matrices are orthogonal.
- For orthogonal matrix $M$, we have $M^{-1}=M^T$

Hence, we can see that:

$$
R_{\text{view}}=(R^{'})^{-1}=(R^{'})^{T}=
\begin{pmatrix}
x_{\hat{g}\times\hat{t}} & y_{\hat{g}\times\hat{t}} & z_{\hat{g}\times\hat{t}} & 0 \\
x_{\hat{t}} & y_{\hat{t}} & z_{\hat{t}} & 0 \\
x_{- \hat{g}} & y_{- \hat{g}} & z_{- \hat{g}} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

## Projection transformation

对于透视投影，我们可以想象为将相机放置在空间中的某一个<ins>点</ins>上，以该点作为起始点向某些方向放出射线构成一个四棱锥，我们将该四棱锥内部某一深度（表示Near clip plane）到另一深度（表示Far clip plane）组成的区域称作frustum（形为一四棱台）。我们将该frustum内的所以物体都显示在Near clip plane的过程称为透视投影。

对于正交投影，我们可以想象为将相机放置在<ins>无限远</ins>的地方，此时Near clip plane的大小将与Far clip plane相同（i.e. frustrum为一长方体）。我们将该frustum内的所以物体都显示在Near clip plane的过程称为正交投影。

- 正交投影与透视投影的区别在于前者没有近大远小的性质而后者有。

- 定义 Canonical cube：$[-1,1]^3$

- 有时我们称 Canonical cube 中的坐标为<ins>正则化空间坐标（canonical space coordinate）</ins>。

📜 Orthographic projection（正交投影）

给定空间中的一个长方体（i.e. 想要显示的区域），其表示为 far, near, left, right, bottom, top 六个数字。

Orthographic projection的目标是把该长方体映射成为Canonical cube。

- <ins>Note</ins>: we use right hand coordinates here

To do this, we first <ins>translate</ins> the centre of the rectangle to the origin, then <ins>scale</ins> the translated rectangle to a canonical cube:

$$
M_{\text{ortho}}=
\begin{pmatrix}
\frac{2}{\text{right}-\text{left}} & 0 & 0 & 0 \\
0 & \frac{2}{\text{top}-\text{bottom}} & 0 & 0 \\
0 & 0 & \frac{2}{\text{near}-\text{far}} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
\begin{pmatrix}
1 & 0 & 0 & -\frac{\text{right}+\text{left}}{2} \\
0 & 1 & 0 & -\frac{\text{top}+\text{bottom}}{2} \\
0 & 0 & 1 & -\frac{\text{near}+\text{far}}{2} \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

📜 Perspective projection（透视投影）

First, in a right hand coordinates, put the camera (which forms the frustum) to the origin with its up direction being $\hat{y}$ and facing the $-z$ direction.

Then, "squish" the frustum into a cuboid such that:

1. All the points on the near clip plane do not change.
2. The centre point on the far clip plane $(0,0,\text{far})$ do not change.
3. Any point in/on the frustum $(x,y,z)$ change to $(x_n,y_n,z^{'})$ where $(x_n,y_n,\text{near})$ is the point on the near clip plane which is on the same ray with $(x,y,z)$.

Now,

- ❓ Show that such "squish" indeed gives the wanted result for perspective projection.

- ❓ Prove that under homogeneous coordinates there <ins>exists</ins> a linear transformation (a $4\times 4$ matrix $M_{\text{persp}\to\text{ortho}}$) that corresponds to this "squish".

- ❓ Prove that:

$$
M_{\text{persp}\to\text{ortho}}=
\begin{pmatrix}
\text{near} & 0 & 0 & 0 \\
0 & \text{near} & 0 & 0 \\
0 & 0 & \text{near}+\text{far} & -\text{near}\times\text{far} \\
0 & 0 & 1 & 0
\end{pmatrix}
$$

❓ Is such $M_{\text{persp}\to\text{ortho}}$ unique?

Then, we have:

$$
M_{\text{persp}}=M_{\text{ortho}}M_{\text{persp}\to\text{ortho}}
$$

---

# Rasterization<a name="rasterization"></a>

📜 光栅化主要应用于<ins>实时应用</ins>（游戏，VR etc.）。

📜 假设我们知道 $\text{near}$ 和 $\text{far}$，则我们可利用以下两点来定义perspective projection中的视锥（frustum）：

- Vertical Field of View (简写为fovY，译为“垂直可视角度”。fov译为“视角”)
- Aspect ratio ($=\frac{\text{width}}{\text{height}}$)

其中 width 和 height 为 near clip plane 的宽度和高度。

<ins>Note</ins>: given aspect ratio, fovY and fovX（译为“水平可视角度”） can always be derived from each other.

注意，此时相机应已在标准位置（位于原点，朝向 $-\hat{z}$，向上方向为 $\hat{y}$），因此我们可 convert fovY and aspect ratio to $\text{left}, \text{right}, \text{bottom}, \text{top}$ of the near clip plane:

$$
\tan{\frac{\text{fovY}}{2}=\frac{\text{top}}{|\text{near}|}}
$$

$$
\text{bottom}=-\text{top}
$$

$$
\text{aspect ratio}=\frac{\text{right}}{\text{top}}
$$

$$
\text{left}=-\text{right}
$$

📜 屏幕是一个<ins>二维</ins>数组，该数组中的每一个元素为一个像素（pixel）。屏幕的大小称为分辨率（resolution），例如 $1920*1080$。

📜 A <ins>simplified</ins> definition of pixel:

- Short for <ins>picture element</ins>.
- A square with <ins>uniform</ins> color.

There are mainly two ways to define the color in a pixel:

- Mixing from red, green and blue, each represented by a number.
- 256 levels each represents a color where 0 is black and 255 is white.

❓ iPhone 6S 的屏幕上每个像素的内部并不是一个uniform的颜色，而是红绿蓝三个平行的条组成的。解释像素的真实内部结构。

❓ 解释 Bayer pattern（被应用于例如Galaxy S5 的屏幕上）。

📜 显示器上显示的信息是被存放在显卡的内存（显存）上的。

❓ 详细分析DAC（Digital to Analog Convertors）的原理。

📜 屏幕是一个典型的光栅成像设备（raster display）。

Raster在德语中为 screen 的意思。Rasterize 意为 drawing onto the screen。

❓ CRT（Cathode Ray Tube）如何控制电子打在屏幕上某点的颜色？

❓ 详细分析 隔行扫描 产生画面撕裂的原因。

- LED（Light Emitting Diode）

❓ LED发出不同颜色的光的原理。

- LCD（Liquid Crystal Display）

液晶的不同排布会影响光的极化（偏振方向），以此我们可以控制偏振光（由backlight（通常为LED）产生的光通过一Polarizer而产生）是否可以（如果可以，光栅与偏振的角度可控制亮度？）通过另一Polarizer显示在屏幕像素上。

❓ 如何定义视网膜的分辨率？

- Electrophoretic (Electronic Ink) Display

亚马逊的kindle电纸书就是用的该原理。

这种屏幕上每个像素中都有电负性相反的黑/白色分子。该种设备通过调整每个像素上的电压来对黑/白色分子进行翻转。

- ❓ 什么是OLED？

📜 Defining the screen space: $(0,0)$ on the bottom left, $y$-axis upwards, $x$ towards right.

Note that this definition is down to convention, different environment can have different definition.

<ins>Note</ins>:

- Pixels' indices are in the form of $(x,y)$ where $x,y \in \mathbb{Z}$.
- Pixels' indices are from $(0,0)$ to $(\text{width}-1,\text{height}-1)$.
- The "actual" pixel represented by $(x,y)$ is centered at $(x+0.5,y+0.5)$.
- The screen covers range $(0,0)$ to $(\text{width},\text{height})$.

📜 Viewport transformation（视口变换）

在得到$[-1,1]^3$的Canonical Cube后，将其$x,y$进行$[-1,1]^2 \to [0,\text{width}] \times [0,\text{height}]$的变换：

$$
M_{\text{viewport}}=
\begin{pmatrix}
\frac{\text{width}}{2} & 0 & 0 & \frac{\text{width}}{2} \\
0 & \frac{\text{height}}{2} & 0 & \frac{\text{height}}{2} \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$

📜 Why using triangle as fundamental shape primitives:

- It is the most basic polygon. All the other polygons can be broken down to triangles.
- The interior of triangle is guaranteed to be <ins>planar</ins>.
- Easy to determine whether a point is inside a triangle or not (by cross products).
- Well-defined method for interpolating values at vertices over triangle (barycentric interpolation).

📜 Sampling

Definition: evaluating a function at a point.

Rasterization can be done through sampling function (with the domain being the screen space, and the range being $\begin{Bmatrix} 0,1 \end{Bmatrix}$, where $0$ means the point in the screen space is outside a specific triangle and $1$ means the opposite) on the pixel locations (i.e. $(x+0.5,y+0.5)$ ).

Such a function can be denoted as:

$$
\text{inside}(t,x,y)=
\begin{cases}
1\qquad\text{Point (x,y) in triangle t} \\ 
0\qquad\text{otherwise}
\end{cases}
$$

📜 Bounding Box

对于给定三角形（即 3个 screen space 里的点：$(x_1,y_1)$, $(x_2,y_2)$ 和 $(x_3,y_3)$），我们可以找到：

$$
x_{min}=min(x_1,x_2,x_3)
$$

$$
x_{max}=max(x_1,x_2,x_3)
$$

$$
y_{min}=min(y_1,y_2,y_3)
$$

$$
y_{max}=max(y_1,y_2,y_3)
$$

这4个数可以表示一个与 $x,y$ 轴平行的正方形。该正方形可作为此三角形的<ins>轴向包围盒（i.e. AABB, Axis Aligned Bounding Box）</ins>。

When we do the sampling for the rasterization, we only need to loop within the AABB.

- ❓ 搞懂光栅化的另一种加速方法：Incremental Triangle Traversal

## Antialiasing（反走样/抗锯齿）

<ins>NOTE</ins>: my current understandings on this topic are really bad. Reference for further reading can be

- Any resources related to the field *Signal processing*
- *Fundamentals of Computer Graphics*, Chapters "*The Graphics Pipeline*" and "*Signal Processing*"
- *GAMES101*, Lecture 6: Rasterization 2 (Antialiasing and Z-Burffering)

<br>

📜 More about sampling:

- Photograph is a sampling (for each pixel) done on the plane containing the information gathered by the image sensor (e.g. from a camera).

- video is a sampling (for each frame) in time.

📜 Definition: Sampling <ins>Artifacts</ins>

This refers to any errors/mistakes/inaccuracies in computer graphics（一切图形学生成的“看上去不太对”的东西）.

例如：

- 采样后形成的锯齿（Jaggies, also called Staircase Pattern）。

- 摩尔纹（Moire Patterns）

  例如用手机拍电脑显示器后常得到的扭曲条纹。
  
  给定一幅（相对清晰的）图片，将其奇数行与奇数列上的像素全部拿掉，后再对在一块形成一张小的图，再将此小图放大至原图大小，则会产生摩尔纹。
  
  ❓ 理解摩尔纹具体产生原因。

- Wagon Wheel illusion (False Motion)

  例如高速转动的车轮产生的运动伪影。
  
  ❓ 理解运动伪影产生的具体原因。

📜 Behind the aliasing artifacts: signals are changing too fast (high frequency), but sampled too slowly.

❓ 深入理解这句话。

📜 The main idea of antialiasing: Blurring (Pre-Filtering) before Sampling.

❓ 深入理解为什么先采样后模糊<ins>不能</ins>达到反走样。

- 先采样后模糊的学名叫做 Blurred Aliasing.

📜 傅里叶级数展开：任何一个周期函数都可以被写成/近似为一个常数项加上一系列$\sin$与$\cos$函数的线性组合。

- 任何一个（周期）函数通过傅里叶级数展开都可分解为不同频率。

📜 傅里叶变换可以将 Spatial Domain（时域）上的函数变成其 Frequency Domain（频域）上的对应函数（频谱）；逆傅里叶变换可以将 Frequency Domain（频域）上的函数变成其 Spatial Domain（时域）上的对应函数。

- 频谱可以让我们看到一个时域上的信号（e.g. 图像）在各个不同频率上都长什么样。

❓ 时域上的信号（e.g. 图像）是如何表示成一个函数的？

❓ 深入理解何为频域，以及傅里叶变换（and thus 频谱）/逆傅里叶变换。

❓ 傅里叶级数与傅里叶变换是否可以作用在非周期性函数上？如果可以，与处理周期性函数有何区别？

📜 Formal definition of Aliases: Two different frequencies that are indistinguishable at a given sampling rate are called "aliases".

❓ 深入理解这个定义。

📜 Filtering: getting rid of certain frequency contents.

- 如果我们把频谱上的低频信息全部去掉后对频谱做逆傅里叶变换，我们将得到原图的edges（注：边界处信号发生剧烈变化）。
  
  这种操作叫做高通滤波（High-pass filter）。
  
- 如果我们把频谱上的高频信息全部去掉后对频谱做逆傅里叶变换，我们将得到原图模糊后的图像。

  这种操作叫做低通滤波（Low-pass filter）。

❓ 深入理解以上两点。

注：数字图像处理（非机器学习）为相关领域。

注：目前很多图像处理都采取机器学习的方法。

📜 Convolution（卷积）

- A simplified definition: point-wise local (weighted) averaging in a "sliding window".

Such "sliding window" is called a filter（滤波器/卷积核）。

❓ what if the evaluated point is in a place such that the filter goes outside the signal?

📜 Convolution Theorem

- Convolution in the spatial domain is equal to multiplication in the frequency domain; convolution in the frequency domain is equal to multiplication in the spatial domain.

❓ How to specifically represent a filter in spatial/frequency domain? (understand the pattern)

📜 若滤波器中各处weighting都一样，则对图像（时域）做卷积相当于对图像做blurring（i.e. 此时滤波器相当于一低通滤波器）。

并且滤波器覆盖面积越大，效果越模糊。

- 特殊情况：当卷积核大小为 $n \times n$ 像素时，我们若只考虑像素中心位置（i.e. 与显示器采样一致），则（对于已光栅化的图像）每一像素点的卷积结果为 以该点为中心该卷积核覆盖的所有像素的平均值。

❓ Understand why the color of the resulting pixel will be brighter if we just add up the values of the pixels covered by the box filter without averaging the sum.

📜 我们可以采样理解为：把一个时域上的连续函数（e.g. 图像信号）与冲激函数相乘所得到的函数。

❓ 上述所得函数与采样后复原出来的图像所对应的时/频域上的函数有什么区别？

通过Convolution Theorem我们可以发现，该resulting function在频域上的频谱相当于在频域上复制粘贴了很多份原函数的频谱。

并且采样率越高，这些原频谱复制粘贴的间隔就越大。

- 我们可以把走样理解为：复制粘贴频谱相重叠的现象。

❓ 深入理解这种对于走样的解释。

📜 频率分析角度下的反走样：首先对原信号做低通滤波器卷积，相当于去掉了频谱上高频的部分。这时再做采样（i.e. 复制频谱）便不会出现粘贴频谱间的重叠。

❓ 这样的做法可以确保让我们采样后得到的图像<ins>相对原信号卷积结果</ins>不产生走样，但它是如何确保该采样结果<ins>相对原信号</ins>不产生走样的呢？

📜 对于一个要被显示在屏幕上的图像信号，我们可以设卷积核与屏幕上的单位像素大小相同。则做卷积时，我们只需将每个像素中的信息求平均（因为最终整个卷积核覆盖的区域内只会显示该区域中心点的信息）。这样，我们便直接得到了卷积（模糊）后的信号的光栅化。遂完成反走样。

<br>

📜 上述过程中我们需要对每个像素内的信息求平均。In practice, we can implement (approximate) this as follows:

- Antialiasing by supersampling (MSAA, multisample anti-aliasing)

  在每个像素内部平均分配 $n \times n$ 个点，对这些点进行采样后求平均，将该平均值作为此像素内信息的平均值。
  
  ❓ 在工业界，MSAA中supersampling采样点的分布其实并不是平均的，而是以某种pattern分布的。同时，一些点会被相邻的像素复用。这样可以用更少的点（and thus 更少的计算量）达到相同的效果。深入理解这些pattern以及复用点的原理。

- FXAA（Fast Approximate AA）

  先直接采样，得到有锯齿的图像，然后在图像层面做后期处理：通过图像匹配的方法把图像中的边界找到，最后将这些边界换成没有锯齿的边界。

- TAA（Temporal AA）

  类似将MSAA中supersampling的点分布在时间上，使得current frame可以复用previous frame的信息。

📜 Super resolution（超分辨率）

将低分辨率图像变成高分辨率图像的技术。涉及猜测（补像素）。

- DLSS（Deep Learning Super Sampling）为典型的超分辨率技术。

## Z-Buffering（also called Depth-Buffer, 深度缓存，深度缓冲）

This is a way to represent visibility（可见性）/occlusion（遮挡）.

<ins>NOTE</ins>: Z-buffer canNOT represent transparent objects!

- Let the Z values in Z-buffer be the absolute distance from the camera to a point (and thus the Z values in Z-buffer are all positive where larger means further away from the camera).

📜 Painter's Algorithm

(Inspired by how painters paint) Paint from back to front, <ins>overwrite</ins> in the framebuffer.

📜 

---

# Shading<a name="shading"></a>

