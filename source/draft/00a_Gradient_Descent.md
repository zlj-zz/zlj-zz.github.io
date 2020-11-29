# Gradient Descent

$$\theta^{\ast}=arg\underset{\theta}{min}\;L(\theta) \qquad L: loss function \quad \theta: parameters$$

## Tip 1: Tuning your learning rates.

- $\theta^i=\theta^{i-1}-\eta^{\nabla}C(\theta^{i-1})$ set the learning rate $\eta$ carefully.

We can visualize this:

| learning rate | loss change        |
| ------------- | ------------------ |
| small         | decrease very slow |
| large         | decrease too fast  |
| very large    | fly, crazzy        |
| just make     | it's our want      |

### Adaptive Learning Rates

- Popular & Simple Idea: Reduce the learning rate by some factor every few epochs.
  - At the beginning, we are far from there destination, so we use larger learning rate.
  - After several epochs, we are close to the destination, so we reduce the learning rate.
  - E.g. $1/t$ decay: $\eta^t=\eta/\sqrt{t+1}$
- Learning rate cannot be one-size-fits-all
  - Giving different parameters different learning rates.

### Adagrad

- Divid the learning rate of each parameter by the **root mean square of its previous derivaties** .

**Vanilla Gradient descent**

$w^{t+1} \leftarrow w^{t}-\eta^{t} g^{t}$

> $\eta^{t}=\frac{\eta}{\sqrt{t+1}}$, $g^{t}=\frac{\partial C\left(\theta^{t}\right)}{\partial w}$, $w$ is one parameters</font>

**Adagrad**

$w^{t+1} \leftarrow w^{t}-\frac{\eta^{t}}{\sigma^{t}} g^{t}$

> $\sigma^t$: **root mean square** of the previous derivatives of parameter $w$ <br> $\sigma^{t}=\sqrt{\frac{1}{t+1} \sum_{i=0}^{t}\left(g^{i}\right)^{2}}$,

$w^{t+1} \leftarrow w^{t}-\frac{\eta}{\sqrt{\sum_{i=0}^{t}\left(g^{i}\right)^{2}}} g^{t}$

The best step is $\frac{\mid \text { First derivative } \mid}{\text { Second derivative }}$

## Tip 2: Stocastic Gradient Descent

### Stochastic Gradient Descent

**Gradient Descent**

$L=\sum_{n}\left(\hat{y}^{n}-\left(b+\sum w_{i} x_{i}^{n}\right)\right)^{2} \qquad \theta^{i}=\theta^{i-1}-\eta \nabla L\left(\theta^{i-1}\right)$

Update after seeing all example

**Stochastic Gradient Descent**

Pick an example $x^n$, Loss for only one example

$L^{n}=\left(\hat{y}^{n}-\left(b+\sum w_{i} x_{i}^{n}\right)\right)^{2} \qquad \theta^{i}=\theta^{i-1}-\eta \nabla L^{n}\left(\theta^{i-1}\right)$

Update for each example. If there are 20 examples, 20 times faster.

## Tip 3: Feature Scaling

Make different features have the same scaling.

> Because if one arrtribute values are small, other one is big, then the two different arrtributes have different effect for Loss function.

**One way:**

![](https://cdn.mathpix.com/snip/images/vuBHy0-6tqAeW4Jam2BRcLi1m5WFpulnwOesi94iGog.original.fullsize.png)

## Theory

Given a point, we can easily find the point with the smallest value nearby.

- **Taylor series:** Let $h(x)$ be any function infinitely differentiable arround $x=x_{0}$.

$\begin{aligned} \mathrm{h}(\mathrm{x}) &=\sum_{k=0}^{\infty} \frac{\mathrm{h}^{(k)}\left(x_{0}\right)}{k !}\left(x-x_{0}\right)^{k} \\ &=h\left(x_{0}\right)+h^{\prime}\left(x_{0}\right)\left(x-x_{0}\right)+\frac{h^{\prime \prime}\left(x_{0}\right)}{2 !}\left(x-x_{0}\right)^{2}+\ldots \end{aligned}$

When $x$ is close $x_{0}$, $\quad h(x) \approx h\left(x_{0}\right)+h^{\prime}\left(x_{0}\right)\left(x-x_{0}\right)$

**Multivariable Taylor Series**

$\begin{aligned} \mathrm{h}(\mathrm{x,y}) &=h\left(x_{0}, y_{0}\right)+\frac{\partial h\left(x_{0}, y_{0}\right)}{\partial x}\left(x-x_{0}\right)+\frac{\partial h\left(x_{0}, y_{0}\right)}{\partial y}\left(y-y_{0}\right) \\ &+something \ related \ to \ \left(x-x_{0}\right)^{2} and \ \left(y-y_{0}\right)^{2}+\ldots \ldots\end{aligned}$

When $x$ and $y$ is close to $x_{0}$ and $y_{0}$, $\quad h(x, y) \approx h\left(x_{0}, y_{0}\right)+\frac{\partial h\left(x_{0}, y_{0}\right)}{\partial x}\left(x-x_{0}\right)+\frac{\partial h\left(x_{0}, y_{0}\right)}{\partial y}\left(y-y_{0}\right)$

**Back to Formal Derivation**

Based on Taylor Series:

If the circle is **small enough** , in the circle.

![](https://cdn.mathpix.com/snip/images/l8cPXBFg1VFK-RRZAkgco6WxwsAhoH6BG_XPmXoOaws.original.fullsize.png)

$\mathrm{L}(\theta) \approx \mathrm{L}(a, b)+\frac{\partial \mathrm{L}(a, b)}{\partial \theta_{1}}\left(\theta_{1}-a\right)+\frac{\partial \mathrm{L}(a, b)}{\partial \theta_{2}}\left(\theta_{2}-b\right)$

> $s=\mathrm{L}(a, b)$,$u=\frac{\partial \mathrm{L}(a, b)}{\partial \theta_{1}}, v=\frac{\partial \mathrm{L}(a, b)}{\partial \theta_{2}}$

$\mathrm{L}(\theta) \approx s+u\left(\theta_{1}-a\right)+v\left(\theta_{2}-b\right)$

Find $\theta_{1}$ and $\theta_{2}$ in the circle **minimizing** $L(\theta)$

![](https://cdn.mathpix.com/snip/images/4WBgHPPhrtsCKc9f9C1kjyss-YnnYw0lcv4cBZB_N3o.original.fullsize.png)

$\left(\theta_{1}-a\right)^{2}+\left(\theta_{2}-b\right)^{2} \leq d^{2}$

> $\Delta \theta_{1}=(\theta_{1} - a), \quad \Delta \theta_{2}=(\theta_{2} - b)$

To minimize $L(\theta)$

$\left[\begin{array}{l}\Delta \theta_{1} \\ \Delta \theta_{2}\end{array}\right]=-\eta\left[\begin{array}{l}u \\ v\end{array}\right] \quad \longrightarrow\left[\begin{array}{l}\theta_{1} \\ \theta_{2}\end{array}\right]=\left[\begin{array}{l}a \\ b\end{array}\right]-\eta\left[\begin{array}{l}u \\ v\end{array}\right]=\left[\begin{array}{l} a \\ b \end{array}\right]-\eta\left[\begin{array}{l} \frac{\partial \mathrm{C}(a, b)}{\partial \theta_{1}} \\ \frac{\partial \mathrm{C}(a, b)}{\partial \theta_{2}} \end{array}\right]$

But not satisfied it the circle(learning rate) is not small enough.

You can consider the second order term, e.g. Newton's method.

## More Limitation of Gradient Descent

Stuck at local minima

Stuck at saddle point

Very slow at the plateau
