---
title:  "MachineLearning - 02(Python)"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
  - Python
last_modified_at: 2020-01-18
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

TensorFlow 및 기타 

1. 그래프 빌드(PlaceHolder) - 노드 만들기 설계
2. 그래프 실행
3. 값 업데이트 및 출력값 리턴

---

# 1. TensorFlow란?
> TensorFlow는 머신러닝을 위한 엔드 투 엔드 오픈소스 플랫폼입니다. 도구, 라이브러리, 커뮤니티 리소스로 구성된 포괄적이고 유연한 생태계를 통해 연구원들은 ML에서 첨단 기술을 구현할 수 있고 개발자들은 ML이 접목된 애플리케이션을 손쉽게 빌드 및 배포할 수 있습니다.       - [출처:텐서플로우 공식 홈페이지](https://www.tensorflow.org/?hl=ko) -

- 구글에서 만든 딥러닝 오픈소스 패키지
- Tensor는 다양한 분야에서 조금씩 다른 의미로 씌이고 있지만 여기서는 '학습데이터가 저장되는 다차원 배열'로 정의
- 데이터 플로우 그래프(Data Flow Graph)로 연산을 나타내는 프로그래밍 시스템

## 1. 데이터 플로우 그래프
- 수학 계산과 데이터의 흐름을 노드(Node)와 엣지(Edge)를 사용한 방향 그래프(Directed Graph)로 표현

  ![data flow graph](https://www.tensorflow.org/images/tensors_flowing.gif)

- Node : 수학젹 계산, 데이터 입/출력, 그리고 데이터 읽기/저장 등의 작업 수행
- Edge : Node들 간 데이터의 입출력 관계

## 2. TensorFlow 특징
- 데이터 플로우 그래프를 통한 풍부한 표현력
- 코드 수정없이 CPU/GPU 모드로 동작
- 아이디어 테스트에서 서비스 단계까지 이용가능
- 계산 구조와 목표함수만 정의하면 자동으로 미분계산 처리
- Python/C++ 지원, [SWIG](https://ko.wikipedia.org/wiki/SWIG)를 통해 다양한 언어 지원 가능

  출처 : [텐서플로우시작하기](https://gist.github.com/haje01/202ac276bace4b25dd3f)

---

# 2. 기본 개념 
## 1.용어
**오퍼레이션(Operation)**
- 그래프 상의 노드는 오퍼레이션(줄임말 op)으로 불림. 오퍼레이션은 하나 이상의 텐서를 받을 수 있고, 계산을 수행하고, 결과를 하나 이상의 텐서로 반환할 수 있음.

**텐서(Tensor)**
- 내부적으로 모든 데이터는 텐서를 통해 표현. 텐서는 일종의 다차원 배열로 그래프 내의 오퍼레이션 간에는 텐서만이 전달. *즉 모든것은 Tensor만으로 전달 및 사용*
- 구조
  - Rank
    - TensorFlow에서는 tesnor는 rank라는 차원 단위로 표현

    Rank | Math Enitiy | Python Example
    -----|---------|--------
    0 | Scalar(magnitude only) | s = 483
    1 | Vector(magnitude and direction) | v = [1.1, 2.2, 3.3]
    2 | Matrix(table of numbers) | m = [[1, 2, 3],[4, 5, 6],[7, 8, 9]]
    3 | 3-Tensor(cube if numbers) | t = [[[2],[4],[6]], [[8],[10],[12]], [[14],[16], [18]]]
    n | n-Tensor(you get the idea) | ...

    - rank2인 tensor는 행렬, rank1인 tensor는 벡터로 보면 된다.
    - rank2인 tensor는 t[i,j] 형식으로 원소에 접근 가능하다
    - rank3인 tensor는 t[i,j,k] 형식으로 원소를 지정할 수 있다.
    
    ~~~bash
    t = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    ~~~
    - 위의 예제 같은 경우 rank2에 속하고, t[3,3]으로 표현 가능하다.

  - Shape
    - Tesnfor 차원을 표현할 때 세가지 기호를 사용

    Rank | Shape | Dimension number | Python Example
    -----|-------|--------|------
    0 | [] | 0-D | A 0-D tensor A scalar
    1 | [D0] | 1-D | A 1-D tensor with shape[5]
    2 | [D0, D1] | 2-D | A 2-D tensor shape[3, 4]
    3 | [D0, D1, D2] | 3-D | A 2-D tensor shape[1, 3, 4]
    n | [D0, D1, ... , Dn-1] | n-D | A tensor with shape [D0, D1,...,Dn-1]
    
  - Data types
    - Tensor는 차원 말고도 데이터 타입도 갖는다.

    Data type | Python tpye | Description
    -----|---------|--------
    DT_FLOAT | tf.float32 | 32비트 부동 소수
    DT_DOUBLE | tf.float64 | 64비트 부동 소수
    DT_INT8 | tf.int8 | 8비트 부호 있는 정수
    DT_INT16 | tf.int16 | 16비트 부호 있는 정수
    DT_INT32 | tf.int32 | 32비트 부호 있는 정수
    DT_INT64 | tf.int64 | 64비트 부호 있는 정수

    ...


**세션(Session)**
- 오퍼레이션의 실행 환경을 캡슐화
- 그래프의 op를 CPU나 GPU같은 디바이스에 배정 및 실행을 위해 메서드 제공

  출처 : [텐서플로우시작하기](https://gist.github.com/haje01/202ac276bace4b25dd3f)

## 2. 연산과정
  ### 1.Construction Phase(구성단계)
    - 텐서플로우 op를 통해 그래프 빌드, 즉 그래프를 조립
    - 단순히 연산을 위해 그래프를 표현해 놓는 과정, 실행을 위해 Session 필요

  ### 2.Execution Phase(실행단계)
    - Session을 이용해 그래프의 op 실행
    - Session은 그래프의 op를 CPU나 GPU같은 디바이스에 배정 및 실행을 위해 메서드 제공

---

# 3. 기본 코드 예제

- 텐서플로우 버전 확인
~~~ python
import tensorflow as tf # 텐서플로우 임포트 후 tf로 정의
tf.__version__ # 버전 확인
~~~
~~~bash
'1.12.0'
~~~
  - 깔린 텐서플로우 마다 결과값은 다르게 나옵니다.

- Hello Tensor Flow! 출력하기
~~~ python
import tensorflow as tf
hello = tf.constant("Hello, TensorFlow!") # 상수 형태로 저장(변하지 않는 수)
sess = tf.Session()
print(sess.run(hello))
~~~
~~~bash
b'Hello, TensorFlow!'
~~~
  - 결과값에 b가 나오는 이유 [링크](https://stackoverflow.com/questions/6269765/what-does-the-b-character-do-in-front-of-a-string-literal)

- Computational Graph
~~~python
node1 = tf.constant(3.0, tf.float32)
node2 = tf.constant(4.0) # also tf.float32 implicitly
node3 = tf.add(node1, node2)

print("node1:", node1, "node2:", node2)
print("node3: ", node3)
~~~
~~~bash
node1: Tensor("Const_1:0", shape=(), dtype=float32) node2: Tensor("Const_2:0", shape=(), dtype=float32)
node3:  Tensor("Add:0", shape=(), dtype=float32)
~~~
- session에 그래프를 넣지 않았기 때문에 출력값은 단지 그래프 값만 표시됨 
- 연산을 위해서는 sesseion을 추가해야 함

~~~python
sess = tf.Session() #session을 sess로 선언
print("sess.run(node1, node2): ", sess.run([node1, node2])) # run 함수를 통해 그래프 실행
print("sess.run(node3): ", sess.run(node3))
~~~
~~~bash
sess.run(node1, node2):  [3.0, 4.0]
sess.run(node3):  7.0
~~~



마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.