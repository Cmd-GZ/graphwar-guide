# Graphwar指南

这是一份简易的Graphwar指南，说它简易是因为该指南假设读者会自行探索游戏中的内容，所以不会对一试便知的内容进行解释，也就是说，该指南不会把你当作白痴一样照顾。这份指南包含了一些玩这游戏应当了解的内人以及一些有用的技巧。我自己的话其实也没玩多久，也算是一个小资历，所以指南多多少少可能会有一些疏漏或者错误，如果你找到了某些问题或者有改进的建议，希望你能告诉我。

总之，Graphwar是一款数学函数射击游戏，你必须使用数学函数来击中你的敌人。你发射的子弹的轨迹由你输入的函数决定，你的目标是避开障碍物和你的队友，击中你的敌人。游戏发生在笛卡尔平面上。——译自官方介绍。

## 目录

- [Graphwar指南](#graphwar指南)
  - [目录](#目录)
  - [你必须知道的事实](#你必须知道的事实)
  - [符号](#符号)
    - [变量](#变量)
    - [二元运算](#二元运算)
    - [函数](#函数)
    - [常量](#常量)
    - [括号](#括号)
  - [有用的公式](#有用的公式)
    - [台阶(Sigmoid)函数:](#台阶sigmoid函数)
    - [尖刺函数:](#尖刺函数)
    - [双绝对值函数:](#双绝对值函数)
    - [Sine and cosine function:](#sine-and-cosine-function)
    - [General step function:](#general-step-function)
    - [Piecewise function approximation:](#piecewise-function-approximation)
    - [Periodic spike function:](#periodic-spike-function)
    - [Periodic general step function:](#periodic-general-step-function)
  - [微分方程模式](#微分方程模式)
    - [Mechanism](#mechanism)
      - [Algorithm](#algorithm)
    - [Skill](#skill)
      - [`y'` mode](#y-mode)
      - [`y''` mode](#y-mode-1)
      - [If the developer unprecedentedly pushes a $y^{(n)}$ mode](#if-the-developer-unprecedentedly-pushes-a-yn-mode)

## 你必须知道的事实

游戏有两支队伍，每个玩家必须只能在其中一支队伍中。

![image-20260625190748015](./README.assets/image-20260625190748015.png)

只有当所有人点击了$\checkmark$按钮并且变成绿色的时候，游戏才会开始，所以，别进入游戏后干楞着，别让其他人等你。

![image-20260625191533507](./README.assets/image-20260625191533507.png)

有点恶心人的一点是由于某些原因，比如有人加入或者离开房间，会让你变回白色。使你必须重新点击按钮。

你和你的队友永远都在左侧 ($x<0$)。你的敌人永远在右侧 ($x>0$)。如果分不清左右的话可以通过士兵的朝向来辨别敌我：朝向右边的是你和你的队友，朝向左边的是你的敌人。这个游戏有友伤，所以别把你队友一枪崩了求求了，我已经因为这样似过好多次了。

![image-20260625191842364](./README.assets/image-20260625191842364.png)

当进入你的回合，你所操控的其中一个士兵会变红，现在你可以在函数输入框内输入任何的函数表达式，点击 "Fire" 按钮或者按回车键就可以发射子弹，子弹将会根据你输入的函数表达式对应的曲线从左向右运动，每一回合你只能发射一次。

![image-20260625195737269](./README.assets/image-20260625195737269.png)

在普通模式 (`y`模式) 中 (至于[微分方程模式](#微分方程模式)，也就是`y'`和`y''`模式，我稍后再讲) 假设你输入的函数为$f$，你的坐标为$(x_0,y_0)$，那么你的子弹的实际运动曲线是:

$$
f_{\text{real}}(x)=f(x)+(y_0-f(x_0))
$$

![https://www.graphwar.com/ss2Graphwar.png](./README.assets/ss2Graphwar.png)

也就是说，游戏通过上下平移你输入的函数来确保你的子弹的实际运动曲线会经过发射点，而不是将发射点设为函数的原点，这就是当你输入诸如$x^2,x^3$之类的函数可能会得到一条非常陡峭的曲线的原因，尝试输入诸如$(x-x_0)^2$或者给函数乘以一个小系数可能会好些。

![image-20260625195704528](./README.assets/image-20260625195704528.png)

显然，函数 $f_{\text{real}}$ 只定义在 $x\ge x_0$ 的区域，所以曲线不存在于 $x<x_0$ 的区域。

由于你只能输入函数表达式，而一个函数意味着一个输入$x$仅能至多对应一个输出$f(x)$，因此多值函数是不可能的，所以显然子弹没办法向左，也就是向$-x$方向运动。

这些黑不拉几的圆圈是障碍，如果子弹击中了它们，子弹会爆炸并产生一个小洞，然后子弹会消失。

![image-20260625201339945](./README.assets/image-20260625201339945.png)

如果子弹击中了敌人或者友军，他们会被子弹击杀，但子弹不会消失，而是会继续运动。

![image-20260625201258139](./README.assets/image-20260625201258139.png)

粗略且理想化地说，战场是一个$[-25,25]\times[-15,15]$的矩形区域，若子弹击中边界，子弹会消失。

![image-20260625202013194](./README.assets/image-20260625202013194.png)

![image-20260625202054414](./README.assets/image-20260625202054414.png)

准确地说，在 Graphwar I，由于某些特性，战场实际上是$[-25,25)\times[-\frac{1125}{77},\frac{1125}{77})$，其中$25=\frac{50\cdot 770}{770\cdot2},\frac{1125}{77}=\frac{50\cdot 450}{770\cdot 2}\approx 14.6103896$

![image-20260705220825755](./README.assets/image-20260705220825755.png)

![image-20260705221447117](./README.assets/image-20260705221447117.png)

如果子弹经过了一个对于函数来说为定义的点，比如$y=\sqrt x$且子弹经过了一个$x<0$的坐标，那么子弹也会消失。

![image-20260625200104823](./README.assets/image-20260625200104823.png)

如果函数值太大，或者更基本地说，当计算函数值时中间值出现了 `NaN` ，则子弹会消失。

![image-20260625201945265](./README.assets/image-20260625201945265.png)

![image-20260629223755887](./README.assets/image-20260629223755887.png)

子弹运动距离，也就是函数曲线的长度，是有限的，如果距离太长，子弹会消失，这就是为什么诸如$\sin(100x)$通常无法跑得太远。

![image-20260625200328666](./README.assets/image-20260625200328666.png)

The map of a game is random. If you get a dumbass map, type `-skip` in the chat box and ask other guys to type it. The game will generate a new random map if everyone types `-skip`.

游戏的地图是随机的，如果你得到了一张傻逼地图，可以在聊天框中输入`-skip`并请求其他人也输入它，若所有人都输入了`-skip`，则游戏会重新生成一张随机地图。

## 符号

### 变量

- `x`:  $x$
- `y`:  $y$
- `y'`: $y'$

### 二元运算

- `+`: $+$
- `-`: $-$
- `*`: $\times$
- `/`: $\div$
- `^`: 幂运算，比如 `x^2` 是 $x^2$

### 函数

- `sqrt()`: 平方根，比如 `sqrt(x)` 是 $\sqrt{x}$
- `ln()`: 自然对数，比如 `ln(x)` 是 $\ln(x)$
- `log()`: 通用对数，比如 `log(x)` 是 $log_{10}(x)$
- `abs()`: 绝对值，比如 `abs(x)` 是 $|x|$
- `sin()` 或 `sen()`: 正弦，比如 `sin(x)` 是 $sin(x)$
- `cos()`: 余弦，比如 `cos(x)` 是 $cos(x)$
- `tan()` 或 `tg()`: 正切，比如 `tan(x)` 是 $tan(x)$
- `exp()`: 指数，比如 `exp(x)` 是 $e^x$

### 常量

- `pi`: $\pi$
- `e`: $e$

### 括号

- `(` and `)`: 括号，多用括号以避免错误的运算顺序。

## 有用的公式

本节中有部分内容参考了 [Graphwar Tutorial Sant Albert '12](https://www.youtube.com/watch?v=E_MmkxTO5kg) 和 [graphwar meta that i use (EN)](https://www.youtube.com/watch?v=EHuQe7SKwkA)

你你可以用加法和乘法阿来组合本节中的公式，抽象地说，加法意味着"或"，乘法意味着"和"。

### 台阶(Sigmoid)函数:

$$
\frac{k}{1+e^{-m(x-a)}}
$$

即：`((k)/((1+exp(-m*(x-(a))))))`

其中$m\in\mathbb{R}_+$是个比较大的正数。

该函数是以下函数的近似：

$$
\begin{cases}
0, & x\lt a\\
k, & x\ge a
\end{cases}
$$

即：若$x\lt a$，函数值为$0$，若$x\ge a$，函数值为$k$。

特别地，令$k$为负数可以让子弹向下移动。

![image-20260625204651483](./README.assets/image-20260625204651483.png)

![1782391863123334113](./README.assets/1782391863123334113.png)

这是一个很有用且简单的函数，可以被用来移动和躲避障碍物。

### 尖刺函数:

$$
\frac{h}{1+m(x-a)^2}
$$

即：`((h)/(1+m*(x-a)^2))`

其中$m\in\mathbb{R}_+$是个比较大的正数。

这个函数会在$x-a$处生成一个尖刺，高度为$h$。

![image-20260629231551089](./README.assets/image-20260629231551089.png)

![image-20260629231124761](./README.assets/image-20260629231124761.png)

比起用类似$\frac{h}{1+m(x-a)^2}+\frac{-h}{1+m(x-a-0.1)^2}$的玩意，这个函数要简洁且易用。

### 双绝对值函数:

$$
\frac{k}{2}(|x-a| - |x-b|)
$$

即：`(0.5*(k)(abs(x-(a))-abs(x-(b))))`

其中$a<b$

该函数与下面的函数等价：

$$
\begin{cases}
-\frac{k}{2}|a-b|, & x\lt a\\
k(x-a)-\frac{k}{2}|a-b|, & x\in[a,b]\\
\frac{k}{2}|a-b|, & x\gt b
\end{cases}
$$

which means the function is $-k/2|a-b|$ when $x\lt a$, a linear function with slope $k$ when $x\in[a,b]$, and $k/2|a-b|$ when $x\gt b$

![image-20260625210514390](./README.assets/image-20260625210514390.png)

![image-20260625210611315](./README.assets/image-20260625210611315.png)

**There are so many cheaters who like to use the combination of this function. If you see someone send a whole string of this stuff with very precise coefficients, it's very likely to be a cheater.**

### Sine and cosine function:

Although $\sin$ and $\cos$ are very basic, they are very useful to sweep a wide area if you give them a big angular velocity.

![image-20260625211651927](./README.assets/image-20260625211651927.png)

### General step function:

$$
\frac{f(x)}{1+e^{-m(x-a)}}
$$

That is: `((f)/((1+exp(-m*(x-(a))))))`

where $m\in\mathbb{R}_+$ is a big positive number.

This function can approximate the following function:

$$
\begin{cases}
0, & x\lt a\\
f(x), & x\ge a
\end{cases}
$$

That is, the function is 0 when $x\lt a$, and $f(x)$ when $x\ge a$.

![image-20260625212035083](./README.assets/image-20260625212035083.png)

![image-20260625211834599](./README.assets/image-20260625211834599.png)

This function can be used to make any function $f(x)$ work only after passing through a certain point. But if you want to let it work only for an interval, the General step function can't help you. Now you need the following stuff.

### Piecewise function approximation:

Let

$$
\mathbf 1_{(a,b)}(x)=\frac{1}{(1+e^{-m(x-a)})(1+e^{m(x-b)})}
$$

That is `(1/((1+exp(-m*(x-(a))))*(1+exp(m*(x-(b))))))`

where $m\in\mathbb{R}_+$ is a big positive number and $a<b$

![1782385252813761707](./README.assets/1782385252813761707.png)

to approximate the indicator function:

$$
\mathbb1_{[a,b]}(x)=
\begin{cases}
1, & x\in[a,b]\\
0, & \text{Otherwise}
\end{cases}
$$

You may notice that the function is just the multiplication of a step function and a step-like function.

(And btw I constructed another version of $\mathbf 1_{(a,b)}(x)$ by combining power functions like: $\mathbf1_{(a,b)}(x)=\left(1+\left(\frac{x-\frac{a+b}{2}}{\frac{a-b}{2}}\right)^{2m}\right)^{-1}$. But it's not useful because you have to replace $a$ and $b$ twice and $m$ must be a natural number.)

Let's consider the piecewise function:

$$
f(x)=
\begin{cases}
f_1(x), & x\in[a_1,b_1]\\
f_2(x), & x\in[a_2,b_2]\\
\vdots\\
f_n(x), & x\in[a_n,b_n]\\
0, & \text{Otherwise}
\end{cases}
$$

where $a_i<b_i$ for $i=1,2,\cdots,n$

And the piecewise function can be approximated by:

$$
f(x)\approx\sum_{i=1}^n\mathbf1_{(a_i,b_i)}(x)f_i(x)
$$

That is, the sum of something like `((f)*(1/((1+exp(-m*(x-(a))))*(1+exp(m*(x-(b)))))))`

![1782385263468249718](./README.assets/1782385263468249718.png)

![image-20260625213603005](./README.assets/image-20260625213603005.png)

If you played with me in the game, you will see that I usually use this function to construct any function I want. It's very useful for me, which is one of the best functions in my mind.

### Periodic spike function:

$$
\frac{h}{(1+m(\sin(\frac{\pi x}{T}))^2)}
$$

That is: `((h)/(1+m*(sin((pi*x)/(T)))^2))`

where $m\in\mathbb{R}_+$ is a big positive number.

The function creates a spike every $T$ units.

![img](./README.assets/5148762dab81b465440e197cfe6d52ff.png)

![image-20260630000214478](./README.assets/image-20260630000214478.png)

This function can also sweep a wide area by creating many spikes.

### Periodic general step function:

$$
\frac{f(x)}{1+e^{-m\left(\sin\left(\frac{\pi(x-p)}{T}\right)\sin\left(-\frac{\pi x}{T}\right)\right)}}
$$

That is: `((f)/(1+exp(-m*(sin(pi*(x-p)/(T))sin(-(pi*x)/(T))))))`

where $m\in\mathbb{R}_+$ is a big positive number, $T$ is the period, and $p\in(0,T)$.

In a period $[kT,kT+T)$, the function will be $f$ if $x\in[kT,kT+p)$, otherwise it will be $0$.

![image-20260630001433312](./README.assets/image-20260630001433312.png)

![image-20260630001359082](./README.assets/image-20260630001359082.png)

Based on this function, you can do many interesting things.

## 微分方程模式

### Mechanism

As you can see, there are 2 modes of ODE mode: `y'` mode and `y''` mode, corresponding to the first and second derivatives of $y$ with respect to $x$.

Like the normal mode, you can input any function expression in the input box to set the trajectory of your bullet. But it allows you to use variables other than $x$, like $y$ in `y'` mode and $y,y'$ in `y''` mode. (For the variable `y'`. There was a bug in the previous version which made it would be matched with `y` instead of `y'`. But it has been fixed in the latest version. So remember to update your game.)

![image-20260629184937660](./README.assets/image-20260629184937660.png)

As [Syntax](#syntax) said, `y` means $y$, and `y'` means $\frac{dy}{dx}$.

Instead of regarding the expression as a function, the game will treat it as an explicit ordinary differential equation (ODE) to describe the moving curve of your bullet. But it is still essentially a function, so like the normal mode, you can't let the bullet to move to left.

Assume you have understood the above stuff, I will show you how the bullet moves in ODE mode.

Assume the coordinates of the launcher are $(x_0,y_0)$

For the first order ODE i.e. `y'` mode, assume the expression you input is $E(x,y)$, then the corresponding moving curve of your bullet is the solution of the following Initial Value Problem (IVP):

$$
\begin{cases}
\frac{dy}{dx}=E(x,y)\\
y(x_0)=y_0
\end{cases}
$$

For the second order ODE i.e. `y''` mode, the moving curve is also the solution of a certain IVP, but it's clear that you need $2$ initial conditions instead of $1$ in this case. The first initial condition is the same, the game takes the firing angle as the second initial condition, which can be modified by pressing the up/down arrow keys in your turn.

![image-20260629185741113](./README.assets/image-20260629185741113.png)

Assume the firing angle is $\theta$, the expression you input is $E(x,y,\frac{dy}{dx})$ (i.e. $E(x,y,y')$), then the corresponding moving curve of your bullet is the solution of the following IVP:

$$
\begin{cases}
\frac{dy}{dx}=E(x,y,\frac{dy}{dx})\\
y(x_0)=y_0\\
\frac{dy}{dx}(x_0)=\tan\theta
\end{cases}
$$

#### Algorithm

Graphwar computes the curve by using the 4th order Runge-Kutta method whether in `y'` mode or `y''` mode because any $m$-order explicit ODE is equivalent to a first-order ODE system, as follows:

$$
\begin{align*}
\frac{d^my}{dx^m}=f(x,y,\frac{dy}{dx},\frac{d^2y}{dx^2},\cdots,\frac{d^{m-1}y}{dx^{m-1}})\\
\end{align*}
$$

is equivalent to

$$
\begin{cases}
\frac{dy_1}{dx}=y_2\\
\frac{dy_2}{dx}=y_3\\
\vdots\\
\frac{dy_{m-1}}{dx}=y_m\\
\frac{dy_m}{dx}=f(x,y_1,y_2,\cdots,y_m)\\
\end{cases}
$$

So generally consider the m-variable first-order ODE system IVP:

$$
\begin{cases}
\frac{d\mathbf y}{dx} = \mathbf f\bigl(x, \mathbf y(x)\bigr)\\
\mathbf y(x_0) = \mathbf y_{0}
\end{cases}
\qquad (1)
$$

where $\mathbf y\in\mathbb R^m,\mathbf f:\mathbb R\times\mathbb R^{m}\to\mathbb R^{m}$.

Let

$$
\begin{aligned}
\mathbf Y(x)
&=
\begin{pmatrix}
Y_0(x)\\
Y_1(x)\\
\vdots\\
Y_m(x)
\end{pmatrix}
{:}=
\begin{pmatrix}
x\\
\mathbf y(x)
\end{pmatrix},
\\
\mathbf F(\mathbf Y)
&=
\begin{pmatrix}
F_0(\mathbf Y)\\
F_1(\mathbf Y)\\
\vdots\\
F_m(\mathbf Y)
\end{pmatrix}
{:}=
\begin{pmatrix}
1\\
\mathbf f(x, \mathbf y)
\end{pmatrix}.
\end{aligned}
\qquad (2)
$$

Then $(1)$ is equivalent to the following self-consistent system:

$$
\begin{cases}
\frac{d\mathbf Y}{dx} = \mathbf F(\mathbf Y)\\
\mathbf Y_0=\mathbf Y(x_0)
\end{cases}
\qquad (3)
$$

Let $h$ be the step size, then the 4th order Runge-Kutta recursive formula is:

$$
\boxed{
\begin{aligned}
&\mathbf{K}_1 = \mathbf{F}(\mathbf{Y}_n),\\
&\mathbf{K}_2 = \mathbf{F}\left(\mathbf{Y}_n + \frac{h}{2}\mathbf{K}_1\right),\\
&\mathbf{K}_3 = \mathbf{F}\left(\mathbf{Y}_n + \frac{h}{2}\mathbf{K}_2\right),\\
&\mathbf{K}_4 = \mathbf{F}\left(\mathbf{Y}_n + h\mathbf{K}_3\right),\\
\hline
&\mathbf{Y}_{n+1} = \mathbf{Y}_n + \frac{h}{6}\Bigl(\mathbf{K}_1 + 2\mathbf{K}_2 + 2\mathbf{K}_3 + \mathbf{K}_4\Bigr).\\
\end{aligned}
} \qquad (4)
$$

Compute $\mathbf{Y}_0\cdots,\mathbf{Y}_n$ using the recursive formula $(4)$, then you get $\{\mathbf{Y}_0,\mathbf{Y}_1,\cdots,\mathbf{Y}_n\}$: the point set of the graph of $(1)$ in $[x_0,x_0+nh]$ with $o(h^4)$ error.

Particularly, in `y'` mode, we have

$$
\mathbf Y=\begin{pmatrix}x\cr y\end{pmatrix},\mathbf F=\begin{pmatrix}1\cr E\end{pmatrix},
$$

and in `y''` mode, we have

$$
\mathbf Y=\begin{pmatrix}x\cr y\cr y'\end{pmatrix},\mathbf F=\begin{pmatrix}1\cr y'\cr E\end{pmatrix}.
$$

The specific implementation of the algorithm of the game as follows:

![image-20260713191738921](./README.assets/image-20260713191738921.png)
![image-20260713191815689](./README.assets/image-20260713191815689.png)

### Skill

Perhaps you'll say that it's too difficult to construct a curve you want in ODE mode, it seems that the useful functions and skills in the normal mode are useless stuff here. Because it looks like you have to compute the first order or even the second order derivative of the above functions, which is too complex and even almost impossible in some cases such as `abs()`. But don't worry, what if I tell you there is a way to approximate the function in normal mode using a simple ODE to avoid any derivative?

Next let's assume the function you want to cook is $f$, the following way can make the moving curve of your bullet **EXACTLY** the approximation to the curve of $f$ i.e. it is not like the normal mode, $f$ won't be shifted up or down. That is, you don't need to guess your $y_0$ coordinate any more if you use the skill I'll tell in ODE mode. So from the point, it seems that the ODE mode is easier than the normal mode.

Let $M$ be a big positive number, normally $M=233$ is enough. (If $M$ is too big, the final curve will oscillate so frequently that the length of the curve will easily exceed the upper bound.)

#### `y'` mode

$$
M(-y+f)
$$

That is: `M*(-y+(f))`

![image-20260629193239745](./README.assets/image-20260629193239745.png)

![image-20260629193547333](./README.assets/image-20260629193547333.png)

![image-20260629193728752](./README.assets/image-20260629193728752.png)

Here is the strict proof of the validity:

**Proposition:**

Let $f$ be a differentiable function where $f'$ is bounded. Consider the following IVP:

$$
\begin{cases}
\frac{dy}{dx}=M(-y+f(x))\\
y(x_0)=y_0
\end{cases}
$$

Prove that the solution will converge to $f(x)$ for any $x>x_0$ when $M\to+\infty$. Particularly, $y(x)=f(x)+\text{O}(\frac{1}{M})$.

**Proof:**

​	Since this is a first-order linear ODE

​	So the solution is:

$$
y(x)=e^{-M(x-x_0)}y_0+e^{-Mx}\int_{x_0}^xMf(t)e^{Mt}dt
$$

​	Then we have:

$$
\begin{aligned}
y(x)&=e^{-M(x-x_0)}y_0+e^{-Mx}\int_{x_0}^xMf(t)e^{Mt}dt\\
&=e^{-M(x-x_0)}y_0+e^{-Mx}\int_{x_0}^xf(t)de^{Mt}\\
&=e^{-M(x-x_0)}y_0+e^{-Mx}\left.f(t)e^{Mt}\right|_{x_0}^x-e^{-Mx}\int_{x_0}^xe^{Mt}df(t)\\
&=e^{-M(x-x_0)}y_0+f(x)e^{Mx}e^{-Mx}-f(x_0)e^{Mx_0}e^{-Mx}-e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt\\
&=f(x)+(y_0-f(x_0))e^{-M(x-x_0)}-e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt\\
&=f(x)+\text{O}(\frac{1}{e^M})-e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt\\
\end{aligned}
$$

​	For $x>x_0$, since:

$$
\lim\limits_{M\to+\infty}(y_0-f(x_0))e^{-M(x-x_0)}=0
$$

​	So

$$
y(x)=f(x)-e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt+\text{O}(\frac{1}{e^M})
$$

​	Since $f'$ is bounded

​	So Consider $\left|e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt\right|$, we have:

$$
\begin{aligned}
\left|e^{-Mx}\int_{x_0}^xe^{Mt}f'(t)dt\right|&\le e^{-Mx}\int_{x_0}^x|e^{Mx}||f'(x)|dx\\
&\le (\sup|f'|)e^{-Mx}\int_{x_0}^xe^{Mx}dx\\
&\le (\sup|f'|)e^{-Mx}\frac{e^{Mx}-e^{Mx_0}}{M}\\
&\le (\sup|f'|)e^{-Mx}\frac{e^{Mx}}{M}\\
&\le \frac{\sup|f'|}{M}\\
\end{aligned}
$$

​	Since

$$
\lim\limits_{M\to+\infty}\frac{\sup|f'|}{M}=0
$$

​	So

$$
y(x)=f(x)+\text{O}(\frac{1}{M})+\text{O}(\frac{1}{e^M})=f(x)+\text{O}(\frac{1}{M})
$$

​	So $y(x)=f(x)+\text{O}(\frac{1}{M})$

​	$\boxed{}$

#### `y''` mode

$$
-M^2y-2M\frac{dy}{dx}+M^2f
$$

That is: `-M^2*y-2*M*y'+M^2(f)`

![image-20260629194811115](./README.assets/image-20260629194811115.png)

![image-20260629195409230](./README.assets/image-20260629195409230.png)

![image-20260629195608406](./README.assets/image-20260629195608406.png)

Also, here is the strict proof of the validity:

**Proposition:**

Let $f$ be a differentiable function where $f'$ is bounded. Consider the following IVP:

$$
\begin{cases}
\frac{d^2y}{dx^2}=-M^2y-2M\frac{dy}{dx}+M^2f(x)\\
y(x_0)=y_0\\
\frac{dy}{dx}(x_0)=y_0'
\end{cases}
$$

Prove that the solution will converge to $f(x)$ for any $x>x_0$ when $M\to+\infty$. Particularly, $y(x)=f(x)+\text{O}(\frac{1}{M})$.

**Proof:**

​	Since $\frac{d^2y}{dx^2}=-M^2y-2M\frac{dy}{dx}+M^2f(x)$ is equivalent to

$$
\frac{d^2y}{dx^2}+2M\frac{dy}{dx}+M^2y=M^2f(x)\cdots(1)\\
$$

​	So the general solution of $(1)$ can be expressed as $y(x)=y_h(x)+y_p(x)$, where $y_p(x)$ is a particular solution and $y_h(x)$ is a solution of

$$
\frac{d^2y}{dx^2}+2M\frac{dy}{dx}+M^2y=0\cdots(2)\\
$$

​	Solve the characteristic equation of $(2)$: $r^2+2Mr+M^2=0$, we get $r_1=r_2=-M$

​	So we get the general solution of $(2)$:

$$
y_h(x)=(C_1+C_2(x-x_0))e^{-M(x-x_0)}
$$

​	By variation of parameters, we get:

$$
y_p(x)=\int_{x_0}^xM^2(x-t)e^{-M(x-t)}f(t)dt
$$

​	is a particular solution of $(1)$

​	So we have:

$$
y(x)=y_h(x)+y_p(x)=(C_1+C_2(x-x_0))e^{-M(x-x_0)}+\int_{x_0}^xM^2(x-t)e^{-M(x-t)}f(t)dt
$$

​	Since $y(x_0)=y_0,\frac{dy}{dx}(x_0)=y_0'$

​	We get:

$$
y(x)=(y_0+(y_0'+My_0)(x-x_0))e^{-M(x-x_0)}+\int_{x_0}^xM^2(x-t)e^{-M(x-t)}f(t)dt
$$

​	Is the solution of the IVP

​	Let $M^2(x-t)e^{-M(x-t)}=\frac{\partial U(x,t)}{\partial t}$

​	So $U(x,t)$ can be $(1+M(x-t))e^{-M(x-t)}$

​	So we have

$$
\begin{aligned}
y(x)&=(y_0+(y_0'+My_0)(x-x_0))e^{-M(x-x_0)}+\int_{x_0}^xM^2(x-t)e^{-M(x-t)}f(t)dt\\
&=(y_0+(y_0'+My_0)(x-x_0))e^{-M(x-x_0)}+\int_{x_0}^xf(t)\frac{\partial U}{\partial t}dt\\
&=(y_0+(y_0'+My_0)(x-x_0))e^{-M(x-x_0)}+\left.U(x,t)f(t)\right|_{x_0}^x-\int_{x_0}^xU(x,t)f'(t)dt\\
&=f(x)+(y_0-f(x_0)+(y_0'+M(y_0-f(x_0)))(x-x_0))e^{-M(x-x_0)}-\int_{x_0}^xU(x,t)f'(t)dt
\end{aligned}
$$

​	For $x>x_0$, since:

$$
\lim\limits_{M\to+\infty}(y_0-f(x_0)+(y_0'+M(y_0-f(x_0)))(x-x_0))e^{-M(x-x_0)}=0
$$

​	So

$$
y(x)=f(x)-\int_{x_0}^xU(x,t)f'(t)dt+\text{O}(\frac{M}{e^{M}})
$$

​	Since $f'$ is bounded

​	So consider $\left|\int_{x_0}^xU(x,t)f'(t)dt\right|$, we have:

$$
\begin{aligned}
\left|\int_{x_0}^xU(x,t)f'(t)dt\right|&\le\int_{x_0}^x|U(x,t)||f'(t)|dt\\
&\le (\sup|f'|)\int_{x_0}^x(1+M(x-t))e^{-M(x-t)}dt\\
&\le (\sup|f'|)\int_{0}^{x-x_0}(1+Mu)e^{-Mu}du\\
&\le(\sup|f'|)\int_{0}^{\infty}(1+Mu)e^{-Mu}du\\
&\le\frac{2\sup|f'|}{M}
\end{aligned}
$$

​	Since

$$
\lim\limits_{M\to+\infty}\frac{2\sup|f'|}{M}=0
$$

​	So

$$
y(x)=f(x)+\text{O}(\frac{1}{M})+\text{O}(\frac{M}{e^{M}})=f(x)+\text{O}(\frac{1}{M})
$$

​	So $y(x)=f(x)+\text{O}(\frac{1}{M})$

​	$\boxed{}$

#### If the developer unprecedentedly pushes a $y^{(n)}$ mode

We can construct it like this:

$$
\sum_{k=0}^{n} \binom{n}{k} M^{n-k}\frac{d^ky}{dx^k} = M^n f
$$

The proof is ~~trivial~~ similar to the above proofs. ~~The reader is invited to do it as an exercise.~~ In fact, ~~this was once revealed to me in a dream~~.