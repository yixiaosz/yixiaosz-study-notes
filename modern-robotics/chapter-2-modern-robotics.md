# Modern Robotics Study Notes

Kevin Lynch's book **Modern Robotics: Mechanics, Planning and Control**

Note created on: 12/13/2025



## Chapter 2: C-Space

Definitions: 

- The **configuration of a robot** is a complete **specification** of the position of every point of the robot.
- The **number of DOF** is minimum number of real-valued (independent) coordinates that represent the configuration.
- The **Configuration Space (C-Space)** is the #-dimensional space that contains all of the robot's configuration. 

### 2.1 DOF of a rigid body

**number of DOF = sum(freedoms of points) - (number of independent constraint)**

- General Rule: The Dimension of the C-Space or the # of DOF equals the sum of the freedoms of the points minus the number of independent constraints acting on those points. 

- For just rigid bodies (applied to this course): 

​	$\text{number of DOF} = \sum(\text{freedoms of bodies}) - (\text{number of independent constraints})$ 



- A planar rigid body has 3 DoF, and **a spatial rigid body has 6 DOF**:

![image-20251213213658618](assets/rigid-body-dof.jpg)

- **The # of DOF** 

​	= **the dimension of its C-Space** 

​	= the # of real numbers needed to specify its configuration

​	= sum(freedoms of bodies) - (# of independent constraint)



- **A rigid body in n-dimensional space has m total degrees of freedom. How many of these m degrees of freedom are angular (not linear)?**

  - `m-n`

    n linear coordinates specify the location of one point of the rigid body, and the remaining m - n coordinates are subject to radius constraints, and hence as angular coordinates


  - `n(n-1)/2`

    The general formula for calculating # of angular DOF, so it's equivalent to `m-n`




### 2.2 DOF of a Robot

![joint-types](assets/joint-types.jpg)

| Joint Type      | DOF (f_i) | Constraints btwn two planar rigid bodies (c_i) | Constraints btwn two spatial rigid bodies (c_i) |
| --------------- | --------- | ---------------------------------------------- | ----------------------------------------------- |
| Revolute (R)    | 1         | 2                                              | 5                                               |
| Prismatic (P)   | 1         | 2                                              | 5                                               |
| Helical (H)     | 1         | N/A                                            | 5                                               |
| Cylindrical (C) | 2         | N/A                                            | 4                                               |
| Universal (U)   | 2         | N/A                                            | 4                                               |
| Spherical (S)   | 3         | N/A                                            | 3                                               |



#### Grubler's formula

- determining the number of degrees of freedom of a robot, simply by counting the number of rigid bodies and joints. 

- As we already known:

  $\text{number of DOF} = \sum(\text{freedoms of bodies}) - (\text{number of independent constraints})$ 

  

- All constraints must be independent, otherwise this formula fails

  $\text{DOF} = \underbrace{m(N-1)}_{\text{rigid body freedoms}} - \underbrace{\sum_{i=1}^{J} c_i}_{\text{joint constraints}}
  \newline
  = m(N-1)-\sum_{i=1}^{J}(m-f_i)
  \newline
  = m(N-1-J)+\sum_{i=1}^{J}(f_i)$ 



$N$ = # of bodies, including ground 

​	(**the ground always counts as one single rigid body**, no matter how many connection it has with the robot)

$J$ = # of joints 

​	(**a single joint can only connect two rigid bodies**, so if there's more than two on one body, there must be more than one joint there)

$m$ = **6 for spatial bodies**, **3 for planar bodies**, 10 for 4-D bodies

​	(constant, the # of C-space dimension)

$f_i$ = the number of freedoms provided by joint i 

​	**(count how many each type of joints there are, then sum their DoFs)**

$c_i$ = the number of constraints provided by joint i

$c_i$ + $f_i$ = $m$ for all i

- If Grubler's formula counts 0 or negative:

  **the constraints implied by the joints may be independent** or the mechanism is incapable of motion. 



#### Example Calculation

![srs-arms-rigid-grab](assets/srs-arms-rigid-grab.jpg)

![srs-sketch](assets/srs-sketch.jpg)

m = 6, N = 10, J = 12

sum(f_i) = 7 * 4 = 28

Using [Grubler's formula](#grublers-formula): DOF = 6(10 - 1 - 12) + 28 = 10

**if there're n arms**: DOF = 6(2+2n-1-3n)+7n = n+6

**if the revolute joints are replaced by universal joints**: m, N, J stays the same; sum(f_i) = 8n; DOF = 6(2+2n-1-3n) +8n = 2n+6



### 2.3 Topology and Representation

- **two spaces are topologically equivalent if one can be continuously deformed into the other without cutting or gluing.** 
- Distinct one-dimensional spaces: 
  - **the circle** $S$ or $S^1$ (meaning a 1-D sphere)
  - **the line** $E$ or $E^1$ (meaning a 1-D Euclidean, flat space)
    - an open interval of the line $(a,b)\subset R^1$ is topologically equivalent to $E^1$ if stretched
  - **a closed interval of the line** $[a,b]\subset R^1$ ($R^1$ means a point on the line; line includes the endpoints $a,b$)
- In higher dimensions:
  - $R^n$ is the $n$-dimensional Euclidean space
  - $S^n$ is the $n$-dimensional surface of a sphere in an $(n+1)$-dimensional space (e.g. $S^2$ is the 2-D surface of a sphere in a 3-D space)
- The topology of a space is **a fundamental property of the space** itself.
- The topology of a space is **independent of how we choose coordinates** to represent points in the space (e.g. ($x,y$) with constraint $x^2+y^2=1$) 

![c-space-topology](./assets/c-space-topology.jpg)

#### C-space as Cartesian Product 

- Used to represent a C-space with two or more spaces of lower dimension.

- ==a rigid body in the plane is $R^2\times S^1$==: the concatenation of a 2D coordinate (x,y) and an angle $\theta$ representing $S^1$.

- ==a rigid body in the 3D space is $R^3 \times S^2 \times S^1$==.  

  > Note that $S^n \times S^1 \neq S^{n+1}$.

  - a point in $R^3$, plus a point on a two-dimensional sphere $S^2$, plus a point on a one-dimensional circle $S^2$.

- a PR (Prismatic + Revolute) robot arm is $R^1 \times S^1$. 

  - if mounted on a planar rigid body, the C-space is $(R^2 \times S^1) \times (R^1\times S^1) = R^2 \times T^2$. 
  - if mounted on a spatial rigid body, the C-space is $(R^3 \times S^2 \times S^1) \times (R^1\times S^1) = R^4 \times S^2 \times T^2$.

- a RR robot arm is ==$S^1 \times S^1 = T^2$==. 

  > Note that $T^n \times S^1 = T^{n+1}$.

#### Numerical representation of C-Space 

- **Explicit parametrization**: minimum number of coordinates 
  - simplicity, but may have poor behavior because the space change

- **Example: to represent the C-space of the surface of a sphere** (pendulum with one fixed end)

  - Explicit: [-90º, 90º] for latitute, [-180º, 180º] for longitude
    - near north and south pole, a very small step gives large changes in coordinates
    - near equator, a large step gives small changes in coordinates
    - the north and south pole are **sigularities** of the representation, loss of dimension, which is problematic

- How to avoid the singularity problem?

  - A. Use **more than one coordinate chart** on the space, each covering only a portion of the space
    - form an **atlas** with multiple charts: always uses the minimum number of numbers

  - B. Use implicit parametrization

#### Implicit parametrization

Make a surface embedded in a higher-dimensional space. (the major perametrization in this book)

- Implicit parametrization of a 2D sphere surface in 3D space: $(x,y,z)$ with constraint $x^2+y^2+z^2=1$ 
- Advantage: Best for curved shape. No singularies. No need for multiple coordinate charts, hence no atlas
- **For robots containing one or more closed loops**, usually an implicit representation is more easily obtained than an explicit parametrization.
- Drawback: the representation has more numbers than the number of DOF

**Implicit representation of a rigid body in space**

- **the rotation matrix**: 9 numbers, subject to 6 constraints (in a 9-6=3 dimension space)
- able to use linear algebra to perform roation or change of reference frame



### 2.4 Configuration and Velocity Constraints

![four-bar-linkage](assets/four-bar-linkage.jpg)

This four-bar linkage can be viewed as a serial chain with 4 revolute joints that:

​	(1) the top of linnk $L_4$ always coincides with the origin

​	(2) the orientation of $L_4$ is always horizontal. 

> Based on [Grubler's formula](#grublers-formula), we knew this mechanism has **1 DOF** in 2D space. 

- Dimensional space: 4-D space

  $ \boldsymbol{\theta} = \begin{bmatrix} \theta_1 \\ \theta_2 \\ \theta_3 \\ \theta_4 \end{bmatrix} \in \mathbb{R}^4 $   

- The **absolute angles** of each linkage:

  - angle of L1: $\theta_1$ 
  - angle of L3: $\theta_1+\theta_2$ 
  - angle of L4: $\theta_1+\theta_2+\theta_3$ 
  - angle of L5: $\theta_1+\theta_2+\theta_3+\theta_4$ 

- **The X-axis constraint** (for position):

  > knowing that x-axis component $x = L\cdot cos(\theta)$ and the linkage have no horizontal movement (=0)
  >
  > L = distance travelled

  (1) $L_1 \cos\theta_1 + L_2 \cos(\theta_1 + \theta_2) + \dots + L_4 \cos(\theta_1 + \dots + \theta_4) = 0$ 

- **The Y-axis constraint** (for position): 

  > knowing that y-axis component $y = L\cdot sin(\theta) $ and the linkage have no vertical movement (=0)

  (2) $L_1 \sin\theta_1 + L_2 \sin(\theta_1 + \theta_2) + \dots + L_4 \sin(\theta_1 + \dots + \theta_4) = 0$ 

- **The angular constraint** (for orientation):

  > Because both ends are on the ground, the sum of angles is a whole loop $2\pi$

  (3) $\theta_1 + \theta_2 + \theta_3 + \theta_4 - 2\pi = 0$ 

- (1), (2), (3) are referred to as the **loop-closure equations**

  - Exist in a four joint space AKA **a 4D space** : ($\theta_1, \theta_2, \theta_3, \theta_4$)
  - **there're 3 equations, but have 4 unknowns**, meaning the system is not locked, and has 1 DOF
  - Hence **the C-space of this 4-bar linkage is just a one-dimensional curve** in a 4-dimensional joint space.

- Thus, the 4-bar linkage can be represented by 4 dimensional joint space (in the form of column vector below) subject to 3 loop constraints.



#### Holonomic Constraints

- **general scenario**: $ \boldsymbol{\theta} = \begin{bmatrix} \theta_1 \\ \theta_2 \\ ... \\ \theta_n \end{bmatrix} \in \mathbb{R}^n $ 

- **The Close-loop equations ARE the ==holonomic constraints== of the mechanism**

​	$g(\theta) = \begin{bmatrix} g_1(\theta_1, \ldots, \theta_n) \\ \vdots \\ g_k(\theta_1, \ldots, \theta_n) \end{bmatrix} = 0,\ k \le n ,\ g(\theta) \in \mathbb{R}^n $ 

​	$n$ = total variables (dimension of the joint space)

​	$k$ = number of holonomic constraints

- The close-loop equations/holonomic constraints properties:

  - A set of $k$ independent queations, with $k \le$ n, meaning that **holonomic constraints reduces the joint space**
  - They constraints on only configuration aka position 
  - C-Space Dimension: $DOF = n - k$  aka $ \text{DOF} = n - (\text{number of independent Holonomic Constraints}) $ 

  

#### Pfaffian Constraints

- We knew that the loop must stay closed so $g(\theta) = 0$ 

- The total derivative with respect to time $t$ is the sum of the partial derivatives for every angle

  $\frac{d}{dt} g(\theta(t)) = \frac{\partial g}{\partial \theta_1} \frac{d\theta_1}{dt} + \frac{\partial g}{\partial \theta_2} \frac{d\theta_2}{dt} + \dots + \frac{\partial g}{\partial \theta_n} \frac{d\theta_n}{dt} = 0$ 

- Because we also know that $\dot{\theta} = \frac{d\theta}{dt}$: 

​	$\frac{\partial g}{\partial \theta_1} \dot{\theta}_1 + \dots + \frac{\partial g}{\partial \theta_n} \dot{\theta}_n = 0 $ 

> Note of the $\dot{\theta}$ notation
>
> - $\theta$  = **Position** (Angle)
> - $\dot{\theta} = \frac{d\theta}{dt}$ = **velocity** (angular velocity)
> - $\ddot{\theta} = \frac{d^2\theta}{dt^2}$ = **acceleration** (angular acceleration)

- Further noted in matrix form:

​	$$\underbrace{\begin{bmatrix} \frac{\partial g_1}{\partial \theta_1} & \dots & \frac{\partial g_1}{\partial \theta_n} \\ \vdots & \ddots & \vdots \\ \frac{\partial g_k}{\partial \theta_1} & \dots & \frac{\partial g_k}{\partial \theta_n} \end{bmatrix}}_{\text{The Jacobian Matrix } A(\theta)} \cdot \underbrace{\begin{bmatrix} \dot{\theta}_1 \\ \vdots \\ \dot{\theta}_n \end{bmatrix}}_{\text{Velocity Vector } \dot{\theta}} = \begin{bmatrix} 0 \\ \vdots \\ 0 \end{bmatrix}$$ 

- **The generic Pfaffian form**: $A(\theta)\dot{\theta} = 0$
  - $\dot{\theta}$ is the movement
  - The dot product being 0 means the motion must be perpendicular to the constraint forces
- **Integrable Pfaffian constraints are holonomic constraints**
- **Non-integrable Pfaffian constraints are nonholonomic constraints**



#### Nonholonomic Constraints

- the non-integrable Pfaffian constraints

![pfaffian](assets/pfaffian.jpg)

#### Integrability test

Example: 

- We have a differential constraint: $(1+cosq_1)\dot{q_1}+(2+sinq_2)\dot{q_2}+(cosq_1+sinq_2+3)\dot{q_3} = 0$ 

- General Pfaffian form: $A_1(q) \dot{q}_1 + A_2(q) \dot{q}_2 + A_3(q) \dot{q}_3 = 0$ 

- The coefficients are: 
  - $A_1 = 1 + \cos q_1$ 
  - $A_2 = 2 + \sin q_2$ 
  - $A_3 = \cos q_1 + \sin q_2 + 3$ 



### 2.5 Task Space and Workspace

- Task space: the space in which the robot's task is naturally expressed. 
  - The task space depends on the task, not the robot.
- Wrokspace: a specification of the reachable configurations of the end effector. 
  - The workspace of a particular robot is independent of the task.



## Supplement 1: Partial Derivatives

> Reference: [Partial Derivatives and the Gradient of a Function by Professor Dave Explains - Youtube](https://youtu.be/AXH9Xm6Rbfc?si=Hi2mEpz1IyRVZb0v) 

- For regular single-variable derivatives, such as $f(x) = sin(x)$, we know $\frac{df}{dx} = cos(x)\,dx$.  

- For **multi-variable derivative**, partial derivatives **treats only one variable and treats others as constant**. 

- Example: $f(x,y) = xy^2 + x^3$ 

  - Derivative with respect to x: 

    $\frac{\partial f}{\partial x} = \frac{\partial}{\partial x}(x)y^2 + \frac{\partial}{\partial x}(x^3) = y^2 + 3x^2$ 

  - Derivative with respect to y: 

    $\frac{\partial f}{\partial y} = \frac{\partial}{\partial y}x(y^2) + \frac{\partial}{\partial y}x^3 = 2xy$ 

  - Thus, the **gradient of the function is a vector made up of all the partial derivatives of the function**. 

    1. Unit vector notation: $\text{grad f} = \frac{\partial f}{\partial x}\hat{i} + \frac{\partial f}{\partial y}\hat{j} = (y^2 + 3x^2)\hat{i} + (2xy)\hat{j}$ 

    2. Vector component notation: $\text{grad f} = <\frac{\partial f}{\partial x} , \frac{\partial f}{\partial y}> = <y^2 + 3x^2 , 2xy>$ 
    3. In 3-D space, the gradient will be $\text{grad f} = \frac{\partial f}{\partial x}\hat{i} + \frac{\partial f}{\partial y}\hat{j} + \frac{\partial f}{\partial z}\hat{k}$ 

- Alternative notation for vector component notation: $\triangledown$, **del** or nobla. 

  - e.g., $\triangledown = <\frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}>$ 

- Example 2: consider now a scalar function $\bar{x}$ with multiple inputs (this time in column vector notation). 

  - Given that: 

    $$\bar{x} = \begin{bmatrix} \ x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{bmatrix}$$

    $$f(x_1,x_2) = 3(x_1)^2+(x_2)^2$$

  - As the gradient of a function follows: 

    $$\triangledown f(\bar{x}) = \frac{\partial f(\bar{x})}{\partial \bar{x}} = \begin{bmatrix}\frac{\partial f(\bar{x})}{\partial x_1} \\\ \frac{\partial f(\bar{x})}{\partial x_2} \\\ \vdots  \\\ \frac{\partial f(\bar{x})}{\partial x_n} \end{bmatrix}$$

    We can calculate: 

    $$\triangledown f(\bar{x}) = \begin{bmatrix} \ \frac{\partial f(\bar{x})}{\partial x_1} \\ \frac{\partial f(\bar{x})}{\partial x_2} \end{bmatrix} = \begin{bmatrix} 6x_1 \\ 3(x_2)^2 \end{bmatrix}$$ 

  - the $x_1$ gradient $\frac{\partial f(\bar{x})}{\partial x_1}$ is the **sensitivity of the function** $f(\bar{x})$ **to change in** $x_1$. 

  -  the $x_2$ gradient $\frac{\partial f(\bar{x})}{\partial x_2}$ is the **sensitivity of the function** $f(\bar{x})$ **to change in** $x_2$. 

- Property of the gradient:

  1. The gradient points in the **direction of maximum change**. 
  2. Its magnitude is equal to the maximum rate of change. 



## Supplement 2: Jacobian Matrix 

> Reference: [The Jacobian Matrix by Christopher Lum - Youtube](https://youtu.be/QexBVGVM690?si=TXnUbeAiJf-dQFwF) 

- A Jacobian Matrix is a partial set of derivatives asking how do each one of the functions change the input variables



