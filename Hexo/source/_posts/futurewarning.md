---
title: Python使用逻辑回归提示FutureWarning
date: 2019-04-08 23:34:42
tags:
  -读书笔记
  -python
---
## Python使用逻辑回归提示FutureWarning: Default solver will be changed to 'lbfgs' in 0.22. Specify a solver to silence this warning.
---
FutureWarning是语言或者库中将来可能改变的有关警告。

根据报警信息和参考相关文档“Default will changed from 'liblinear' to 'lbfgs' in 0.22.”，默认的solver参数在0.22版本中，将会由“liblinear”变为“lbfgs”，且指定solver参数可以消除该warning。

LogisticRegression算法的solver仅支持以下几个参数：
- ‘ liblinear ’
- ‘ newton-cg ’
- ‘ lbfgs ’
- ‘ sag ’
- ‘ saga ’

**解决方法：**

传入参数后即可消除警告：`model = LogisticRegression(solver = 'liblinear')``
