---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage Object Detection "
date:   2021-09-27 21:30:10 +0530
categories: Object Detection
tag: [CV, Object Detection, Detection, Competition]
---

# Object Detection Overview

## Backgrownd for Object Detection Task

### 2.1 Task

![](https://i.imgur.com/BABP6SM.png)

이미지 안에서 객체를 검출하는 Task이다.

![](https://i.imgur.com/KpuCS7D.png)

Semantic segmentation 은 객체의 영역을 구분하는 Task 이다. 같은 클래스를 구분하진 않는다.

반면에 Instance Segmentation 객체의 영역을 구분하고, 어떤 클래스인지에 대해서 까지 구분하는 Task이다.

### 2.2 Real World

![](https://i.imgur.com/onsQRRi.png)

![](https://i.imgur.com/fkTSrGo.png)


### 2.3 History

![](https://i.imgur.com/DHkQ7ul.png)



### 2.4 Evaluation


- 성능
    - mAP (mean average precision) : 각 클래스당 AP의 평균
    

- 속도
    - FPS
    - Flops
    
---

**mAP를 계산하기 위해 필요한 개념**
- Confusion matrix
- Precision & Recall
- PR curve
- AP (Average Precision)
- IOU (Intersection Over union)

#### Confusion matrix

![](https://i.imgur.com/n5kfX1p.png)

#### Precision & Recall

![](https://i.imgur.com/hCYSIWS.png)

![](https://i.imgur.com/kMJ87HP.png)

![](https://i.imgur.com/Jsv1L6a.png)


#### PR Curve

![](https://i.imgur.com/GThRIQs.png)

![](https://i.imgur.com/yhf5kNO.png)

![](https://i.imgur.com/K8LXp2G.png)

#### AP

![](https://i.imgur.com/FAkL4BE.png)

PR Curve를 그린 후 아래 면적을 구하면 AP가 된다.

#### mAP

![](https://i.imgur.com/3MccyLN.png)


#### IOU

![](https://i.imgur.com/PMjZNSp.png)

![](https://i.imgur.com/Kxji6cc.png)

#### FPS

![](https://i.imgur.com/8zNpmuD.png)

#### FLOPs (Floating Point Operations)

![](https://i.imgur.com/UJpWKP0.png)

![](https://i.imgur.com/eplO0f5.png)


### 2.5 Library

![](https://i.imgur.com/wDky8KS.png)

![](https://i.imgur.com/K7qsTTi.png)

![](https://i.imgur.com/JW9CPZO.png)

![](https://i.imgur.com/NrBqDNn.png)


## Lecture

### 3.1 Curriculum

1. Onject Detection Overview
2. 2 Stage Object Detectors
3. Object Detection Library
4. Neck
5. 1 Stage Object Detectors
6. EfficientDet
7. Advanced Object Detection 1
8. Advanced Object Detection 2
9. Ready For Competition !
10. Object Detection in Kaggle

![](https://i.imgur.com/pdX4Z8C.png)



### 3.2 Special Mission


![](https://i.imgur.com/ZX79z3S.png)

### 3.3 ETC

![](https://i.imgur.com/cCgF80G.png)
