---
title:  "MachineLearning - 05. Linear Regression의 cost 최소화 알고리즘"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
last_modified_at: 2020-01-23
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# 0. 이번장 할 내용
먼저 정리 부분에서 넘어 갔던 Cost 함수의 최소값을 구하기 위한 Gradient Descent 알고리즘에 대해 정리입니다.
~~~python
#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)
~~~


경사를 따라 내려가는 알고리즘 Gradient descent algorithm


---

# 1. Hypothesis 간결화
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost01.jpg?raw=true)
![Cost02](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost02.jpg?raw=true)

- b의 값(그래프 상으로는 y축의 절편값)을 없애 공식을 간편화 시킨다.
- 현재는 공식을 간폅게 만들기 위해 없애지만 실제로 W의 갯수가 많아지면, b를 행렬에 넣어 공식을 간단하게 만든다.

# 2. CostFunction의 모양
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost03.jpg?raw=true)

- 주어진 데이터(X,Y)에 W(0, 1, 2)일때의 값을 넣어 cost(W)를 값을 구해본다.
- cost(0) = 4.67, cost(1) = 0, cost(2) = 4.67의 값이 나온다.
- 이런식으로 계산값을 그래프화 시키면 아래의 그래프 처럼 이차함수 그래프가 나온다.
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/CostGraph.jpg?raw=true)
- W: X축, Cost(W) : Y축

# 3. Cost의 최소값 구하기
## 1. Gradient Descent 알고리즘 원리
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/CostGraph_Add.jpg?raw=true)

- Gradient Descent는 말그대로 경사를 따라 내려가는 알고리즘이다
- 위의 그래프를 보면 Cost의 최소값(별표)은 0이고 이를 구하기 위해 W와 b를 찾는게 목적이다.
- 임의의 W를 찍었을 경우, 미분을 통해 그 점의 기울기를 구할 수 있다.
- 구한 기울기를 기준으로 α만큼 이동을 그 지점에서 다시 기울기를 구하고 이를 반복한다.
- 결국 기울기가 수평일때, Cost의 값은 최소값이 된다.
- Gradient Descent 알고리즘 정리
  1. 최저비용을 계산하는 함수
  1. 다양한 최저계산에 사용
  3. CostFunction에 대해 최소가 되는 기울기(W)와 y절편(b) 탐색
  4. 변수가 여려개일 경우데도 적용 가능

## 2. Gradient Descent 알고리즘 공식화
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost05-01.jpg?raw=true)
- Cost(W)를 미분하여 어떤 방향으로 변화할지 기울기를 구한다.
- 구한 미분값을 α값(learning_rate)를 곱해 현재 W에 빼면 다음 번 W의 위치를 구할 수있다.
  (α값이 너무 크면 구한 값이 부정확할 수 있고, α값이 너무 작으면 학습 시간이 오래걸린다. 그래서 적당한 α값을 구해야 한다)
- 미분값을 빼주는 이유
  - 최소값 기준 오른쪽 경사일 경우 : 양수
  - 최소값 기준 왼쪽 경사일 경우 : 음수
  - 즉 최소값을 다가가기 위해 그래프상으로 감소함으로 빼준다.
- 기존 Cost(W)에서 m대신 2m을 대입한 이유는 미분시 공식 단순화 하기 위함이다
  (우리는 Cost(W)의 최소값을 구하기 위해 W아 b를 찾는게 목적이다. 그래서 W와 b가 계산이 끝난 뒤 m값이 적용이 되기 때문에 2m으로 바꾸어도 Cost(w)의 최소값을 구하는 데는 영향이 없다)

![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost06.jpg?raw=true)
- 미분화 과정

![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost07.jpg?raw=true)
- 최종 공식

## 3. Gradient Descent 알고리즘 사용시 주의 사항
![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Convexfunction01.jpg?raw=true)
- 위의 사진 처럼 CostFunction을 구하면 Gradient Descent 알고리즘을 사용하기 힘들다

![Cost01]](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Convexfunction01.jpg?raw=true)
- 그림 처럼 위에서 사용했던 CostFunction을 사용하면 밥그릇 형태로 그래프가 그려진다
- 이러한 형태 면 어느 지점이든지 최소값을 구할 수 있다
- 이러한 형태를 ConvexFunction이라고 한다.



마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.