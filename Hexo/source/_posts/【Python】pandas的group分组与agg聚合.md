---
title: 【Python】pandas的group分组与agg聚合
date: 2019-05-08
tags:
  -Python

  -数据分析
categories:
  -服务端
---
```python
import pandas as pd

df = pd.DataFrame({'Country':['China', 'China', 'India', 'India', 'America', 'Japan', 'China', 'India'],
                    'Income':[10000, 10000, 5000, 5002, 40000, 50000, 8000, 5000],
                    'Age':[5000, 4321, 1234, 4010, 250, 250, 4500, 4321]})
```
构造的数据如下：
```
Age  Country  Income
0  5000    China   10000
1  4321    China   10000
2  1234    India    5000
3  4010    India    5002
4   250  America   40000
5   250    Japan   50000
6  4500    China    8000
7  4321    India    5000
```
## 分组
#### 单列分组
```python
df_gb = df.groupby('Country')
for index, data in df_gb:
    print(index)
    print(data)
```
输出：
```
America
   Country  Income  Age
4  America   40000  250

China
  Country  Income   Age
0   China   10000  5000
1   China   10000  4321
6   China    8000  4500

India
  Country  Income   Age
2   India    5000  1234
3   India    5002  4010
7   India    5000  4321

Japan
  Country  Income  Age
5   Japan   50000  250
```

#### 多列分组
```python
df_gb = df.groupby(['Country', 'Income'])
for (index1, index2), data in df_gb:
    print((index1, index2))
    print(data)
```
输出：
```
('America', 40000)
   Country  Income  Age
4  America   40000  250

('China', 8000)
  Country  Income   Age
6   China    8000  4500

('China', 10000)
  Country  Income   Age
0   China   10000  5000
1   China   10000  4321

('India', 5000)
  Country  Income   Age
2   India    5000  1234
7   India    5000  4321

('India', 5002)
  Country  Income   Age
3   India    5002  4010

('Japan', 50000)
  Country  Income  Age
5   Japan   50000  250
```

## 聚合
#### 对分组后数据进行聚合
</br>
&emsp;默认情况对分组之后其他列进行聚合
```python
df_agg = df.groupby('Country').agg(['min', 'mean', 'max'])
print(df_agg)
```
输出：
```
Income                        Age
   min          mean    max   min         mean   max
Country
America  40000  40000.000000  40000   250   250.000000   250
China     8000   9333.333333  10000  4321  4607.000000  5000
India     5000   5000.666667   5002  1234  3188.333333  4321
Japan    50000  50000.000000  50000   250   250.000000   250
```
#### 对分组后的部分列进行聚合
</br>
&emsp;某些情况只需要对部分数据进行不同的聚合操作，可以通过字典来构建
```python
num_agg = {'Age':['min', 'mean', 'max']}
print(df.groupby('Country').agg(num_agg))
```
输出：
```
Age
min         mean   max
Country
America   250   250.000000   250
China    4321  4607.000000  5000
India    1234  3188.333333  4321
Japan     250   250.000000   250
```
