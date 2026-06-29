# Graphwar Guide

Chinese version is here: [中文版本在这里](https://www.bilibili.com/read/readlist/rl1068466)

This is a simple guide for Graphwar, which contains some stuff should be note and some useful skill. And I'm aslo a beginer of this game who started to play it only a few days ago (And my English is not very good). So there may be some errors in this guide. If you find any errors or have any suggestions, please let me know.

Anyway, Graphwar is an artillery game in which you must hit your enemies using mathematical functions. The trajectory of your shot is determined by the function you wrote, and your goal is to avoid the obstacles and your teammates and hit your enemies. The game takes place in a Cartesian Plane.

## Something you should know

There are 2 temms in a game, and any player should and only could in one team.

![image-20260625190748015](./README.assets/image-20260625190748015.png)

After everyone chicking the $\checkmark$ button and getting green, the game will start after 5 seconds. So click the button, don't let other guys wait you.

![image-20260625191533507](./README.assets/image-20260625191533507.png)

A kinda annoying point is that there are many reason, such as someone join or leave the room, will make you turn back to white. Now you have to click the button again.

Your teamates and you are always on the left side ($x<0$). Your enemies are always on the right side ($x>0$). Or you can check someone's orientation to judge which side they are on: facing right is your teamates, facing left is your enemies. And you can kill your teamates in this game. So PLZ! don't kill your teamates! You know what? I've been killed because of this for many times.

![image-20260625191842364](./README.assets/image-20260625191842364.png)

The character you control will become red when it's your turn. Now you can input any function expression in the input box. Chick "Fire" Button or press "Enter" to fire. The bullet will move according to the corresponding curve of your function from left to right. And you can only fire once per turn.

![image-20260625195737269](./README.assets/image-20260625195737269.png)

In the normal mode (`y` mode) (I'll talk about ODE mode (`y'` and `y''` mode) later), assume the function you input is $f$, and your coordinates are $(x_0,y_0)$, then the real moving curve of your bullet is:

$$
f_{\text{real}}(x)=f(x)+(y_0-f(x_0))
$$

![https://www.graphwar.com/ss2Graphwar.png](./README.assets/ss2Graphwar.png)

That is, instead of setting the posision of launcher be the origin of your function curve, the game will ensure that the function curve is always pass through the launcher's position by shifting the curve up or down. That's why you will get a very steep curve if you use $x^2$ or $x^3$. Trying use $(x-x_0)^2$ or $(x-x_0)^3$ will make it kinda normal.

![image-20260625195704528](./README.assets/image-20260625195704528.png)

Clearly, $f_{\text{real}}$ is defined only for $x\ge x_0$. So the curve doesn't exist for $x<x_0$.

Since you can only input a function expression in the input box, which means for any input $x$ there is at most one corresponding $f(x)$. It's impossible that an input correspond to multiple values. So it's clearly that the bullet can not move to the left, that is, the $-x$ direction.

The black fucking circles are obstacles. If your bullet hits them, it will explode and make a small hole, and the bullet will disappear.

![image-20260625201339945](./README.assets/image-20260625201339945.png)

If your bullet hit your teamates or your enemies, they will be killed but your bullet will continue to move.

![image-20260625201258139](./README.assets/image-20260625201258139.png)

The battlefield is in a $[-25,25]\times[-15,15]$ rectangular area. If the bullet hits the boundary, it will disappear.

![image-20260625202013194](./README.assets/image-20260625202013194.png)

![image-20260625202054414](./README.assets/image-20260625202054414.png)

If the function is undefined at the point that bullet passes through, like $y=\sqrt{x}$ and the bullet passes through some $x<0$, the bullet will disappear.

![image-20260625200104823](./README.assets/image-20260625200104823.png)

If the function value is too large, or more fundamentally and more generally, the function value is `NaN`, the bullet will disappear.

![image-20260625201945265](./README.assets/image-20260625201945265.png)

![image-20260629223755887](./README.assets/image-20260629223755887.png)

The distance your bullet can move, that is , the length of your function curve, is finite. If the distance is too long, the bullet will disappear. That's why something like $sin(100x)$ usually doesn't work.

![image-20260625200328666](./README.assets/image-20260625200328666.png)

The map of a game is random. If you get a dumbass map, type `-skip` in the chat box and ask other guys to type it. The game will change a new random map if everyone types `-skip`.

## Syntax

### Variables

- `x`:  $x$
- `y`:  $y$
- `y'`: $y'$

### Binary Operators

- `+`: $+$
- `-`: $-$
- `*`: $\times$
- `/`: $\div$
- `^`: power, such as `x^2` is $x^2$

### Functions

- `sqrt()`: square root, such as `sqrt(x)` is $\sqrt{x}$
- `ln()`: natural logarithm, such as `ln(x)` is $ln(x)$
- `log()`: common logarithm, such as `log(x)` is $log_{10}(x)$
- `abs()`: absolute value, such as `abs(x)` is $|x|$
- `sin()` or `sen()`: sine, such as `sin(x)` is $sin(x)$
- `cos()`: cosine, such as `cos(x)` is $cos(x)$
- `tan()` or `tg()`: tangent, such as `tan(x)` is $tan(x)$
- `exp()`: exponential, such as `exp(x)` is $e^x$

### Constants

- `pi`: $\pi$
- `e`: $e$

### Brackets

- `(` and `)`: brackets. Using brackets more frequently to avoid incorrect order of operations.

## Useful functions

Some of following contents are from [Graphwar Tutorial Sant Albert '12](https://www.youtube.com/watch?v=E_MmkxTO5kg) and [graphwar meta that i use (EN)](https://www.youtube.com/watch?v=EHuQe7SKwkA)

And you can combine them by summing and multiplying. Abstractly, summing means "OR", and multiplying means "AND".

### Step (Sigmoid) function:

$$
\frac{k}{1+e^{-m(x-a)}}
$$

That is: `((k)/((1+exp(-m*(x-(a))))))`

where $m\in\mathbb{R}_+$ is a big positive number.

This function can approximate the following function:

$$
\begin{cases}
0, & x<a\\
k, & x\ge a
\end{cases}
$$

That is, the function is 0 when $x<a$, and $k$ when $x\ge a$.

Particularly, let $k$ be a negative number to let the bullet move down.

![image-20260625204651483](./README.assets/image-20260625204651483.png)

![1782391863123334113](./README.assets/1782391863123334113.png)

It's a very useful and simple function used to move and dodge the objects.

### Spike function:

$$
\frac{h}{1+m(x-a)^2}
$$

That is: `((h)/(1+m*(x-a)^2))`

where $m\in\mathbb{R}_+$ is a big positive number.

The function creates a spike at $x=a$ with height $h$.

![image-20260629231551089](./README.assets/image-20260629231551089.png)

![image-20260629231124761](./README.assets/image-20260629231124761.png)

Compared to use something like $\frac{h}{1+m(x-a)^2}+\frac{-h}{1+m(x-a-0.1)^2}$, the function is simpler and easier to use.

### Double absolute function:

$$
\frac{k}{2}(|x-a| - |x-b|)
$$

That is: `(0.5*(k)(abs(x-(a))-abs(x-(b))))`

where $a<b$

The function eqvalents to the following function:

$$
\begin{cases}
-\frac{k}{2}|a-b|, & x<a\\
k(x-a)-\frac{k}{2}|a-b|, & x\in[a,b]\\
\frac{k}{2}|a-b|, & x>b
\end{cases}
$$

which means the function is $-k/2|a-b|$ when $x<a$, a linear function will slope $k$ when $x\in[a,b]$, and $k/2|a-b|$ when $x>b$

![image-20260625210514390](./README.assets/image-20260625210514390.png)

![image-20260625210611315](./README.assets/image-20260625210611315.png)

**There are so many cheaters like to use the combination of the function. If you see someone send a whole string of this stuff with verey precise coefficients, it's very likely to be a cheater.**

### Sine and cosine function:

Although $\sin$ and $\cos$ are very basic, they are very useful to sweep a wide area if you give them a big angular velocity.

![image-20260625211651927](./README.assets/image-20260625211651927.png)

### General setp function:

$$
\frac{f(x)}{1+e^{-m(x-a)}}
$$

That is: `((f)/((1+exp(-m*(x-(a))))))`

where $m\in\mathbb{R}_+$ is a big positive number.

This function can approximate the following function:

$$
\begin{cases}
0, & x<a\\
f(x), & x\ge a
\end{cases}
$$

That is, the function is 0 when $x<a$, and $f(x)$ when $x\ge a$.

![image-20260625212035083](./README.assets/image-20260625212035083.png)

![image-20260625211834599](./README.assets/image-20260625211834599.png)

This function can be used to make any function $f(x)$ works only after pass through a certain point. But if you want to let it works only for a interval, General setp function can't help you. Now you need the flowing stuff.

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

(And btw I constructed another version of $\mathbf 1_{(a,b)}(x)$ by combining power function like: $\mathbf1_{(a,b)}(x)=\left(1+\left(\frac{x-\frac{a+b}{2}}{\frac{a-b}{2}}\right)^{2m}\right)^{-1}$. But it's not useful because you have to replace $a$ and $b$ twice and $m$ must be a natural number.)

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

If you played with me in the game, you will see that I usually use this function to construct any function I want. It's very useful for me, which is one of the best function in my mind.

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

### Periodic general setp function:

$$
\frac{f(x)}{1+e^{-m\left(\sin\left(\frac{\pi(x-p)}{T}\right)\sin\left(-\frac{\pi x}{T}\right)\right)}}
$$

That is: `((f)/(1+exp(-m*(sin(pi*(x-p)/(T))sin(-(pi*x)/(T))))))`

where $m\in\mathbb{R}_+$ is a big positive number, $T$ is the period, and $p\in(0,T)$.

In a period $[kT,kT+T)$, the function will be $f$ if $x\in[kT,kT+p)$, otherwise it will be $0$.

![image-20260630001433312](./README.assets/image-20260630001433312.png)

![image-20260630001359082](./README.assets/image-20260630001359082.png)

Based on this function, you can do many interesting things.

# Unfinished