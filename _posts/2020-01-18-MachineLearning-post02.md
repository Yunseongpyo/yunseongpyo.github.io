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
last_modified_at: 2020-01-17
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
- 데이터 플로우 그래프(Data Flow Graph) 방식 사용

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
## 용어
**오퍼레이션(Operation)**
- 그래프 상의 노드는 오퍼레이션(줄임말 op)으로 불림. 오퍼레이션은 하나 이상의 텐서를 받을 수 있고, 계산을 수행하고, 결과를 하나 이상의 텐서로 반환할 수 있음.

**텐서(Tensor)**
- 내부적으로 모든 데이터는 텐서를 통해 표현. 텐서는 일종의 다차원 배열로 그래프 내의 오퍼레이션 간에는 텐서만이 전달.

**세션(Session)**
- 오퍼레이션의 실행 환경을 캡슐화

  출처 : [텐서플로우시작하기](https://gist.github.com/haje01/202ac276bace4b25dd3f)

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
~~~ python


마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.