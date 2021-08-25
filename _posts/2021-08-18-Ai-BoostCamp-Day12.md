---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 12 - Pytorch(Autograd,Optimizer) "
date:   2021-08-18 15:13:20 +0530
categories: PyTorch
---


-


![image](https://user-images.githubusercontent.com/61610411/129822452-ebbb18ba-f945-47ad-b774-0aa693ec3a3a.png)



---
## torch.nn.Module

- 딥러닝을 구성하는 Layer의 Base class

- Input, Output. Forward, Backward 정의

- 학습의 대상이 되는 parameter(tensor) 정의


![image](https://user-images.githubusercontent.com/61610411/129822759-92034cab-d876-47c4-8283-93229d83a40e.png)


---


## torch.nn.Parameter

- Tensor 객체의 상속 객체

- nn.Module 내에 attribute가 될 때는 required_grad=True로 지정되어 학습 대상이 되는 Tensor

- 우리가 직접 지정할 일은 잘 없음
    : 대부분의 layer에는 weight 값들이 지정되어 있음


```py
class MyLiner(nn.Module):
    def __init__(self, in_features, outfeatures, bias=True):
        super().__init__()
        self.in_features = in_feartures
        self.out_features = out_features

        self.weights = nn.Parameter(
            torch.rand(infeatures, out_featrues))

        self.bias = nn.Parameter(torch.randn(out_features))
        
    def forward(self, x : Tensor):
        return x @ self.weights + self.bias
```


---
## Backward

- Layer에 있는 Parameter들의 미분을 수행

- Forward의 결과값 (model의 output = 예측치) 과 실제값간의 차이(loss) 에 대해 미분을 수행

- 해당 값으로 Parameter 업데이트

```py

for epoch in range(epochs):

    # Clear gradient buffers because we don't want any gradient from previous epoch to carry forward
    optimizer.zero_grad()

    # get output from the model, given the inputs
    outputs = model(inputs)  # y^

    # get loss for the predicted output
    loss = criterion(outputs, labels)  # y^ , y
    print(loss)
    
    # get gradient w.r.t to parameters
    loss.backward()

    # update paramteters
    optimizer.step()
```


---
## Backward from the scratch

- 실제 backward는 module 단계에서 직접 지정가능

- Module에서 backward 와 optimizer 오버라이딩

- 사용자가 직접 미분 수식을 써야하는 부담
    -> 쓸일은 없으나 순서는 이해할 필요는 있음


![image](https://user-images.githubusercontent.com/61610411/129824911-3c49ed64-aee2-45a9-b06f-b1cfe52cd1ad.png)



---
## 데이터를 모델에 입력하는 방법

![image](https://user-images.githubusercontent.com/61610411/129832680-d3587b9f-1795-442d-910d-bfe0f47b98f3.png)


---
## Dataset 클래스

-  데이터 입력 형태를 정의하는 클래스

- 데이터를 입력하는 방식의 표준화

- Image, Text, Audio 등에 따른 다른 입력 정의


```py

import torch
from torch.utils.data import Dataset


class CustomDataset(Dataset):
    def __init__(self, text, labels):
        self.labels = labels
        self.data = text

    def __len__(self):
        return len(self.labels)

    def __getitem__(self, idx):
        label = self.labels[idx]
        text = self.data[idx]
        sample = {"Text" : text, "Class":label}
        return sample

```


---
## Data set 클래스 생성시 유의점

- 데이터 형태에 따라 각 함수를 다르게 정의함

- 모든 것을 데이터 생성 시점에 처리할 필요는 없음
    :  image의 Tensor 변화는 학습에 필요한 시점에 변환

- 데이터 셋에 대한 표준화된 처리방법 제공 필요
    -> 후속 연구자 또는 동료에게는 빛과 같은 존재

- 최근에는 HuggingFace 등 표준화된 라이브러리 사용


---
## DataLoader 클래스

- Data의 Batch를 생성해주는 클래스

- 학습 직전(GPU feed전) 데이터의 변환을 책임

- Tensor 로 변환 + Batch 처리가 메인 업무

- 병렬적인 데이터 전처리 코드의 고민 필요

```py
text = ['Happy','Amazing','Sad','Unhappy','Glum']
labels = ['positive','positive','negative','negative','negative']


Mydattaset = CustomDataset(text,labels)

MyDataLoader = DataLoader(MyDataset, batch_size=2, shuffle=True)
next(iter(MyDataLoader))

# {'Text':['Glum','Sad'],'Class':['negative','negative']}

MydataLoader = DataLoader(Mydataset, batch_size=2, shuffle=True)
for dataset in MyDataLoader:
    print(dataset)

# {'Text':['Glum','Unhappy'], 'Class':['negative','negative']}
# {'Text':['Sad','Amazing'], 'Class':['negative','positive']}
# {'Text':['happy'], 'Class':['positive']}
```

```py
DataLoader(dataset, batch_size=1, shuffle=False, sampler=None,
            batch_sampler=None, num_workers=0, colaate_fn=None,
            pin_memory=False, drop_last=False, timeout=0,
            worker_init_fn = None, *, prefetch_factor=2,
            persistent_workers = False)
```

## Casestudy

- 데이터 다운로드부터 loader까지 직접 구현해보기

- NotMNIST 데이터의 다운로드 자동화 도전
