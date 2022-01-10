---
layout: post
title:  "부스트캠프 AI Tech 2기 Level3 Product Serving - 1"
date:   2021-12-06 15:30:10 +0530
categories: Product Serving
tag: [Product Serving, Serving]
---


product serving

# Product Serving

## 강의의 목표

- AI 엔지니어가 되기 위한 기본 소양을 갖추기
- 문제 정의에 대해 고민을 하는 사람

## 1. MLOps 개론

### 모델 개발 프로세스 - Research

- 문제정의 - EDA - Feature Engineering - Train - Predict

### 모델 개발 프로세스 - Production

- 문제정의 - EDA - Feature Engineering - Train - Predict - **Deploy**

![](https://i.imgur.com/kJmFmQg.png)

**✨MLOps의 목표는 빠른 시간 내에 가장 적은 위험을 부담하며 아이디어 단계부터 Production 단계까지 ML 프로젝트를 진행할 수 있도록 기술적 마찰을 줄이는 것✨**


![](https://i.imgur.com/FWzta0C.png)

1. MLOps가 필요한 이유 이해하기
    - Production을 하기 위한 과정임으로 중요함

2. MLOps의 각 Componenet에 대해 이해하기
    - Server infra = 어떻게 server를 이용할 지에 대한 고민
    - GPU infra = 가동하려는 모델과 회사 운영자원 및 개발환경에 맞게끔 설정
    - Serving = 실질적으로 소비자에게 어떤 방식으로 전해질지
    - Experiment, Model Management = Model test 및 성능 개선을 위한 파라미터 튜닝, 다양한 모델 테스트
    - Feature Store = 필요한 모델 템플릿 만들기
    - Data Validation = 데이터 검증(기존 데이터, Train 데이터와의 차이 확인)
    - Continuous Training = 지속적인 성능 개선
    - Monitoring = 모아지는 데이터를 분석하고 여기서 결과를 얻어 이를 활용해 개선방향 도모
    - AutoML = 자동으로 성능 튜닝



3. MLOps 관련된 자료, 논문 읽어보며 강의 내용 외에 어떤 부분이 있는지 파악해보기
    -  https://github.com/EthicalML/awesome-production-machine-learning

4. MLOps Component 중 내가 매력적으로 생각하는 TOP3을 정해보고 왜 그렇게 생각했는지 작성해보기
    - Server infra 와 GPU infra가 가장 중요하다고 여기고 그 이유는 아무래도 직접적인 회사 운영에 가장 큰 영향이 드는 금전적인 부분을 담당하고 있기 때문에 이 두개의 Component에서 비용을 절감한다면 그 잔여비용으로 다른 Component 개선에 큰 영향을 줄 수 있을 것이라고 생각함. 또 마지막으로 Monitoring을 통해 앞으로 나아갈 방향성과 개선점에 대해서 얻을 수 있어서 중요하다고 생각함.