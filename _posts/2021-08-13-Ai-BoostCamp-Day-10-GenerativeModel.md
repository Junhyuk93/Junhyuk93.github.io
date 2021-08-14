---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 10 - Generaltive Model "
date:   2021-08-13 21:34:35 +0530
categories: DL
---

-

## Generative Model

![image](https://user-images.githubusercontent.com/61610411/129374222-d7425715-57da-4d26-a6a1-7dde732f2a88.png)



Auto-regressive model

NADE

Gaussian Mixture Model

Pixel RNN

---
## Taxonomy of Machine Learning

GAN 모델을 설명하기 전에 딥러닝을 크게 두 가지로 나누면, Supervised Learning과 Unsupervised Learnging이 있다.

![image](https://user-images.githubusercontent.com/61610411/129442689-18d16f3e-a91f-4268-b6be-5e4bbdfeeb70.png)

![image](https://user-images.githubusercontent.com/61610411/129442702-ce611041-a1ec-4b0f-b644-35e0bf699a6b.png)



- **Supervised Learning** 지도 학습

대표적인 모델로 **Discriminative Model**이 있으며, 로지스틱 회귀분석, 뉴럴 네트워크 등이 해당된다. input에 해당하는 클래스를 맞추기 위해 학습하게 된다.

- **Unsupervised Learning** 비지도 학습

label이 없는 데이터를 잘 학습하는 것이다 **Generative Model**에는 Naive Bayes, Gaussian discriminanat analysis (GDA, 가우시안 판별분석)이 있다. 학습데이터에서 분포를 학습 한 뒤, 학습 데이터와 유사한 데이터를 만드는게 목표이다. 학습이 진행될 수록 실제 데이터와 생성한 데이터의 분포가 유사해진다.

![image](https://user-images.githubusercontent.com/61610411/129442769-480c075a-70d5-4583-8746-455cf167b067.png)

파란색은 학습셋 데이터의 불포이며, 빨간색은 모델이 생성한 이미지의 분포이다. 데이터의 분포를 실제와 근사하게 만드는게 Generative Model의 목표이다.

---

## Generative Adversarial Networks

Generative Adversarial Networks는 편의상 GAN, 겐, 간으로도 부른다. Adversarial Network는 적대적인 신경망이 서로 경쟁하면서 가짜 모델의 성능을 개선한다.

**Discriminator**는 진짜 이미지를 진짜(1)로, 가짜를 가짜(0)로 구분하도록 학습한다. 반대로, **generator**(Neural Network)는 랜덤한 코드를 통해서 이미지를 생성하며, **Discriminator**를 속여 가짜 이미지를 진짜처럼 만드는게 목표이다. 즉, **실제 데이터의 분포**와 **모델이 생성한 데이터의 분포** 간의 차이를 줄이는 것이다.


GAN을 처음 제안한 lan Goodfellow는 <경찰과 위조지폐범>으로 비유하였다.

지폐위조범이 처음에는 돈을 제대로 못 만들어 경찰이 위조지폐를 제대로 구분하여 검거에 성공했다. 이 후, 지폐위조범은 더욱 발전된 기술로 지폐를 위조한다. 위조지폐범은 진짜 같은 위조지폐를 만들어(**생성, generator**) 경찰을 속이고, 경찰은 진짜와 가짜 화폐를 구분(**분류, discriminator**)하기 위해 노력한다.

결국 위조지폐범은 구분하기 어려운 위조지폐를 만들게 된다. 경찰은 이게 진짜인지 가짜인지 구별하기 가장 어려운 50% 확률로 수렴하게 된다.

![image](https://user-images.githubusercontent.com/61610411/129442554-7b21af37-6a24-4e25-975b-1590a6e6207d.png)

![image](https://user-images.githubusercontent.com/61610411/129443031-04b098e9-1143-42ed-84e1-f5f84a1ed213.png)


모델 관점에서 다시 해석하면, generator에서 input에서 쓰레기(garbage) 값을 보내도 output(위조지폐, 실제는 이미지)은 실제와 가짜를 구분할 수 없게끔(adversarial) 만들게 된다.

1. Generator는 기존 샘플(training, real) 분포를 파악하여 새로운 샘플(fake)을 생성한다.

2. Discriminator는 샘플이 Generator 또는 Training 중 어디에서 온건지 확률을 평가함 **(Minimax tow-player game)**

3. Generator 가 Discriminator 분포를 완벽한 수준으로 복원하면 Discriminator가 generator의 산출물(fake)와 Training(real) 을 구분할 확률은 1/2가 된다.

---

## Theoretical Results


- Q_model(x|z) : 정의하고자 하는 z값을 줬을때 x 이미지를 내보내는 모델

- P_data(x) : x 라는 data distribution은 있지만 어떻게 생긴지는 모르므로, P 모델을 Q 모델에 가깝게 가도록 한다.

- 파란점선 --- : discriminator distribution (분류 분포) > 학습을 반복하다보면 가장 구분하기 어려운 구별 확률인 1/2 상태가 된다.

- 녹색 선 - : generative distribution (가짜 데이터 분포)

- 검은색 점선 --- : data generating distribution (실제 데이터 분포)


![image](https://user-images.githubusercontent.com/61610411/129443419-17cd1c0c-d985-4c8d-8ff1-6060ae323add.png)

---

## Minimax Problem of GAN

![image](https://user-images.githubusercontent.com/61610411/129443446-b0a4e8de-ef4a-46f4-901d-eb5669697a2e.png)
---

### Theoretical Results

위에 정의한 **Minimax problem (최소최대문제)**가 실제로 풀 수 있는 문제인지 확인이 필요하다. 이를 위해 (1) 실제 정답이 있는지 (existence)와 (2) 해가 존재한다면 유일한지 (uniqueness) 검증이 필요하다.


### Two Step Approach

아래 두 가지를 증명해야 우리가 원하는 바를 해결할 수 있다고 볼 수 있다.

1. Global Optimality of P_g = P_data
  GAN의 Minimax problem이 global optimality를 가지고 있다.
  P_data (data distribution)이 generative한 model distribution이 정확히 일치할 때 global optimality 이며, 그때 global optimality(P_g = P_data)를 갖는가?

2. Convergence of Algorithm 1
  우리가 제안하는 알고리즘(discrimiator <-> distribution model 학습하는 과정의 모델)이 실제로 global optimality(P_g = P_data)을 찾을 수 있는가?

  ![image](https://user-images.githubusercontent.com/61610411/129443626-32c9ac8f-0bab-4888-9cf3-bcf597d76c56.png)

  실제 데이터 분포와 모델 분포 간의 거리가 최소이기 때문에 GAN은 성립한다.
  
---

## Adventages and Disadventages 


### A. Adventages

1. 기존의 어떤 방법보다도 사실적인 결과물을 만들 수 있음

2. 데이터의 분포를 학습하기 때문에, 확률 모델을 명확하게 정의하지 않아도 Generator가 샘플(fake)을 생성할 수 있음

3. MCMC(Markov Chain Monte Carlo)를 사용하지 않고, backprop을 이용하여 gradient를 구할 수 있음(한번에 샘플을 생성 가능)

4. 네트워크가 매우 샤프하고 degenerator(변형된) 분포를 표현할 수 있음.
  Markov Chain 기반 모델에서는 체인 간에 혼합될 수 있도록 분포가 다소 선명하지 않음

5. Adversarial model은 generator의 파라미터가 샘플 데이터에 의해 직접 업데이트 하지 않고, disciminator의 gradient를 이용하여 통계적 이점을 얻음


```
MCMC(Markov Chain Monte Carlo) 는 어떤 목표 확률분포(Target Probability Distribution)로 부터 랜덤 샘플을 얻는 방법이다.

MCMC를 이해하기 위해서는 마코프 연쇄(Markov Chain)과 Monte Carlo 두 가지 개념을 이해해야 한다.

마코프 연쇄는 시간에 따른 계의 상태의 변화를 나타내며, 매 시간마다 계는 상태를 바꾸거나 같은 상태를 유지하며 상태의 변화를 '전이'라고 한다. 마르코프 성질은 과거와 현재 상태가 주어졌을 때의 미래 상태의 조건부 확률 분포가 과거 상태와는 독립적으로 현재 상태에 의해서만 결정된다는 것을 뜻한다.

몬테 카를로 방법은 난수를 이용하여 함수의 값을 확률적으로 계산하는 알고리즘을 부르는 용어이다. 수학이나 물리학 등에서 자주 사용된다. 이 방법은 계산하려는 값이 닫힌 형식(closed form)으로 표현되지 않거나 매우 복잡한 경우에 확률/근사적으로 계산할 때 사영되며 유명한 도박의 도시 몬테 카를로의 이름을 본따 만들어 졌다.
```


### B. Difficulties & Disadvantage


1. Simple Example : unstable
  Minimax 최적화 문제를 해결하는 과정이기 때문에, oscillation이 발생하여 모델이 불안정 할 수 있다.
  -> 두 모델의 성능 차이가 있으면 한쪽으로 치우치기 때문에 DCGAN(Deep Convolution GAN)으로 문제 개선

2. Minibatch Discrimination
  컨버전스가 안되는 경우는 여러 케이스를 보여준다. (use other examples as side information)

3. Ask Somebody
  평가를 어떻게 해야할까? 정말 사람 같은 이미지라고 평가할 수 있지만 주관적이다. 또한, 언제 멈춰야 되는지에 대한 기준이 부족하다.
  -> *Inception score* 사용하여 문제 해결
  : classification 하는 모델에 학습이 끝난 모델에서 나온 샘플을 넣어서 클래스가 잘 나오는지 (=하나의 클래스로 분류하는지)에 따라서 점수를 측정한다.

4. Mode Collapes (sample diversity)
  :minmaxGD = maxminDG ->
  - Generator 입장에서는 Discriminator가 가장 성능이 안좋게 판단한 최소값 하나만 보내주면 됨
  - 예를 들어, 1~10 을 학습할 때 1의 성능이 좋지 않다면 1만 보내주다가, 나중에는 1은 무조건 실제가 아닌것으로 판단함.[Unrolled-Generative-Adversarial-Model]<https://arxiv.org/abs/1611.02163>

5. 학습 데이터의 분포를 따라가기 때문에 어떤 데이터가 생성될지 예측하기 어려움
  -> cGAN(Condtional GAN)을 이용하여 문제 개선

6. HELvetica scenario 를 피하기 위해 G는 D가 업데이트 하기 전에 학습을 너무 많이하면 안됨
  Helvetica scenario : generator가 서로 다른 z들을 하나의 output point로 맵핑할 때 발생하는 문제
  [kangbk0120.github.io/articles/2017-08/tips-from-goodfellow]<kangbk0120.github.io/articles/2017-08/tips-from-goodfellow>

---


## GAN PyTorch Implementation

MNIST를 활용하여 GAN을 구현하는 방법은 [Woosung Choi's blog <GAN PyTorch 구현: 손글씨 생성>]<https://ws-choi.github.io/blog-kor/seminar/tutorial/mnist/pytorch/gan/GAN-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/> 에서 확인


참고자료 : (https://wegonnamakeit.tistory.com/54),(https://www.youtube.com/watch?v=odpjk7_tGY0&t=69s),(https://www.youtube.com/watch?v=L3hz57whyNw&feature=youtu.be)