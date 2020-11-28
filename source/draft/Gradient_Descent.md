# Gradient Descent

$$\theta^{\star}=arg\underset{\theta}{min}\;L(\theta) \qquad L: loss function \quad \theta: parameters$$

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
