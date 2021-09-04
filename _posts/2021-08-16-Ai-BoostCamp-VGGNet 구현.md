---
layout: post
title:  "부스트캠프 AI Tech 2기 - VGGNet 구현해보기"
date:   2021-08-16 22:15:12 +0530
categories: DL TIL
---

-

![image](https://user-images.githubusercontent.com/61610411/129582681-c4830718-6f64-47ca-980d-81fd8d997a31.png)

## VGGNet 설명


VGGNet -> 망의 깊이가 어떠한 영향을 주는지 밝히기 위한 실험이 시초

![image](https://user-images.githubusercontent.com/61610411/129577251-9d68d59e-6c4b-4ac5-8bf0-7f2535241a31.png)


![image](https://user-images.githubusercontent.com/61610411/132096623-c6d8cf16-059f-4771-a2b9-45d2cb367c56.png)


#### 작은 필터로 네트워크층을 깊게 쌓은 모델

#### 같은 layer 들을 묶어서 block 으로 처리함.



---

## 3x3 Conv를 사용하는 이유

![image](https://user-images.githubusercontent.com/61610411/132096706-d88559ff-5955-43e4-8fa0-48dc715a22fa.png)

#### 정보를 압축하고, 연산량을 줄일 수 있다.

---

## VGGNet 구현해보기

```python
import torch
print(torch.__version__)
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
```


### Transform, Data Set 준비

```python
import torchvision
import torchvision.transforms as transforms

transform = transforms.Compose([
                                transfroms.Resize(256),
                                transforms.RandomCrop(224), # 오려내기
                                transforms.ToTensor(),
                                transforms.Normalize((0.5,0.5,0.5),(0.5,0.5,0.5)),
])

trainset = torchvision.datasets.STL10(root='./data', split='train', download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=32,shuffle=True)

testset = torchvision.datasets.STL10(root='./data', split='test', download=True, transform=transform)
testloader = torch.utils.data.DataLoader(testset, batch_size=32,shuffle=False)
```

### VGGNet의 구조

![image](https://user-images.githubusercontent.com/61610411/129583063-671d5503-afa2-447a-bfbd-a4dbe843463f.png)


우리는 VGG-A를 구현해볼것 (나머지와 층수만 차이남)


#### What is Tensor Factorization ?

Tensor Factorization 은 Tensor Decomposition과 같은 말로 TF는 행렬을 주어진 요소들로 쪼개는 위의 문제를 고려한다. 하지만 MF(Matrix Factorization) 알고리즘은 2차원의 행렬에 적합하다. 이제는 이 문제를 일반화 시키고, 행렬의 차원을 증가시키는 것이다. 이렇게 확장된 행렬을(Tensor) Factorization 하는 것을 Tensor Factorization 이라고 부른다.

![image](https://user-images.githubusercontent.com/61610411/129584366-d5f76d4a-f21c-460a-9bed-3a355e42c58e.png)


### 모델 설계


```py
import torch
import torch.nn as nn

class VGG_A(nn.Module):
    def __init__(self, num_classes:int=1000, init_weights : bool = True):
        super(VGG_A, self).__init__()
        self.net = nn.Sequential(
            # input channel (RGB : 3)
            nn.Conv2d(in_channels=3, out_channels=64, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2), # 224 -> 112

            nn.Conv2d(in_channels=64, out_channels=128, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2), # 112 -> 56

            nn.Conv2d(in_channels=128, out_channels=256, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(in_channels=256, out_channels=256, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2), # 56 -> 28

            nn.Conv2d(in_channels=256, out_channels=512, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(in_channels=512, out_channels=512, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2), # 28 -> 14

            
            nn.Conv2d(in_channels=512, out_channels=512, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(in_channels=512, out_channels=512, kernel_size=3, padding=1, stride=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2), # 14 -> 7
        )
        self.fclayer = nn.Sequential(
            nn.Linear(512 * 7 * 7, 4096),
            nn.ReLU(inplace=True),
            nn.Dropout(0.5),
            nn.Linear(4096,4096),
            nn.ReLU(inplace=True),
            nn.Dropout(0.5),
            nn.Linear(4096, 1000),
            #nn.Softmax(dim=1), # loss인 Cross Entropy Loss에서 softmax를 포함한다

        )

    def forward(self, x:torch.Tensor):
        x = self.net(x)
        x = torch.flatten(x,1)
        x = self.fclayer(x)
        return x

        

            

print("Done.")
```


### Train

```python
import time
vgg_a = VGG_A(num_classes=10)
vgg_a = vgg_a.to(device)

classes = ('airplance','bird','car','cat','deer','dog','horse','monkey','ship','truck')

import torch.optim as optim

criterion = nn.CrossEntropyLoss().cuda()
optimizer = optim.Adam(vgg_a.parameters(), lr=0.001)
start_time = time.time()
for epoch in range(10): # loop over the dataset multiple times
    running_loss = 0.0
    for i, data in enumerate(trainloader,0):
        #get the inputs
        inputs, labels = data
        inputs, labels = inputs.to(device), labels.to(device)

        #zero the parameter gradients

        optimizer.zero_grad()

        #print(input.shape)
        outputs = vgg_a(inputs)

        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        running_loss += loss.item()

        if i % 30 == 29:
            print('[%d, %5d] loss: %.3f' % (epoch + 1, i + 1, running_loss/50))
            running_loss = 0.0

print(time.time()-start_time)
print('Finished Training')
```


### Validation


```python
class_correct = list(0. for i in range(10))
class_total = list(0. for i in range(10))

with torch.no_grad():
    for data in testloader:
        images, labels = data
        images = images.cuda()
        labels = labels.cuda()
        outputs = vgg_a(images)
        _, predicted = torch.max(outputs,1)
        c = (predicted == labels).squeeze()

        for i in range(4):
            label = labels[i]

            class_correct[label] += c[i].item()
            class_total[label] += 1


for i in range(10):
    print('Accuracy of %5s : %2d %%' % (
        classes[i], 100 * class_correct[i] / class_total[i]))

```