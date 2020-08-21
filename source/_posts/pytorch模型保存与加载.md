---
title: pytorch模型保存与加载
date: 2019-07-23 09:51:53
categories: pytorch
tags:
- 深度学习
- 笔记
---

[官方文档](https://pytorch.apachecn.org/docs/1.0/saving_loading_models.html)

## 模型保存相关的三个核心功能

**torch.save:** 将序列化对象保存到磁盘。此函数使用Python的pickle模块进行序列化，使用此模型可以保存如模型、tensor、字典等各种对象。  
**torch.load:** 使用pickle的unpicking功能将pickle对象文件反序列化到内存。此功能还可以有助于设备加载数据。  
**torch.nn.Moudle.load_state_dict:** 使用反序列化函数*state_dict*来加载模型的参数字典。  

## 状态字典

&emsp;&emsp;在pytorch中，`torch.nn.Module`模型的可学习参数（即权重和偏差）包含在模型的*parameters*中，（使用`model.parameters()`可以进行访问）。*state_dict*仅仅是python字典对象，它将每一层映射到其参数张量。注意，只有具有可学习参数的层（如卷积层、线性层等）的模型才具有*state_dict*这一项。优化目标`torch.optim`也有*state_dict*属性，它包含有关优化器的状态信息，以及使用的超参数。

### 示例

```python
# Define model
class TheModelClass(nn.Module):
    def __init__(self):
        super(TheModelClass, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
# Initialize model
model = TheModelClass()

# Initialize optimizer
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)

# Print model's state_dict
print("Model's state_dict:")
for param_tensor in model.state_dict():
    print(param_tensor, "\t", model.state_dict()[param_tensor].size())

# Print optimizer's state_dict
print("Optimizer's state_dict:")
for var_name in optimizer.state_dict():
    print(var_name, "\t", optimizer.state_dict()[var_name])
```

**输出：**

```ruby
Model's state_dict:
conv1.weight     torch.Size([6, 3, 5, 5])
conv1.bias   torch.Size([6])
conv2.weight     torch.Size([16, 6, 5, 5])
conv2.bias   torch.Size([16])
fc1.weight   torch.Size([120, 400])
fc1.bias     torch.Size([120])
fc2.weight   torch.Size([84, 120])
fc2.bias     torch.Size([84])
fc3.weight   torch.Size([10, 84])
fc3.bias     torch.Size([10])

Optimizer's state_dict:
state    {}
param_groups     [{'lr': 0.001, 'momentum': 0.9, 'dampening': 0, 'weight_decay': 0, 'nesterov': False, 'params': [4675713712, 4675713784, 4675714000, 4675714072, 4675714216, 4675714288, 4675714432, 4675714504, 4675714648, 4675714720]}]
```

## 保存和加载推断模型

### 保存/加载`state_dict`(推荐使用)

**保存：**

```python
torch.save(model.state_dict(), PATH)
```

**加载:**

```python
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.eval()
```

&emsp;&emsp;用保存的模型进行推断的时候，只需要保存模型学习到的参数，使用`torch.save()`函数来保存模型*state_dict*，所用的资源要少于保存完整模型。在进行推断之前，要调用`model.eval()`去设置dropout和batch normalization层为评估模式。在传入`load_state_dict()`函数之前，需要使用`torch.load()`对*state_dict*进行反序列化。

### 保存/加载完整模型

**保存：**

```python
torch.save(model, PATH)
```

**加载：**

```python
model = torch.load(PATH)
model.eval()
```

### 保存`torch.nn.DataParallel`模型

**保存：**

```python
model = TheModelClass(*args, **kwargs)
model = torch.nn.DataParallel(model)
torch.save(model.state_dict(), PATH)
```

**加载：**

```python
model = TheModelClass(*args, **kwargs)
model = torch.nn.DataParallel(model)
model.load_state_dict(torch.load(PATH))
model.eval()
```

**在加载模型继续训练的时候，加载了两次`torch.nn.DataParallel`，保存的模型进行推断也需要加载两次才能进行推断。可以通过以下方法将保存的模型转化为非DataParallel模式的模型（所有key的名字前去掉modules）**

```python
from collections import OrderedDict
from efficientnet import efficientnet_b0b
import torch.nn as nn
import torch

model_path = 'Result/efficientnet/07-22_13-15-51/1net_params.pkl'
state_dict = torch.load(model_path)
new_state_dict = OrderedDict()

for k, v in state_dict.items():
    name = k[7:]
    new_state_dict[name] = v

two_state_dict = OrderedDict()

for k, v in new_state_dict.items():
    name = k[7:]
    two_state_dict[name] = v

net = efficientnet_b0b((224, 224), num_classes=1852)
net = nn.DataParallel(net)
net.load_state_dict(new_state_dict)
```
