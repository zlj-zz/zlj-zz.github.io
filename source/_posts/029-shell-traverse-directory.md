---
title: shell 遍历目录（for and while）
date: 2020-05-25
category: Linux
---

# 使用 for 循环

这种方法最常见，所以直接给出代码

```bash
#！/usr/bin/env bash

read -p "input dir path" dir

for item in `ls $dir`; do
	echo $item
done
```

# 使用 while 循环

想使用 while ，目标就是得到一个包含目录下每一个项目的数组。

如果直接 `./*`, 这样变量拿到的其实是一个字符串，这不满足我们的要求。

所以我们可以使用小括号，想这样：`(./*)`。小括号在 shell 中有初始化数组的作用，所以可以将得到的字符串，按空格分割每一项得到一个数组。

然后，我们遍历数组输出每一项就好了。

```bash
#！/usr/bin/env bash

read -p "input:" dir

files=($dir/*)
i=0

while [[ $i -lt ${#files[*]} ]]; do
    echo ${files[$i]}
    let i++
done
```
