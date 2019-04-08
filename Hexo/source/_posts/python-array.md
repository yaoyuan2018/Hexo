---
title: Python矩阵中matrix()和array()函数区别
date: 2019-04-08 23:32:39
tags:
  -Python
  -矩阵
categories:
  -服务端
---
## python矩阵中matrix()和array()函数区别

***
本篇主要介绍内容是矩阵中matrix()和array()函数的区别。主要从以下几方面说起：

1. 使用numpy库生成指定矩阵的方法差异
2. 矩阵性质的差异
3. 在矩阵乘法的不同体现
4. matrix()和array()关于秩的区别
5. array()函数和mat()函数之间的转换
6. 一些基本知识

### 1、具体矩阵生成方式的不同：

我们指定生成以下矩阵：
<div align="center">
![简单矩阵](https://img-blog.csdn.net/20170907125434040?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGZqNzQyMzQ2MDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center  "简单矩阵")
</div>
选取此矩阵的原因是：二阶方便计算；矩阵可逆；逆函数容易得到(可口算出)。

```python
import numpy as np

a1 = np.array([[1, 2], [3, 4]])
b1 = np.mat([[1, 2], [3, 4]])

a2 = np.array(([1, 2], [3, 4]))
b2 = np.mat(([1, 2], [3, 4]))

a3 = np.array(((1, 2), (3, 4)))
b3 = np.mat(((1, 2), (3, 4)))

b4 = np.mat('1 2; 3 4')

print("\n",a1,"\n",b1,"\n",a2,"\n",b2,"\n",a3,"\n",b3,"\n",b4)
```

输出结果为：
```python
[[1 2]
[3 4]]
```
上述函数的变化无非就是把大括号内的"[]"换成"()"，但括起来的认为一个整体，不同之处在于b4内用括号、空格和分号来产生矩阵、这个方法只可以在maxtrix()函数中使用，不可以写成 a4 = np.array('1 2; 3 4')
