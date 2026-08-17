# Graphwar `floor` construction

本文专注于如何在Graphwar中构造出 $\operatorname{floor}$ 函数的拟合，仅考虑拟合而不是 $0$ 误差的构建是因为在整数附近 $\operatorname{floor}$ 函数存在Graphwar无法处理的断点，导致我们不得不退而求其次

事实上，我们可以把这个问题拆分成两个子问题：

1. 不考虑能否在Graphwar中正常运行，仅用Graphwar允许的算子构造出 $\operatorname{floor}$ 函数近似
2. （如果需要的话）进一步处理**1**中的函数，使最终的函数能够在Graphwar中正常运行

### 前置知识

Graphwar允许且仅允许以下算子：
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

对应的符号为：

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

### 平凡解

注意到可以利用傅里叶级数来拟合 $\operatorname{floor}$ 函数
$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{1}{\pi}\sum_{n=1}^N\frac{\sin(2n\pi x)}{n}
$$
或者用多个Sigmoid函数（Step函数）来拟合，由于Graphwar的场地横坐标范围为 $[-25,25)$ ，所以可以进行如下构造
$$
\lfloor x\rfloor\approx-26+\sum_{n=-25}^{25}s(x-n)
$$
其中
$$
s(x)=\frac{1}{1+e^{-Mx}}
$$
$M$是一个大数

虽然这两个构造都自然地同时解决了子问题**2**，但他们的坏处要远远大于这点好处

傅里叶近似的收敛特别慢，累加几十项后精度仍然不高，叠加60项后在原理整数点处最大误差高达 $0.02$ ，况且无论叠加多少项，在整数点附近都会存在吉布斯现象，函数会过冲震荡，这部分误差无法消除

多个Sigmoid的确可以得到高精度的结果，但代价是特别长的表达式（傅里叶近似也有这个问题），在实战中几乎不可能用到

### Discord群u的回答

经过初步尝试无果后，我把这个问题发到了Graphwar的discord服务器上寻求帮助，没多久群u**Finnley**给出了相当不错的答案
$$
\lfloor x\rfloor\approx x-\left(\frac{\left(\sin\left(\frac{\pi x}{2}\right)\right)^{2}}{1+e^{-500\sin\left(\pi x\right)}}+\frac{\left(\sin\left(\frac{\pi\left(x-1\right)}{2}\right)\right)^{2}}{1+e^{500\sin\left(\pi x\right)}}+0.1\sin\left(2\pi x\right)\left(1+0.077\sin\left(\frac{3\pi x}{\sin\left(\pi x\right)}\right)\right)\right)
$$
和
$$
\lfloor x\rfloor\approx x-\frac{1}{2}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{1}{2}\right)}{2}\right)}}}{3+e^{-500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}+\frac{2-\frac{4}{1+e^{-1.9\tan\left(\frac{\pi\left(x-\frac{3}{2}\right)}{2}\right)}}}{3+e^{500\cos\left(\pi\left(x-\frac{1}{2}\right)\right)}}
$$
尽管仍然很长，但它们在保证高精度（非整数点附近处最大误差分别约为 $0.02$ 和 $0.0005$ ）的同时表达式相较于两个平凡解要短了许多，也都很自然地解决了子问题**2**

### 反正切

我的进一步尝试选择了一条截然不同的路径

考虑$\arctan(\cot(x))$（其中$\cot(x)=\frac{1}{\tan(x)}$），我们观察到他是一个满足以下关系的周期函数
$$
\arctan(\cot(x))=-x+\frac{\pi}{2}+k\pi,x\in(k\pi,(k+1)\pi),k\in\Z
$$
经过简单的变形我们可以得到
$$
\lfloor x \rfloor=x-0.5+\frac{1}{\pi}\arctan\left(\cot\left(\pi x\right)\right),x\notin\Z
$$
似乎我们用这个表达式解决了第一个字问题，但很可惜Graphwar的提供的表达式中不存在反三角函数，所以我们还得构造 $\arctan$ 的近似

#### 近似1

首先我想到了这个恒等式 $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$

然后用 $\frac{\pi}{2}x$ 来近似 $\arcsin$ ，得到 $\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{1+x^2}}$

为 $\frac{x}{\sqrt{1+x^2}}$ 分母添加待定常数项 $c$ ，得到 $\frac{x}{\sqrt{1+x^2}+c}$

设 $f(x)=\frac{x}{\sqrt{1+x^2}+c}$ ，考虑到 $\arctan(x)$ 在 $x=0$ 处导数为 $0$ ，所以我们也令 $f'(0)=0$ ，解得 $c=\frac{\pi}{2}-1$

于是
$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{1+x^2}+\frac{\pi}{2}-1}
$$
最大误差约为 $0.013$

所以
$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{1}{2}\frac{\cot\left(\pi x\right)}{\sqrt{1+\cot\left(\pi x\right)^{2}}+\frac{\pi}{2}-1}
$$
非整数点附近最大误差约为 $0.004$

#### 近似2

在我卡在 $\arctan(x)=\arcsin(\frac{x}{\sqrt{1+x^2}})$ 这个恒等式出不来，试图通过进一步近似 $\arcsin(x)$ 来进一步近似 $\arctan(x)$ 时，**Finnley**找到了一个更好的 $\arctan(x)$ 近似

令 $f(x)=\frac{ax}{b+\sqrt{c+x^2}}$

带入 $\arctan(x)$ 相关数据，注意到 $\arctan'(0)=1,\arctan(1)=\frac{\pi}{4},\lim\limits_{x\to\infty}\arctan(x)=\frac{\pi}{2}$

解把 $\arctan(x)$ 替换为 $f(x)$ 得到的方程组，得到
$$
\begin{cases}
a=\frac{\pi}{2}\\
b=\frac{12-\pi^2}{4(4-\pi)}\approx0.620\\
c=\frac{(6-\pi)^2(2-\pi)^2}{16(4-\pi)^2}\approx0.903
\end{cases}
$$
于是我们有
$$
\arctan(x)\approx\frac{\pi}{2}\frac{x}{\sqrt{0.903+x^2}+0.62}
$$
最大误差约为 $0.002$

所以
$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{1}{2}\frac{\cot\left(\pi x\right)}{\sqrt{0.903+\cot\left(\pi x\right)^{2}}+0.62}
$$
非整数点附近最大误差约为 $0.0007$

### 近似3

最后我在[Inigo Quilez](https://iquilezles.org/)的个人网站上找到了这个近似
$$
\arctan(x)\approx\frac{\pi^2x}{4+\sqrt{34+(2\pi x)^2}}
$$
最大误差约为 $0.001524$

最终得到
$$
\lfloor x \rfloor \approx x-\frac{1}{2}+\frac{\pi\cot\left(\pi x\right)}{4+\sqrt{34+(2\pi\cot\left(\pi x\right))^{2}}}
$$
非整数点附近最大误差约为 $0.000485$

最终我选择了由近似3得到的 $\operatorname{floor}$ 近似

### 去除断点

上面我们解决了第一个子问题，但第二个子问题并没有得到解决，使用上面的表达式在实际游玩中往往会在函数经过整数点时由于断点处斜率过大，或者取样点正好在断点上，而终止，不过所幸这个问题很容易解决

由半角公式可以得到
$$
\cot(x)=\frac{\sin(2x)}{1-\cos(2x)}
$$
于是我们可以给分母加上一个很小的正常数项得到
$$
\cot(x)\approx\frac{\sin(2x)}{1-\cos(2x)+\varepsilon}
$$
将上面的式子中的 $\cot(x)$ 替换为这个，通过控制这个 $\varepsilon$ 的大小，以牺牲些许精度为代价，可以在消除断点的同时控制函数上升速率，使得在实际游玩中函数不会终止
$$
\lfloor x \rfloor \approx x-0.5+\frac{\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon}}{4+\sqrt{34+(2\pi\frac{\sin(2\pi x)}{1-\cos(2\pi x)+\varepsilon})^{2}}}
$$
在Graphwar II中，由于函数计算方法的改进，$\varepsilon$可以被设置为任意接近$0$，但在Graphwar I中由于函数求值算法的缺陷所以无法这么做，在单 $\operatorname{floor}$ 下我测得最小的 $\varepsilon$ 约为 $0.0007$ ，但若考虑与其他函数复合，这个值的大小就需要具体情况具体分析了，这里不多赘述

---

最后是Graphwar中可用的floor表达式：`x-0.5+(pisin(2pix)/(1-cos(2pix)+0.0007))/(4+sqrt(34+(2*pisin(2pix)/(1-cos(2pix)+0.0007))^2))`
