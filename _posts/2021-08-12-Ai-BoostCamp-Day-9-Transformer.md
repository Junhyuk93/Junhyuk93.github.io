---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 9 - Transformer "
date:   2021-08-12 14:34:32 +0530
categories: DL
---


## Transformer


### Sequential data 의 어려운점


- 입력 데이터가 길고 길이가 일정하지 않을 수 있다.

- 입력 심볼의 다양성. 입력신호를 어떤 단위화된 심볼로 본다면 그 다양성이 매우 크다. 심볼의 크기도 시간 단위, 혹은 윈도우 크기를 무엇으로 정하냐에 따라 많은 심볼이 가능하다.

- 긴시간 동산의 context 학습이 필요하다.

- 심볼 간의 dependency도 학습이 필요하다. 어떠한 심볼들의 순차적 출현이 중요한 패턴일지 습해야 하므로 꼭 학습이 필요하다.

![image](https://user-images.githubusercontent.com/61610411/129124954-2c073065-8244-4203-8f73-7a8e14ec3ed9.png)

---

## Transformer

![image](https://user-images.githubusercontent.com/61610411/129306041-b36f28e6-f826-46f8-8203-e7f2396db65b.png)

모델의 자세한 부분을 무시하고 이를 하나의 black box 라고 보자. 기계 번역의 경우를 생각해본다면, 모델은 어떤 한 언어로 된 하나의 문장을 입력으로 받아 다른 언어로 된 번역을 출력으로 내놓을 것이다.

그 black box를 열어 보게 되면 우리는 encoding 부분, decoding 부분, 그리고 그 사이를 이어주는 connection들을 보게 된다.

![image](https://user-images.githubusercontent.com/61610411/129306307-fa978930-3499-4d51-ae04-ecb7d0b1fcf9.png)


encoding 부분은 여러개의 encoder 를 쌓아 올려 만든 것이다. (논문에서는 6개를 쌓았다고 하나, 꼭 6개를 쌓아야 하는 것은 아니고 각자의 세팅에 맞게 얼마든지 변경하여 실험할 수 있다.) decoding 부분은 encoding 부분과 동일한 개수만큼의 decoder를 쌓은 것을 말한다.


![image](https://user-images.githubusercontent.com/61610411/129306445-6aed187d-d0fb-4fea-9eff-ae5f16b2bc3b.png)


encoder 들은 모두 정확히 똑같은 구조를 가지고 있다. 그러나 그들 간의 같은 weight를 공유하진 않는다. 하나의 encoder를 나눠보면 아래와 같이 두개의 sub-later로 구성되어 있다.

![image](https://user-images.githubusercontent.com/61610411/129306521-d17fe299-e283-472c-8aad-214566a4acf8.png)


인코더에 들어온 입력은 일단 먼저 self-attention layer를 지나가게 된다. 이 layer는 encoder가 하나의 특정한 단어를 encode 하기 위해서 입력 내의 모든 다른 단어들과의 관계를 살펴본다. 


입력이 self-attention 층을 통과하여 나온 출력은 다시 feed-forward 신경망으로 들어가게 된다. 똑같은 feed-forward 신경망이 각 위치의 단어마다 독립적으로 적용돼 출력을 만든다.


decoder 또한 encdoer에 있는 두 layer를 모두 가지고 있다. 그러나 그 두 층 사이의 [seq2seq 모델](https://nlpinkorean.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)의 attention과 비슷한 encoder-decoder attention이 포함되어 있다. 이는 decoder가 입력 문장 중에서 각 타임 스텝에서 가장 관려 있는 부분에 집중할 수 있도록 해준다.


![image](https://user-images.githubusercontent.com/61610411/129307015-35ec7d54-e673-404e-a328-1a71f01c3971.png)


### 벡터들을 기준으로 그림 그려보기

이제 우리는 Transformer 모델의 주요 부분들에 대해서 다 알아보았다. 이제 입력으로 들어와서 출력이 될때까지 이 부분들 사이를 지나가며 변환될 벡터/ 텐서들을 기준으로 모델을 살펴보자.

현대에 들어 대부분 NLP 관련 모델에서 그러듯, 먼저 입력 단어들을 [embedding 알고리즘](https://medium.com/deeper-learning/glossary-of-deep-learning-word-embedding-f90c3cec34ca)을 이용해 벡터로 바꾸는 것부터 해야 한다.


![image](https://user-images.githubusercontent.com/61610411/129307530-3cb7f360-3b7e-48a9-b44e-0f280402cc5c.png)


각 단어들은 크기 512의 벡터 하나로 embed된다. 우리는 이 변환된 벡터들을 위와 같은 간단한 박스로 나타낸다.

이 embedding은 가장 밑단의 encoder에서만 일어난다. 이렇게 되면 우리는 이렇게 뭉뚱그려 표현할 수 있게 된다. 모든 encoder들은 크기 512의 벡터의 리스트를 입력으로 받는다. 이 벡터는 가장 밑단의 encoder의 경우에는 word embedding이 될 것이고, 다른 encoder들에서는 바로 전의 encoder의 출력이 될 것이다. 이 벡터 리스트의 사이즈는 hyperparameter로 우리가 마음대로 정할 수 있다. 가장 간단하게 생각한다면 우리의 학습 데이터 셋에서 가장 긴 문장의 길이로 둘 수 있다.

입력 문장의 단어들을 embedding 한 후에, 각 단어에 해당하는 벡터들을 encoder 내의 두 개의 sub-layer으로 들어가게 된다.

![image](https://user-images.githubusercontent.com/61610411/129308014-2eab047c-b7a4-4fd5-a6bd-648bc915a7b1.png)

여기서 우리는 각 위치에 있는 그 단어가 그만의 path를 통해 encoder에서 흘러간다는 Transformer 모델의 주요 성질을 볼 수있다. Self-attention 층에서 이 위치에 따른 path들 사이에 다 dependency가 있다. 반명 feed-forward 층은 이런 dependency가 없기 때문에 feed-forward layer 내의 이 다양한 path 들은 병렬처리될 수 있다.

### Encoding

앞서 설명한것과 같이, encoder는 입력으로 벡터들의 리스트를 받는다. 이 리스트를 먼저 self-atttention layer에, 그 다음으로 feed-forward 신경망에 통과 시키고 그 결과물을 그 다음 encoder에게 전달한다.

![image](https://user-images.githubusercontent.com/61610411/129308192-53d71c9e-7aac-4f8d-b855-4b77f8a1e5c7.png)

각 위치의 단어들은 각각 다른 self-encoding 과정을 거친다. 그 다음으로는 모두에게 같은 과정인 feed-forward 신경망을 거친다.

### Self-Attention 자세히 보기

이번 섹션에서는 먼저 여러가지 벡터들을 통해서 어떻게 self-attention을 계산할 수 있는지 볼 것이다. 그 후 행렬을 이용하여 이것이 실제로 어떻게 구현돼 있는지 확인해보자.

self-attention 계산의 가장 첫 단계는 encoder에 입력된 벡터들(이 경우에서는 각 단어의 embedding 벡터이다)에게서 부터 각 3개의 벡터를 만들어 내는 일이다. 우리는 각 단어에 대해서 Query 벡터, Key 벡터, 그리고 Value 벡터를 생성한다. 이 벡터들은 입력 벡터에 대해서 세개의 학습 가능한 행렬들을 각각 곱함으로써 만들어진다.

여기서 한가지 짚고 넘어갈 것은 이 새로운 벡터들이 기존의 벡터들 보다 더 작은 사이즈를 가진다는 것이다. 기존의 입력 벡터들은 크기가 512인 반면 이 새로운 벡터들은 크기가 64이다. 그러나 그들이 꼭 이렇게 더 작아야만 하는 것은 아니며, 이것은 그저 multi-head attention의 계산 복잡도를 일정하게 만들고자 내린 구조적인 선택일 뿐이다.

![image](https://user-images.githubusercontent.com/61610411/129309639-d46489f1-e78b-4112-8885-e65864eda305.png)


x1를 weight 행렬인 WQ로 곱하는 것은 현재 단어와 연관된 query 벡터린 q1을 생성한다. 같은 방법으로 우리는 입력 문장에 있는 각 단어에 대한 query, key, value 벡터를 만들 수 있다.

그렇다면 정확히 이 query, key, value 벡터란 무엇을 의미하는 것일까?

그것은 attention 에 대해서 생각하고 계산하려할 때 도움이 되는 추상적인 개념이다. 이제 곧 다루게 될 테지만 어떻게 attention 이 실제로 계산되는지를 알게 되면, 자연스럽게 세 개 의 벡터들이 어떤 역할을 하는지 알 수 있게 된다.

self-attention 계산의 두 번째 스텝은 점수를 계산하는 것이다. 아래 예시의 첫번째 단어인 "Thinking"에 대해서 self-attention 을 계산한다고 하자. 우리는 이 단어와 입력 문장 속의 다른 모든 단어들에 대해서 각각 점수를 계산해야 한다. 이 점수는 현재 위치의 이 단어를 encode 할 때 다른 단어들에 대해서 얼마나 집중을 해야 할지를 결정한다.

점수는 현재 단어의 query vector와 점수를 매기려 하는 다른 위치에 있는 단어의 key vector의 내적으로 계산된다. 다시말해, 우리가 위치 #1에 있는 단어에 대해서 self-attention 을 계산한다 했을 때, 첫 번째 점수는 q1과 k1의 내적일 것이다. 그리고 동일하게 두번째 점수는 q1과 k2 의 내적일 것이다.

![image](https://user-images.githubusercontent.com/61610411/129310866-48477249-40de-4447-9f80-c1a636a326f5.png)

세번째와 네번째 단계는 이 점수들을 8로 나누는 것이다. 이 8이란 숫자는 key 벡터의 사이즈인 64의 제곱근이라는 식으로 계산이 된 것이다. 이 나눗셈을 통해 우리는 더 안정적인 gradient를 가지게 된다. 그리고 난 다음 이 값을 softmax 계산을 통과시켜 모든 점수를 양수로 만들고 그 합을 1로 만들어 준다.


![image](https://user-images.githubusercontent.com/61610411/129310996-8c7a20c8-f569-4d61-b680-3ab23bbf9a80.png)


이 softmax 점수는 현재 위치의 단어의 encoding에 있어서 얼마나 각 단어들의 표현이 들어갈 것 인지를 결정한다. 당연하게 현재 위치의 단어가 가장 높은 점수를 가지며 가장 많은 부분을 차지하게 되겠지만, 가끔은 현재 단어에 관련이 있는 다른 단어에 대한 정보가 들어가는 것이 도움이 된다.

다섯 번째 단계는 이제 입력의 각 단어들의 value 벡터에 이 점수를 곱하는 것이다. 이것을 하는 이유는 우리가 집중을 하고 싶은 관련이 있는 단어들을 그래도 남겨두고, 관련이 없는 단어들은 0.001과 같은 작은 숫자(점수)를 곱해 없애버리기 위함이다.

마지막 여섯 번째 단계는 이 점수로 곱해진 weighted valuie 벡터들을 다 합해 버리는 것이다. 이 단계의 출력이 바로 현재 위치의 대한 self-attention layer의 출력이 된다.

![image](https://user-images.githubusercontent.com/61610411/129311289-08b98077-c7fd-4fc8-9bfd-84614ebf2b67.png)


이 여섯 가지 과정이 바로 self-attention의 계산 과정이다. 우리는 이 결과로 나온 벡터를 feed-forward 신경망으로 보내게 된다. 그러나 실제 구현에서는 빠른 속도를 위해 이 모든 과정들이 벡터가 아닌 행렬의 형태로 진행하게 된다. 우리는 이 때까지 각 단어 레벨에서의 계산과 그 이유에 대해서 얘기해봤다면, 이제 이 행렬 레벨의 계산을 살펴보도록 하자.

### Self-attention 의 행렬 계산

가장 첫 스텝은 입력 문장에 대해서 Query, Key, Value 행렬들을 계산하는 것이다. 이를 위해 우리는 우리의 입력 벡터들 (혹은 embedding 벡터들)을 하나의 행렬 X로 쌓아 올리고, 그것을 우리가 학습할 weight 행렬들인 WQ, WK, WV로 곱한다.


![image](https://user-images.githubusercontent.com/61610411/129312507-af7ef5c5-c144-413f-86d4-8e66ab6b4c1a.png)


행렬 X의 각 행은 입력 문장의 각 단어에 해당합니다. 우리는 여기서 다시 한번 embedding 벡터들(크기 512, 그림에서는 4)과 query,key,value 벡터들(크기 64, 그림에서는 3) 간의 크기 차이를 볼 수있다.

마지막으로 우리는 현재 행렬을 이용하고 있으므로 앞서 설명했던 self-attention 계산 단계 2부터 6까지를 하나의 식으로 압축 할 수 있다.

![image](https://user-images.githubusercontent.com/61610411/129312668-bf34168b-557b-415b-8a64-a8a2852410aa.png)


## The Beast With Many Heads

본 논문은 이 self-attention layer 에다 "multi-headed" attention 이라는 메커니즘을 더해 더욱 더 이를 개선한다. 이것은 두 가지 방법으로 attention later의 성능을 향상시킨다.

1. 모델이 다른 위치에 집중하는 능력을 확장시킨다.

2. attention layer가 여러개의 "representation 공간"을 가지게 해준다.


![image](https://user-images.githubusercontent.com/61610411/129312953-cea14d89-6ad5-42b4-b4ac-d8e60f017b6f.png)


multi-headed attention을 이용하기 위해서 우리는 각 head를 위해서 각각의 다른 query/key/value weight 행렬들을 모델에 가지게 된다. 이전에 설명한 것과 같이 우리는 입력 벡터들의 모음인 행렬X를 WQ/WX/WV 행렬들로 곱해 각 head에 대한 Q/K/V행렬들을 생성한다.

위에 설명했던 대로 같은 self-attention 계산 과정을 8개의 다른 weight 행렬들에 대해 8번 거치게 되면, 우리는 8개의 서로 다른 Z 행렬을 가지게 된다.


![image](https://user-images.githubusercontent.com/61610411/129313720-a95f133c-97ba-4cc5-a4ae-f86a4ea0f241.png)


그러나 문제는 이 8개의 행렬을 바로 feed-forward layer으로 보낼 수 없다는 것이다. feed-forward layer 은 한 위치에 대해 오직 한 개의 행열만을 input으로 받을 수 있다. 그러므로 우리는 이 8개의 행렬을 하나의 행렬로 합치는 방법을 고안해 내야 한다.

어떻게 할 수 있을까? 간단하다. 일단 모두 이어 붙여서 하나의 행렬로 만들어버리고 그 다음 하나의 또 다른 weight 행렬인 W0을 곱해버린다.

![image](https://user-images.githubusercontent.com/61610411/129314659-ea1d378d-b65f-4057-b002-f4a9dc753394.png)

사실상 multi-headed self-attention 은 이게 전부이다.
이제 이 모든 것을 하나의 그림으로 표현해서 모든 과정을 한 눈에 정리해보도록 하자.

![image](https://user-images.githubusercontent.com/61610411/129314828-34c1abf9-a21d-4924-9b53-99c0266ad5fb.png)

[참고자료](https://nlpinkorean.github.io/illustrated-transformer/)

위 참고자료를 필사한 내용입니다