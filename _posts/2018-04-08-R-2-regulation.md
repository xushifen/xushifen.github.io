---
layout: post
title:  "R书写规则"
date:   2018-04-08 23:48:00
categories: Program
tags: R
---



## R书写规则
![](https://raw.githubusercontent.com/HuaZou/HuaZou.github.io/master/_posts/img/R.regulation.png)

### 一、 基本语法
R赋值一般使用 **<-** 符号，等价于 **=** 

### 二、基本规则

1). 脚本扩展名：以 .R (大写) 结尾

2). 命名规则：variable.name, FunctionName, kConstantName

3). 行宽：最多80字符

4). 缩进：两个空格而不是制表符

5). 空格：在二元操作符（包括关系符和运算符，如 <- = + 等）的两侧加空格，除非它在函数调用语句中。逗号的右侧空格

```R
plot(x    = x.coord,
     y    = data.mat[, MakeColName(metric, ptiles[1], "roiOpt")],
     ylim = ylim,
     xlab = "dates",
     ylab = metric,
     main = (paste(metric, " for 3 samples ", sep = "")))
```

6). 花括号：左花括号不单独一行，右花括号总是单独一行。如果有连续的花括号，这样书写

```R
 if (condition) {
    one or more lines
  } else {
    one or more lines
  }
```

7). 注释准则: 所有注释以 # 开始, 后接一个空格; 行内注释需要在 #前加两个空格

### 三、代码布局

> * 注释：版权声明
> * 注释：作者注
> * 注释：文件描述，包括功能、输入、输出
> * source() 与 library() 语句
> * 函数定义
> * 执行语句
> * 注释规则：
>   - 行间注释：注释符“#”顶格，之后空一格，书写注释
>   - 行尾注释：仿上，并在注释符与语句尾之间空两格
>   - TODO风格：全篇TODO的风格请统一
> * 函数规则：
>   - 函数定义时，必选参数在前，可选参数在后。
>   - 如果要折行，请在逗号后进行，而不是参数赋值的等号后。
>   - 函数文档：在函数定义行结束后，请跟进一个函数文档，包括：
> * 描述块：一句话总结函数的作用
> * 参数块：以“Args: ”开头，每个参数一行，阐明各参数（包括数据类型）。
> * 返回值块：以“Returns: ”开头，阐明返回值。
> * 各块之间留一个空行。文档效果以不阅读代码即能使用函数为准
>


```R
CalculateSampleCovariance <- function(x, y, verbose = TRUE) {
  # Computes the sample covariance between two vectors.
  #
  # Args:
  #   x: One of two vectors whose sample covariance is to be calculated.
  #   y: The other vector. x and y must have the same length, greater than one, with no missing values.
  #   verbose: If TRUE, prints sample covariance; if not, not. Default is TRUE.
  #
  # Returns:
  #   The sample covariance between x and y.
  n <- length(x)
  # Error handling
  if (n <= 1 || n != length(y)) {
    stop("Arguments x and y have different lengths: ",
         length(x), " and ", length(y), ".")
  }
  if (TRUE %in% is.na(x) || TRUE %in% is.na(y)) {
    stop(" Arguments x and y must not have missing values.")
  }
  covariance <- var(x, y)
  if (verbose)
    cat("Covariance = ", round(covariance, 4), ".\n", sep = "")
  return(covariance)
}
```

* 用 stop() 抛出函数错误

##### Reference

1. [入门风格](https://wklchris.github.io/R-learning-basic.htmlf)
2. [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml)

参考文章如引起任何侵权问题，可以与我[联系](https://github.com/HuaZou/)，谢谢。
