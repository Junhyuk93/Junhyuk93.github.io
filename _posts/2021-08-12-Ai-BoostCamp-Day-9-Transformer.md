---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 9 - Transformer "
date:   2021-08-12 14:34:32 +0530
categories: DL
---


Transformer


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


---

### 벡터들을 기준으로 그림 그려보기

이제 우리는 Transformer 모델의 주요 부분들에 대해서 다 알아보았다. 이제 입력으로 들어와서 출력이 될때까지 이 부분들 사이를 지나가며 변환될 벡터/ 텐서들을 기준으로 모델을 살펴보자.

현대에 들어 대부분 NLP 관련 모델에서 그러듯, 먼저 입력 단어들을 [embedding 알고리즘](https://medium.com/deeper-learning/glossary-of-deep-learning-word-embedding-f90c3cec34ca)을 이용해 벡터로 바꾸는 것부터 해야 한다.


![image](https://user-images.githubusercontent.com/61610411/129307530-3cb7f360-3b7e-48a9-b44e-0f280402cc5c.png)


각 단어들은 크기 512의 벡터 하나로 embed된다. 우리는 이 변환된 벡터들을 위와 같은 간단한 박스로 나타낸다.

이 embedding은 가장 밑단의 encoder에서만 일어난다. 이렇게 되면 우리는 이렇게 뭉뚱그려 표현할 수 있게 된다. 모든 encoder들은 크기 512의 벡터의 리스트를 입력으로 받는다. 이 벡터는 가장 밑단의 encoder의 경우에는 word embedding이 될 것이고, 다른 encoder들에서는 바로 전의 encoder의 출력이 될 것이다. 이 벡터 리스트의 사이즈는 hyperparameter로 우리가 마음대로 정할 수 있다. 가장 간단하게 생각한다면 우리의 학습 데이터 셋에서 가장 긴 문장의 길이로 둘 수 있다.

입력 문장의 단어들을 embedding 한 후에, 각 단어에 해당하는 벡터들을 encoder 내의 두 개의 sub-layer으로 들어가게 된다.

![image](https://user-images.githubusercontent.com/61610411/129308014-2eab047c-b7a4-4fd5-a6bd-648bc915a7b1.png)

여기서 우리는 각 위치에 있는 그 단어가 그만의 path를 통해 encoder에서 흘러간다는 Transformer 모델의 주요 성질을 볼 수있다. Self-attention 층에서 이 위치에 따른 path들 사이에 다 dependency가 있다. 반명 feed-forward 층은 이런 dependency가 없기 때문에 feed-forward layer 내의 이 다양한 path 들은 병렬처리될 수 있다.


---
### Encoding


앞서 설명한것과 같이, encoder는 입력으로 벡터들의 리스트를 받는다. 이 리스트를 먼저 self-atttention layer에, 그 다음으로 feed-forward 신경망에 통과 시키고 그 결과물을 그 다음 encoder에게 전달한다.

![image](https://user-images.githubusercontent.com/61610411/129308192-53d71c9e-7aac-4f8d-b855-4b77f8a1e5c7.png)

각 위치의 단어들은 각각 다른 self-encoding 과정을 거친다. 그 다음으로는 모두에게 같은 과정인 feed-forward 신경망을 거친다.


---
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


---


### Self-attention 의 행렬 계산

가장 첫 스텝은 입력 문장에 대해서 Query, Key, Value 행렬들을 계산하는 것이다. 이를 위해 우리는 우리의 입력 벡터들 (혹은 embedding 벡터들)을 하나의 행렬 X로 쌓아 올리고, 그것을 우리가 학습할 weight 행렬들인 WQ, WK, WV로 곱한다.


![image](https://user-images.githubusercontent.com/61610411/129312507-af7ef5c5-c144-413f-86d4-8e66ab6b4c1a.png)


행렬 X의 각 행은 입력 문장의 각 단어에 해당합니다. 우리는 여기서 다시 한번 embedding 벡터들(크기 512, 그림에서는 4)과 query,key,value 벡터들(크기 64, 그림에서는 3) 간의 크기 차이를 볼 수있다.

마지막으로 우리는 현재 행렬을 이용하고 있으므로 앞서 설명했던 self-attention 계산 단계 2부터 6까지를 하나의 식으로 압축 할 수 있다.

![image](https://user-images.githubusercontent.com/61610411/129312668-bf34168b-557b-415b-8a64-a8a2852410aa.png)

---


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

여기까지 attention heads에 대해서 설명해 보았는데요, 이제 다시 우리의 예제 문장인 multi-head attention 과 함께 보도록 하자. 그중에서도 특히 "그것"이란 단어를 encode 할 때 여러 개의 attention이 각각 어디에 집중하는지를 보도록 하자.

![image](https://user-images.githubusercontent.com/61610411/129364378-387fe9ec-3419-479d-b792-6420ae4af5d0.png)


우리가 "그것"이란 단어를 encode 할 때, 주황생의 attention head는 "그 동물"에 가장 집중하고 있는 반면 초록색의 head는 "피곤"이라는 단어에 집중을 하고 있다. 모델은 이 두개의 attention head를 이용하여 "동물"과 "피곤" 두 단어 모두에 대한 representation을 "그것"의 representation에 포함 할 수 있다.


그러나 이 모든 attention head들을 하나의 그림으로 표현하면, 이제 attention의 의미는 해석하기가 어려워진다.


![image](https://user-images.githubusercontent.com/61610411/129364610-099b48c2-d404-47dd-91c1-5f880cf148d4.png)


---
### Positional Encoding 을 이용해서 시퀀스의 순서 나타내기

우리가 이때까지 설명해온 Transformer 모델에서 한가지 부족한 부분은 이 모델이 입력 문장에서 단어들의 순서에 대해서 고려하지 있지 않다는 점이다.

이것을 추가하기 위해서, Transformer 모델은 각각의 입력 embedding에 "positional encoding"이라고 불리는 하나의 벡터를 추가한다. 이 벡터들은 모델이 학습하는 특정한 패턴을 따르는데, 이러한 패턴은 모델이 각 단어의 위치와 시퀀스 내의 다른 단어 간의 위치 차이에 대한 정보를 알 수 있게 해준다. 이 벡터들을 추가하기로 한 배경에는 이 값들을 단어들의 embedding에 추가하는 것이 query/key/value 벡터들로 나중에 투영되었을 때 단어들 간의 거리를 늘릴 수 있다는 점이 있다.

![image](https://user-images.githubusercontent.com/61610411/129365158-89a7255c-10dc-4933-b232-6f7db93332bb.png)


모델에게 단어의 순서에 대한 정보를 주기 위하여, 위치 별로 특정한 패턴을 따르는 positional encoding 벡터들을 추가한다.

만약 embedding의 사이즈가 4라고 가정한다면, 실제로 각 위치에 따른 positional encoding은 아래와 같은 것이다.


![image](https://user-images.githubusercontent.com/61610411/129365286-7a556774-96e4-4d83-94b0-0de51ce27989.png)



위는 크기가 4인 embedding의 positional encoding에 대한 실제 예시이다.

실제로는 이 패턴이 어떻게 될까?

다음 그림을 보자. 그림에서 각 행은 하나의 벡터에 대한 positional encoding에 해당한다. 그러므로 첫번째 행은 우리가 입력 문장의 첫 번째 단어의 embedding 벡터에 더할 positional encoding 벡터이다. 각 행은 사이즈 512인 즉 512개의 셀을 가진 벡터이며 각 셀의 값은 1과 -1 사이를 가진다. 다음 그림에서 이 셀들의 값들에 대해 색깔을 다르게 나타내어 positional encoding 벡터들이 가지는 패턴을 볼 수 있도록 시각화했다.


![image](https://user-images.githubusercontent.com/61610411/129365545-7bd05a17-a226-4c1f-9644-7ccf05f3bf3e.png)


20개의 단어와 그의 크기 512인 embedding에 대한 positional encoding의 실제 예시이다. 그림에서 볼 수 있듯이 이 벡터들은 중간 부분이 반으로 나뉘어져있다. 그 이유는 바로 왼쪽 반은 (크기 256) sine 함수에 의해서 생성되었고, 나머지 오른쪽 반은 또 다른 함수인 cosine 함수에 의해 생성되었기 때문이다. 그 후 이 두 값들은 연결되어 하나의 positional encoding 벡터를 이루고 있다.

이 positional encoding에 대한 식은 논문의 section 3.5에 설명되어 있다. 실제로 이 벡터들을 생성하는 부분의 코드인 [get_timing_signal_1d()]<https://github.com/tensorflow/tensor2tensor/blob/23bd23b9830059fbc349381b70d9429b5c40a139/tensor2tensor/layers/common_attention.py>를 참고해도 좋다. 이것은 사실 positional encoding에 대해서만 가능한 방법은 아니다. 하지만 이것은 본 적이 없는 길이의 시퀀스에 대해서도 positional encoding 을 생성할 수 있기 때문에 scalability에서 큰 이점을 가진다.(예를 들어, 이미 학습된 모델이 자신의 학습 데이터보다도 더 긴 문장에 대해서 번역을 해야 할 때에도 현재의 sine과 cosine으로 이루어진 식은 positional encoding을 생성해낼 수 있다.)


---
### The Residuals

encoder를 넘어가기 전에 그의 구조에서 한가지 더 언급하고 넘어가야 하는 사항은, 각 encoder 내의 sub-layer가 residual connection 으로 연결되어 있으며, 그 후에는 [layer-normalization]<https://arxiv.org/abs/1607.06450> 과정을 거친다는 것이다.

![image](https://user-images.githubusercontent.com/61610411/129366395-663393bf-4e40-4f0d-910f-b383a386926e.png)

이 벡터들과 layer-normalization 과정을 시각화해보면 다음과 같다.

![image](https://user-images.githubusercontent.com/61610411/129366473-cdd88f2a-7ce5-4108-ba2a-236a36d33d9e.png)

이것은 decoder 내에 있는 sub-layer 들에도 똑같이 적용되어 있다. 만약 우리가 2개의 encoder과 decoder으로 이루어진 단순한 형태의 Transformer 를 생각해본다면 다음과 같은 모양일 것이다.

![image](https://user-images.githubusercontent.com/61610411/129366660-8399160c-d9c3-45c3-a364-0cf078a72209.png)


---
### The Decoder Side

이때까지 encoder 쪽의 대부분의 개념들에 대해서 얘기했기 때문에, 우리는 사실 decoder의 각 부분이 어떻게 작동하는지에 대해서는 이미 알고 있다고 봐도 된다. 하지만, 이제 우리는 이 부분들이 모여서 어떻게 같이 작동하는지에 대해서 보아야 한다.

encoder가 먼저 입력 시퀀스를 처리하기 시작한다. 그다음 가장 윗단의 encoder의 출력은 attention 벡터들인 K와 V로 변형된다. 이 벡터들을 이제 각 decoder의 "encoder-decoder attention" layer에서 decoder가 입력 시퀀스에서 적절한 장소에 집중할 수 있도록 도와준다.

![transformer_decoding_1](https://user-images.githubusercontent.com/61610411/129367141-d6314049-a100-45f7-84cf-3a9cab6ac501.gif)

이 encoding 단계가 끝나면 이제 decoding 단계가 시작된다. decoding 단계의 각 스텝은 출력 시퀀스의 한 element를 출력한다. (현재 기계 번역의 경우에는 영어 번역 단어이다)

decoding step은 decoder가 출력을 완료했다는 special 기호인 **<end of sentence>**를 출력할 때까지 반복된다. 각 스텝마다의 출력된 단어는 다음 스텝의 가장 밑단의 decoder에 들어가고 encoder와 마찬가지로 여러개의 decoder를 거쳐 올라간다. encoder의 입력에 했던 것과 동일하게 embed를 한 후 positional encoding을 추가하여 decoder에게 각 단어의 위치 정보를 더해준다.

![transformer_decoding_2](https://user-images.githubusercontent.com/61610411/129367662-3a44b9e7-d424-448b-9f3f-64ba8eaf8dcc.gif)

decoder 내애 있는 self-attention layer 들은 encoder와는 조금 다르게 작동한다.

decoder 에서의 self-attention layer은 output sequence 내에서 현재 위치의 이전 위치들에 대해서만 attend 할 수 있다. 이것은 self-attention 계산 과정에서 softmax를 취하기 전에 현재 스텝 이후의 위치들에 대해서 masking(즉 그에 대해서 -inf로 치환하는것)을 해줌으로써 가능해진다.

"Encoder-Decoder Alttention" layer은 multi-head self-attention 과 한 가지를 제외하고는 똑같은 방법으로 작동한다. 그 한가지 차이점은 Query 행렬들을 그 밑의 layer에서 가져오고 Key와 Value 행렬들을 encoder의 출력에서 가져온다는 점이다.

--- 
### 마지막 Linear Layer와 Softmax Layer

여러 개의 decoder를 거치고 난 후에는 소수로 이루어진 벡터 하나가 남게 된다. 어떻게 이 하나의 벡터를 단어로 바꿀 수 있을까? 이것이 바로 마지막에 있는 Linear layer와 Softmax layer가 하는 일이다.

Linear layer는 fully-connected 신경망으로 decoder가 마지막으로 출력한 벡터를 그보다 훨 씬 더 큰 사이즈의 벡터인 logits 벡터로 투영시킨다.

우리의 모델이 training 데이터에서 총 10,000개의 영어 단어를 학습하였다고 가정하자 (이를 우리는 모델의 "output vocabulary"라고 부른다). 그렇다면 이 경우에 logits vector의 크기는 10,000이 될 것이다 - 벡터의 각 셀은 그에 대응하는 각 단어에 대한 점수가 된다. 이렇게 되면 우리는 Linear layer의 결과로서 나오는 출력에 대해서 해석을 할 수 있게 된다.

그 다음에 나오는 softmax layer는 이 점수들을 확률로 변환해주는 역할을 한다. 셀들의 변환된 확률 값들은 모두 양수 값을 가지며 다 더하게 되면 1이 된다. 가장 높은 확률 값을 가지는 셀에 해당하는 단어가 해당 스텝의 최종 결과물로서 출력되게 된다.

![image](https://user-images.githubusercontent.com/61610411/129369096-1d6f7447-b241-43b7-85e0-611751debde7.png)


위의 그림에 나타나 있는 것과 같이 decoder에서 나온 출력은 Linear layer 와 softmax layer를 통과하여 최종 출력 단어로 변환된다.

---
### 학습 과정 다시 보기

이제 학습된 Transformer의 전체 forward-pass 과정에 대해서 알아 보았으므로, 이제 모델을 학습하는 방법에 대해서 알아보자.

학습 과정 동안, 학습이 되지 않은 모델은 정확히 같은 forward pass 과정을 거칠 것이다. 그러나 우리는 이것을 label된 학습 데이터 셋에 대해 학습시키는 중이므로 우리는 모델의 결과를 실제 label 된 정답과 비교할 수 있다.

이 학습 과정을 시각화하기 위해, 우리의 output vocabulary 가 6개의 단어만 (("a","am","i","thanks","student",and "<end of sentence>")) 포함하고 있다고 가정하자.

![image](https://user-images.githubusercontent.com/61610411/129369565-5a2efb97-a9e9-4da2-99ff-05567e64981f.png)


모델의 output vocabulary는 학습을 시작하기 전인 preprocessing 단계에서 완성된다.

이 output vocabulary를 정의한 후에는, 우리는 이 vocabulary의 크기만 한 벡터를 이용하여 각 단어를 표현할 수 있다. 이것은 one-hot encoding이라고 불린다. 그러므로 우리의 예제에서는 단어 "am"을 다음과 같은 벡터로 나타낼 수 있다.

![image](https://user-images.githubusercontent.com/61610411/129369842-64e3b46d-85e5-440d-ac4e-a205487d5a33.png)

이제 모델의 loss function에 대해서 얘기해보자. 이것은 학습 과정에서 최적화함으로써 모델을 정확하게 학습시킬 수 있게 해주는 값이다.


---

### Loss Function


우리가 모델을 학습하는 상황에서 가장 첫 번째 단계라고 가정하자. 그리고 우리는 학습을 위해 "merci"라는 불어를 "thanks"로 번역하는 간단한 예시를 생각해보자.

이말은 즉, 우리가 원하는 모델의 출력은 "thanks" 라는 단어를 가리키는 확률 벡터라는 것이다. 하지만 우리의 모델은 아직 학습이 되지 않았기 때문에, 아직 모델의 출력이 그렇게 나올 확률은 매우 작다.


![image](https://user-images.githubusercontent.com/61610411/129370304-433c763e-b457-4916-af7f-a8f7aaf56278.png)


학습이 시작될 때 모델의 parameter들 즉 weight들은 랜덤으로 값을 부여하기 때문에, 아직 학습이 되지 않은 모델은 그저 각 cell(word)에 대해서 임의의 값을 출력한다. 이 출력된 임의의 값을 가지는 벡터와 데이터 내의 실제 출력값을 비교하여, 그 차이에 backpropagation 알고리즘을 이용해 현재 모델의 weight들을 조절해 원하는 출력값에 더 가까운 출력이 나오도록 만든다.

그렇다면 두 화귤ㄹ 벡터를 어떻게 비교할 수 있을까? 간단하다. 하나의 벡터에서 다른 하나의 벡터를 빼버린다. [Cross-entropy]<https://colah.github.io/posts/2015-09-Visual-Information/> 와[Kullback-Leibler-divergence]<https://www.countbayesie.com/blog/2017/5/9/kullback-leibler-divergence-explained>를 보면 이 과정에 대해 더 자세한 정보를 얻을 수 있다.

하지만 여기서 하나 주의할 것은 우리가 고려하고 있는 예제가 지나치게 단순화된 경우라는 것이다. 조금 더 현실적인 예제에서는 한 단어보다는 긴 문장을 이용할 것이다. 예를 들면 입력은 불어 문장 "je suis étudiant"이며 바라는 출력은 "i am a student"일 것이다. 이말인 즉, 우리가 우리의 모델이 출력할 확률 분포에 대해서 바라는 것은 다음과 같다.

- 각 단어에 대한 확률 분포는 output vocabulary 크기를 가지는 벡터에 의해서 나타내진다.(우리의 간단한 예제에서는 6이지만 실제 실험에서는 3,000 혹은 10,000과 같은 숫자일 것이다)

- decoder가 첫 번째로 출력하는 확률 분포는 "i"라는 단어와 연관이 있는 cell에 가장 높은 확률을 줘야 한다.

- 두 번째로 출력하는 확률 분포는 "am"라는 단어와 연관이 있는 cell에 가장 높은 확률을 줘야 한다.

- 이와 동일하게 마지막 '<end of sentence>'를 나타내는 다섯 번째 출력까지 이 과정은 반복된다.


![image](https://user-images.githubusercontent.com/61610411/129371218-f1bf3faf-0c46-4377-89e4-28d94e4f646d.png)

위의 그림은 학습에서 목표로 하는 확률 분포를 나타낸 것이다.

모델을 큰 사이즈의 데이터 셋에서 충분히 학습을 시키고 나면, 그 결과로 생성되는 확률 분포들은 다음과 같아질 것이다.

![image](https://user-images.githubusercontent.com/61610411/129371376-6e2ae4d2-7a87-4200-ad68-a65ede4b33bb.png)

학습 과정 후에, 바라건대, 모델은 정확한 번역을 출력할 것이다. 물론, 우리가 예제로 쓴 문장이 학습 데이터로 써졌다는 보장은 없다. 그리고 한가지 여기서 특이한 점은 학습의 목표로 하는 벡터들과는 달리, 모델의 출력값은 비록 다른 단어들이 최종 출력이 될 가능성이 거의 없다 해도 모든 단어가 0보다는 조금씩 더 큰 확률을 가진다는 점이다. 이것은 학습 과정을 도와주는 softmax layer의 매우 유용한 성질이다.

모델은 한 타임 스텝 당 하나의 벡터를 출력하기 때문에 우리는 모델이 가장 높은 확률을 가지는 하나의 단어만 저장하고 나머지는 버린다고 생각하기 쉽다. 그러나 그것은 greedy decoding이라고 부르는 한가지 방법일 뿐이며 다른 방법들도 존재한다. 예를 들어 가장 확률이 높은 두 개의 단어를 저장할 수 있다.(위의 예시에서는 'i' 와 'student') 그렇다면 우리는 모델을 두번 돌리게 된다. 한번은 첫 번째 출력이 'i'라고 가정하고 다른 한 번은 'student'.라고 가정하고 두 번째 출력을 생성해보는 것이다. 이렇게 나온 결과에서 첫 번째와 두 번째 출력 단어를 동시에 고려했을 때 더 낮은 에러를 보이는 결과의 첫 번째 단어가 실제 출력으로 선택된다.

이과정을 두 번째, 세 번째, 그리고 마지막 타임 스텝까지 반복해 나간다. 이렇게 출력을 결정하는 방법을 우리는 "beam search"라고 부르며, 고려하는 단어의 수를 beam size, 고려하는 미래 출력 개수를 top_beams라고 부른다. 우리의 예제에서는 두 개의 단어를 저장 했으므로 beam size가 2이며, 첫 번째 출력을 위해 두 번째 스텝의 출력까지 고려했으므로 top_beams 또한 2인 beam search를 한 것이다. 이 beam size와 top_beams는 모두 우리가 학습전에 미리 정하고 실험해볼 수 있는 hyperparameter들 이다.



---
[참고자료](https://nlpinkorean.github.io/illustrated-transformer/)

위 참고자료를 필사한 내용입니다