---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV Conditional Generative Model "
date:   2021-09-15 18:30:10 +0530
categories: GAN
tag: [CV, GAN, Generative, Conditional]
---

Conditional Generative Model

# 1. Conditional generative model

## 1.1 Conditional generative model

![image](https://user-images.githubusercontent.com/61610411/133294158-dcba3027-eb78-4290-b7f8-568585890f0e.png)

서로 다른 두 도메인을 translation 한다.! -> 하나의 정보(조건)이 주어졌으므로 condition 이라 칭함.

![image](https://user-images.githubusercontent.com/61610411/133294985-27715974-75d6-4193-815c-f48b3fc11e9a.png)

기본적인 Generative Model은 영상이나 샘플은 생성할 수는 있었지만 조작은 할 수 없었다. 생성을 더 유용하게 하기 위 해서는 어떤 형태로든 유저의 의도가 반영할 수 있다면 무수히 많은 응용 가능성이 생긴다. 그런 생성 모델 방식을 Conditional generative model 이라고 한다.

![image](https://user-images.githubusercontent.com/61610411/133295324-c4fae0f1-729f-427b-9ab2-fad4e4ccef01.png)

Low quality audio을 high quality audio로 높혀주는 것도 Conditional Generative model의 일종이다.

![image](https://user-images.githubusercontent.com/61610411/133295648-38542c6a-3a77-40d8-9299-3b5e1f06d60e.png)


![image](https://user-images.githubusercontent.com/61610411/133295798-a900220c-5b2d-4e1c-9dbd-122456afba9e.png)

![image](https://user-images.githubusercontent.com/61610411/133296033-226fcdd4-e8fd-4564-b36b-82d85d6a7cf1.png)

## 1.2 Conditional GAN and image translation

![image](https://user-images.githubusercontent.com/61610411/133296289-cc624003-1866-4c14-a1ab-65337bed4ced.png)


## 1.3 Example : Super resolution 

![image](https://user-images.githubusercontent.com/61610411/133296508-2e1c7be7-3593-4d4a-a560-00678a9a9e27.png)

![image](https://user-images.githubusercontent.com/61610411/133296589-4fa3534a-50c2-473a-9797-372e6d7e4656.png)

![image](https://user-images.githubusercontent.com/61610411/133561662-ce23b7b0-2ab3-4f09-b1ea-8a66838b3c5e.png)


![image](https://user-images.githubusercontent.com/61610411/133296862-5b683638-a961-492e-bc0c-a736cc512302.png)

![image](https://user-images.githubusercontent.com/61610411/133297708-ac286903-7b74-44c1-9bee-9e751ae3999f.png)

![image](https://user-images.githubusercontent.com/61610411/133297822-6217887e-a78a-48fc-b78f-5db9c2b17a28.png)


# 2. Image translation GANs

## 2.1 Pix2Pix

### Task definition


![image](https://user-images.githubusercontent.com/61610411/133297942-5e276cdf-e6d5-44a3-ad05-7768821e2db0.png)

### Loss function of Pix2Pix

![image](https://user-images.githubusercontent.com/61610411/133298157-a49f4763-4165-4c6f-a47d-842d48a02bbc.png)



Total loss (GAN loss + (L1 loss = MAE loss)) 를 쓰는 이유

1. y 가 ground Truth 인데 x 라는 입력을 넣었을때 기대하고 있는 출력 페어를 가지고 있는 데이터에 대해 학습을 진행한다.(supervised learning) 이 경우에, GAN Loss 만 사용한다면 입력된 두개의 페어(x,y)를 직접 비교하지않고 독립적으로 Discriminator 해서 real 이냐 fake 이냐에 대해서만 판별을 한다. 따라서 입력이 무엇이 들어와도 y와 직접 비교하지않기 때문에 GAN만 사용한다면 y와 비슷한 결과를 만들어 낼 수 없다. 따라서 L1 Loss 를 사용해서 기대하는 결과인 y 와 비슷한 영상이 나오고 GAN Loss 를 이용하여 reality 한 영상을 만들 수 있게 해준다.

2. GAN만으로 학습하기엔 굉장히 불안정하고 어렵기 때문에 학습이 조금 더 안정적으로 이루어질 수 있도록 보조하는 역할도 한다.

![image](https://user-images.githubusercontent.com/61610411/133354395-4cc80905-ef56-4727-9856-34f6b5d28bcc.png)

## 2.2 CycleGAN

![image](https://user-images.githubusercontent.com/61610411/133354535-1dc51d7a-5a76-4436-a8af-a1488a31d028.png)

지금까지의 Pix2Pix는 supervised learning 기법을 사용했기 떄문에 pairwise data가 필요하다.

하지만 pairwise data 를 항상 얻는게 어렵기 때문에 Unpaired data 데이터를 사용해서 X 와 Y 의 대응관계가 없이 data set으로만 주어졌을때 활용하는 방법에 대해서 고민했고 해결책으로 CycleGAN 이 제시되었다.


![image](https://user-images.githubusercontent.com/61610411/133357041-2bc21fe0-4f74-4d6e-969b-0f8cda98a2b9.png)

CycleGAN은 도메인간의 non-pairwise datasets 으로 translation 이 가능하도록 하게 해줌.

### Loss funtion of CycleGAN



![image](https://user-images.githubusercontent.com/61610411/133357231-16b978c6-ad5a-42ee-9a69-43631bb6bc08.png)

X -> Y 스타일로 가는 방향과 Y -> X 스타일로 가는 방향을 동시에 학습하여 모델링을 한다.

### GAN loss in CycleGAN

![image](https://user-images.githubusercontent.com/61610411/133357337-35564851-5cb5-4672-9878-d7f3f91cc2de.png)



### If we sloely use GAN loss

![image](https://user-images.githubusercontent.com/61610411/133357515-742c0b9d-720b-4f87-8297-4c78aa8d5663.png)

GAN Loss만 사용할 경우 (Mode Collapese issue) input에 상관없이 하나의 output 만 계속 출력하는 형태로 학습한다. -> GAN Loss 기준에선 좋은 Loss를 갖게 된다!

### Solution: Cycle-consistency loss to preserve contents

![image](https://user-images.githubusercontent.com/61610411/133357661-1531d0c2-e457-4933-b8b7-7b465351409b.png)



content 보존을 위한 Loss. x를 input으로 G를 활용해 생성한 y^가 F를 활용해 x^를 생성했을때 x와 x^가 비슷하도록 학습.


## 2.3 Perceptual loss

![image](https://user-images.githubusercontent.com/61610411/133469490-ffbc873a-e62e-4c50-acc3-de4f156c8deb.png)


![image](https://user-images.githubusercontent.com/61610411/133358037-5bb82611-39ce-4d94-ad11-132ac7b1700a.png)

high quality output 을 얻기 위한 Loss

### GAN is hard to train

![image](https://user-images.githubusercontent.com/61610411/133358377-4f5dfa1f-9c89-4c90-9ad9-0de1719144d2.png)

![image](https://user-images.githubusercontent.com/61610411/133358764-d429e800-f2d5-49cb-a871-508d49a2703f.png)

Loss 하나만을 추가함으로서 쉽게 style transfer를 할 수 있는 generator를 학습시킬 수 있다.

![image](https://user-images.githubusercontent.com/61610411/133358802-0f37cbb0-aeea-429f-98ef-7ce0b329ec63.png)

Image Transform Net : input image가 주어지면 하나의 style로 transformed 을 해준다.

Loss network(Fix된 VGG16) : image classification network -> feature 추출

![image](https://user-images.githubusercontent.com/61610411/133359098-deb688e5-7d4c-4055-9a05-f2b453362076.png)

input x 에서부터 y^ 생성한다.

y^이 의도한 바로 transform이 되기 위해 VGG 를 사용해 feature 를 추출하고 backpropagation 을 해서 중간 transform을 하려는 f를 학습한다.

Transformed image가 input x에 포함되어 있었던 Content 정보를 유지하고 있는지 권장해주는(?) Loss가 바로 Feature reconstruction loss 이다.

Content Target 은 원본 x 를 입력으로 넣어주고, 이후 VGG 를 통과하여 feature 를 추출한다. 그 다음 마찬가지로 transform된 이미지에 대해서도 feature를 추출한다. 이 두가지를 비교해서 Loss 를 분해한다.

이 두개를 분해할땐 L2 Loss 를 통해 backpropagation 을 이용하여 y를 업데이트 할 수 있게끔 해준다.

### Style reconstruction loss

![image](https://user-images.githubusercontent.com/61610411/133472780-d5c891a6-265b-41dd-916b-eebd439d0c37.png)

변환하고 싶은 style image 를 style target 으로 입력한다.

style target 과 transformed image 를 VGG에 입력으로 넣어줘서 feature를 추출한다.

**Gram Matrices** 

이미지 전반에 걸친 통계적 특성을 담기 위함.


# 3. Various GAN applications

## 3.1 Deepfake 
