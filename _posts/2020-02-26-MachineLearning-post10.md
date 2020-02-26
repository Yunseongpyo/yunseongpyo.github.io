---
title:  "MachineLearning - 10. Logistic Classification의 가설함수"
excerpt: "MachineLearning 정리."
toc: true
toc_sticky: true
categories:
  - MachineLearning
tags:
  - MachineLearning
last_modified_at: 2020-02-26
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# 1. Logistic Classification

## 1. Logistic Classification이란?
- Classification의 사전적 의미 : 분류
- Linear Regression을 활용해 데이터를 분류 하는 모델
- 분류에서 가장 단순한 모델로 2가지 중 하나를 찾음(Binary)

## 2. 쓰이는 예제
- 사용되는 곳 모두 기존 패턴 데이터를 기반으로 분류
- 프로그래밍에서 봤을때 0,1로 구분
  - Spam 메일 구분(Spam VS Ham)
  - 페이스북 Feed(Show VS Hide)
  - 신용카드 사기 사용 확인(평소 쓰는 패턴 VS 평소 안쓰는 패턴)
  - 영상의학 분야(정상적인 몸 VS 이상이 있는 몸)
  - 주식시장(오를까 VS 내릴까)

## 3. 기존 Linear Regression 사용시 문제점
1. 기울기 문제
  ![Linear RegressionProblem](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/10.LogisticClassification/LinearRegressionProblem.jpg?raw=true)
  - Linear Regression는 초과되는 새로운 데이터 발생시 기울기 변화가 생김
  - 즉 학습된 모델(기울기)로 예측이 실패 하기 때문에 다시 학습해야 되는 상황 발생
  
2. Binary 문제
  - 분류의 경우 0 or 1 일 경우만 필요함
  - Linear Regression일 경우 1이상의 예측값이 나올 수 있음

3. 해결책
  - 초과되는 값이 나오더라도 모델 수정이 필요 없는 방법
  - 예측값이 0~1사이 에서만 나오는 방법

## 4. 해결책 - Sigmoid
![sigmoid](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/10.LogisticClassification/Sigmoid02.png?raw=true)
- Sigmod함수는 X축 값이 어떤 값이든 Y값은 0~1사이에서만 나온다
- g(z)로 표현

![sigmoid02](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/10.LogisticClassification/Sigmoid03.png?raw=true)
- g(z)에 기존 LinearRegression 값 H(X)= Wx+b를 Z에 대입시 최종 공식


마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.