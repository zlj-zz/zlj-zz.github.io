---
title: shell edit
date: 2020-03-04 
category: Linux
tags: shell

---
基础的 shell 编程知识, 还有一些对应的小例子.
<!--more-->


# shell简介
**shelle**十一个命令行解释器，为用户提供了向`linux内核`发送请求以便运行程序的界面系统程序，用户可以用`shell`来启动、挂起、停止甚至编写程序。

```mermaid
graph LR
A[外层应用程序] --> B[bash命令解释器]
B --> C[linux内核]
C --> D[硬件]
```



#### 格式要求
以`#!/bin/bash`开头，指定bash解释器; 这里指定了`bash`
#### 权限要求
执行shell脚本，一般给脚本添加`执行权限`,两种增加权限方式：
```bash
sudo chmod +x xxx.sh
sudo chmod 777 xxx.sh
```
#### 执行方式
两种执行方式：
```bash
./xxx.sh
sh xxx.sh  # 没有执行权限也可执行
```
#### 注释方法

```bash
# 单行注释

：<<!
多行注释
注释内容
!
```

# shell变量
### 系统变量
系统预先定义的，可以直接使用，如：`$HOME`, `$PWD`, `$bash`, `$USER` 等
使用 `set` 命令可以查看当前bash中所有变量
### 自定义变量
通过 `变量名=值` 定义变量， 使用 `unset` 可以撤销变量;  `readonly` 声明静态变量，该类型不可 `unset`。例子：
```bash
#!/bin/bash

readonly GREET="hello word"
NAME="Zachary"
echo $GREET
echo ${NAME}
unset NAME
echo $NAME
```
>- 变量名可由字母、数字、下划线组成，不能以`数字开头`
>- `=` 两侧不能有空格
>- 变量一般使用`大写`方式
>- 调用变量时，需要使用 `$`符号或者`${}`

可以将`执行指令的结果`赋值给变量，例子：
```bash
#!/bin/bash

RESULT1=`ls -l /home`
RESULT2=$(ls -l /home)
echo $RESULT1
echo $RESULT2
```
>- 两种方式，推荐使用第二种  `RESULT=$(ls -l /home)` 

### 位置参数变量
我们可以获取命令行输入的参数，
`Sn` : `n`是数字，`$0`表示命令本身，从`$1～$9`, 表述命令行输入的第一到第九个参数，第十个及以上要使用`{}`, 如 `${10}`
`S*` : 所有参数，但看作一个整体 
`S@` :  所有参数，但区分个体
`S#` :  所有参数个数

#### 预定义变量
bash设计者已经定义好的，可以直接使用，
`$$`: 当前进程号（PID）
`$!`: 后台运行的最后一个进程号（PID）
`$?`: 最后一次执行命令的返回状态， `0` 表示正确执行，否则错误执行

# shell运算
**操作**：加、减、乘、除、模; **运算符号**：`+`、`-`、`*`、`/`、`%`
**形式**：`$((运算式))`、`$[运算式]`、`expr a - b`, 例子：
```bash
#!/bin/bash

RESULT1=$(((1+2)*3))
RESULT2=$[(1+2)*3]
TEMP=`expr 1 + 2`
RESULT3=`expr $TEMP \* 3`
echo $RESULT1
echo $RESULT2
echo $RESULT3
```
>- `expr` 方式中运算符两侧要有空格，整个元算式需要`反引号`包裹
>- 推荐使用 `$[运算式]` 方式

# 读取控制台输入
基本语法：`read 选项 参数`
`-p` 指定读取时的提示符
`-t` 指定读取等待时间，时间内未输入则不再等待输入
```bash
#!/bin/bash

read -t 10 -p "请输入:" A
echo $A
```

# shell数组
一对括号表示是数组，数组元素用`空格`符号分割开。如：`a=(1 2 3 4 5)`

**获取**:用`$\{\#数组名\[@或\*\]\}`可以得到数组长度, 用`${数组名[下标]}`可以得到指定下标的值，下标是从0开始; 用`${数组名[@或*]}` 可以得到整个数组内容

```bash
###### 获取 ######
echo ${#a[@]}
echo ${#a[*]}
echo ${a[2]}
echo ${a[@]}
echo ${a[*]}
```

**赋值**：直接通过 `数组名[下标]=值`就可以对其进行引用赋值，如果下标不存在，自动添加新一个数组元素
```bash
###### 赋值 ######
a[1]=100
```

**删除**：`unset 数组[下标] `可以清除相应的元素，不带下标，清除整个数据。
```bash
###### 删除 ######
unset a[1]
# unset 数组[下标] 不带下标，清除整个数据。
unset a
```

**截取**：截取数组 `${数组名[@或*]:起始位置:长度}`返回字符串，中间用“空格”分开; 如果加上”()”，将得到切片数组。
```bash
###### 截取 ######
echo ${a[@]:0:3}
# 如果加上”()”，将得到切片数组，上面例子：c 就是一个新数据。
c=(${a[@]:1:4})
```

**替换**：`${数组名[@或*]/查找字符/替换字符}` 该操作不会改变原先数组内容。
```bash
###### 替换 ######
echo ${a[@]/3/100}
echo ${a[@]}
# 如果需要需求，重新赋值给变量a
a=(${a[@]/3/100})
echo ${a[@]}
```

# 条件判断
**形式** `[ condition ]`
>条件语句与`[]`之间要有空格，`[]`内是空返回`false`

**常用判断条件**
<table>
	<tr>
		<td colspan=2>字符串类型</td>
	</tr>
	<tr>
		<td >=</td><td>等于</td>
	</tr>
	<tr>
		<td>！=</td><td>不等于</td>
	</tr>
	<tr>
		<td>-z 字符串</td><td>字符串的长度为零则为真</td>
	</tr>
	<tr>
		<td>-n 字符串</td><td>字符串的长度不为零则为真</td>
	</tr>

</table>

<table>
	<tr>
		<td colspan=2>整数类型</td>
	</tr>
	<tr>
		<td >-lt</td><td>小于</td>
	</tr>
	<tr>
		<td>-le</td><td>小于等于</td>
	</tr>
	<tr>
		<td>-gt</td><td>大于</td>
	</tr>
	<tr>
		<td>-ge</td><td>大于等于</td>
	</tr>
	<tr>
		<td>-eq</td><td>等于</td>
	</tr>
	<tr>
		<td>-ne</td><td>不等于</td>
	</tr>
</table>

<table>
	<tr>
		<td colspan=2>文件权限</td>
	</tr>
	<tr>
		<td>-r</td><td>有可读权限</td>
	</tr>
	<tr>
		<td>-w</td><td>有可写权限</td>
	</tr>
	<tr>
		<td>-x</td><td>有可执行权限</td>
	</tr>
</table>

<table>
	<tr>
		<td colspan=2>文件类型</td>
	</tr>
	<tr>
		<td>-e</td><td>是否文件存在</td>
	</tr>
	<tr>
		<td>-f</td><td>是否是普通文件</td>
	</tr>
	<tr>
		<td>-d</td><td>是否是目录</td>
	</tr>
</table>

# 流程控制
### if判断
```bash
if [ condition ];then
	content
fi
```

```bash
if [ condition ]
then
	content
	elif [ condition ]
	then
		content
else
	content
fi
```
> 推荐使用第二种

例子：
```bash
#!/bin/bash

# 提示用户输入年份后判断该年是否为闰年
# 能被4整除并且并不能被100整除的年份是闰年
# 能被400整除的年份也是闰年
read -p "请输入一个年份:" YEAR
 
if [ "$YEAR" = "" ];then
    echo "没有输入年份"
    exit
fi
#使用正则测试变量 year 中是否包含大小写字母
if [[ "$YEAR" =~ [a‐Z] ]];then
    echo "你输入的不是数字"
    exit
fi
# 判断是否为闰年
if [ $[YEAR % 4] -eq 0 ] && [ $[YEAR % 100] -ne 0 ];then
    echo "$YEAR年是闰年"  
elif [ $[YEAR % 400] -eq 0 ];then
    echo "$YEAR年是闰年"
else
    echo "$YEAR年不是闰年"
fi

```
### case语句
```bash
case $variable_name in
"value")  # 传入值与value相等执行content的语句
	content
	;;
"value")
	content
	;;
esac
```
例子：
```bash
#!/bin/bash

# 判断用户输入的数据类型(字母、数字或其他) 
read -p "请输入一个字符:" KEY
case "$KEY" in
[a‐z]|[A‐Z])
        echo "字母" 
        ;;
[0‐9])
        echo "数字" 
        ;;
*)
        echo "空格、功能键或其他控制字符"
esac

```
### for循环
```bash
for 变量 in value1 value2 value3 ...
do
	content
done
```

```bash
for ((初始值;循环控制条件;变量变化))
do
	content
done
```
例子：
```bash
#!/bin/bash

# 打印国际象棋棋盘
# 设置两个变量,i 和 j,一个代表行,一个代表列,国际象棋为 8*8 棋盘
# i=1 是代表准备打印第一行棋盘,第 1 行棋盘有灰色和蓝色间隔输出,总共为 8 列
# i=1,j=1 代表第 1 行的第 1 列;i=2,j=3 代表第 2 行的第 3 列
# 棋盘的规律是 i+j 如果是偶数,就打印蓝色色块,如果是奇数就打印灰色色块
# 使用 echo ‐ne 打印色块,并且打印完成色块后不自动换行,在同一行继续输出其他色块
for i in {1..8}
do
  	for j in {1..8}
  	do
  		sum=$[i+j]
		if [  $[sum%2] -eq 0 ];then
 			echo -ne "\033[46m  \033[0m"
		else
			echo -ne "\033[47m  \033[0m"
		fi
  	done
  	echo
done
```

```bash
#!/bin/bash

# 对 100 以内的所有正整数相加求和(1+2+3+4...+100)
SUM=0
for i in `seq 100`
do
  	SUM=$[SUM+i]
done
echo "总和是:$SUM"
```
### while循环
```bash
while [ condition]
do
	content
done
```
例子：
```bash
#!/bin/bash

# 读取用户输入的账户名称,将账户名写入到数组保存 
# 定义数组名称为 name,数组的下标为 i,小标从 0 开始,每输入一个账户名,下标加 1,继续存下一个账户
# 最后,输入 over,脚本输出总结性信息后脚本退出
i=0
while :
do
	read -p "请输入账户名,输入 over 结束:" KEY
	if [ $KEY == "over" ];then 
		break
  	else
		NAME[$i]=$KEY
		let i++
  	fi
done
echo "总账户名数量:${#NAME[*]}"
echo "${NAME[@]}"
```
# 函数
### 系统函数
`basename`
### 自定义函数
```bash
function func_name(){
	content
	return ...
}
func_name param1 param2 ...
```
> `function` 关键字可以不写，但推荐写; 可以没有`return`

例子：
```bash
#!/bin/bash
 
# 脚本生成一个 100 以内的随机数,提示用户猜数字,根据用户的输入,提示用户猜对了,
# 猜小了或猜大了,直至用户猜对脚本结束。
# RANDOM 为系统自带的系统变量,值为 0‐32767的随机数
# 使用取余算法将随机数变为 1‐100 的随机数
function guess(){
        num=$[RANDOM%100+1]
        echo "$num"
         
        while  :
        do
                read -p "计算机生成了一个 1‐100 的随机数,你猜: " cai
            if [ $cai -eq $num ]
            then
                echo "恭喜,猜对了"
                exit
                elif [ $cai -gt $num ]
                then
                        echo "猜大了"
                else
                        echo "猜小了"
                fi
        done
}
guess
```
暂时这么多笔记
