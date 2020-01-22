---
title:  "MachineLearning - 01.머신러닝이란?"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
last_modified_at: 2020-01-17
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.


# 1. 머신러닝이란?
> "Field of study that gives computers the ability to learn  
   without being explicitly programmed”   
                                   - Arthur Samuel (1959) -

- 기계학습이라는 뜻으로 컴퓨터가 학습할 수 있도록 알고리즘과 기술을 개발 하는 분야  
  즉 '데이터에서 법칙성(규칙)을 추출하는 통계적 방법' 임
- 딥러닝 같은 경우는 머신러닝의 한부분으로 인간두뇌의 신경세포를 모방한 신경망 모델
- AI >> 머신러닝 >> 딥러닝

# 2. 머신러닝 학습 방법
## 1. Supervised Learning(지도학습)
- 미리 제공된 데이터(정답인 데이터)를 통해 학습을 한다
- 사용 예제
  1. Image Labeling : 분류된 이미지로 이미지 구별
  1. Email spam filter : 스팸인지 아닌지 구별된 데이터로 스팸메일 구별
  1. Predicting exam score : 이전 시험공부 시간 대비 점수로 이루어진 데이터로 점수 예상
- 지도학습 분류
  - Regression : 연속적인 트레이닝 데이터에 기초해 정답을 추론하는 방법  
  
  예) Predicting fianl exam score based on time spent

      X(Hours) | Y(Score)
      :---------:|:--------:
      10 | 90
      9 | 80
      3 | 50
      2 | 30
  
      위의 테이블을 통해 다른 시간일 때 점수 예상 가능(7시간 공부 하면 75점 정도로 예측)
  - Binary Classification : 두가지의 경우로 정답을 분류하는 방법
  
  예) Pass or Non-pass based on time spent

      |X(Hours) | Y(Pass/Fail)|
      |:---:|:---:|
      |10 | P|
      |9 | P|
      |3 | F|
      |2 | F|
  
  - Multi-label Classification : 여러가지 경우로 정답을 분류하는 방법
  
  예) Letter grade(A, B, C, E, F) based on time spent
 
      |X(Hours) | Y(Grade)|
      |:---:|:---:|
      |10 | A
      |9 | B
      |3 | D
      |2 | F

## 2. Unsupervised Learning(비지도학습)
- 정답인 데이터를 제공하지 않고 오로지 주어진 데이터를 분류만 함
- 공장에서 불량품 찾는 곳에서 활용가능

## 3. Reinforcement Learning(강화학습)
- 반복 훈련을 통해 행동에 대한 보상을 기반으로 올바른 정답을 구하면 상점, 틀린답을 구하면 벌점을 주어 최상의 결과를 찾는 방법
- Unity의 ML-Agent가 강화학습으로 구성


마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.