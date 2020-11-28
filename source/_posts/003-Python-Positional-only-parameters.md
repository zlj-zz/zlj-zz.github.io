---
title: Python 仅限位置参数介绍
date: 2019-12-01 18:46:51
category: Python
---

The new feature of Python3.8 is `Positional-only parameters`, you know? Let's find out.

<!--more-->

在 python3.8 之前，python 中有**位置参数**、**默认参数**、**可变参数**和**关键字参数**。在此，我们先回顾一下，下面先给出两个方法：

```python
def print_name(name):
  print("I am %s" % name)

def sum(a, b):
  return a + b
```

在调用这两个方法时，按照参数的位置将参数给进去，这种方式就是位置参数：

```python
print_name("tom")
sum(10, 20)
```

接下来，看看默认参数，比如下面有一个 `power `方法可以计算 `x` 的 `n `次方：

```python
def power(x, n):
  return x ** n
```

那我们可不可以让它在默认的情况下就计算平方呢？我们可以这么写：

```python
def power(x, n=2):
  return x ** n
```

这样在不传入 `n `的情况下，`n `就使用默认给的值，如果在调用时给了 `n` 的值，那么`n`的值就会被覆盖，这种就属于默认参数

再接着就一起看看，可变参数和关键字参数，有这样一个方法：

```python
def person(name, gender, age=18, city="Beijing", *args, **kwargs):
  print("My name is %s. I'am a %s." % (name, gender))
  print("I'm %d years old. I come from %s" % (age, city))
  print(args)
  for key, value in kwargs:
    print(key, value)
```

用`*`修饰参数就是一个可变参数，函数内部接受到的是一个 `tuple`，所以我们可以先组装一个 `list` 或` tuple` 再传入; 用 `**` 修饰的就是一个关键字参数，函数内部接受到的是一个 `dict`，同样我们可以先组装一个 `dict` 再传入。

让我们看看使用的方式：

```python
In [5]: person("tom", "boy", 20, "wuhan", "hello", "hi",hoby="football")
My name is tom. I'am a boy.
I'm 20 years old. I come from wuhan
('hello', 'hi')
{'hoby': 'football'}
​
​
In [6]: args = ["hello world", "I'm so happy"]
In [7]: kwargs = {'fathre':'Bob', 'mother':'lily'}
In [8]: person("tom", "boy", 20, *args, **kwargs)
My name is tom. I'am a boy.
I'm 20 years old. I come from hello world
("I'm so happy",)
{'fathre': 'Bob', 'mother': 'lily'}
```

然后，在 python3.8 中加入了**仅限位置形参**，新增了一个函数形参语法 `/` 用来指明某些函数形参必须使用仅限位置而非关键字参数的形式。

```python
def f1(a, b, c, d, e, f):
    print(a, b, c, d, e, f)
```

在上面 `f1()` 中，如下几种方式都可以：

```python
f1(10, 20, 30, d=40, e=50, f=60)
f1(10, b=20, c=30, d=40, e=50, f=60)
f1(10, 20, 30, 40, 50, f=60)
```

让我们来看看 `f2()`：

```python
def f2(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)
```

在 `f2()` 中只有第一种是正确的

```python
f(10, b=20, c=30, d=40, e=50, f=60)   # b cannot be a keyword argument
f(10, 20, 30, 40, 50, f=60)           # e must be a keyword argument
```

因为在函数中 `/ `和 `*` 限制参数的类型，在 `/` 左边必须是位置参数，在 `* `右边必须是关键字参数，而在 `/` 和 `* `直接的则两者都可以。

这样可以在不需要参数名称时排除掉关键字参数，避免了这种笨拙的调用形式。而且由于在 / 左侧的形参不会被公开为可用关键字，其他形参名仍可在 \*\*kwargs 中使用，就像这样：

```python
>>> def f(a, b, /, **kwargs):
...     print(a, b, kwargs)
...
>>> f(10, 20, a=1, b=2, c=3)         # a and b are used in two ways
10 20 {'a': 1, 'b': 2, 'c': 3}
```

参考资料
[1]Python Developer's Guide: https://www.python.org/dev/peps/pep-0570/
