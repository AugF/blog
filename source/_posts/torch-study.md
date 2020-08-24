---
title: torch-study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-11-03 21:46:03
tags:
categories:
---
https://blog.csdn.net/qq_31590831/article/details/90764240


## pytorch
https://www.cnblogs.com/denny402/p/7520063.html

一个完整的pipeline
```
from torch.utils.data import Dataset, DataLoader
from torch.autograd import Variable

class MyDataset(Dataset):
    def __init__(self, txt, transform,):
        self.imgs = 
        self.transform =
    def __getitem__(self, index):
        pass
    def __len__(self):
        pass

train_data = MyDataset(txt=".txt", transform)
train_loader = DataLoader(dataset=, batch_size=, shuffle=True)

class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = torch.nn.Sequential(
            torch.nn.Conv2d(3, 32, 3, 1, 1),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(2)
        )
        self.conv2 = torch.nn.Sequential(
            torch.nn.Conv2d(32, 64, 3, 1, 1),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(2)
        )
    def forward(self, x):
        conv1_out = self.conv1(x)
        conv2_out = self.conv2(conv1_out)
        conv3_out = self.conv3(conv2_out)
        out = self.dense(conv3_out)
        return out

model = Net()
print(model)

optimizer = torch.optim.Adam(model.parameters())
loss_func = torch.nn.CrossEntropyLoss()

for epoch in range(10):
    print('epoch {}'.format(epoch + 1))
    # training
    train_loss = 0
    train_acc = 0
    for batch_x, batch_y in train_loader:
        batch_x, batch_y = Variable(batch_x), Variable(batch_y)
        out = model(batch_x)
        loss = loss_func(out, batch_y)
        train_loss += loss.data[0]
        pred = torch.max(out, 1)[1]
        train_correct = (pred == batch_y).sum()
        train_acc += train_correct.data[0]
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    print('Train Loss: {:.6f}, Acc: {:.6f}'.format(train_loss / (len(
        train_data)), train_acc / (len(train_data))))
```

net.zero_grad():
optimizer.zero_grad():
在下一次计算梯度之前清空梯度

optimizer.step(): 进行下一步更新


查看grad
https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html#variablehttp://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html#variable

```
import torch
x = torch.ones(2, 2, requires_grad=True)
y = 2 * x + 2 #y.data, y.grad, y.grad_func
out = y.sum()
out.backward()
x.grad # [[2, 2], [2, 2]]
```

单看Variable和Tensor没什么区别，个人理解对于Variable可能能保证计算过程一直是grad