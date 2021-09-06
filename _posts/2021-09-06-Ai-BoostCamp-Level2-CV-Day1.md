---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV Day 1 "
date:   2021-09-06 21:30:10 +0530
categories: Computer Vision
tag: [CV, augmentation]
---


-

# Data augmentation

---

### 1.1 Learning representation of data set

데이터 셋은 대부분 편향적이다.

![image](https://user-images.githubusercontent.com/61610411/132165638-cd4f18f4-2a08-4689-8c22-e3d0af8e3e80.png)

#### train data (=camera image) 들은 실제와 다른 image 들을 표현한다.

![image](https://user-images.githubusercontent.com/61610411/132165763-a1138af7-e2be-4691-bfb7-80bc78c50f7d.png)

![image](https://user-images.githubusercontent.com/61610411/132165802-7e803fb6-2130-4bf2-90e4-a9fa04674cbb.png)

#### 편향된 이미지로부터 모델은 Robust하게 학습할 수 없다. (=근본적으로 dataset이 real data를 충분히 표현하지 못했기 때문에)

![image](https://user-images.githubusercontent.com/61610411/132165930-e3492c4e-1954-4ec0-93f3-a7a571ebf404.png)

#### 몇몇 데이터들이 주어졌을 때, 이 데이터에 옵션을 추가해서 모델이 더욱 다양성 있게 학습할 수 있도록 해주는 것이 Augmentation 이다.

---

### 1.2 Data augmentation

- dataset 으로 부터의 다양한 이미지 변환을 적용시키기

    - Crop, Shear, Brightness, Perspective, Rotate 등

- OpenCV나 NumPy 라이브러리에 사용하기 쉽게 잘 구현되어 있다.

### **목적 : training dataset과 real dataset의 분포를 유사하게 만드는 것!**

---

### 1.3 Various data augmentation methods

- Various brightness in dataset

![image](https://user-images.githubusercontent.com/61610411/132166441-c5cb18e4-461d-400e-bcfc-18c7fff62251.png)

```py
def brightness_augmentation(img):
    #numpy array img has RGB value(0~255) for each pixel
    img[:,:,0] = img[:,:,0] + 100 # add 100 to R value
    img[:,:,1] = img[:,:,1] + 100 # add 100 to R value
    img[:,:,2] = img[:,:,2] + 100 # add 100 to R value

    img[:,:,0][img[:,:,0]>255] = 255 # clip R values over 255
    img[:,:,1][img[:,:,1]>255] = 255 # clip G values over 255
    img[:,:,2][img[:,:,2]>255] = 255 # clip B values over 255
```

- Rotate, flip

```py

img_rotated = cv2.rotate(image, cv2.ROTATE_90_CLOCKWISE)
img_flipped = cv2.rotate(image, cv2.ROTATE_180)

```

![image](https://user-images.githubusercontent.com/61610411/132169107-da039e24-b9cc-4334-9c4c-52b4254d314f.png)


- Crop

```py
y_start = 500
crop_y_size = 400
x_start = 300
crop_x_size = 800
img_cropped = image[y_start : y_start + crop_y_size, x_start : x_start + crop_x_size, :]
```

![image](https://user-images.githubusercontent.com/61610411/132169584-7d5e21ef-a721-4eb6-9f9f-32178f542995.png)


- Affine transformation

    - Preserve 'line', 'length ratio', and 'parallelism' in image

```py
rows, cols, ch = image.shape
pts1 = np.float32([[50,50],[200,50],,[50,200]])
pts2 = np.float32([[100,100],[200,50],,[100,250]])
M = cv2.getAffineTransform(pts1,pts2)
shear_img = cv2.warpAffine(image, M, (cols,rows))
```

![image](https://user-images.githubusercontent.com/61610411/132169724-c58456a3-56a1-4bf5-b1d9-f2267b0853fb.png)

![image](https://user-images.githubusercontent.com/61610411/132169987-6f832eca-328c-4869-a1de-b89748947902.png)



### Modern augmentation techniques

- Cutmix

    - Mixing both images and labels

![image](https://user-images.githubusercontent.com/61610411/132170236-2b79daec-a3e2-4f53-83aa-8713fc672384.png)

- RandAugment

    - 다양한 augmentation 방법이 존재한다. 적용시킬 최적의 augmentation of sequence를 자동으로 찾아준다.

![image](https://user-images.githubusercontent.com/61610411/132170503-5f3c172b-6eb4-48d8-a90b-431cd89ae37e.png)

- 어떠한 augmentation 을 사용할 지? 강도(magnitude)는 얼마나 적용시킬 것 인지? 를 parameter로 받는다.


---

## 2. Leverageing pre-trained information

### 2.1 Trainsfer learning

- 기존의 미리 학습시켜놓은 사전 지식을 활용하여 연관된 새로운 Task 의 적은 노력으로도 더 높은 성능으로 도달이 가능한 방법이다.

- A Dataset 에서 배운 지식을 B Dataset 에서 활용 가능하다.

![image](https://user-images.githubusercontent.com/61610411/132175370-bfdbf323-fab5-4581-9bb2-350dee96447f.png)


#### Approach 1 : pre-trained 된 모델의 마지막 Layer 를 새로운 FC Layer 로 추가하여 학습시키는 방법.

![image](https://user-images.githubusercontent.com/61610411/132175727-0f61a31d-5627-419f-a705-a668c77a7798.png)


#### Approach 2 : pre-trained 된 모델의 마지막 Layer 를 새롭게 교체하고 전체 model set 에 대하여 learning rate를 다르게 주어 학습하는 방법.

![image](https://user-images.githubusercontent.com/61610411/132176076-1f77a2f0-f95d-473c-9f4c-5378d4095985.png)

### 2.2 Knowledge distillation

- 이미 학습된 Teacher Network 지식을 더 작은 모델인 Student Network 에 주입을 시켜 학습한다. (big model -> small model로 지식 전달)

- 최근엔 Teacher 에서 생성된 output 을 Unlableled 된 데이터에 가짜 label으로 자동 생성하는 매커니즘으로 사용한다. 그렇게해서 가짜 label로 더 큰 student network 를 사용할 때도, regularization 역할로 사용할 수 있다. ( 더 많은 데이터를 사용할 수 있기 때문에)

![image](https://user-images.githubusercontent.com/61610411/132176650-099ca628-5f2e-42de-af50-b70b0793c721.png)

soft / hard voting

![image](https://user-images.githubusercontent.com/61610411/132238357-b169bca0-b314-4f35-85ac-3e2fb55a0730.png)

softmax with temperature

![image](https://user-images.githubusercontent.com/61610411/132238456-195c43f5-4abb-42f8-a864-4bcb01553793.png)

Distillation Loss

- KLdiv(Soft label, Soft prediction)
- Loss = difference between the teacher and student network's inference
- Learn what teacher network knows by mimicking

Student Loss

- CrossEntropy(Hard label, Soft prediction)
- Loss = difference between the student network's inference and true label
- Learn the "right answer"


## 3. Leveraging unlabeled dataset for training