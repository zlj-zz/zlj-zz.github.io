---
title: 线性回归及python代码实现
date: 2020-02-26 20:00:45
category: Machine learning
tags:
- line regression
- Python

---


# 回归分析

监督学习中，如果预测的变量是离散的，我们称其为分类（如决策树，支持向量机等），如果预测的变量是连续的，我们称其为回归。
- **离散**：连续的对应（就是反义词）就是离散 。离散就是不连续。例如像整数`1,2,3,4,5,...`这种就是离散的

在统计学中，**回归分析**（**regression analysis**)指的是确定两种或两种以上变量间相互依赖的定量关系的一种统计分析方法。

在大数据分析中，**回归分析**是一种预测性的建模技术，它研究的是**因变量**（目标）和**自变量**（预测器）之间的关系。这种技术通常用于预测分析，时间序列模型以及发现变量之间的因果关系。例如，司机的鲁莽驾驶与道路交通事故数量之间的关系，最好的研究方法就是回归。

 1. 回归分析按照**涉及的变量的多少**，分为一元回归和多元回归分析；
 2. 按照**因变量的多少**，可分为简单回归分析和多重回归分析；
 3. 按照**自变量和因变量之间的关系类型**，可分为线性回归分析和非线性回归分析
<br>

# 线性回归

线性回归是利用数理统计中回归分析，来确定两种或两种以上变量间相互依赖的定量关系的一种统计分析方法，运用十分广泛。其表达形式为 $y = w'x+e$，其中 $e$ 为误差服从均值为 0 的正态分布。
- 回归分析中，只包括一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。
- 如果回归分析中包括两个或两个以上的自变量，且因变量和自变量之间是线性关系，则称为多元线性回归分析。

#### 例子

假如现在我想去找银行贷款，那我能不能在贷款之前先预测一下我能贷到多少钱呢？下面有这样一张表：

| 工资 | 年龄 | 贷款额度 |
|--|--|--|
| 4000 | 25 | 20000 |
| 8000 | 30 | 70000 |
| 5000 | 28 | 35000 |
| 7500 | 33 | 50000 |
| 12000 | 40 | 85000 |

通过这张表我们可以比较明显的看到工资、年龄与贷款额度之间是有一个关系的，大概是工资越高、年龄越小贷款的额度就相应的越高。那么我能不能根据自己的工资和年龄去预测一个具体的贷款值？

这里我们就可以引入回归分析了，我们可以把**工资**和**年龄**当作两个**自变量**，**贷款额度**当作**因变量**，通过求解自变量和因变量的线性关系，从而得到一个线性回归方程。利用得到的线性回归方程我们就可以进行预测了，那这种统计分析中利用线性回归方程对自变量和应变量建模的回归分析就叫做线性回归。
<br>

# 函数拟合

继续上面的问题，现在我们把工资和年龄设为 $x_1,x_2$ (**特征值**)，贷款额度设为 $h$ (**目标值**)，去求一个线性函数，那么我们可以假设方程为：$h=\alpha_0+\alpha_1x_1+\alpha_2x_2$，接着假设定义$x_0$的值为 1，上面的式子可以表示成: $h = \displaystyle\sum_{i=0}^n \alpha_i x_i=\alpha^ T X$。

实际情况中，变量之间不一定有线性关系，这时我们就要通过拟合的方式来解决。也就是找到一组最佳的参数 $\alpha_0,\alpha_1,\alpha_2$ 使得预测值最接近真实值。最后我们可能得到这样的结果：

![dk-1](https://img-blog.csdnimg.cn/20200225214559219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

线性回归模型经常用**最小二乘**逼近来拟合，当然也可能用别的方法来拟合。
<br>

# 高斯分布

通过上面的图，可以看出真实值和预测值之间一般是存在误差(用 $\epsilon$ 来表示误差)，于是(真实值) $y =\displaystyle\sum_{i=0}^n \alpha_i x_i=\alpha^ T X+\epsilon$，对于每个样本有: $y^{(i)} = \alpha^Tx^{(i)}+\epsilon^{(i)}$，误差$\epsilon^{(i)}$是独立并且具有相同的分布，并且服从均值为0，方差为$\sigma^2$的高斯分布。

通常我们认为误差往往是很微小的，根据“[中心极限定理](https://baike.baidu.com/item/%E4%B8%AD%E5%BF%83%E6%9E%81%E9%99%90%E5%AE%9A%E7%90%86?fr=aladdin)” ：在自然界与生产中，一些现象受到许多相互独立的随机因素的影响，如果每个因素所产生的影响都很微小时，总的影响可以看作是服从正态分布的。中心极限定理就是从数学上证明了这一现象 。所以高斯分布对误差假设来说是一种很好的模型。。

正态分布（Normal distribution），也称“常态分布”，又名高斯分布（Gaussian distribution）;正态曲线呈钟型，两头低，中间高，左右对称因其曲线呈钟形，因此人们又经常称之为钟形曲线。

若随机变量$X$服从一个数学期望为$\mu$、方差为$\sigma^2$的正态分布，记为$N(\mu,\sigma^2)$。其概率密度函数为正态分布的期望值$\mu$决定了其位置，其标准差$\sigma$决定了分布的幅度。当$\mu=0,\sigma=1$时的正态分布是标准正态分布。

通过概率密度函数来定义高斯分布：

![](https://img-blog.csdnimg.cn/20200226004806895.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

高斯分布的概率密度函数：$p(y)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y-\mu)^2}{2\sigma^2}}$

由于误差服从高斯分布，将其带入正太分布函数中($\mu=0$)，可以得到：$p(y^{(i)}|x^{(i)};\alpha)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y^{(i)} - \alpha ^Tx^{(i)})^2}{2\sigma^2}}$
<br>

# 似然函数

在数理统计学中，似然函数是一种关于统计模型中的参数的函数，表示模型参数中的似然性。**似然性**是用于在已知某些观测所得到的结果时，对有关事物的性质的参数进行估计。给定输出x时，关于参数 $\theta$ 的似然函数 $L(\theta|x)$（在数值上）等于给定参数 $\theta$ 后变量X的概率：

$$L(\theta|x)=p(X=x|\theta)$$

似然函数的主要用法在于比较它相对取值，虽然这个数值本身不具备任何含义。例如，考虑一组样本，当其输出固定时，这组样本的某个未知参数往往会倾向于等于某个特定值，而不是随便的其他数，此时，似然函数是最大化的。

**最大似然估计**：对同一个似然函数，如果存在一个参数值，使得它的函数值达到最大的话，那么这个值就是最为“合理”的参数值。是似然函数最初也是最自然的应用。在 $\theta$ 的所有取值上，使这个函数最大化。这个使可能性最大的值即被称为 $\theta$ 的最大似然估计。

现在我们要求解的就是最佳参数，也就是满足最大似然估计的 $\alpha$ ，所以将 $p(y^{(i)}|x^{(i)};\alpha)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y^{(i)} - \alpha ^Tx^{(i)})^2}{2\sigma^2}}$带入：

$$L(\alpha)=\displaystyle\prod_{i=1}^mp(y^{(i)}|x^{(i)};\alpha)=\displaystyle\prod_{i=1}^m\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y^{(i)} - \alpha ^Tx^{(i)})^2}{2\sigma^2}}$$

**对数似然**：涉及到似然函数的许多应用中，更方便的是使用似然函数的自然对数形式，即“对数似然函数”。求解一个函数的极大化往往需要求解该函数的关于未知参数的偏导数。由于对数函数是单调递增的，而且对数似然函数在极大化求解时较为方便，所以对数似然函数常用在最大似然估计及相关领域中。

$$\log  L(\alpha)=\log \displaystyle\prod_{i=1}^mp(y^{(i)}|x^{(i)};\alpha)=\log \displaystyle\prod_{i=1}^m\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y^{(i)} - \alpha^T x^{(i)})^2}{2\sigma^2}}$$

$$= \displaystyle\sum_{i=1}^m\log\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(y^i - \alpha^T x^i)^2}{2\sigma^2}}$$

$$\quad \quad =m\log \frac{1}{\sigma\sqrt{2\pi}}-\frac{1}{\sigma^2}\cdot\frac{1}{2}\displaystyle\sum_{i=1}^m(y^{(i)} - \alpha^T x^{(i)})^2$$

目标是要使的对数似然函数最大，$m\log \frac{1}{\sigma\sqrt{2\pi}}$是一个常数，所以就得让后面减去的最小，得到目标函数：

$$J(\alpha)=\frac{1}{2}\displaystyle\sum_{i=1}^m(y^{(i)} - \alpha^T x^{(i)})^2=\frac{1}{2}\displaystyle\sum_{i=1}^m(Y(x^{(i)})-y^{(i)})^2=\frac{1}{2}(X\alpha-y)^T(X\alpha-y) $$

对 $\alpha$ 求偏导：

$$\nabla_\alpha J(\alpha)=\nabla_\alpha(\frac{1}{2}(X\alpha-y)^T(X\alpha-y))=\nabla_\alpha(\frac{1}{2}(\alpha^TX^T-y^T)(X\alpha-y))$$

$$=\nabla_\alpha\frac{1}{2}(\alpha^TX^TX\alpha-\alpha^TX^Ty-y^TX\alpha-y+y^Ty))$$

$$ =\frac{1}{2}(2X^TX\alpha-X^Ty-(y^TX)^T)=X^TX\alpha-X^Ty$$

令偏导等于 0：

$$\alpha=(X^TX)^{-1}X^Ty\quad\quad（最小二乘）$$
<br>

# 最小二乘法

假如我们在研究两个变量$（x,y）$之间的相互关系时，通常可以得到一系列成对的数据$（x1,y1.x2,y2... xm,ym）$；将这些数据描绘在 `x-y` 直角坐标系中，若发现这些点在一条直线附近,可以设这条直线方程为：$y_i=a_0+a_1x(其中 a_0、a_1任意实数)$。

为了建立这条直线，那么我们就要确定$a_0$和$a_1$的值，我们将实际值  $y_i$与计算值 $y_j(y_j=a_0+a_1x )\quad[1]$ 的差$(y_i-y_j)$的平方和  $\varphi=\displaystyle\sum_{i=1}^{n} (y_i-y_j)^2\quad[2]$ 最小作为**优化判据**。

我们把[1]式带入[2]式中，得到 $\varphi=\displaystyle\sum_{i=1}^{n} (y_i-a_0-a_1x_i )^2 \quad[3]$

为了使 $\varphi$ 的值最小，我们可以通过求解最小值的方法，让函数 $\varphi$ 对 $a_0、a_1$ 求偏导，令两个偏导数等于零。这样我们就的到了式[4]和式[5]。

$$\begin{cases}\displaystyle\sum_{i=1}^{n} 2(a_0+a_1x_i-y_i )=0 \quad[4]\\\displaystyle\sum_{i=1}^{n} 2x_i(a_0+a_1x_i-y_i )=0 \quad[5]\end{cases}$$

求解方程组，可以得到 $a_0=\frac{1}{n}\displaystyle\sum_{i=1}^n(y_i-a_1x_i) , a_1=\frac{n\displaystyle\sum_{i=1}^nx_iy_i-\displaystyle\sum_{i=1}^nx_i\displaystyle\sum_{i=1}^ny_i}{n\displaystyle\sum_{i=1}^nx_i^2-\displaystyle\sum_{i=1}^nx_i\displaystyle\sum_{i=1}^nx_i}$ ，将 $a_0$ 和 $a_1$ 带入[1]式中就的到了我们的一元线性方程，即：数学模型。

这里以最简单的一元线性模型来解释最小二乘法。回归分析中，如果只包括一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。如果回归分析中包括两个或两个以上的自变量，且因变量和自变量之间是线性关系，则称为多元线性回归分析。对于二维空间线性是一条直线；对于三维空间线性是一个平面，对于多维空间线性是一个超平面。
<br>

# 梯度下降

梯度下降是迭代法的一种,沿着这个函数下降的方向找，逼近最小值,可以用于求解最小二乘问题。在求解机器学习算法的模型参数，即无约束优化问题时，梯度下降（Gradient Descent）是最常采用的方法之一，另一种常用的方法是最小二乘法。

在求解损失函数的最小值时，可以通过梯度下降法来一步步的迭代求解，得到最小化的损失函数和模型参数值。

其迭代公式为 $a_{k+1}=a_k+\rho k^{\bar{s}^{(k)}}$ ,其中 $\bar{s}^{(k)}$ 代表梯度负方向，$\rho k$ 表示梯度方向上的搜索步长。梯度方向我们可以通过对函数求导得到，步长的确定比较麻烦，太大了的话可能会发散，太小收敛速度又太慢。
<br>

# 评估
通常使用RSE（残差标准差）和 R 来评估模型

$R^2=1-\frac{RSS}{TSS}=1-\frac{\displaystyle\sum_{i=1}^m(\hat{y_i}-y_i)^2}{\displaystyle\sum_{i=1}^m(y_i-\bar{y})^2}\quad\quad\quad\quad \frac{残差平方和}{类似方差项}$

$R^2$ 可以衡量因变量的变化比例，并用自变量 $x$ 表述。因此，假设在一个线性方程中，自变量 $x$ 可以解释因变量，那么变化比例就高， $R^2$ 将接近 1。反之，则接近0。所以 $R^2$的取值越接近 1 ，我们就认为模型拟合的与好
<br>

# 简单线性回归python案例

导入用到的包

```python
import numpy as np
import matplotlib.pyplot as plt
```

生成数据

```python
# 设置随机生成算法的初始值
np.random.seed(300)
data_size = 100
#  生出size个符合均分布的浮点数，取值范围为[low, high)，默认取值范围为[0, 1.0)
x = np.random.uniform(low=1.0, high=10.0, size=data_size)
y = x * 20 + 10 + np.random.normal(loc=0.0, scale=10.0, size=data_size)
```

查看数据

```python
print(x)
print(y.shape)
plt.scatter(x, y)
plt.show()
```

    [5.06010312 2.98920108 4.32173127 3.61698133 3.16970135 8.0384179
     8.29984929 4.60711487 1.23399282 6.48435743 6.66179289 6.86728789
     8.72731994 6.28122472 7.33751513 5.37311508 7.15974709 2.01131411
     2.64152172 7.27343316 4.31517483 4.84567523 4.59836046 6.83840906
     4.7201361  2.47241009 5.57532693 5.65184176 4.13384671 3.72027221
     3.04603181 1.49130937 7.68238581 3.78143155 9.79455216 4.27478265
     5.42937585 7.76009388 5.53962905 2.12052548 8.35322843 4.32938243
     3.18827513 8.28250053 3.09192118 3.2946656  4.92652317 8.48630166
     6.78218775 7.45073433 7.36576515 7.96927182 4.13018028 7.03576017
     6.43589797 7.3106855  9.21539573 9.0112781  9.5537087  7.9801062
     5.44528565 6.62427759 4.46709413 5.58802991 8.67279532 3.2084603
     9.61530432 8.82526244 2.18656259 5.71768665 7.5517145  3.89302569
     8.74359262 2.08295081 5.91832925 5.46471639 9.43168746 1.58599839
     7.63747752 4.87551191 8.56518562 7.88432931 3.48748529 1.72525194
     6.83057399 2.76268027 6.48435105 8.16047282 2.30107294 2.63178986
     8.74522455 5.82819158 6.58947199 5.36626558 4.85639013 6.27787997
     3.56740106 7.88396527 1.66851535 4.97195607]
    (100,)

![](https://img-blog.csdnimg.cn/20200226164326462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

区分训练集和测试集

```python
# 返回一个随机排列
shuffled_index = np.random.(data_size)
x = x[shuffled_index]
y = y[shuffled_index]
split_index = int(data_size * 0.75)
x_train = x[:split_index]
y_train = y[:split_index]
x_test = x[split_index:]
y_test = y[split_index:]
```

线性回归类

```python
class LinerRegression():

    def __init__(self, learning_rate=0.01, max_iter=100, seed=None):
        np.random.seed(seed)
        self.lr = learning_rate
        self.max_iter = max_iter
        self.a = np.random.normal(1, 0.1)
        self.b = np.random.normal(1, 0.1)
        self.loss_arr = []

    def fit(self, x, y):
        self.x = x
        self.y = y
        for i in range(self.max_iter):
            self._train_step()
            self.loss_arr.append(self.loss())

    def _f(self, x, a, b):
        """一元线性函数"""
        return x * a + b

    def predict(self, x=None):
        """预测"""
        if x is None:
            x = self.x
        y_pred = self._f(x, self.a, self.b)
        return y_pred

    def loss(self, y_true=None, y_pred=None):
        """损失"""
        if y_true is None or y_pred is None:
            y_true = self.y
            y_pred = self.predict(self.x)
        return np.mean((y_true - y_pred)**2)

    def _calc_gradient(self):
        """梯度"""
        d_a = np.mean((self.x * self.a + self.b - self.y) * self.x)
        d_b = np.mean(self.x * self.a + self.b - self.y)
        print(d_a, d_b)
        return d_a, d_b

    def _train_step(self):
        """训练频度"""
        d_a, d_b = self._calc_gradient()
        self.a = self.a - self.lr * d_a
        self.b = self.b - self.lr * d_b
        return self.a, self.b
```

训练数据

```python
regr = LinerRegression(learning_rate=0.01, max_iter=10, seed=314)
regr.fit(x_train, y_train)
```

展示

```python
print(f'cost: \t{regr.loss():.3}')
print(f"f={regr.a:.2} x + {regr.b:.2}")
plt.scatter(np.arange(len(regr.loss_arr)), regr.loss_arr, marker='o', c='green')
plt.show()
```

    cost: 	1.11e+02
    f=2.1e+01 x+4.2

![](https://img-blog.csdnimg.cn/20200226180220573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20200226181102306.gif)

评估

```python
y_pred = regr.predict(x_test)
def sqrt_R(y_pred, y_test):
    y_c = y_pred - y_test
    rss = np.mean(y_c ** 2)
    y_t = y_test - np.mean(y_test)
    tss = np.mean(y_t ** 2)
    return (1 - rss / tss)
r = sqrt_R(y_pred, y_test)
print(f"R: {r}")
```

    R: 0.9327511073300032

<br>

# sklearn案例

导包

```python
from sklearn.datasets import load_boston
from sklearn.externals import joblib
from sklearn.linear_model import LinearRegression, SGDRegressor, Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
```

载入数据

```python
"""线性回归预测房子价格"""
# 获取数据
lb = load_boston()
print(lb)
```

处理数据

```python
# 分割数据集 （训练集、测试集）
x_train, x_text, y_train, y_test = train_test_split(lb.data, lb.target, test_size=0.25)
# 进行标准化
# 特征值 目标值 都标准化
std_x = StandardScaler()
x_train = std_x.fit_transform(x_train)
x_text = std_x.fit_transform(x_text)

std_y = StandardScaler()
y_train = std_y.fit_transform(y_train.reshape(-1, 1))  # 传入数据必须二维
y_test = std_y.fit_transform(y_test.reshape(-1, 1))
```

方程求解

```python
# estimator预测
# 正规方程求解方式
lr = LinearRegression()
lr.fit(x_train, y_train)

# print(lr.coef_)

y_lr_predict = std_y.inverse_transform(lr.predict(x_text))
# print("预测：\n", y_lr_predict)
print("均方误差：", mean_squared_error(std_y.inverse_transform(y_test), y_lr_predict))
print(f"R : {lr.score(x_test, y_test)}")
```

    [[-0.10633041  0.10911523  0.00409775  0.06205962 -0.23187842  0.29700813
      -0.00083156 -0.32863065  0.2570794  -0.18807137 -0.22742163  0.10321854
      -0.39041034]
    均方误差： 21.92038221080247
    R : 0.6956028823910707

梯度下降

```python
# 梯度下降方式
sgd = SGDRegressor()
sgd.fit(x_train, y_train)

y_sgd_predict = std_y.inverse_transform(sgd.predict(x_text))
print(y_sgd_predict)
print("均方误差：", mean_squared_error(std_y.inverse_transform(y_test), y_sgd_predict))
print(f"R : {sgd.score(x_test, y_test)}")
```

    均方误差： 17285.95206027733
    R : 0.6907551016044395

