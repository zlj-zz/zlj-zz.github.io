# New Optimizers for Deep Learning

Background Konwledge

- $\mu$-strong convexity
- Lipschitz continuity
- Bregman proximal inequality

---

- SGD
- SGD with momentum
- Adagrad
- RMSProp
- Adam

Some Notations

- $\theta_{t}$: model parameters at time step $t$
- $\Delta L(\theta_{t})$ or $g_{t}$: gradient at $\theta_{t}$, used to compute $\theta_{t+1}$
- $m_{t+1}$: momentum accumulated from time step $0$ to time step $t$, which is used to compute $t_{t+1}$

What is Optimization about?

- Find a $\theta$ to get the lowest $\sum_{x} L(\theta;x)$
- Or, Find a $\theta$ to get the lowest $L(\theta)$

**On-line vs Off-line**

- On-line: one pair of $(x_{t}, \hat{y}_{t})$ at a time step

![](/images/deep-learn-1/on-line.png)

- Off-line: pour all $(x_{t}, \hat{y}_{t})$ into the model at every time step

![](/images/deep-learn-1/off-line.png)

**SGD**

![](/images/deep-learn-1/SGD.png)

**SGD with Momentum(SGDM)**

![](/images/deep-learn-1/SGDM.png)

**Adagrad**

$\theta_{t}=\theta_{t-1}-\frac{\eta}{\sqrt{\sum_{i=0}^{t-1}\left(g_{i}\right)^{2}}} g_{t-1}$

What if the gradients at the first few time steps are extremely large

**RMSProp**

$\theta_{t}=\theta_{t-1}-\frac{\eta}{\sqrt{v_{t}}} g_{t-1}$

$v_{1}=g_{0}^{2}$

$v_{t}=\alpha v_{t-1}+(1-\alpha)\left(g_{t-1}\right)^{2}$

Exponential moving average(EMA) of squared gradients is not monotonically increasing

**Adam**

![](/images/deep-learn-1/Adam.png)

**Adam vs SGDM**

- Adam: fast training, large generalization gap, unstable
- SGDM: stable, little generalization gap, better convergence(?)

**Advices**

| SGDM              | Adam                     |
| ----------------- | ------------------------ |
| - Computer vision | - NLP                    |
|                   | - Speedch synthesis      |
|                   | - GAN                    |
|                   | - Reinforcement learning |
