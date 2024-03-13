---
title: "Python数学建模算法与应用第4章线性规划和整数规划模型"
date: 2023-06-21T21:30:22+08:00
lastmod: 2023-06-21T21:30:22+08:00
author: ["RM"]
categories:
- 数学
tags:
- 数学建模
- Python
math: true
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示当前路径
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
---






## 第4章 线性规划和整数规划模型

### 4.1求解最大值的线性规划问题

$max z = 72x_1+64x_2$

{{<keepit>}}
s.t.$$\begin{cases}
x_1+x_2\leq 50,\\
12x_1+8x_2\leq 480,\\
3x_1\leq 100,\\
x_1 ,x_2\geq 0
\end{cases}
$$
{{</keepit>}}

用Python软件求得最优解$x_1=20,x_2=30$,目标函数的最大值为3360。

#### 直接求解
```python
import cvxpy as cp

x = cp.Variable(2,pos=True)

obj = cp.Maximize(72*x[0]+64*x[1])

con = [x[0]+x[1]<=50, 12*x[0]+8*x[1]<=480,3*x[0]<=100]

prob = cp.Problem(obj,con)

prob.solve(solver="GLPK_MI")

print('最优值为：',prob.value)

print('最优解为：\n',x.value)
```

输出结果：
```txt
最优值为： 3360.0 
最优解为： 
[20. 30.]
```

#### 矩阵计算
```python
import cvxpy as cp

import numpy as np

x = cp.Variable(2,pos=True)#x有2个，且都取正数

c = np.array([72,64])

a = np.array([[1,1],[12,8],[3,0]])

b = np.array([50,480,100])

obj = cp.Maximize(c@x)#目标函数

con = [a@x<=b]#约束条件

prob = cp.Problem(obj,con)

prob.solve(solver="GLPK_MI")

print('最优值为：',prob.value)

print('最优解为：\n',x.value)
```
输出结果：
```txt
最优值为： 3360.0 
最优解为： 
[20. 30.]
```

### 4.2 求解最小值的线性规划问题

$min Z = 20x_1+90x_2+80x_3+70x_4+30x_5$

{{<keepit>}}
s.t.$$\begin{cases}
x_1+x_2+x_5\geq 30,\\
x_3+x_4\geq 30,\\
3x_1+2x_3\leq 120,\\
3x_2+2x_4+x_5\leq48,\\
x_j\geq0且为整数,j=1,2,3,4,5.
\end{cases}
$$
{{</keepit>}}

把上述线性规划问题改写成$min z = c^Tx$,

s.t.$\begin{cases}Ax \leq b,\\x_j\geq0且为整数,j=1,2,3,4,5.\end{cases}$

$c=\begin{bmatrix}20 \\90 \\80 \\70 \\30\end{bmatrix},$$A=\begin{bmatrix}-1&-1&0&0&-1 \\0&0&-1&-1&0 \\3&0&2&0&0 \\0&3&0&2&1 \\\end{bmatrix}$$b=\begin{bmatrix}-30 \\-30 \\120 \\48 \end{bmatrix},$

Python软件求得最优解$x_1=30,x_2=x_5=0,x_3=6,x_4=24$,目标函数的最小值为2760。

```python
import cvxpy as cp

x = cp.Variable(5,integer=True)

c = np.array([20,90,80,70,30])

a = np.array([[-1,-1,0,0,-1],[0,0,-1,-1,0],[3,0,2,0,0],[0,3,0,2,1]])

b = np.array([-30,-30,120,48])

obj = cp.Minimize(c@x)#目标函数

con = [a@x<=b,x>=0]#约束条件

prob = cp.Problem(obj,con)

prob.solve(solver="GLPK_MI")

print('最优值为：',prob.value)

print('最优解为：\n',x.value)
```
输出结果：
```txt
最优值为： 2760.0 
最优解为： 
[30. 0. 6. 24. 0.]
```








