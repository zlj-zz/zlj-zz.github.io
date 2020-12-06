# Logistic Regreesion

**Step 1: Function Set**

We want to find $P_{w,b}(C_{1}|x)$

$P{w,b}(C_{1}|x)=\sigma(z) \qquad \sigma(z) = \frac{1}{1+\exp(-z)}, z=w \cdot x + b = \sum_{i}w_{i}x_{i} + b$

![](/images/deep-learn-1/sigmoid.png)

Function set:

$f_{w, b}(x) = P_{w, b}(C_{1} | x) \qquad$ Including all different $w$ and $h$

![](/images/deep-learn-1/lr-1.png)

|         | Logistic Regreesion                                                                                                   | Linear Regreesion                                                        |
| ------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Step 1: | $f_{w, b}(x)=\sigma(\sum_{i}w_{i}x_{i}+b)$                                                                            | $f_{w, b}(x)=\sum_{i}w_{i}x_{i}+b$                                       |
|         | output: between 0 and 1                                                                                               | output: any value                                                        |
| Step 2: | Training data: $(x^{n},\hat{y}^{n})$                                                                                  | Training data: $(x^{n},\hat{y}^{n})$                                     |
|         | $\hat{y}^{n}$: 1 for class 1, 0 for class 2                                                                           | $\hat{y}^{n}$: a real number                                             |
|         | $L(f)=\sum_{n}\text{Cross entropy}(f(x^{n}, \hat{y}^{n}))=-[\hat{y}^{n}\ln{f(x^{n})+(1-\hat{y}^{n})\ln{1-f(x^{n})}}]$ | $L(f)=\frac{1}{2}\sum_{n}(f(x^{n})-\hat{y}^{n})^{2}$                     |
| Step 3: | $w_{i} \gets w_{i}-\eta \sum_{n}-(\hat{y}^{n}-f_{w, b}(x^{n}))x_{i}^{n}$                                              | $w_{i} \gets w_{i}-\eta \sum_{n}-(\hat{y}^{n}-f_{w, b}(x^{n}))x_{i}^{n}$ |

## **Multi-class Classification**

![](/images/deep-learn-1/multi-class-classification.png)

![](/images/deep-learn-1/m-c-1.png)

## Limitation of Logistic Regreesion

- **Feature Transformation**

  Not always easy to find a good transformation.

- **Cascading logistic regression models**

  ![](/images/deep-learn-1/m-c-2.png)
