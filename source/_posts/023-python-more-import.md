---
title: Python最好减少使用 from...import，这里告诉你为什么
date: 2020-06-11
category: Python
---

在 **Python** 中我们可以通过三种方式来引入外部的模块：

- `import`
- `from...import`
- `__import__`

其中 `__import__` 比较少使用，而它本身和 `import` 比较相似，区别在于前者显示的将模块用字符串的形式传递到命名空间。

```python
__import__(name[, globals[, locals[, fromlist[, level]]]])
__import__('sys')
```

而在使用 `import` 时要注意以下几点：

- 一般情况下尽量优先使用 `import a` 形式，如访问 `B` 时需要使用 `a.B` 的形式。
- 有节制地使用 `from a import B` 形式，可以直接访问 `B`。
- 尽量避免使用 `from a import *`，因为这会污染命名空间，并且无法清晰地表示导入了哪些对象。

那么为什么要注意 i 这些呢？

这就不得不说说 Python 中的 `import` 机制。Python 在初始化运行环境的时候会预先加载一批内建模块到内存中，这些模块相关的信息被存放在 `sys.modules` 中。导入 `sys` 模块后在 Python 解释器中输入 `sys.modules.items()` 便可显示所有预加载模块的相关信息。当加载一个模块的时候，解释器实际上要完成以下动作：

1. 在 `sys.modules` 中进行搜索看看该模块是否已经存在，如果存在，则将其导入到当前局部命名空间，加载结束。
2. 如果在 `sys.modules` 中找不到对应模块的名称，则为需要导入的模块创建一个字典对象，并将该对象信息插入 `sys.modules` 中。
3. 加载前确认是否需要对模块对应的文件进行编译，如果需要则先进行编译。
4. 执行动态加载，在当前模块的命名空间中执行编译后的字节码，并将其中所有的对象放入模块对应的字典中。

![demo](https://img-blog.csdnimg.cn/2020061101451629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

从输出结果可以看出，对于用户定义的模块，`import` 机制会创建一个新的 module 将其加入当前的局部命名空间中，与此同时，`sys.modules` 也加入了该模块的相关信息。

![demo2](https://img-blog.csdnimg.cn/20200611015113603.png#pic_center)

但从它们的 id 输出结果可以看出，本质上是引用同一个对象。

需要注意的是，直接使用 `import` 和使用 `from a import B` 形式这两者之间存在一定的差异，后者直接将 `B` 暴露于当前局部空间，而将 `a` 加载到 `sys.modules` 集合。

那么 `from ... import` 有什么不好的地方呢？

**命名空间冲突**

假设我们现在有两个模块 `a` 和 `b`，内容如下：

```python
# a.py
def C():
	print("This is a's C.")
```

---

```python
# b.py
def C():
	print("This is b's C.")
```

我们在某一个文件中要同时导入这两个模块

```python
from a import C
from b import C

C()
```

运行后会得到 `This is b's C.`, 从结果中可以看到后导入的包中的 `add()` 覆盖了先导入的。特别是在一个较大的项目中如果频繁的使用 `from ... import ...` 就会大概率的出现命名空间冲突的问题。 所以一般最好不使用该方式，若在非常明确不会导致冲突的前提在，以下情况可以考虑：

1. 只导入少数方法或属性时;
2. 使用 `模块名.名称` 的方式过于繁琐的时候;
3. 模块文档明确要求要使用该方法， 如： `from urllib import request`。

**循环嵌套导入问题**

现在有两个文件 `c1` 和 `c2`， 内容如下：

```python
# c1.py
from c2 import y

def x():
	pass
```

---

```python
# c2.py
from c1 import x

def y():
	pass
```

运行结果：

![demo-3](https://img-blog.csdnimg.cn/20200611021819519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70#pic_center)

这时无论运行哪一个都会抛出 `ImportError` 异常。

这是因为在执行 `c1.py` 的加载过程中，需要创建新的模块对象 `c1` 然后执行 `c1.py` 所对应的字节码。在这过程中先遇到语句`from c2 import y`。

而 `c2` 在 `sys.modules` 也不存在，故此时会去创建与 `c2` 对应的模块对象并执行 `c2.py` 所对应的字节码。

而在创建 `c2` 中遇到 `from c1 import x` 时，由于 `c1` 已经存在，于是便去其对应的字典中查找 `y`，但 `c1` 模块对象虽然创建但初始化的过程并未完成，因此其对应的字典中并不存在 `y` 对象，此时便抛出 `ImportError: cannot import name 'y' from 'c2'` 异常。而解决循环嵌套导入问题的一个方法是直接使用 `import` 语句。

说到这里就差不多聊完啦。
