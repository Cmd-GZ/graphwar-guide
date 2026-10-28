# Graphwar `floor` construction

The article focus on how to construct approximations of $\text{floor}$ in Graphwar. The reason of just consider approximations instead of $0$ error is that there are some breakpoints of $\text{floor}$ that Graphwar can't handle on $x\in\mathbb{Z}$.

In fact, it contains 2 subquestions:

1. How to construct a $\text{floor}$ approximation only with operators in Graphwar without consider if it can run in Graphwar

2. How to further process of the function in **1** (if it is needed) such that the final function can run in Graphwar normally

## Content

- [Graphwar `floor` construction](#graphwar-floor-construction)
  - [Content](#content)
  - [Prerequisites](#prerequisites)
  - [Trivial solutions](#trivial-solutions)
  - [The answers from Finnley](#the-answers-from-finnley)
  - [Arctangent](#arctangent)
    - [Approximation](#approximation)
      - [1st try](#1st-try)
      - [2nd try](#2nd-try)
      - [3rd try](#3rd-try)
    - [Eliminate breakpoints](#eliminate-breakpoints)
      - [Continuous cotangent](#continuous-cotangent)
  - [The End?](#the-end)
  - [DLC](#dlc)
    - [ODE Mode](#ode-mode)
      - [`y'` mode](#y-mode)
      - [`y''` mode](#y-mode-1)
    - [Fix points](#fix-points)
    - [Rademacher function series](#rademacher-function-series)

## Prerequisites

Graphwar allows and only allows the following operators:

$$
\begin{cases}
\square+\square\\
\square-\square\\
\square\times\square\\
\frac\square\square\\
\square^\square\\
\sqrt\square\\
\ln\square\\
\log\square\\
|\square|\\
\sin\square\\
\cos\square\\
\tan\square\\
\exp\square
\end{cases}
$$

Their syntaxes are:

```
()+()
()-()
()*()
()/()
()^()
sqrt()
ln()
log()
abs()
sin() or sen()
cos()
tan() or tg()
exp()
```

## Trivial solutions

Notice that Fourier series can be used to approximate $\text{floor}$:

$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{1}{\pi}\sum_{n=1}^N\frac{\sin(2n\pi x)}{n}
$$

Or, we can approximate it with the sum of several sigmoid (step) functions. Since the $x$-axis range of Graphwar is $[-25,25)$, we get the following construction:

$$
\lfloor x\rfloor\approx-26+\sum_{n=-25}^{25}s(x-n)
$$

where

$$
s(x)=\frac{1}{1+e^{-Mx}}
$$

and $M$ is a large number

Both of them have huge problems although they can also solve the 2nd subquestion naturally.

The Fourier approximation converges so slowly that the precision is still very low after accumulating dozens of terms: the  maximum error at distance $\gt 0.05$ from the integers is about $0.02$ if $N=60$. Moreover, regardless of $N$ as big as, the Gibbs phenomenon exists near integer points, that is, the function will overshoot and oscillate, whose error can' t be eliminated.

Indeed the sum of Sigmoid functions construction can yield a high precision result, but its expression is too long (and so does Fourier form) to be used in actual situation.

## The answers from Finnley

After initial attempts failed, I sent this question to the official discord server of Graphwar to ask for help. One of the member of the server, Finnley gave two pretty nice answers:

$$
\lfloor x\rfloor\approx x-\left(\frac{\left(\sin\left(\frac{\pi x}{2}\right)\right)^{2}}{1+e^{-500\sin\left(\pi x\right)}}+\frac{\left(\sin\left(\frac{\pi\left(x-1\right)}{2}\right)\right)^{2}}{1+e^{500\sin\left(\pi x\right)}}+0.1\sin\left(2\pi x\right)\left(1+0.077\sin\left(\frac{3\pi x}{\sin\left(\pi x\right)}\right)\right)\right)
$$

and:

$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{1}{2}\right)}{2}\right)}}}{3+e^{-500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{3}{2}\right)}{2}\right)}}}{3+e^{500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}
$$

They are shorter then trivial solutions while maintaining high precision, where the corresponding maximum error at distance $\gt 0.05$ and $\gt 0.2$ from the integers is about $0.02$ and $0.0005$ and also solves the 2nd subquestion naturally.

## Arctangent

My further attempts took a different path, consider $\arctan(\cot(x))$, where $\cot(x)=\frac{1}{\tan(x)}$, observe that it's a periodic function that satisfies the following relation:

$$
\arctan(\cot(x))=-x+\frac{\pi}{2}+k\pi,x\in(k\pi,(k+1)\pi),k\in\mathbb{Z}
$$

After simple transformation, we get:

$$
\lfloor x \rfloor=x-\frac{1}{2}+\frac{1}{\pi}\arctan\left(\cot\left(\pi x\right)\right),x\notin\mathbb{Z}
$$

### Approximation

It seems that the 1st subquestion have been solved by the expression, but it's a pity that there are no inverse trigonometric functions in Graphwar. So we have to construct the approximation of $\arctan$.

#### 1st try

My first thought was the the identity: $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$

Approximating $\arcsin$ with $\frac{\pi}{2}x$, we get $\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{1+x^2}}$

Adding an undetermined constant term $c$ to the denominator of $\frac{x}{\sqrt{1+x^2}}$ gives $\frac{x}{\sqrt{1+x^2}+c}$

Let $f(x)=\frac{x}{\sqrt{1+x^2}+c}$, considering that $\arctan'(0)=1$, let $\frac{\pi}{2}f'(0)=1$, we get $c=\frac{\pi}{2}-1$

So

$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{1+x^2}+\frac{\pi}{2}-1}
$$

The maximum error is $0.013$

So

$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{1}{2}\frac{\cot\left(\pi x\right)}{\sqrt{1+\cot\left(\pi x\right)^{2}}+\frac{\pi}{2}-1}
$$

The maximum error at points far from integers is about $0.004$

#### 2nd try

When I lost in the identity $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$ and try to get a further $\arcsin(x)$ approximation,  Finnley found a better $\arctan(x)$ approximation:

Let $f(x)=\frac{ax}{b+\sqrt{c+x^2}}$

Notice that $\arctan'(0)=1,\arctan(1)=\frac{\pi}{4},\lim\limits_{x\to\infty}\arctan(x)=\frac{\pi}{2}$

Solve the equation system obtained by replace $\arctan(x)$ with $f(x)$, we get:

$$
\begin{cases}
a=\frac{\pi}{2}\\
b=\frac{12-\pi^2}{4(4-\pi)}\approx0.620\\
c=\frac{(6-\pi)^2(2-\pi)^2}{16(4-\pi)^2}\approx0.903
\end{cases}
$$

So we have:

$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{0.903+x^2}+0.62}
$$

The maximum error is $0.002$

So

$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{1}{2}\frac{\cot\left(\pi x\right)}{\sqrt{0.903+\cot\left(\pi x\right)^{2}}+0.62}
$$

The maximum error at points far from integers is about $0.0007$

#### 3rd try

Finally I found the approximation in [Inigo Quilez](https://iquilezles.org/)'s website:

$$
\arctan(x)\approx\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}
$$

The maximum error is $0.001525$

Finally we get:

$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{\pi\cot\left(\pi x\right)}{4+\sqrt{34+(2\pi\cot\left(\pi x\right))^{2}}}
$$

The maximum error at points far from integers is about $0.000485$

I chose the 3rd approximation as the final result

### Eliminate breakpoints

The 1st subquestion has been solved, but the 2nd one haven't. The above expression will be terminated when it passes through the integers because their slope is too high to be handled by Graphwar or any discontinuous point is used as the sampling point.

#### Continuous cotangent

Consider the following identity:

$$
\cot(x)=\frac{\sin(2x)}{1-\cos(2x)}
$$

Add a very small positive number to the denominator of it, we get:

$$
\cot(x)\approx\frac{\sin(2x)}{1-\cos(2x)+\varepsilon}
$$

Replace $\cot(x)$ with it in the above formula in **3rd try**, by controlling the size of $\varepsilon$, at the cost of some precision, we can control the function's slope while eliminating breakpoints to ensure that the function does not terminate in Graphwar.

$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon}}{4+\sqrt{34+(2\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon})^{2}}}
$$

In Graphwar II, because of the algorithm improvements, $\varepsilon$ can be as small as about $10^{-11}$; at $10^{-12}$ it breaks down (that is a floating-point limit, not an algorithmic one). But in Graphwar I can't do that because of the algorithm defects. The smallest value of $\varepsilon$ is about $0.0007$ if you just input a $\text{floor}$ function. But if you want to compose it with any other function, the value should be analyzed specifically.

Finally it's the expression of $\text{floor}$ that can be used in Graphwar：`x-0.5+(pisin(2pix)/(1-cos(2pix)+0.0007))/(4+sqrt(34+(2*pisin(2pix)/(1-cos(2pix)+0.0007))^2))`

## The End?

It's hard to control the value of $\varepsilon$ to ensure the above formula maintains high precision and does not terminate early sometime if it's used to combine with other functions in Graphwar I. Furthermore, $x$ appears so many times that if you want to construct a composite function like $\lfloor\sin(x)\rfloor$, you have to replace $x$ for $5$ times. Is there a new way to get a better $\text{floor}$ approximation?

Consider the following relationship between $\arcsin$ and $\arctan$:

$$
\arcsin(x)=\arctan\left(\frac{x}{\sqrt{1-x^2}}\right)
$$

Replace $x$ with $\frac{x}{\sqrt{1-x^2}}$ in the $\arctan$ approximation in [3rd try](#3rd-try), we get a $\arcsin$ approximation with $0.001525$ maximum error:

$$
\arcsin(x)\approx\frac{\pi^2\frac{x}{\sqrt{1-x^2}}}{4+\sqrt{34+(2\pi \frac{x}{\sqrt{1-x^2}})^2}}
$$

Similarly, the rest inverse trigonometric function approximations can be obtained in the same way.

After getting the construction, just for fun, I tried constructing an interesting expression with it in Graphwar:

$$
\frac{\pi^2\frac{\sin(x)}{\sqrt{1-\sin^2(x)}}}{4+\sqrt{34+(2\pi \frac{\sin(x)}{\sqrt{1-\sin^2(x)}})^2}}\approx\arcsin(\sin(x))=
\begin{cases}
x-2k\pi,&x\in[-\frac{\pi}{2}+2k\pi,\frac{\pi}{2}+2k\pi]\\
-x+\pi+2k\pi,&x\in[\frac{\pi}{2}+2k\pi,\frac{3\pi}{2}+2k\pi]
\end{cases}\\
k\in\mathbb{Z}
$$

Finnley realized immediately that it can also be used to make another $\text{floor}$ approximation

Consider the similar form and let it be $\phi(x)$:

$$
\phi(x)=\frac{\pi^2\frac{\cos(x)}{\sqrt{1-\cos^2(x)}}}{4+\sqrt{34+(2\pi \frac{\cos(x)}{\sqrt{1-\cos^2(x)}})^2}}\approx\arcsin(\cos(x))=
\begin{cases}
x+\frac{\pi}{2}-2k\pi,&x\in[-\pi+2k\pi,2k\pi]\\
-x+\frac{\pi}{2}+2k\pi,&x\in[2k\pi,\pi+2k\pi]
\end{cases}\\
k\in\mathbb{Z}
$$

Observe that if $x\ne k\pi$, we have:

$$
\arcsin(\cos(x))\text{sgn}(\sin(x))=
\begin{cases}
-x-\frac{\pi}{2}+2k\pi,&x\in(-\pi+2k\pi,2k\pi)\\
-x+\frac{\pi}{2}+2k\pi,&x\in(2k\pi,\pi+2k\pi)
\end{cases}
=-x+\frac{\pi}{2}+k\pi,\quad x\in(k\pi,(k+1)\pi)
=\arctan(\cot(x))
$$

where

$$
\text{sgn}(x)=
\begin{cases}-1,&x\lt 0\\
0,&x=0\\
1,&x\gt 0
\end{cases}
$$


By [Arctangent](#arctangent), we get

$$
\lfloor x \rfloor=x-\frac{1}{2}+\frac{1}{\pi}\arcsin(\cos(\pi x))\text{sgn}(\sin(\pi x)),x\notin\mathbb{Z}
$$

A continuous approximation of $\text{sgn}(x)$ which can also eliminate breakpoints is:

$$
\text{sgn}(x)\approx\tanh\left(\frac{M}{2}x\right)=1-\frac{2}{1+e^{Mx}}
$$

where $M$ is a large number

Combine them, we get:

$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{1}{\pi}\phi(\pi x)\tanh\left(\frac{M}{2}\sin(\pi x)\right)=x-0.5+\frac{\frac{\pi\cos\left(\pi x\right)}{\sqrt{1-\cos^{2}\left(\pi x\right)}}\left(1-\frac{2}{1+e^{M\sin\left(\pi x\right)}}\right)}{4+\sqrt{34+\left(\frac{2\pi\cos\left(\pi x\right)}{\sqrt{1-\cos^{2}\left(\pi x\right)}}\right)^{2}}}
$$

Now, without controlling $\varepsilon$ and loss of precision, we get a stable approximation that can be combine with almost any function and works normally in Graphwar I

However, it's longer than the approximation in [3rd try](#3rd-try), and $x$ appears $6$ times, a higher frequency.

Notice that

$$
\cot^2(\pi x)=\frac{1}{\tan^2(\pi x)}=\left(\frac{\cos(\pi x)}{\sqrt{1-\cos^2(\pi x)}}\right)^2
$$

We get

$$
x-0.5+\frac{\frac{\pi\cos\left(\pi x\right)}{\sqrt{1-\cos^2\left(\pi x\right)}}\left(1-\frac{2}{1+e^{M\sin\left(\pi x\right)}}\right)}{4+\sqrt{34+\left(\frac{2\pi}{\tan(\pi x)}\right)^{2}}}
$$

A natural idea is: What about $\frac{\cos\left(\pi x\right)}{\sqrt{1-\cos^2\left(\pi x\right)}}$?

It's clearly that:

$$
\sin(x)=
\begin{cases}
\sqrt{1-\cos^2(x)},&\sin(x)\ge0\\
-\sqrt{1-\cos^2(x)},&\sin(x)\le0
\end{cases}
$$

So we have

$$
\sin(x)=\text{sgn}(\sin(x))\sqrt{1-\cos^2(x)}
$$

So

$$
\cot(\pi x)=\frac{1}{\tan(\pi x)}=\frac{\cos(\pi x)}{{\sin(\pi x)}}=\frac{\cos(\pi x)}{\text{sgn}(\sin(\pi x))\sqrt{1-\cos^2(\pi x)}}
$$

That is:

$$
\frac{\cos\left(\pi x\right)}{\sqrt{1-\cos^2\left(\pi x\right)}}=\frac{\text{sgn}(\sin(\pi x))}{\tan(\pi x)}\approx\frac{1-\frac{2}{1+e^{M\sin\left(\pi x\right)}}}{\tan(\pi x)}
$$

Finally we get:

$$
\lfloor x\rfloor\approx x-0.5+\frac{\pi\left(1-\frac{2}{1+e^{M\sin\left(\pi x\right)}}\right)^{2}}{\tan(\pi x)\left(4+\sqrt{34+\left(\frac{2\pi}{\tan(\pi x)}\right)^{2}}\right)}
$$

which is much shorter than the [3rd try](#3rd-try) form and there are only $4$ $x$'s in the formula smaller than the original $5$.

---

Similarly, here is the expression that can be used in Graphwar: `x-0.5+(pi(1-2/(1+e^(Msin(pix))))^2)/(tan(pix)(4+sqrt(34+(2pi/tan(pix))^2)))`

## DLC

### ODE Mode

#### `y'` mode

Consider the Dirac function $\delta(x)$ with the following property:

$$
\int\delta(x)\psi(x)dx=\psi(0)\\
\delta(ax)=\frac{1}{|a|}\delta(x)\\
\delta(\psi(x))=\sum_{\psi(r)=0}\frac{\delta(x-r)}{|\psi'(r)|}
$$

Where $\psi(x)$ is an arbitrary function.

Let $\psi(x)=\sin(\pi x)$. Its roots are $r=k\in\mathbb{Z}$, with $\psi'(k)=\pi\cos(k\pi)=(-1)^k\pi$, so $|\psi'(k)|=\pi$.

So:

$$
\delta(\sin(\pi x))=\sum_{k}\frac{\delta(x-k)}{|\psi'(k)|}=\frac{1}{\pi}\sum_{k}\delta(x-k)
\qquad\Longrightarrow\qquad
\pi\ \delta(\sin(\pi x))=\sum_{k}\delta(x-k)
$$

Consider the IVP (the bullet only ever moves to the right, so we only need $x\ge x_0$):

$$
\begin{cases}
\dfrac{dy}{dx}=\pi\delta(\sin(\pi x))=\sum_{k}\delta(x-k)\\
y(x_0)=y_0
\end{cases}
$$

Integrating from $x_0$, and using $\int^{x}\delta(t-k)\ dt=H(x-k)$ with $H$ the Heaviside step ($x_0\notin\mathbb{Z}$):

$$
y(x)=y_0+\int_{x_0}^{x}\sum_{k}\delta(t-k)\ dt=y_0+|\lbrace k\in\mathbb{Z}: x_0\lt k\le x\rbrace|
$$

The number of integers in $(x_0,x]$ is exactly $\lfloor x\rfloor-\lfloor x_0\rfloor$, hence

$$
y(x)=\lfloor x\rfloor+y_0-\lfloor x_0\rfloor
$$


Which means that if we can find a Dirac function approximation, we can easily get the floor function approximation in `y'` mode.

It's easy to get them:

**Gaussian**

$$
\delta_a(x)=\frac{1}{a\sqrt{\pi}}e^{-(x/a)^2}
$$

$$
y'=\frac{\sqrt{\pi}}{a}e^{-\left(\frac{\sin\pi x}{a}\right)^2}
$$

```text
sqrt(pi)/a*e^(-(sin(pix)/a)^2)
```

**Lorentzian (Cauchy)**

$$
\delta_a(x)=\frac{a}{\pi(x^2+a^2)}
$$

$$
y'=\frac{a}{\sin^2\pi x+a^2}
$$

```text
a/(sin(pix)^2+a^2)
```

**$\text{sech}^2$**

$$
\delta_a(x)=\frac{1}{2a}\text{sech}^2\frac{x}{a}
$$

$$
y'=\frac{\pi}{2a}\text{sech}^2\left(\frac{\sin\pi x}{a}\right)
$$

```text
2*pi/a*e^(-2abs(sin(pix)/a))/(1+e^(-2abs(sin(pix)/a)))^2
```

**Rectangular (box)**

$$
\delta_a(x)=\frac{1}{2a}\mathbf{1}_{\lbrace|x|\lt a\rbrace}
$$

$$
y'=\frac{1}{2a}\cdot\frac{1}{1+e^{-M(\cos 2\pi x-\cos 2\pi a)}}
$$

```text
1/(2*a)/(1+e^(-M*(cos(2pix)-cos(2*pi*a))))
```

#### `y''` mode

For this mode, I'll just consider the trival way, that is, the derivative of the Dirac function $\delta'(x)$, just because it's the easiest way.

Similarly, for $\delta'(x)$, we have the following property:

$$
\int\delta'(x)\psi(x)dx=-\psi'(0)\\
\delta'(ax)=\frac{1}{a|a|}\delta'(x)\\
\delta'(\psi(x))=\sum_{\psi(r)=0}\frac{\delta'(x-r)}{\psi'(r)|\psi'(r)|}
$$

Where $\psi(x)$ is an arbitrary function.

Consider $\pi^2\cos(\pi x)\delta'(\sin\pi x)$. With $\psi(x)=\sin(\pi x)$ again and $\psi'(k)=(-1)^k\pi$, the composition rule gives

$$
\delta'(\sin\pi x)=\sum_{k}\frac{\delta'(x-k)}{\psi'(k)|\psi'(k)|}=\sum_{k}\frac{\delta'(x-k)}{(-1)^k\pi^2}
$$

Also $\cos(\pi x)\delta'(x-k)=\cos(\pi k)\delta'(x-k)=(-1)^k\delta'(x-k)$: by the product rule the extra term is $-\cos'(\pi k)\delta(x-k)=0$, since $\cos'(\pi k)=-\pi\sin(\pi k)=0$. Therefore

$$
\pi^2\cos(\pi x)\delta'(\sin\pi x)=\pi^2\sum_{k}\frac{(-1)^k\delta'(x-k)}{(-1)^k\pi^2}=\sum_{k}\delta'(x-k)
$$

Consider the IVP:

$$
\begin{cases}
\frac{d^2y}{dx^2}=\pi^2\cos(\pi x)\delta'(\sin\pi x)\\
y(x_0)=y_0\\
y'(x_0)=0
\end{cases}
$$

For simple, we assume $x_0\notin\mathbb{Z}$, we have:

$$
\frac{dy}{dx}=y'(x_0)+\int_{x_0}^{x}\pi^2\cos(\pi t)\delta'(\sin\pi t)\ dt
=0+\sum_{k}\delta(x-k)=\pi\delta(\sin(\pi x))
$$

Here $\int\delta'(t-k)\ dt=\delta(t-k)$, and $\delta(x_0-k)=0$ because $x_0\notin\mathbb{Z}$.

That means the IVP reduces to the `y'` mode IVP, which has been proved above.

Notice that the IVP above takes $y'(x_0)=0$. Generally, let $\theta$ be the launch angle set by player, then:

$$
y'(x)=\tan\theta+\sum_{k}\delta(x-k)
$$

so the whole staircase comes out tilted by $\tan\theta$.

Differentiating any of the kernels above gives a usable $\delta'$; all of them satisfy $\int\delta'=0$, $\int x\ \delta'(x)\ dx=-1$ and scale as $a^{-2}$.

**Gaussian**

$$
\delta'_a(x)=-\frac{2u\ e^{-u^2}}{\sqrt{\pi}a^2},\qquad u=\frac{x}{a}
$$

$$
y''-\frac{2\pi^2\cos(\pi x)\sin(\pi x)}{\sqrt{\pi}\ a^3}e^{-\left(\frac{\sin\pi x}{a}\right)^2}
$$

```text
-2sin(pix)*cos(pix)*pi^2/(sqrt(pi)*a^3)*e^(-(sin(pix)/a)^2)
```

**Lorentzian**

$$
\delta'_a(x)=-\frac{2u}{\pi a^2(1+u^2)^2},\qquad u=\frac{x}{a}
$$

$$
y''=-\frac{2\pi\cos(\pi x)\sin(\pi x)}{a^3\left(1+\left(\frac{\sin\pi x}{a}\right)^2\right)^2}
$$

```text
-2pi*sin(pix)*cos(pix)/(a^3*(1+(sin(pix)/a)^2)^2)
```

**$\text{sech}^2$**

$$
\delta'_a(x)=-\frac{\text{sech}^2u\ \tanh u}{a^2},\qquad u=\frac{x}{a}
$$

$$
y''=-\frac{\pi^2\cos(\pi x)}{a^2}\ \text{sech}^2\left(\frac{\sin\pi x}{a}\right)\tanh\left(\frac{\sin\pi x}{a}\right)
$$

```text
-pi^2*cos(pix)*(4/(e^(sin(pix)/a)+e^(-sin(pix)/a))^2)*(1-2/(1+e^(2sin(pix)/a)))/a^2
```
### Fix points

Let $f(u)=u-\sin u$ and $M\in\mathbb{N}$ be a big number, consider:

$$
g_M(x)=\frac{1}{2\pi}f^M\left(2\pi\left(x-\frac{1}{2}\right)\right)
$$

We have:

$$
\lim\limits_{M\to\infty}g_M(x)=
\begin{cases}
\lfloor x\rfloor, & x\notin \mathbb{Z}\\
\lfloor x\rfloor-\frac{1}{2}, & x\in \mathbb{Z}
\end{cases}
$$

The half-integer value at integers is because the initial point $2\pi\left(x-\frac12\right)=(2x-1)\pi$ is exactly one of the repelling fixed points $(2m-1)\pi$ of $f$, where the iteration does not move.

But the length of the expression and the slow convergence near the integers make it a poor choice for Graphwar:

- $M=1$: `((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))/(2*pi))`
- $M=2$: `(((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))-sin((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))))/(2*pi))`
- $M=3$: `((((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))-sin((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))))-sin(((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))-sin((2*pi*(x-0.5)-sin(2*pi*(x-0.5)))))))/(2*pi))`
- ...

Away from the integers the iteration converges cubically, $d_{k+1}\approx d_k^3/6$; near an integer it first has to escape from the repelling fixed point, which takes $O\left(\log_2\frac{1}{\delta}\right)$ steps, so a tolerance $\delta$ around the integers needs roughly $M\approx\log_2(1/\delta)+2$.

### Rademacher function series

This construction is essentially a variant of the second [Trivial solution](#trivial-solutions).

Let $r_k(x)=\text{sgn}\sin\frac{\pi x}{2^{k}}$ be the $k$-th Rademacher function, and

$$
t_k(x)=\frac{2^{k}}{1+e^{M\cdot2^{k}\cdot\sin\left(\frac{\pi x}{2^{k}}\right)}}
$$

be an approximation of $2^k\frac{1-r_k(x)}{2}$, where $M$ is a big number. Then, for $u\notin\mathbb{Z}$,

$$
\lfloor u\rfloor=\frac{2^{n+1}-1}{2}-\frac{1}{2}\sum_{k=0}^{n}2^{k}r_k(u),\qquad u\in[0,2^{n+1})
$$

so

$$
\lfloor x\rfloor\approx\sum_{k=0}^{n}t_k(x),\qquad x\in[0,2^{n+1})
$$

For Graphwar, since the range of $x$ is $[-25,25)$, we can use $n=5$ and get

$$
\lfloor x\rfloor\approx-32+\sum_{k=0}^{n}t_k(x+32),\qquad x\in[-32,32)
$$

Notice that the coefficient in the exponent of $t_k(x)$ is $M2^{k}$. That is because at the flip points $m\in2^{k}\mathbb{Z}$ of $t_k$, we have:

$$
t_k'(m)=\frac{2^{k}\pi M}{4}
$$

Assume the sharp increase of $t_0(x)$ around $m$ has length $L$. Since $t_k$ is centrally symmetric about $m$ on $\left(m-2^{k-1},\ m+2^{k-1}\right)$, its height is $2^{k}$ times larger and its speed is $2^{k}$ times larger as well, so the length becomes

$$
L\frac{\text{height}}{\text{speed}}=L\frac{2^{k}}{2^{k}}=L
$$

Which means that the $2^{k}$ in the exponent of $t_k(x)$ keeps the length of the segment unchanged, so that the unexpected spikes are avoided.
