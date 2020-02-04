---
title:  "MachineLearning - 07. Multivariable Linear Regression"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
last_modified_at: 2020-02-04
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# 0. Multivariable Linear Regression
- 받는 값이 여러가지 경우 단순하게 생각하자
- Matrix multiplication 활용

---

# 1. One-Variable/Featrue VS Multi-Variable/Featrue
- One-Variable/Featrue

    X(Hours) | Y(Score)
    :---------:|:--------:
    10 | 90
    9 | 80
    3 | 50
    2 | 30
    11| 40

- Multi_variable/Featrue

    X1(Quiz1) | X2(Quiz2) | X3(Quiz3) | Y(Final)
    :---------:|:--------:
    73 | 80 | 75 | 152
    93 | 88 | 93 | 185
    89 | 91 | 90 | 180
    96 | 98 | 100 | 196
    73 | 66 | 70 | 142

    - 기존 예제에서 X값이 추가 되었다

![formula01](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/formula01.png?raw=true)
- x값이 3개로 늘어남에 따라 w값 또한 3개로 늘어났다
- b값은 여전히 하나이다

![formula02](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/formula02.png?raw=true)
- CostFunction또한 x값이 늘어남에 따라 위의 공식처럼 표현한다.
- Hypothesis 입력값이 늘어난거 뿐이기 때문에 CostFuncton은 신경쓸게 없다

![formula03](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/formula03.png?raw=true)
- 첫번째 Hypothesis : x의 갯수가 3개 일때
- 두번째 Hypothesis : x의 갯수가 n개 일때

# 2. Matrix multiplication 활용한 계산방법
- 변수가 여러개일때 계산을 편하기 위해 행렬(Matrix)로 표현한다.

![matrix01](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/MatrixMultiplication02.PNG?raw=true)
- 위처럼 Hypothesis 수식을 행렬로 표현할 수 있다.(우선 b의 값은 생략)
- `H(x) = wx`에서 `H(X) = XW`로 바뀌었지만, 곱셈이기 때문에 x와 w의 순서는 상관 없다
- 매트릭스는 보통 대문자로 표현하기 때문에 수식은 `H(X) = XW`로 표현

## 1. 계산 방법
![matrix02](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/MatrixMultiplication01.PNG?raw=true)
- 2X3행렬과 3X2행렬의 곱셈
- 2X3행렬의 1행 값과 3X2행렬의 1열의 값을 깨리 곱하고 더해준다
- 그러면 최종적으로 2X2 행렬이 나온다.

![matrix03](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/MatrixMultiplication03.PNG?raw=true)
- [5,3]*[3,1] = [5,1]이 나온다
- 곱셈을 하기 위해서는 [5,a]*[b,1] 에서 a=b이여야 한다.
- 그리고 최종 값은 첫번째 행렬의 행값과 두번재 행렬의 열값으로 나온다.

# 3. 최종 공식 정리
![formula04](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/08Multi_variableFeatrue/formula04.png?raw=true)
- 첫번째 공식 : 이론공식
- 두번째 공식 : 실제 텐서플로우 계산시 활용되는 공식

마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.