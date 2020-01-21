---
title:  "MachineLearning - 03. Linear Regression"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
last_modified_at: 2020-01-21
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---

# 1. Linear Regression(선형 회귀)
- 연속적인 트레이닝 데이터에 기초해 정답을 추론하는 알고리즘
- 주어진 데이터를 대표하는 하나의 직선을 찾는 방식(직선:회귀선, 함수: 회귀식) 

--- 

# 2. 학습 과정 
예) Predicting fianl exam score based on time spent

![Ex01](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/03LinearRegression/LinearRegression01.PNG?raw=true)

## 1. Hypothesis
- 주어진 데이터에 맞는 선을 찾는 것이 학습이다
- 주어진 데이터를 기준으로 Hypothesis(가설)을 세워야 한다.

  ![Hypothesis](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/03LinearRegression/LinearRegression02.PNG?raw=true “Hypothesis”)

- 파란선을 기준으로 찾은 노란선과 빨간선이 Hypothesis이다
- 수식적으로는 `H(x) = Wx +b` Hypothesis(가설) 정의
- 이젠 가장 잘 맞는 선 찾는 과정이 필요 -> Cost Function

## 2.Cost Function(Lost Function)

  ![CostFunction01](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/03LinearRegression/LinearRegression04.PNG?raw=true)

- 실제 데이터와 모델(가설)과의 거리를 비교하는 함수(거리측정)
- 거리가 멀수록 학습이 잘못된 것이고, 거리가 가까울 수록 학습이 잘된 것이다.
- `(H(x)-y)**2`[^footnote] : 하나의 데이터에 대한 Cost값

[^footnote]: (H(x)(가설) - y(모델))로도 쓸수 있지만 음수가 나올수 있기 때문에 제곱으로 표현

  ![CostFunction02](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/03LinearRegression/LinearRegression03.PNG?raw=true)

- 평균 Cost 값을 구한다(m: 실제 데이터 갯수)

  ![CostFunction03](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/03LinearRegression/LinearRegression05.PNG?raw=true)

- 최종적으로 수식으로 표현했을 경우 위와 같이 표현된다.
- **가장 Cost를 찾는 것이 Lnear Regression의 학습 목표** 


마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.