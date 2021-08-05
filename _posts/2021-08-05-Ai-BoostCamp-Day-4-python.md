---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 4 - python"
date:   2021-08-04 23:15:36 +0530
categories: TIL
---


## Python 강의 공부


### list comprehension


- 장점 : 일반적으로 for + append 보다 속도가 빠름.

```python
# 일반적인 for 문

result_1 = []
for i in range(10):
    result.appen(i)


# cmprehension

result_2 = [i for i in range(10)]

result_3 = [i for i in range(10) if i % 2 == 0 if i <= 6] # 이와 같이 조건 부여도 가능
```


- 리스트 출력할때 **pprint** 로 깔끔하게 출력 


### Lambda

```python
# 일반적인 함수
def f(x,y):
    return x + y

print(f(1,4))

# lambda 문

(lambda x, y : x + y)(1,4)
```

- 익숙하지 않아 거리감이 있었는데 연습해보려고 다시 써보니 역시 왜 쓰는지 잘 모르겠음.

### return 과 yield 의 차이

return 을 이용해서 랜덤한 수를 반환해주는 함수.

임의의 수를 생성하고 이를 10회 반복하려 return 하도록 구성.
![image](https://user-images.githubusercontent.com/61610411/128296545-d43f09e3-4580-418b-8ce5-2784eaca34fa.png)


이렇게 작성할 시, return이 호출되면 반복을 멈추고, 수를 반환하기 때문에 hello 가 출력되지 않음.

그리고 이 때 만들었던 임의의 수는 메모리에서 할당 해제되어 다시 호출할 수 없음.


반면 같은 기능을 하는 함수를 return 대신 yield를 사용해서 작성.

![image](https://user-images.githubusercontent.com/61610411/128296117-9e405fc6-6119-403d-8369-32e1c9cfd628.png)


yield가 호출되면 함수는 그 시점에서 일시 정지됨.

그리고 그 시점에 함수 안에 선언되어 있는 변수들을 기억하고 next()를 통해 실행되면

yield 바로 다음 라인부터 다시 실행을 이어나감.


**즉 yield 를 호출하면 원하는 값을 리턴하며, 실행 흐름을 일시 정지하여 함수를 재활용할 수 있는 상태로 만듬.**

(참고자료:https://yeomko.tistory.com/11)