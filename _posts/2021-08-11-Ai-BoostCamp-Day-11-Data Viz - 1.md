---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 11 - Data Viz - 1 "
date:   2021-08-11 14:34:32 +0530
categories: Vizualization
---


- 

## Bar Plot


### 1.1 bar plot 이란 ?

- **Bar plot** 이란 직사각형 막대를 사용하여 데이터의 값을 표현하는 차트/그래프

- 막대 그래프, bar chart, bar graph 등의 이름이 사용됨

- 범주(category)에 따른 수치 값을 비교하기에 적합한 방법
    - 개별 비교, 그룹 비교 모두 적합

    
---

- 막대의 방향에 따른 분류 (.bar() / .barh())
    - 수직(vertical) : x 축에 범주, y 축에 값을 표기(default)
    - 수평(horizontal) : y축에 범주, x 축에 값을 표기(범주가 많을때 적합)


### 2. Multi Bar Plot

- Bar Plot 에서는 범주에 대한 각 값을 표현-> 즉 1개의 feature에 대해서만 보여줌

- 여러 Group을 보여주기 위해서는 여러가지 방법이 필요.


**1. 플롯을 여러개 그리는 방법**

**2. 한 개의 플롯에 동시에 나타내는 방법**
- 쌓아서 표현하는 방법
- 겹쳐서 표현하는 방법
- 이웃에 배치하여 표현하는 방법

---
### 2.2 Stacked Bar Plot

- 2개 이상의 그룹을 쌓아서(stack) 표현하는 bar plot ( 각 bar에서 나타나는 그룹의 순서는 항상 유지)

- 맨 밑의 bar의 분포는 파악하기 쉽지만 그 외의 분포들은 파악하기가 어려움. 2개의 그룹이 positive/negative라면 축 조정 가능

- 응용하여 전체에서 비율을 나타내는 **Percentage Stacked Bar Chart**가 있음.


### 2.3 Overlapped Bar Plot

- 2개 그룹만 비교한다면 겹쳐서 만드는 것도 하나의 선택지

- 같은 축을 사용하니 비교가 쉬움

### 2.4 Grouped Bar plot

- 그룹별 범주에 따른 bar를 이웃되게 배치하는 방법