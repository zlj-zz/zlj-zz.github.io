# Classification

![](https://cdn.mathpix.com/snip/images/rzx7IAfdcZ8korgq8IbcrqGTLFpGNv1lsCdrUS_KmvA.original.fullsize.png)

**Ideal Alternatives**

- Function(Model):

  $x \longrightarrow \begin{array}{lcl}g(x)>0 \quad \text{ Output }=\text{ class } 1 \\ \text{ else } \quad \text{ Output }=\text{ class } 2\end{array}$

- Loss function:

  $L(f)=\sum_{n} \delta\left(f\left(x^{n}\right) \neq \hat{y}^{n}\right)$

  The number of times $f$ get incorrect results on training data.

- Find the best function:

  - Example: Perceptron, SVM

**Probability form Class - Feature**

$f_{\mu, \Sigma}(x)=\frac{1}{(2 \pi)^{D / 2}} \frac{1}{|\Sigma|^{1 / 2}} \exp \left\{-\frac{1}{2}(x-\mu)^{T} \Sigma^{-1}(x-\mu)\right\}$

Input: vector $x$, output: probability of sampling $x$

The shape of the function determines by **mean $\mu$** and **covariance matrix $\Sigma$**

**Maximum Likelihood**

The Gaussian with any mean $\mu$ and covariance matrix $\Sigma$ can generate these points.

Likelihood of a Gaussian with mean $\mu$ and covariance matrix $\Sigma$ = the probability of the Gaussian samples $x^1, x^2, x^3, \ldots, x^{79}$

$L(\mu, \Sigma)=f_{\mu, \Sigma}\left(x^{1}\right) f_{\mu, \Sigma}\left(x^{2}\right) f_{\mu, \Sigma}\left(x^{3}\right) \ldots \ldots f_{\mu, \Sigma}\left(\begin{array}{c}\left.x^{79}\right) \end{array}\right.$

We have the "Water" type Pokemons: $x^1, x^2, x^3, \ldots, x^{79}$

We assume $x^1, x^2, x^3, \ldots, x^{79}$ generate form the Gaussian $(\mu^{*},\Sigma^{*})$ with the **maximum likelihood**

$\mu^{*}, \Sigma^{*}=\arg \max _{\mu, \Sigma} L(\mu, \Sigma)$

$\mu^{*}=\frac{1}{79} \sum_{n=1}^{79} x^{n} \quad \text{(average)}, \qquad \Sigma^{*}=\frac{1}{79} \sum_{n=1}^{79}\left(x^{n}-\mu^{*}\right)\left(x^{n}-\mu^{*}\right)^{T}$

**Now we can do classification**

$P\left(C_{1} \mid x\right)=\frac{P\left(x \mid C_{1}\right) P\left(C_{1}\right)}{P\left(x \mid C_{1}\right) P\left(C_{1}\right)+P\left(x \mid C_{2}\right) P\left(C_{2}\right)}$

**Modifying Model**

- Maximum likelihood

  $"Water" type Pokemons: \qquad "Normal" type Pokemons:$

  $x^{1}, x^{2}, x^{3}, \ldots, x^{79} \qquad  \qquad \qquad  \qquad x^{80}, x^{81}, x^{82}, \ldots, x^{140}$

  $\mu^{1} \nearrow \qquad \qquad \qquad \quad \nwarrow \Sigma \nearrow \qquad \qquad \qquad \nwarrow \mu^{2}$

Find $\mu^{1}, \mu^{2}, \Sigma$ maximizing the likelihood $L(\mu^{1}, \mu^{2}, \Sigma)$

$L(\mu^{1}, \mu^{2}, \Sigma) = f_{\mu^{1}, \Sigma}(x^{1})f_{\mu^{1}, \Sigma}(x^{2}) \ldots f_{\mu^{1}, \Sigma}(x^{3}) \times f_{\mu^{2}, \Sigma}(x^{80})f_{\mu^{2}, \Sigma}(x^{81}) \ldots f_{\mu^{2}, \Sigma}(x^{140})$

$\mu^{1}$ and $\mu^{2}$ is the same $\Sigma = \frac{79}{140}\Sigma^{1} + \frac{61}{140}\Sigma^{2}$

**Three Steps**

- Function Set(Model):

  $x \longrightarrow \begin{array}{lcl} P\left(C_{1} \mid x\right)=\frac{P\left(x \mid C_{1}\right) P\left(C_{1}\right)}{P\left(x \mid C_{1}\right) P\left(C_{1}\right)+P\left(x \mid C_{2}\right) P\left(C_{2}\right)} \\ \text{If} \quad P(C_{1} \mid x)>0.5 \quad \text{ Output }=\text{ class } 1 \\ \text{ Otherwise, } \quad \text{ Output }=\text{ class } 2\end{array}$

- Goodness of a function:

  - The mean $\mu$ and covariance $\Sigma$ that maximizing the likelihood(the probability of generating data)

- Find the best function: easy

**Probability Distribution**

- You can always use the distribution you like
