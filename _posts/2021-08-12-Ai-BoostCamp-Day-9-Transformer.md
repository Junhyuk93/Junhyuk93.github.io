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


이러한 데이터들의 구조파악 어렵

텍스트 to 이미지

DALL-E : 트랜스포머의 디코더 사용