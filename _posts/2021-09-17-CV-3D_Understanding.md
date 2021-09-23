---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV 3D Understanding "
date:   2021-09-17 18:30:10 +0530
categories: 3D Understanding
tag: [CV, 3D, recognition, object detection, detection, segmentation]
---


# 3D Understanding

## Seeing the world in 3D persprective

### We live in a 3D space

3D가 중요한 이유?

- 우리가 3D World 에 존재하고 살아가기 때문에 3D space에 대한 이해가 중요하다.

![](https://i.imgur.com/0wRnffd.png)

### 3D application - AR/VR/Medical application
    
![](https://i.imgur.com/qqLvgXa.png)

![](https://i.imgur.com/2r1puUB.png)

- 우리의 몸을 구성하는 요소들이 전부 3D로 구성되어있기 때문에 이러한 과정을 이해하는 것이 중요하다.

## 1.2 The way we observe 3D


![](https://i.imgur.com/coAQPzF.png)

한 가지 point 에 대한 두 가지 위치정보를 알고 있다면 3D point 를 알 수 있는데 이것을 Triangulation 이라고 한다.

## 1.3 3D data representation

![](https://i.imgur.com/PwapHuv.png)

3D image를 2D array에 각각 value 를 저장함. (color일 경우 3차원)

![](https://i.imgur.com/kkghlwA.png)

**Multi-view image** 에서는 각각의 위치에서 저장한 사진을 이용해 3D를 표현하는 방법이다.

**Volumetric(voxel)** 은 3D space를 격자로 나누어 격자가 3D object가 차지하고 있는지에 대한 것을 0과 1의 binary 형태로 나타낸다.

**part assembly**는 3D object를 간단한 기본적인 도형으로 parametic한 집합으로 part를 표현하는 방식이다.

**Point cloud**는 3D 상에 있는 point들의 집합을 통해 표현하는 방법이다. (point 는 x,y,z의 위치정보를 나타냄)

**Mesh(Graph CNN)** 는 (x,y,z)의 3D point를 삼각형 형태로 나타내고(vortax) 두 점을 잇는 선(edge)과 같이 포함하여 삼각형(면)을 나타내어 표시한다.

**Implicit shape**는 3D를 고차원의 function 형태로 나타내고 0과 교차하는 부분을 표시하면 3D로 나오게 되는 방법이다.

## 1.4 3D datasets

![](https://i.imgur.com/ckiMsVD.png)

![](https://i.imgur.com/dCYFbIN.png)

- part들의 instance도 구별되어 있음.

![](https://i.imgur.com/u8TSUXP.png)

![](https://i.imgur.com/uPO4jkp.png)

![](https://i.imgur.com/x0LixNN.png)

- 자율주행을 고려한 Data set

# 2. 3D tasks

## 2.1 3D recognition

![](https://i.imgur.com/YgXY9Bo.png)

- Recognition 이나 Detection (3D bounding box) 을 활용할 수 있다.

![](https://i.imgur.com/HIgyJKV.png)

## 2.2 3D object detection

![](https://i.imgur.com/GoTPhmW.png)

- 무인 차 application 에서 활용 가능성

## 2.3 3D semantic segmentation

![](https://i.imgur.com/kE1SBhA.png)

<center>3D object에 대해 각 part를 segmenation
</center>

## 2.4 Condition 3D generation

![](https://i.imgur.com/b0R3Szh.png)

![](https://i.imgur.com/IdvUOFx.png)

한 개의 ROI 에서 각각의 feature(Bounding box, Class, Mask) brach 들을 추출한다.

![](https://i.imgur.com/SeOsDw0.png)

<center>Mash R-CNN 에선 3D branch(mash) 가 추가된다.</center>

![](https://i.imgur.com/lNqycXx.png)

<center>더 정교한 3D를 구성하기 위한 3D object Decomposing</center>
