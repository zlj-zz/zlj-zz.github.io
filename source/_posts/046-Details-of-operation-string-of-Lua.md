---
title: 在 Lua 中操作字符串隐藏的坑
date: 2022-06-15
category: Lua
---

在编程中，操作字符串是必不可少的操作。相信大家对此一定很熟练，但我从其他语言转到使用 lua 时在操作字符串上遇到了一些意想不到的坑点，这里就来盘一盘。

提到字符串相关的操作，正则表达式是不得不提及的强大功能。大多数语言中都有其独立的正则库，一个完整的正则支持的库通常是需要非常庞大的代码量，而 lua 作为一个精致、快速、小巧的语言并没有实现完整的正则库。而是实现了一个独立的串匹配的方式，来适配部分正则的功能，其集成在 `string` 这个模块下。使用 lua 的串匹配和正则极其相似，可以认为就是一个另类、简化的正则。具体的使用方法可以参考[菜鸟教程](https://www.runoob.com/lua/lua-strings.html) 。

由于 lua 的串匹配是集成在 string 模块中，所以往往有时候在操作字符串时会忽视掉当前是串匹配模式。比如一个检查是否包含子串的操作：

java 中：

```java
String  str1  =  "hello world!";
String  str2  =  "hello";

if (str1.indexOf(str2) == 0) {

    System.out.println("包含子串");

} else {

    System.out.println("不包含子串");

}

```

python 中：

```python
str1 = 'hello world!'
str2 = 'hello'

if str2 in str1:
    print('包含子串')
else:
    print('不包含子串')

```

lua 中：

```lua
local str1 = 'hello world!'
local str2 = 'hello'

if string.find(str1, str2) ~= nil then
    print('包含子串')
else
    print('不包含子串')
end

```

但其实上面的 lua 代码里有一个隐藏的坑点，先来看一看 lua 文档对这个函数的解释。

> **string.find (s, pattern [, init [, plain]])**

> Looks for the first match of pattern (see §6.4.1) in the string s. If it finds a match, then find returns the indices of s where this occurrence starts and ends; otherwise, it returns fail. A third, optional numeric argument init specifies where to start the search; its default value is 1 and can be negative. A true as a fourth, optional argument plain turns off the pattern matching facilities, so the function does a plain "find substring" operation, with no characters in pattern being considered magic.

可以看到 `string.find` 是支持串匹配的，往往在单纯的使用这个函数的时候很容易忽视这个点。如果想要寻找的子串包含 `()`、 `[]` 这些有特殊含义的符号，就无法按照预期去运行了。解决的方法就是用 `%` 对这些特殊的字符做转译，或着关闭 `string.find` 函数的串匹配模式。

```lua
local str1 = 'hello world! (smile)'
local str2 = 'world! (smile)'

-- 无法按预想匹配
if string.find(str1, str2) ~= nil then
    print('包含子串')
else
    print('不包含子串')
end

-- 正确
if string.find(str1, str2, 1, true) ~= nil then
    print('包含子串')
else
    print('不包含子串')
end

-- 正确
if string.find(str1, 'world! %(smile%)') ~= nil then
    print('包含子串')
else
    print('不包含子串')
end

```

所以在操作字符串时，往往都要考虑到使用的函数是否支持串匹配模式。这里也列举一下支持的函数：

- `string.find`
- `string.gmatch`
- `string.gsub`
- `string.match`

这些函数中也只有 `string.find` 是支持关闭串匹配的，这里也是需要注意的。
