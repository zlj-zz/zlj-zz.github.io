---
title: 使用 matplotlib 绘制图标
date: 2019-11-02 18:34:05
category: Python
tags: matplotlib

---
怎么用 Python 绘制图标呢? 最常用的一定是 matplotlib 库吧. 那么来看看怎么使用吧.
<!--more-->

首先绘制一张简单的图

```python
import matplotlib.pyplot as plt

plt.plot([1,2,3,4])
plt.show()
```

绘图的结果：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2NUQxb0tHc0l5NHllNGR2MzAxZmlhbERJQk16eWJON3RENHR4Z28xR3Fwck9YS0VDd1hySjdjUS82NDA?x-oss-process=image/format,png)

很容易，你只需要导入包、输入数据，然后你就可以得到你想要的图表了。

但是看起来似乎十分的单调，让我们来看看下面这段代码。

### 折线图

```python
import matplotlib.pyplot as plt
import numpy as np

a = np.arange(10)
plt.plot(a, a*1.5, linestyle='--', label="Firstline", color="gold")
plt.plot(a, a*2.5, linestyle=':', label="Secondline")
plt.plot(a, a*3.5, linestyle='-', label="Thirdline")
plt.plot(a, a*4.5, linestyle='-.', label="Fourthline")
# linestyle supported values are '-', '--', '-.', ':', 'None', ' ', '', 'solid', 'dashed', 'dashdot', 'dotted'
plt.title('title')
plt.xlabel('x label')
plt.ylabel('y label')
plt.legend()
plt.show()
```

结果如下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2NVpza3JKM1VzTDY0RkswMWliVmxRMDIyVUtDQXpsU3poellhNTBSRHRSQ1ZIeTMxdU9ySmZpYXcvNjQw?x-oss-process=image/format,png)

现在图表是不是看起来好多了。在代码中我们可以通过title来指定图表的标题，通过`xlablel`和`ylabel`来指定`x`轴和`y`轴的标题。在绘制折线图的`plot()`方法中可以通过`color`参数来指定颜色，`linestyle`参数来指定线的样式。通过`legend()` 方法可以让图表显示图例，图例的文字可以通过plot中的`label`参数来指定。

花完了折线图，接下来让我们条形图和直方图

有了plt.plot()的基础之后，对后面的理解就很容易了。

直接上例子：

### 条形图

```python
import matplotlib.pyplot as plt
import random

plt.bar([1, 3, 5, 7, 9], [random.randint(1,10) for i in range(5)], label="Example one")
plt.bar([2, 4, 6, 8, 10],[random.randint(1,10) for i in range(5)], label="Example two", color='orange')
plt.legend()
plt.xlabel('bar number')
plt.ylabel('bar height')
plt.title('Bar Title')

plt.show()
```

运行结果如下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2WmRxVUJhVkNsV1FITkltSmtDWVdMVkJDNzQ1akFncWJtcDBuZEgyOERpYm9NeHoycDBQbW8wQS82NDA?x-oss-process=image/format,png)

利用`bar()`方法就可绘制条形图了

接下来我们尝试添加一些属性信息：

```python
import matplotlib.pyplot as plt
import matplotlib
import random

r1 = plt.bar([1, 3, 5, 7, 9], [random.randint(1, 10) for i in range(5)], label="Example one", align="center")
r2 = plt.bar([2, 4, 6, 8, 10], [random.randint(1, 10) for i in range(5)], label="Example two", color='orange',
             align="center")
plt.legend()
plt.xlabel('bar number')
plt.ylabel('bar height')
plt.title('Bar Title')

# 设置x轴
_xtick_lables = ["one", "two", "three", "four", "five"]
plt.xticks([i / 1.5 for i in range(3, 18, 3)], _xtick_lables, rotation=45)
plt.grid(alpha=0.7)

# 添加值标
for rec in r1:
    x = rec.get_x()
    height = rec.get_height()
    plt.text(x + 0.3, 1.02 * height, str(height))
for rec in r2:
    x = rec.get_x()
    height = rec.get_height()
    plt.text(x + 0.3, 1.02 * height, str(height))

plt.show()
```

运行结果如下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2WlBhaEVnVFhoSUNndlQ1M2JpYXZYWExHT3ZxNDZZeVJPaWFQZE96QXhYOWNvODNtbW1sVGpzVUEvNjQw?x-oss-process=image/format,png)

看完了条形图，接下来看看直方图：

### 直方图

```python
import matplotlib.pyplot as plt
import matplotlib
import random

population_ages = [random.randint(0, 130) for i in range(60)]
bins = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100, 110, 120, 130]
plt.hist(population_ages, bins, histtype='bar')
plt.xlabel('x', fontsize='large')
plt.ylabel('y', rotation=0, fontsize='large')
plt.legend()
plt.show()
```
运行结果如下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2VzBQblZVWk9maWJIZUdEdVRkaWE1Nzc3dWdtbThzREVMT24xN256NzhyNFRNc3M4WlJ5VmhPMmcvNjQw?x-oss-process=image/format,png)

直方图用的是`hist()`方法，是不是看起和条形图来很像，其实两者区别非常大

- 条形图的长度是当前类别的频数，而它的宽是没有意义的，各个矩阵之间通常是分开排列的，主要是用于展示分类数据
- 直方图使用矩阵的面积表示各组频度的，高代表频率、宽为组距，由于分组之间的数据具有连续性通常矩形直之间连续排列，主要用于展示数值型数


最后让我们来看看散点图和饼图：

### 散点图

直接甩代码：

```python
import matplotlib.pyplot as plt
import matplotlib
import random

x = [random.randint(25,50) for i in range(300)]
y = [random.randint(25,50) for i in range(300)]

x1 = [random.randint(1,25) for i in range(300)]
y1 = [random.randint(1,25) for i in range(300)]
plt.scatter(x,y, label='dot-1', color="red", s=25, marker="o")
plt.scatter(x1,y1, label='dot-2', color="gold", s=25, marker="o")
plt.xlabel('x')
plt.ylabel('y')
plt.title('Title')
plt.legend()
plt.show()
```

运行结果如下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2cjJ1cmZjaWEwYkhEZG1CazFPZEtpYkcwb0kxSjZ5eFI3UW9CZ0w2MElTRXRkSlFuYU83YTVLaWJBLzY0MA?x-oss-process=image/format,png)

散点图是指在回归分析中，数据点在直角坐标系平面上的分布图，散点图表示因变量随自变量而变化的大致趋势，据此可以选择合适的函数对数据点进行拟合。

用两组数据构成多个坐标点，考察坐标点的分布，判断两变量之间是否存在某种关联或总结坐标点的分布模式。散点图将序列显示为一组点。值由点在图表中的位置表示。类别由图表中的不同标记表示。散点图通常用于比较跨类别的聚合数据。


### 饼状图

通常，饼图用于显示部分对于整体的情况，通常以`％`为单位。幸运的是，Matplotlib 会处理切片大小以及一切事情，我们只需要提供数值。

示例程序：

```python
import matplotlib.pyplot as plt

slices = [8, 3, 8, 5, 3]
activities = ["sleeping", "eating", "working", "studying", "playing"]
cols = ['grey', 'red', 'pink', 'gold', 'green']
plt.pie(slices, labels=activities, colors=cols, startangle=90, shadow=True, explode=(0, 0.1, 0, 0, 0),
        autopct='%1.1f%%')
plt.title('Title')
plt.show()
```

运行结果：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWVhVdHVHaWJlYlBXN2tSUWZlNVI5aWI2cFRKUklkWkp0NUJtdmJKNDZYR3VtY3VpYndiaWNpYjlpYlN3aWJzWGNMSGFTWWVmN2VzeWlhaWJqaGJYdy82NDA?x-oss-process=image/format,png)

在`pie()`中，我们需要通过指定切片，这是每个部分的大小，它会自动求出百分比。

然后，我们通过`labels`指定每个部分的名称，通过`colors`指定颜色列表。

接下来，我们可以选择指定图形的起始角度。这使你可以在任何地方开始绘图。例子中，我们为饼图选择了 90 度角。

我们也可以选择给绘图添加一个字符大小的阴影，我们甚至可以使用`explode`拉出一个切片，使用explode参数就可以了，如果我们不想拉出任何切片，我们传入0,0,0,0,0。如果我们想要拉出第一个切片，我们传入0,0.1,0,0,0。

最后，我们使用`autopct`，选择将百分比放置到图表上面。
