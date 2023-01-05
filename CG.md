# Xiaoyang Liu's Notes on Computer Graphics

## Menu<a name="menu"></a>

- [Overview](#overview)
- [Linear Algebra](#la)
- [Transformation](#transformation)
- [Rasterization](#rasterization)

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

📜 Orthographic projection（正交投影）

给定空间中的一个长方体（i.e. 想要显示的区域），其表示为 far, near, left, right, bottom, top 六个数字。

Orthographic projection的目标是把该长方体映射成为Canonical cube。

- <ins>Note</ins>: we use right hand coordinates here

To do this, we first <ins>translate</ins> the centre of the rectangle to the origin, then <ins>scale</ins> the translated rectangle to a canonical cube:

$$
M_{ortho}=
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

❓ NEED BETTER UNDERSTANDING!!!

First, in a right hand coordinates, put the camera (which forms the frustum) to the origin with its up direction being $\hat{y}$ and facing the $-z$ direction.

Then, "squish" the frustum into a cuboid such that:

- All the points on the near clip plane do not change.
- The centre point on the far clip plane (which is $(0,0,\text{far})$) do not change.
- Any point in/on the frustum $(x,y,z)$ change to $(x_n,y_n,z^{'})$ where $(x_n,y_n,\text{near})$ is the point on the near clip plane which is on the same ray with $(x,y,z)$.

❓ 如何证明该矩阵确实表示该挤压过程？

---

# Rasterization<a name="rasterization"></a>

---
