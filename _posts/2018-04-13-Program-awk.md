---
layout: post
title:  "awk：shell下方便快捷的文本处理工具"
date:   2018-04-12 11:10:00
updated: 2019-08-30
categories: 编程入门
tags: awk
---


### 用途：

* 文本处理
* 输出格式化的文本报表
* 执行算数运算
* 执行字符串操作等等


### 工作方式：读取-> 执行 -> 重复

![](https://raw.githubusercontent.com/HuaZou/HuaZou.github.io/master/_posts/img/awk.png)


### 工作方式：

**1)**. Read

* AWK从输入流（文件，管道或者标准输入）中读取一行，然后存储到内存中。

**2)**. Execute

* 所有的AWK命令都依次在输入上执行。默认情况下，AWK会对每一行执行命令，我们可以通过提供模式限制这种行为。

**3)**. Repeat

* 处理过程不断重复，直到到达文件结尾



### 程序结构

**1)**. BEGIN 语句块

- BEGIN语句块的语法

```awk
BEGIN {awk-commands}
```

  - BEGIN语句块在程序开始的使用执行，它只执行一次，在这里可以初始化变量。BEGIN是AWK
    的关键字，因此它必须为大写，注意，这个语句块是可选的。

**2)**. BODY 语句块

- BODY语句块的语法

```awk
/pattern/ {awk-commands}
```

- BODY语句块中的命令会对输入的每一行执行，我们也可以通过提供模式来控制这种行为。注意，BODY语句块没有关键字。

**3)**. END 语句块

- END语句块的语法'

```awk
  - END {awk-commands}
```

  - END语句块在程序的最后执行，END是AWK的关键字，因此必须为大写，它也是可选的。

```awk
awk 'BEGIN{printf "Sr No\tName\tSub\tMarks\n"}{print}' marks.txt
```

### 基础语法：

**1)**. AWK命令行：

```awk
awk [options] file …   awk '{print}' marks.txt
```

**2)**. AWK程序文件: 

```awk
awk   [options] -f file ....
```

- 首先，创建一个包含下面内容的文本文件 command.awk

```bash
echo '{print}'  > command.awk
```

- 现在，我们可以让AWK执行该文件中的命令，这里我们实现了和上例同样的结果

```awk
awk -f command.awk marks.txt
```

**3)**.  AWK标准选项

- -v 变量赋值选项(传递参数)

- 该选项将一个值赋予一个变量，它会在程序开始之前进行赋值，下面的例子描述了该选项的使用

```awk
awk -v name=Jerry 'BEGIN{printf "Name = %s\n", name}'   ==>  Name = Jerry
```


### 用法

**1)**. 打印某行或者字段：

```awk
awk '{print $3 "\t" $4}' marks.txt
```
**2)**. 打印匹配模式的列： /a/匹配到a字母

```awk
awk '/a/ {print $3 "\t" $4}' marks.txt
```
**3)**. 统计匹配模式的行数

```awk
awk  '/a/{++cnt} END {print "Count = ", cnt}' marks.txt
```
**4)**. 内建变量

- ARGC 命令行参数个数

```awk
awk 'BEGIN {print "Arguments =", ARGC}'   One Two Three Four    Argument==> 5
```

- ARGV 命令行参数数组:存储命令行参数的数组，索引范围从0 - ARGC - 1

```awk
awk 'BEGIN { 
	for (i = 0; i < ARGC - 1; ++i) { 
		printf "ARGV[%d] = %s\n", i, ARGV[i] 
	} 
}' one two three four
ARGV[0] = awk
ARGV[1] = one
ARGV[2] = two
ARGV[3] = three
```

**5)**. FILENAME当前文件名

```awk
awk 'END {print FILENAME}' marks.txt  marks.txt 
```

**6)**. FS 输入字段(列)的分隔符:代表了输入字段的分隔符，默认值为空格，可以通过-F选项在命令行选项中修改它

```awk
awk -F , 'BEGIN {print "FS = " FS}' | cat   -vte 
```

**7)**. NF 字段数目:代表了当前行中的字段数目，例如下面例子打印出了包含大于两个字段的行

```bash
echo -e "One Two\nOne Two Three\nOne Two Three Four" | awk 'NF > 2'

# One Two Three
# One Two Three Four
```

**8)**. NR 行号

```bash
echo -e "One Two\nOne Two Three\nOne Two Three       Four" | awk 'NR < 3'

# One Two
# One Two Three
```

**9)**. FNR 行号（相对当前文件）

- 与NR相似，不过在处理多文件时更有用，获取的行号相对于当前文件

**10)**. OFS 输出字段分隔符: 输出字段分隔符，默认为空格

```bash
awk 'BEGIN {print "OFS = " OFS}' | cat       -vte
```

**11)**. ORS 输出行分隔符:  默认值为换行符

```bash
awk 'BEGIN{print "ORS = " ORS}' | cat -vte

# ORS = $
# $
```

**12)**. RS 输入行分隔符:默认值为换行符

```bash
awk 'BEGIN{print "RS = " RS}' | cat -vte

# RS = $
# $
```

**13)**. RLENGTH : 代表了 match 函数匹配的字符串长度。

```bash
awk 'BEGIN { if (match("One Two       Three", "re")) { print RLENGTH } }'

# 2
```

**14)**. RSTART: match函数匹配的第一次出现位置

```bash
awk 'BEGIN {if (match("One Two Three", "Thre")){print RSTART}}

# 9
```

### 操作符

**1)**. 算数操作符 :  就是+-*/%

```bash
awk 'BEGIN{ a = 50; b = 20; print "(a + b) = ", (a + b) }'

# (a + b) =  70
```

**2)**. 增减运算符: 自增自减与C语言一致

```bash
awk 'BEGIN {a = 10; b = ++a; printf "a = %d, b = %d\n", a, b }'

#a = 11, b = 11
```

**3)**. 赋值操作符

```bash
awk 'BEGIN {name = "Jerry"; print "My name is", name }'

# My name is Jerry
```

**4)**. 关系操作符

```bash
awk 'BEGIN {a = 10; b = 10; if (a <= b) print "a <= b" }'

# a <= b
```

**5)**.  逻辑操作符        &&  ||  !

```bash
awk 'BEGIN {num = 5; if (num >= 0 && num <= 7) 
			printf "%d is in octal format\n", num
}'

# 5 is in octal format
```

**6)**. 三元操作符

```bash
awk 'BEGIN {a = 10; b = 20; 
			(a > b) ? max = a : max = b; print "Max =",max}'

#Max = 20
```

**7)**. 字符串连接操作符

```bash
awk 'BEGIN {str1 = "Hello, "; str2 = "World"; 
			str3 = str1 str2; print str3 }'

# Hello, World
```

**8)**. 数组成员操作符
```bash
awk 'BEGIN{arr[0] = 1; arr[1] = 2; arr[2] = 3; 
		for (i in arr) 
			printf "arr[%d] = %d\n", i, arr[i]
}'

# arr[2] = 3
# arr[0] = 1
# arr[1] = 2
```

**9)**. 正则表达式操作符: 正则表达式操作符使用 ~ 和 !~ 分别代表匹配和不匹配

```bash
awk '$0 ~ 'ab'' marks.txt 
awk '$1 !~ 'time''  marks.txt
```

**10)**. 正则表达式

```bash
echo -e 'adb\nacb\nacd' | awk '/a.b/' 
# acb 
# adb 

echo -e 'The time\nthe time' | awk '/^The/'  
# The time  

echo -e 'The time the\nthe time The' | awk '/the$/'  
# ... the

echo -e 'ac\nbc\dc' | awk '/[ab]c/' 
# ac  
# bc 

echo -e 'ac\nbc\dc' | awk '/[^ab]c/' 
# dc 

echo -e 'ac\nbc\dc' | awk '/[ac|bc]/'
```


**11)**. 流程控制

```bash
awk 'BEGIN{num = 11; 
		if (num % 2 == 0) 
			printf "%d is even number.\n", num; 
		else 
			printf "%d is odd number.\n", num 
}'
```

**12)**. 循环:包括 for，while，do...while，break，continue 语句，还有一个 exit语句用于退出脚本执行

```bash
for(initialisation; condition; increment/decrement)
	action
awk 'BEGIN{for(i=1; i<=6; ++i)print i}'

while (condition)
	action
awk 'BEGIN{i=1; while(i<=6){print i;++i}}'

do
	action
while (condition)
awk 'BEGIN{i=1; do{print i;++i}; while(i<=6)}'


awk 'BEGIN{sum = 0;
		for(i=1; i<=50; ++i){
          sum +=i;
          if(sum > 40)
          	break;
          else
          	print "Sum = ", sum
		}
}'
```

**13)**. 函数

* [字符操作](http://czmmiao.iteye.com/blog/1885280)

```bash
gsub(regex, sub, string)
split(str, arr, regex)
```

* [自定义函数](https://blog.csdn.net/Cooling88/article/details/52279784)

```bash
function add(num1, num2){
  result = num1 + num2
  return result
}
BEGIN {
  res = add(10, 20)
  print "Sum = " res
}

## 函数存入 add.awk文件

awk -f add.awk 
```

**14)**. 输出重定向

> Command > filename   把标准输出重定向到一个新文件中
>
> Command >> filename   把标准输出重定向到一个文件中（追加）
>
> Command > filename   把标准输出重定向到一个文件中
>
> Command > filename 2>&1   把标准输出和错误一起重定向到一个文件中
>
> Command 2 > filename   把标准错误重定向到一个文件中
>
> Command 2 >> filename   把标准输出重定向到一个文件中（追加）
>
> Command >> filename2>&1   把标准输出和错误一起重定向到一个文件（追加





### Reference 

1. [awk参考](https://book.saubcy.com/AwkInAction/section_1/chapter_2_5.html)


参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。
