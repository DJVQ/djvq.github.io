---
title: "二维Arnold变换"
date: 2022-03-27
tags: ["学习","Arnold","图像处理"]
description: "一种置乱方法"
---

## 公式

### arnold

$$
\left[\begin{matrix}\hat{x} \\ \hat{y}\end{matrix}\right]  =  
\left[\begin{matrix}1 & a \\ b & ab+1\end{matrix}\right] \left[\begin{matrix}x \\ y\end{matrix}\right] mod (N)
$$

### dearnold

$$
\left[\begin{matrix}x \\ y\end{matrix}\right]  =  
\left[\begin{matrix}ab + 1 & -a \\ b & 1\end{matrix}\right] \left[\begin{matrix}\hat{x} \\ \hat{y}\end{matrix}\right] mod (N)
$$
&nbsp;
&nbsp;

## 对图像进行arnold变换的python代码

```python
def arnold(img, a=1, b=1, time=1):
    img_array = np.asarray(img)
    shape = img_array.shape
    dst = np.zeros(shape)
    r = shape[0]
    c = shape[1]
    dstr = []
    dstr.append(img)
    for t in range(time):
        if t>0:
            img = np.array(dst)
        for i in range(r):
            for j in range(c):
                x = (i + b * j) % r
                y = (a * i + (a * b +1) * j) % c
                dst[x, y] = img[i, j]
        dstr.append(np.array(dst))
    return dstr

def dearnold(img, a=1, b=1, time=1):
    img_array = np.asarray(img)
    shape = img_array.shape
    dst = np.zeros(shape)
    r = shape[0]
    c = shape[1]
    dstr = []
    for t in range(time):
        if t>0:
            img = np.array(dst)
        for i in range(r):
            for j in range(c):
                x = ((a * b + 1) * i - b * j) % r
                y = (-a * i + j) % c
                dst[x, y] = img[i, j]
        dstr.append(np.array(dst))
    return dstr

```

## 说明

$mod(N)$限制了最终变换的图像的大小，所以$N$一般为图像的高或宽，整个变换只改变像素位置不改变像素值

## 补充

方阵的$arnold$变换可还原，且具有周期性，一般$T < $ $N^2\over2$，而对于非方阵，进行$arnold$变换无法还原
