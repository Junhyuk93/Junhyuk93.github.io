---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV Object detection "
date:   2021-09-08 21:30:10 +0530
categories: Computer Vision
tag: [CV, Object Detection, Detection, R-CNN, Fast R-CNN, YOLO, SSD, RPN, NMS RetinaNet, Focal Loss]
---
object detection


# Object detection

---

## 1. Object detection

### 1.1 what is object detection?

#### image 인식 분류의 기초

![image](https://user-images.githubusercontent.com/61610411/132944092-a7238332-6588-40f9-84dc-8df154fa3a4a.png)

Semantic segmentation와 imstance segmentation, panoptic segmentation 의 차이점은 같은 클래스라도 객체가 다르다면 구분이 가능한지 아닌지의 여부이다. 

![image](https://user-images.githubusercontent.com/61610411/132944207-5f0cb526-0a92-40a3-ab35-081ae7c6e501.png)


Object detection 은 Classification 과 Box localization 을 동시에 추정하는 문제.


### 1.2 What are the applications of object detection?

![image](https://user-images.githubusercontent.com/61610411/132944247-ddc09ead-9580-441c-aff4-44254d68ae3e.png)

![image](https://user-images.githubusercontent.com/61610411/132944273-816f9510-de04-40e4-918c-ebc82f1aa4ab.png)

그림과 같이 자율주행에 이용되기도 한다.

![image](https://user-images.githubusercontent.com/61610411/132944363-10209144-a305-40e2-a17d-f04c6c7eea0b.png)

![image](https://user-images.githubusercontent.com/61610411/132944339-cbac10c5-b214-41e7-a230-780f8d7aaf56.png)

Optical Character Recognition 에서도 복잡한 배경 속에서 글자를 인식하여 위치를 특정해야 하기 때문에, object detection이 필요하다.

---


## 2. **Two-stage detector** (Regional proposal과 Classification이 순차적으로 진행)

![image](https://user-images.githubusercontent.com/61610411/132944469-f31d34d5-6e3f-4916-bc04-50fc87446d0f.png)

Neural network 이전의 object detection 을 해결하기 위한 과정들.

**Selective search**

![image](https://user-images.githubusercontent.com/61610411/132944563-268c02fc-1668-49f8-bb92-7cee7ddcf2fc.png)

1. 다양한 물체들에 대해서 Bounding box 를 제한을 해준다. 

2. 영상을 비슷한 색끼리 잘게 분할한 후(Over-segmentation) 분할된 영역들을 비슷한(색깔이나 gradient의 유사성) 영역끼리 반복해서 합쳐준다. 

3. 이를 반복하면 큰 segmentation 이 만들어지고, 이를 포함하는 Bounding box를 설정해 물체의 후보군으로 사용한다.

![image](https://user-images.githubusercontent.com/61610411/132944923-aaab11b1-36d2-4b4e-b681-b86b4b15031d.png)


- 기존 Object detection 의 단점

    - 기존에는 sliding window 방식으로, 물체가 있을만 한 곳을 하나씩 탐색하여 물체를 탐색했다. Sliding window 방식은 크기가 고정되지 않아 여러 크기로도 순차적으로 계산해야 하기 때문에 경우의 수가 폭발적으로 증가하여 이를 분류하기 위해서는 연산량이 많아지고 연산시간이 오래걸려 비효율적이다.

### 2.1 R-CNN  

![image](https://user-images.githubusercontent.com/61610411/132944705-b05db4f1-6a07-4625-a799-e5e45e54ef69.png)


첫 deep learning 기반 object detection 

R-CNN은 기존 object detection의 단점을 보완하고자 Region Proposal 이라는 방식을 제안한다.

Selective search를 활용해 물체가 있을 법한 장소를 2000개 탐색하고, 이에 대해서 분류를 진행한다.

[R-CNN 논문](https://arxiv.org/abs/1311.2524 "R-CNN 논문")

위 논문에서는 물체가 있을법한 장소를 추출한 뒤, 크기에 상관없이 227x227로 이미지를 압축한다.

그리고 이렇게 모인 이미지를 CNN을 거쳐 feature vector를 계산하고 Linear SVM을 통해 object를 분류하게 된다.


![image](https://user-images.githubusercontent.com/61610411/132945027-d2a00430-bddf-4f63-b57c-7870dd40676f.png)


### 2.2 Fast R-CNN 

#### 이전 R-CNN 의 문제점
    1. 학습이 multi-stage pipline으로 이루어진다.

    2. 학습에서 경제성이 떨어진다.

    3. object detection이 느리다.

![image](https://user-images.githubusercontent.com/61610411/132945224-a4e0f7be-4bea-433a-878c-15af5d125ebd.png)

**이러한 문제를 해결하기 위한 Fast R-CNN 의 출현**


#### Fest R-CNN 모델 구조

    1. input image와 multiple regions of interest(ROI = 이미지상 내가 관심있어하는 일부 영역)가 입력으로 사용된다.

    2. 각각의 ROI는 ConvNet 연산을 통해 고정된 크기의 ROI에 해당하는 feature map 으로 pooling되고,
    
    FC layers 를 통해 feture vector로 맵핑된다.

    3. ROI 별 두개의 output을 가지고, 더 정밀한 bounding box 를 추정하기 위해
    
    bounding box regressor과 softmax 를 통한 classification 을 수행한다. 


[Fast R-CNN](https://arxiv.org/abs/1504.08083 "Fast R-CNN 논문")

![image](https://user-images.githubusercontent.com/61610411/132945640-e7ac4908-7b00-4762-8018-449dff7e4a1d.png) - ROI Pooling layer


### 2.3 Faster R-CNN


![image](https://user-images.githubusercontent.com/61610411/132946044-0a907b2c-cd90-4518-b22d-a2edfa325b27.png)

IOU = (두 영역의 교집합 / 두 영역의 합집합)

##### Anchor boxes

![image](https://user-images.githubusercontent.com/61610411/132947184-eca288bf-576e-4a61-bf37-477f212c0552.png)

- 직관적으로 설명하자면, 동일한 크기의 sliding window를 이동시키며 window의 위치를 중심으로 사전에 정의된 다양한 비율/크기의 anchor box들을 적용하여 feature를 추출하는 것이다. 이는 image/feature pyramids처럼 image 크기를 조정할 필요가 없으며, multiple-scaled sliding window처럼 filter 크기를 변경할 필요도 없으므로 계산효율이 높은 방식이라 할 수 있다. 논문에서는 3가지 크기와 3가지 비율의, 총 9개의 anchor box들을 사용하였다. positive 한 box 들과 negative 한 box 들의 loss를 줘서 학습하도록 유도한다.

![image](https://user-images.githubusercontent.com/61610411/132947235-3d7a90cc-86da-426a-a473-ec7c66cd769b.png)

**core contribution = RPN(Region Proposal Network)**

![image](https://user-images.githubusercontent.com/61610411/132947299-206ce91f-a427-41c7-a51d-f326183e77dd.png)

#### RPN 의 구조

    - feature map 관점에서 fully convolution 하게 sliding window 방식으로 이동하며 매 위치마다 k 개의 anchor box를 고려한다.
    
    각 위치에서 256 dimension 의 feature map 을 추출하고 여기서 2k 개의 classification score 를 출력한다. 
    
    그리고 4k 개의 정교한 bounding box를 만들기 위한 regression output 을 출력한다.

### NMS (Non Maximum Suppression)

- 1. 동일한 클래스에 대해 높은-낮은 confidence 순서로 정렬한다. 

- 2. 가장 confidence가 높은 boundingbox와 IOU가 일정 이상인 boundingbox는 동일한 물체를 detect했다고 판단하여 지운다.

![image](https://user-images.githubusercontent.com/61610411/133033841-ac80b73b-c776-4b83-940b-843cfbfcf707.png)


![image](https://user-images.githubusercontent.com/61610411/132947440-c139d679-1d92-43e0-9a27-31fd11cc7ced.png)


### R-CNN Famaily

![image](https://user-images.githubusercontent.com/61610411/132947513-08aa90f6-c425-4a97-a758-533c1a1914e3.png)

---

## 3. Single-stage detector (Region Proposal과 Detection 를 한번에 수행)

#### One-stage vs tow-stage

![image](https://user-images.githubusercontent.com/61610411/132947579-aae1da68-09d3-449b-b7b3-dcb61340194e.png)

- Single-stage detection 은 정확도를 조금 포기하더라도 속도를 확보해 real-time detection 을 가능하도록 설계하는 것에 목적을 둠.

### 3.1 You only look once (YOLO)

![image](https://user-images.githubusercontent.com/61610411/132947639-c53011b6-f341-4977-b4e5-42565be09530.png)

1. input 이미지를 SxS grid로 나눈 후, 각 grid 에 대해서 b 개의 bounding box(4개의 좌표)와 bounding box의 confidence score 를 갖는다. (*confidence score 란 박스가 객체를 보유하고 있다고 생각하는 모델의 신뢰도와 예측하는 박스의 정확도를 반영한다.)

2. 각 위치마다 Class probability map 

![image](https://user-images.githubusercontent.com/61610411/132947792-a03757c6-aa62-47c5-b7c2-1efcb9f1eda0.png)

최종 결과로 NMS 알고리즘을 통해 전개된 bounding box 만을 출력한다. (bounding box 구성 : x, y, w, h, confidence)

![image](https://user-images.githubusercontent.com/61610411/132947821-567d9f6a-65af-469a-a507-23e5b1ebf3ea.png)

##### YOLO 구조

![image](https://user-images.githubusercontent.com/61610411/132948126-5ef2dfd7-f951-481c-b634-e236329b6ba3.png)

[YOLO 논문](https://arxiv.org/abs/1506.02640 "YOLO 논문")


### 3.2 Single Shot MultiBox Detector (SSD)

YOLO 는 마지막 layer에서 1번만 prediction 을 하기 때문에 localization 정확도가 떨어지는 결과를 나타냈다. 이를 보완하기 위해 SSD가 나왔다. SSD는 Multi-scale object를 더 잘 처리하기 위해서 중간 feature map을 각 해상도에 적절한 bounding box 들을 출력할 수 있도록 multi-scale 구조를 만들었다.

![image](https://user-images.githubusercontent.com/61610411/132948005-fd9fca6b-916c-4c5a-9318-c366d6473edd.png)

##### SSD 구조

![image](https://user-images.githubusercontent.com/61610411/132948080-bb837926-c180-4a53-bf22-ba841c6373d2.png)


## 4. Two-stage detector vs. one-stage detector

### 4.1 Focal Loss

![image](https://user-images.githubusercontent.com/61610411/132948511-335ea2ba-6adf-4455-bd65-bc16a6c5f7ba.png)

single-stage 방법들은 ROI pooling이 없다보니 모든 영역에서의 Loss가 계산이 되고, 일정 gradient 가 발생한다. 따라서 유용하지 않은 영역에서 오는 정보들로 Class imbalance 문제를 가지고 있다.

이러한 Class imbalance 해결하기 위한 방법으로 Focal Loss 가 제안되었다. 

![image](https://user-images.githubusercontent.com/61610411/132948584-09de7701-25e9-4c38-99aa-411af3c421bf.png)

Focal loss는 분류 에러에 근거한 loss에 가중치를 부여하는데,  샘플이 CNN에 의해 이미 올바르게 분류되었다면 그것에 대한 가중치는 감소한다. 즉, 좀 더 문제가 있는 loss에 더 집중하는 방식으로 불균형한 클래스 문제를 해결했다. Focal loss는 Sigmoid activation을 사용하기 때문에, Binary Cross-Entropy loss라고도 할 수 있다. 특별히, $r$ = 0 일때 Focal loss는 Binary Cross Entropy Loss와 동일하다. (정답에 대한 gradient 보다 오답에 대한 gradient 를 중점으로 두고, 기울기에 대해 생각해볼 것 -> $r$ 이 클때 오답에 대한 Loss 의 기울기가 더 크다.)

### 4.2 RetinaNet

![image](https://user-images.githubusercontent.com/61610411/132948739-49d55e2e-ea0b-49c9-8f10-c9495b4c4285.png)

Low level 특징과 high level 특징을 둘다 잘 활용하면서도 각 scale 별로 물체를 잘 찾기 위한 Multi-scale 구조를 갖기 위해 이렇게 생겼음.

## 5. Detection with Transformer


### DETR


![image](https://user-images.githubusercontent.com/61610411/132950272-2dc93246-a750-4019-8d50-1bb5d9385f63.png)


![image](https://user-images.githubusercontent.com/61610411/132950287-674584d9-a959-43d4-abd2-d523b86ca754.png)

DETR 은 Transformer 를 object detection 에 적용한 사례이다. 기본적인 CNN 의 feature 와 각 위치의 multi-dimension 으로 표현한 encoding을 쌍으로 해서 input token 을 만들어준다. 이렇게 encoding 이 된 후 transformer encoder 를 거치게 되고, 정리된 특징들을 decoder 에 넣어준다. 이후 object queries 를 활용하여 token 이 나타내는 object 가 무엇인지에 대한 질의를 한다. 이후 token 의 위치 정보를 파싱해서 결과값이 출력된다.


[DETR 설명 나동빈님 유튜브](https://www.youtube.com/watch?v=hCWUTvVrG7E "DETR 설명 나동빈님 유튜브")

![image](https://user-images.githubusercontent.com/61610411/132950345-6ea0c91e-479a-478d-a11b-a4b944ee354c.png)

