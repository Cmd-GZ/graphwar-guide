# Inverse Trigonometric Functions

Unfortunately, there are no inverse trigonometric functions in normal mode in Graphwar. But we can construct approximations of them.

## Identities

There are several identities for the inverse trigonometric functions:

$$
\arcsin x + \arccos x = \frac{\pi}{2}\\
\arctan x + \text{arccot} x = \frac{\pi}{2}\\
\text{arcsec} x = \arccos\left(\frac{1}{x}\right),|x|\ge1\\
\text{arccsc} x = \arcsin\left(\frac{1}{x}\right),|x|\ge1\\
\arcsin(x)=\arctan\left(\frac{x}{\sqrt{1-x^2}}\right),|x|\lt 1\\
\arctan(x)=\arcsin\left(\frac{x}{\sqrt{1+x^2}}\right)
$$

So clearly, if you have a certain inverse trigonometric function approximation, you can get the other inverse trigonometric function approximations by using these identities.

## Arctangent

### Analytical Way

Consider the odd function $f(x)=\frac{ax}{b+\sqrt{c+x^2}}$ with the following properties:

$$
f'(0)=1\\
\lim\limits_{x\to+\infty}f(x)=\frac{\pi}{2}\\
f(1)=\frac{\pi}{4}
$$

Which gives the solution:

$$
a=\frac{\pi}{2},b=\frac{12-\pi^2}{4(4-\pi)}\approx0.620,c=\frac{(6-\pi)^2(2-\pi)^2}{16(4-\pi)^2}\approx0.903
$$

That is:

$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\frac{12-\pi^2}{4(4-\pi)}+\sqrt{\frac{(6-\pi)^2(2-\pi)^2}{16(4-\pi)^2}+x^2}}\approx\frac{\pi}{2}\frac{x}{0.62+\sqrt{0.903+x^2}}
$$

The maximum error is $2.088\times 10^{-3}$ ($2.206\times 10^{-3}$ with the coefficients rounded to three significant digits).

```
(pi/2*x)/(0.62+sqrt(0.903+x^2))
```

### Inigo Quilez's Formula

[Inigo Quilez](https://iquilezles.org/maths/arctan/) found a good approximation:

$$
\arctan(x)\approx\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}
$$

```
(pi^2*x)/(4+sqrt(34+(2*pi*x)^2))
```

The constants are easy to remember, asymptotes are precise, and the maximum error is only $1.525\times 10^{-3}$, but the derivative at the origin is $\frac{\pi^2}{4+\sqrt{34}}\approx1.0039317$, which is not exactly $1$ but a little bit steeper than the true arc-tangent.

### Numerical Way

Let $f(x)=\frac{ax}{b+\sqrt{c+dx^2}}$, and measure an approximation by the maximum error $\max\limits_{x\in\mathbb{R}}\lvert f(x)-\arctan x\rvert$. We can search for the best $a,b,c,d$ under different restrictions.

#### Precise Origin Derivative and Asymptotes

Fixing $d=1$ and requiring both $f'(0)=1$ and $\lim\limits_{x\to+\infty}f(x)=\frac{\pi}{2}$ leaves $a=\frac{\pi}{2}$ and $b=\frac{\pi}{2}-\sqrt{c}$, so only one parameter is left. A one-dimensional search gives $c=0.8904022079$:

$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\frac{\pi}{2}-\sqrt{0.8904022079}+\sqrt{0.8904022079+x^2}}\approx\frac{\pi}{2}\frac{x}{\frac{\pi}{2}-\sqrt{0.890}+\sqrt{0.890+x^2}}
$$

The maximum error is $8.622\times 10^{-4}$ ($8.946\times 10^{-4}$ with the coefficients rounded to three significant digits).

```
(pi/2*x)/(pi/2-sqrt(0.890)+sqrt(0.890+x^2))
```

#### Precise Asymptotes

Fixing $d=1$ and requiring $\lim\limits_{x\to+\infty}f(x)=\frac{\pi}{2}$ leaves $a=\frac{\pi}{2}$, and a two-dimensional search over $b,c$ gives $b=0.629944$, $c=0.878519$:

$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{0.629944+\sqrt{0.878519+x^2}}\approx\frac{\pi}{2}\frac{x}{0.630+\sqrt{0.878+x^2}}
$$

The maximum error is $5.214\times 10^{-4}$ ($5.657\times 10^{-4}$ with the coefficients rounded to three significant digits).

```
(pi/2*x)/(0.630+sqrt(0.878+x^2))
```

#### Precise Origin Derivative

Fixing $c=1$ and requiring $f'(0)=1$ leaves $a=b+1$, and a two-dimensional search over $b,d$ gives $b=0.660738$, $d=1.118782$:

$$
\arctan(x)\approx\frac{1.660738x}{0.660738+\sqrt{1+1.118782x^2}}\approx\frac{1.661x}{0.661+\sqrt{1+1.119x^2}}
$$

The maximum error is $6.924\times 10^{-4}$ ($7.380\times 10^{-4}$ with the coefficients rounded to three significant digits).

```
(1.661*x)/(0.661+sqrt(1+1.119*x^2))
```

#### No Restriction

With no restriction the search gives $a=1.671702$, $b=0.668493$, $c=1$, $d=1.133225$:

$$
\arctan(x)\approx\frac{1.671702x}{0.668493+\sqrt{1+1.133225x^2}}\approx\frac{0.88x}{0.352+\sqrt{0.277+0.314x^2}}
$$

The maximum error is $4.311\times 10^{-4}$ ($4.517\times 10^{-4}$ with the coefficients rounded to three significant digits).

```
(0.88*x)/(0.352+sqrt(0.277+0.314*x^2))
```

### Rough Approximation

Consider the form $f(x)=\frac{ax}{b+|x|}$, which can help us to get some rough but shorter approximations.

Let $f(1)=\frac{\pi}{4},\lim\limits_{x\to+\infty}f(x)=\frac{\pi}{2}$, we get $a=\frac{\pi}{2},b=1$:

$$
\arctan(x)\approx \frac{\frac{\pi }{2}x}{1+|x|}
$$

with maximum error $7.1\times 10^{-2}$.

```
pi/2*x/(1+abs(x))
```

With some numerical search, let $a=1.63, b=1.1$:

$$
\arctan(x)\approx \frac{1.63x}{1.1+|x|}
$$

with maximum error $5.9\times 10^{-2}$.

```
1.63*x/(1.1+abs(x))
```

## The other arcs

With the above identities and approximations, we can get the other arcs just by combining them, like for example, we use Inigo Quilez's approximation:

$$
\arcsin(x)=\arctan\left(\frac{x}{\sqrt{1-x^2}}\right)\approx\frac{\pi^2\frac{x}{\sqrt{1-x^2}}}{4+\sqrt{34+(2\pi \frac{x}{\sqrt{1-x^2}})^2}}\\
\arccos(x)=\frac{\pi}{2}-\arcsin(x)\approx\frac{\pi}{2}-\frac{\pi^2\frac{x}{\sqrt{1-x^2}}}{4+\sqrt{34+(2\pi \frac{x}{\sqrt{1-x^2}})^2}}\\
\arctan(x)\approx\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}\\
\text{arccot}(x)=\frac{\pi}{2}-\arctan(x)\approx\frac{\pi}{2}-\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}\\
\text{arcsec}(x)=\arccos(\frac{1}{x})=\frac{\pi}{2}-\frac{\frac{\pi^2}{\sqrt{x^2-1}}}{4+\sqrt{34+(\frac{2\pi}{\sqrt{x^2-1}})^2}}\\
\text{arccsc}(x)=\arcsin(\frac{1}{x})=\frac{\frac{\pi^2}{\sqrt{x^2-1}}}{4+\sqrt{34+(\frac{2\pi}{\sqrt{x^2-1}})^2}}
$$

Because of the various combinations, we invite you to construct your own approximations by yourself.

## ODE Mode

Unlike in normal mode, it's easy to construct an inverse trigonometric function itself in ODE mode.

$$
\frac{d}{dx}\arcsin(x)=\frac{1}{\sqrt{1-x^2}}\\
\frac{d}{dx}\arccos(x)=-\frac{1}{\sqrt{1-x^2}}\\
\frac{d}{dx}\arctan(x)=\frac{1}{1+x^2}\\
\frac{d}{dx}\text{arccot}(x)=-\frac{1}{1+x^2}\\
\frac{d}{dx}\text{arcsec}(x)=\frac{1}{|x|\sqrt{x^2-1}}\\
\frac{d}{dx}\text{arccsc}(x)=-\frac{1}{|x|\sqrt{x^2-1}}\\
$$

```
1/sqrt(1-x^2)
-1/sqrt(1-x^2)
1/(1+x^2)
-1/(1+x^2)
1/(abs(x)*sqrt(x^2-1))
-1/(abs(x)*sqrt(x^2-1))
```