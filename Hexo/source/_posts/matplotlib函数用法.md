---
title: matplotlib函数用法
date: 2019-03-28
tags:
  -读书笔记
  -python
---

## matplotlib函数用法
`import matplotlib.pyplot as plt`
***

### matplotlib图像显示配置

- **plt.rcParams[‘参数’]**

参数 | 用法 | 实例
:--:|:--:|:---:
savefig.dpi|图片像素|`plt.rcParams['savefig.dpi'] = 300 `
figure.dpi|分辨率|`plt.rcParams['figure.dpi'] = 300 `
figure.fugsize|设置figure_size尺寸|`plt.rcParams['figure_size'] = (8.0, 4.0)`
image.interpolation|设置interpolation style|`plt.rcParams['image.interpolation'] = 'nearst'`
image.cmap|设置颜色style|`plt.rcParams['image.cmap'] = 'gray'`
font.serif|设置正文字体|`plt.reParams['font.serif'] = ['SimHei']`
font.sans-serif|设置表格内字体|`plt.rcParams['font.sans-serif'] = ['SimHei']`


[^1]: SimHei指库自带字体--'黑体'

Serif和Sans-serif字体的区别 : [网页链接](https://blog.csdn.net/wdjhzw/article/details/78327041)

- **字体参照表**

字体 | 参数
:--:|:--:|:--:
黑体|SimHei
微软雅黑|Microsoft YaHei
微软正黑体|Microsoft JhengHei
新宋体|NSimSun
新细明体|PMingLiU
细明体|MingLiU
标楷体|DFKai-SB
仿宋|FangSong
楷体|KaiTi
仿宋_GB2312|FangSong_GB2312
楷体_GB2312|KaiTi_GB2312

- Matplotlib画图出现中文乱码的问题

 - Matplotlib输出中文显示问题 (CSDN博客，作者：木子木泗) : [网页链接](https://blog.csdn.net/u010758410/article/details/71743225)

 - Python3下使用Matplotlib工具画图，中文显示乱码的问题 (CSDN博客，作者：CC丶Z) : [网页链接](https://blog.csdn.net/ccblogger/article/details/79613335)

![Matplotlib画图中文乱码显示](https://img-blog.csdn.net/20180319162347149 "Matplotlib画图出现中文乱码")

![Matplotlib画图中文正常显示](https://img-blog.csdn.net/20180319163132710 "Matplotlib画图中文正常显示")

***
### plt.legend()——用于显示图例
legend()的一个用法：

```
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(1, 11)

fig = plt.figure(1)
ax1 = plt.subplot(2, 1, 1)
ax2 = plt.subplot(2, 1, 2)
l1, = ax1.plot(x, x*x, 'r')
l2, = ax2.plot(x, x*x, 'b')

plt.legend([l1, l2], ['first', 'second'], loc = 'upper right')    #loc表示位置

plt.show()
```
 - **loc参数表**

参数 | 对应数字
:--:|:--:
'best'|0
'upper right'|1
'upper left'|2
'lower left'|3
'lower right'|4
'right'|5
'center left'|6
'center right'|7
'lower center'|8
'upper center'|9
'center'|10
