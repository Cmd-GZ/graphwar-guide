# Graphwar `floor` construction

The article focus on how to construct approximations of $\text{floor}$in Graphwar. The reason of just consider approximations instead of $0$ error is that there are some breakpoints of $\text{floor}$ that Graphwar can't handle on $x\in\Z$.

In fact, it contains 2 subquestions:

1. How to construct a $\text{floor}$ approximation only with operators in Graphwar without consider if it can run in Graphwar

2. How to further process of the function in **1** (if it is needed) such that the final function can run in Graphwar normally

### Prerequisites

Graphwar allows and only allows the following operators:
$$
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

### Trivial solutions

Notice that Fourier series can be used to approximate $\text{floor}$:
$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{1}{\pi}\sum_{n=1}^N\frac{\sin(2n\pi x)}{n}
$$
Or, we can approximate it with the sum of several sigmoid (step) functions. Since the $x$-axis range of Graphwar is [-25,25)$, we get the following construction:
$$
\lfloor x\rfloor\approx-26+\sum_{n=-25}^{25}s(x-n)
$$
where
$$
s(x)=\frac{1}{1+e^{-Mx}}
$$
and $M$ is a large number

Both of them have huge problems although they can also solve the 2nd subquestion naturally.

The Fourier approximation converges so slowly that the precision is still very low after accumulating dozens of terms: the  maximum error at points far from integers is about $0.02$ if $N=60$. Moreover, regardless of $N$ as big as, the Gibbs phenomenon exists near integer points, that is, the function will overshoot and oscillate, whose error can' t be eliminated.

Indeed the sum of Sigmoid functions construction can yield a high precision result, but its expression is too long (and so does Fourier form) to be used in actual situation.

### The answers from a discord server member

After initial attempts failed, I sent this question to the official discord server of Graphwar to ask for help. **Finnley** gave two pretty nice answers:
$$
\lfloor x\rfloor\approx x-\left(\frac{\left(\sin\left(\frac{\pi x}{2}\right)\right)^{2}}{1+e^{-500\sin\left(\pi x\right)}}+\frac{\left(\sin\left(\frac{\pi\left(x-1\right)}{2}\right)\right)^{2}}{1+e^{500\sin\left(\pi x\right)}}+0.1\sin\left(2\pi x\right)\left(1+0.077\sin\left(\frac{3\pi x}{\sin\left(\pi x\right)}\right)\right)\right)
$$
and:
$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{1}{2}\right)}{2}\right)}}}{3+e^{-500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{3}{2}\right)}{2}\right)}}}{3+e^{500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}
$$
They are shorter then trivial solutions while maintaining high precision, where the corresponding maximum error at the points far from integers is about 0.02$ and$0.0005$ and also solves the 2nd subquestion naturally.

### Arctangent

My further attempts took a different path, consider $\arctan(\cot(x))$, where $\cot(x)=\frac{1}{\tan(x)}$, observe that it's a periodic function that satisfies the following relation:
$$
\arctan(\cot(x))=-x+\frac{\pi}{2}+k\pi,x\in(k\pi,(k+1)\pi),k\in\Z
$$
After simple transformation, we get:
$$
\lfloor x \rfloor=x-0.5+\frac{1}{\pi}\arctan\left(\cot\left(\pi x\right)\right),x\notin\Z
$$
It seems that the 1st subquestions have been solved by the expression, but it's a pity that there are no inverse trigonometric functions in Graphwar. So we have to construct the approximation of $\arctan$.

#### 1st approximation:

My first thought was the the identity: $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$

Approximating $\arcsin$ with $\frac{\pi}{2}x$, we get $\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{1+x^2}}$

Adding an undetermined constant term $c$ to the denominator of $\frac{x}{\sqrt{1+x^2}}$ gives $\frac{x}{\sqrt{1+x^2}+c}$

Let $f(x)=\frac{x}{\sqrt{1+x^2}+c}$, considering that $\arctan'(0)=0$, let$f'(0)=0$, we get $c=\frac{\pi}{2}-1$

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

#### 2nd approximation:

When I lost in the identity $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$ and try to get a further $\arcsin(x)$ approximation,  **Finnley** found a better $\arctan(x)$ approximation:

Let $f(x)=\frac{ax}{b+\sqrt{c+x^2}}$

Notice that$\arctan'(0)=1,\arctan(1)=\frac{\pi}{4},\lim\limits_{x\to\infty}\arctan(x)=\frac{\pi}{2}$

Solve the equation system obtained by replace $\arctan(x)$with $f(x)$, we get:
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

### 3rd approximation

Finally I found the approximation in [Inigo Quilez](https://iquilezles.org/)'s website:
$$
\arctan(x)\approx\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}
$$
The maximum error is $0.001524$

Finally we get:
$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{\pi\cot\left(\pi x\right)}{4+\sqrt{34+(2\pi\cot\left(\pi x\right))^{2}}}
$$
The maximum error at points far from integers is about $0.000485$

I chose the 3rd approximation as the final result

### Remove breakpoints

The 1st subquestion has been solved, but the 2nd one haven't. The above expression will be terminated when it passes through the integers because their slope is too high to be handled by Graphwar or any discontinuous point is used as the sampling point. But luckily it's very easy to be solved.

Consider the following identity:
$$
\cot(x)=\frac{\sin(2x)}{1-\cos(2x)}
$$
Add a very small positive number to the denominator of it, we get:
$$
\cot(x)\approx\frac{\sin(2x)}{1-\cos(2x)+\varepsilon}
$$
Replace $\cot(x)$ with it in the above formula, by controlling the size of $\varepsilon$, at the cost of some precision, we can control the function's slope while eliminating breakpoints to ensure that the function does not terminate in Graphwar.
$$
\lfloor x \rfloor \approx x-0.5+\frac{\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon}}{4+\sqrt{34+(2\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon})^{2}}}
$$
In Graphwar II, beacuse of the algorithm improvements, $\varepsilon$ can be arbitrarily close to $0$. But in Graphwar I can't do that because of the algorithm defects. The smallest value of $\varepsilon$ is about $0.0007$ if you just input a $\text{floor}$ function. But if you want to compose it with any other function, the value should be analyzed specifically.

---

Finally it's the expression of $\text{floor}$ that can be used in Graphwar：`x-0.5+(pisin(2pix)/(1-cos(2pix)+0.0007))/(4+sqrt(34+(2*pisin(2pix)/(1-cos(2pix)+0.0007))^2))`
