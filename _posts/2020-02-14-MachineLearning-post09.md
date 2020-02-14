---
title:  "MachineLearning - 09. TensorFlow로 파일 데이터 로드(Python)"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
  - Python
  - Numpy
last_modified_at: 2020-02-14
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.
---
# TensorFlow로 파일 데이터 로드 구현
- 슬라이싱과 Numpy라이브러리
- csv파일로 저장 및 Numpy로 데이터 로드

---

# 1. 슬라이싱과 Numpy라이브러리

## 1. 슬라이싱
~~~python
nums = range(5)    # range는 파이썬에 구현되어 있는 함수, 정수들로 구성된 리스트만듬
print nums         # 출력 "[0, 1, 2, 3, 4]"
print nums[2:4]    # 인덱스 2에서 4(제외)까지 슬라이싱; 출력 "[2, 3]"
print nums[2:]     # 인덱스 2에서 끝까지 슬라이싱; 출력 "[2, 3, 4]"
print nums[:2]     # 처음부터 인덱스 2(제외)까지 슬라이싱; 출력 "[0, 1]"
print nums[:]      # 전체 리스트 슬라이싱; 출력 ["0, 1, 2, 3, 4]"
print nums[:-1]    # 마지막 인덱스를 맨 나머지 리스트; 출력 ["0, 1, 2, 3]"
nums[2:4] = [8, 9] # 슬라이스된 리스트에 새로운 리스트 할당
print nums         # 출력 "[0, 1, 8, 9, 4]"
~~~
- `range(N)`함수 같은 경우 0부터 N-1 까지의 정수 리스트 출력
- 슬라이싱 할 경우 [X:N]일때 X부터 N-1까지 출력
- [-1]는 리스트 끝부분을 뜻함


## 2. Numpy라이브러리
- Numpy란?
  - 계산과학분야에 이용되는 라이브러리
  - 고성능의 다차원 배열 객체 및 툴 제공

- Numpy 배열
  - Tensor와 같은 다차원 배열 구조를 가진다
  - 차이점

    Numpy | Tensor 
    :---------:|:--------:
    기본형(속도빠름) | 참조형
    Dynamic typing 불가능 | Dynamic typing 가능
    벡터연산 가능 | 벡터연산 불가능

~~~python
import numpy as np

a = np.array([1, 2, 3])  # rank가 1인 배열 생성
print type(a)            # 출력 "<type 'numpy.ndarray'>"
print a.shape            # 출력 "(3,)"
print a[0], a[1], a[2]   # 출력 "1 2 3"
a[0] = 5                 # 요소를 변경
print a                  # 출력 "[5, 2, 3]"

b = np.array([[1,2,3],[4,5,6]])   # rank가 2인 배열 생성
print b.shape                     # 출력 "(2, 3)"
print b[0, 0], b[0, 1], b[1, 0]   # 출력 "1 2 4"
~~~
- Numpy사용을 위해서 `import numpy as np` 입력
- `array`를 통해 배열 생성

- 기본적인 배열 함수 제공
~~~python
import numpy as np

a = np.zeros((2,2))  # 모든 값이 0인 배열 생성
print a              # 출력 "[[ 0.  0.]
                     #       [ 0.  0.]]"

b = np.ones((1,2))   # 모든 값이 1인 배열 생성
print b              # 출력 "[[ 1.  1.]]"

c = np.full((2,2), 7) # 모든 값이 특정 상수인 배열 생성
print c               # 출력 "[[ 7.  7.]
                      #       [ 7.  7.]]"

d = np.eye(2)        # 2x2 단위행렬 생성
print d              # 출력 "[[ 1.  0.]
                     #       [ 0.  1.]]"

e = np.random.random((2,2)) # 임의의 값으로 채워진 배열 생성
print e                     # 임의의 값 출력 "[[ 0.91940167  0.08143941]
                            #                  [ 0.68744134  0.87236687]]"

~~~

- 더 자세한 내용 : [Numpy](http://aikorea.org/cs231n/python-numpy-tutorial/#numpy)

---
# 2. csv파일로 저장 및 Numpy로 데이터 로드


## 1. 전체 코드
~~~python
import numpy as np
import tensorflow as tf
tf.set_random_seed(777) # 랜덤시드 생성

#Numpy로 파일 로드 및 배열 만들기
xy = np.loadtxt('data_01.csv', delimiter=',', dtype=np.float32)

#슬라이싱
x_data = xy[:,:-1]
y_data = xy[:,[-1]]

print(x_data.shape, x_data, len(x_data))
print(y_data.shape, y_data)

X = tf.placeholder(tf.float32, shape=[None,3])
Y = tf.placeholder(tf.float32, shape=[None,1])

W = tf.Variable(tf.random_normal([3,1], name='weight'))
b = tf.Variable(tf.random_normal([1], name='bias'))

hypothesis = tf.matmul(X,W) +b

cost = tf.reduce_mean(tf.square(hypothesis-Y))

optimizer = tf.train.GradientDescentOptimizer(learning_rate= 1e-5)
train = optimizer.minimize(cost)

sess = tf.Session()
sess.run(tf.global_variables_initializer())

for step in range(2001):
    cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                          feed_dict={X: x_data, Y: y_data}) 
    if(step % 10 ==0):
        print(step, "Cost :", cost_val, "\nPrediction:\n", hy_val)

print(sess.run(hypothesis, feed_dict={X: [[100,47,101], [90,35,125]]}))
~~~

- data_01.csv 내용
~~~bash
#EXAM1	EXAM2	EXAM3	Fianl
73,	80,	75,	152
93,	88,	93,	185
89,	95,	90,	180
96,	98,	100, 196
73,	66,	70,	142
53,	46,	55,	101
~~~

## 2. 코드분석
### 1. Build graph using TF operatios by placeholder
~~~python
import numpy as np
import tensorflow as tf
tf.set_random_seed(777) # 랜덤시드 생성

#Numpy로 파일 로드 및 배열 만들기
xy = np.loadtxt('data_01.csv', delimiter=',', dtype=np.float32)
# [[ 73.  80.  75. 152.]
#  [ 93.  88.  93. 185.]
#  [ 89.  95.  90. 180.]
#  [ 96.  98. 100. 196.]
#  [ 73.  66.  70. 142.]
#  [ 53.  46.  55. 101.]]
~~~
- np.loadtxt
  1. *.csv의 경우 엑셀 또는 메모장으로 만들면 된다.
  1. 같은 폴더내에 있어야 로드 가능
  1. `delimiter=','` : ,제거
  1. `dtype=np.float32` : float32타입으로 로드


~~~python
#슬라이싱
x_data = xy[:,:-1]
# [[ 73.  80.  75.]
#  [ 93.  88.  93.]
#  [ 89.  95.  90.]
#  [ 96.  98. 100.]
#  [ 73.  66.  70.]
#  [ 53.  46.  55.]]
y_data = xy[:,[-1]]
# [[152.]
#  [185.]
#  [180.]
#  [196.]
#  [142.]
#  [101.]]
print(x_data.shape, x_data, len(x_data))
print(y_data.shape, y_data)

X = tf.placeholder(tf.float32, shape=[None,3])
Y = tf.placeholder(tf.float32, shape=[None,1])

W = tf.Variable(tf.random_normal([3,1], name='weight'))
b = tf.Variable(tf.random_normal([1], name='bias'))
~~~
- x_data = xy[:,:-1] : shape[6,3]
- y_data = xy[:,[-1]] ; shape[6,1]

### 2. Run & Udapte graph an get resluts
~~~python
print(sess.run(hypothesis, feed_dict={X: [[100,47,101], [90,35,125]]}))
# 학습 결과 실험 : [[152.40878] [115.21241]]
~~~
- 나머지 코드는 똑같아서 생략
- 학습된 결과에 새로운 값을 대입하여 결과 확인

## 3. 결과값
~~~bash
............
1980 Cost : 5.5494294 
Prediction:
 [[152.84468]
 [182.90678]
 [184.5525 ]
 [193.5945 ]
 [142.18741]
 [ 99.71366]]
1990 Cost : 5.530891 
Prediction:
 [[152.83754]
 [182.91144]
 [184.54568]
 [193.59401]
 [142.1925 ]
 [ 99.72224]]
2000 Cost : 5.5124497
Prediction:
 [[152.83041]
 [182.91606]
 [184.53885]
 [193.5935 ]
 [142.19757]
 [ 99.73078]]
~~~


참고사이트[출처](http://aikorea.org/cs231n/python-numpy-tutorial/)

마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.