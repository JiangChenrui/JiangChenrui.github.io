---
title: pytorch数据增广
date: 2019-09-06 17:26:26
categories: pytorch
tags:
- 深度学习
- 笔记
---

**常用的数据增广方法**
    1. 对图片进行按比例缩放
    2. 对图片进行随机位置的截取
    3. 对图片进行随机的水平和竖直翻转
    4. 对图片进行随机角度的旋转
    5. 对图片进行亮度、对比度和颜色的随机变化

下面使用torchvision演示一下这些数据增强方法。

```python
import sys
from PIL import Image
from torchvision import transforms
```

```python
# 载入图片
img = Image.open('img.jpg')
img
```

![png](pytorch数据增广/output_2_0.png)

## 随机比例缩放

&emsp;&emsp;随机比例缩放使用的是`torchvision.transforms.Resize()`函数，函数有两个参数，第一个参数为缩放大小，如果为一个值则会按比例缩放，否则按传入的值缩放；第二个参数表示缩放图片使用的方法，默认的是双线性差值。

```python
# 比例缩放
print('缩放前尺寸为:{}'.format(img.size))
new_img = transforms.Resize(224)(img)
print('缩放后尺寸为:{}'.format(new_img.size))
new_img

    缩放前尺寸为:(134, 43)
    缩放后尺寸为:(698, 224)
```

![png](pytorch数据增广/output_4_1.png)

```python
new_img = transforms.RandomCrop(224, padding=8)(new_img)
new_img
```

![png](pytorch数据增广/output_5_0.png)

```python
new_img = transforms.Resize((224, 224))(img)
print('缩放后尺寸为:{}'.format(new_img.size))
new_img

    缩放后尺寸为:(224, 224)
```

![png](pytorch数据增广/output_6_1.png)

## 随机位置截取

&emsp;&emsp;随机位置截取能够提取图片中的局部信息，使得网络接受的输入具有多尺度的特征，所以能够有较好的效果，在torchvision中主要有以下两种方式，一个是`torchvision.transforms.RandomCrop()`，传入的参数是截取出图片的长和宽，在图片的随机位置进行截取；第二个是`torchvision.transforms.CenterCrop()`，同样传入图片的长和宽，会在图片的中心进行截取。

```python
# 随机位置截取 100x100 的区域
random_img = transforms.RandomCrop(100)(new_img)
random_img
```

![png](pytorch数据增广/output_8_0.png)

```python
# 中心裁剪出 100x100 的区域
center_img = transforms.CenterCrop(100)(new_img)
center_img
```

![png](pytorch数据增广/output_9_0.png)

```python
# 进行填充后随机裁剪
random_img2 = transforms.RandomCrop(224, padding=8)(new_img)
random_img2
```

![png](pytorch数据增广/output_10_0.png)

## 随机水平翻转和竖直翻转

`torchvision.transforms.RandomHorizontalFlip()`和`torchvision.transforms.RandomVerticalFlip()`

```python
# 随机水平翻转
h_flip = transforms.RandomHorizontalFlip()(new_img)
h_flip
```

![png](pytorch数据增广/output_12_0.png)

```python
# 随机竖直翻转
v_flip = transforms.RandomVerticalFlip()(new_img)
v_flip
```

![png](pytorch数据增广/output_13_0.png)

## 随机角度旋转

`torchvision.transforms.RandomRotation()`

```python
rot_im = transforms.RandomRotation(30)(new_img)
rot_im
```

![png](pytorch数据增广/output_15_0.png)

## 亮度、对比度和颜色变化

`torchvision.transforms.ColorJitter()`函数有四个参数，第一个参数为亮度，第二个参数为对比度，第三个参数为饱和度，第四个参数为颜色

```python
# 亮度
bright_img = transforms.ColorJitter(brightness=1)(new_img)
bright_img
```

![png](pytorch数据增广/output_17_0.png)

```python
# 对比度
contrast_img = transforms.ColorJitter(contrast=1)(new_img)
contrast_img
```

![png](pytorch数据增广/output_18_0.png)

```python
# 饱和度
saturation_img = transforms.ColorJitter(saturation=1)(new_img)
saturation_img
```

![png](pytorch数据增广/output_19_0.png)

```python
# 颜色 随机变换颜色
color_img = transforms.ColorJitter(hue=0.5)(new_img)
color_img
```

![png](pytorch数据增广/output_20_0.png)

```python
compose_img = transforms.ColorJitter(0.5, 0.5, 0.5)(new_img)
compose_img
new_img
```

![png](pytorch数据增广/output_21_0.png)

```python
import matplotlib.pyplot as plt
%matplotlib inline

img_transform = transforms.Compose([
    transforms.Resize(232),
    transforms.RandomCrop(224),
    transforms.ColorJitter(0.15, 0.15, 0.15)
])

nrows = 5
ncols = 5
figsize = (10, 10)

_, figs = plt.subplots(nrows, ncols, figsize = figsize)
for i in range(nrows):
    for j in range(ncols):
        figs[i][j].imshow(img_transform(new_img))
        figs[i][j].axes.get_xaxis().set_visible(False)
        figs[i][j].axes.get_yaxis().set_visible(False)
plt.show()
```

![png](pytorch数据增广/output_22_0.png)
