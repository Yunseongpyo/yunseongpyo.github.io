---
title:  "MachineLearning - 08. Multy-variable Linear Regression 구현(Python)"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
  - Python
last_modified_at: 2020-02-08
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# Multy-variable Linear Regression 구현

X1(Quiz1) | X2(Quiz2) | X3(Quiz3) | Y(Final)
:---------:|:--------:
73 | 80 | 75 | 152
93 | 88 | 93 | 185
68 | 91 | 90 | 180
96 | 98 | 100 | 196
73 | 66 | 70 | 142

- 위의 결과값을 이용한 Multy-variable Linear Regression 구현
  1. 일반적인 구현 방법 
  1. 매트릭스를 활용한 구현 방법

---

# 1. 일반적인 구현


## 1. 전체 코드
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정

#학습할 데이터 자료
x1_data = [75., 93., 68., 96., 73.]
x2_data = [80., 88., 91., 98., 66.]
x3_data = [75., 93., 90., 100., 70.]
y_data = [152.,185.,180.,196.,142.]

#데이터 받을 변수들
x1 = tf.placeholder(tf.float32)
x2 = tf.placeholder(tf.float32)
x3 = tf.placeholder(tf.float32)
Y = tf.placeholder(tf.float32)

#구해야 할 W(w1,w2,w3) 및 b값 랜덤값으로 정의
w1 = tf.Variable(tf.random_normal([1]), name='weight1')
w2 = tf.Variable(tf.random_normal([1]), name='weight2')
w3 = tf.Variable(tf.random_normal([1]), name='weight3')
b= tf.Variable(tf.random_normal([1]), name='bias')

#가설 공식
hypothesis = x1*w1 + x2*w2 + x3*w3 +b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - Y))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate= 1e-5)
train = optimizer.minimize(cost)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(2001):
    cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                          feed_dict={x1: x1_data, x2: x2_data, x3: x3_data, Y: y_data}) 
    if(step % 10 ==0):
        print(step, "Cost :", cost_val, "\nPrediction:\n", hy_val)

~~~
## 2. 코드분석
### 1. Build graph using TF operatios
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정

#학습할 데이터 자료
x1_data = [75., 93., 68., 96., 73.]
x2_data = [80., 88., 91., 98., 66.]
x3_data = [75., 93., 90., 100., 70.]
y_data = [152.,185.,180.,196.,142.]

#데이터 받을 변수들
x1 = tf.placeholder(tf.float32)
x2 = tf.placeholder(tf.float32)
x3 = tf.placeholder(tf.float32)
Y = tf.placeholder(tf.float32)

#구해야 할 W(w1,w2,w3) 및 b값 랜덤값으로 정의
w1 = tf.Variable(tf.random_normal([1]), name='weight1')
w2 = tf.Variable(tf.random_normal([1]), name='weight2')
w3 = tf.Variable(tf.random_normal([1]), name='weight3')
b= tf.Variable(tf.random_normal([1]), name='bias')

~~~
- 학습 입력 데이터(x1_data, x2_data, x3_data)와 결과 데이터(y_data) 정의 
- 데이터 받을 변수들(x1, x2, x3, Y) `placeholder`로 정의
- 컴퓨터가 출력하는 변수들(w1, w2, w3, b) `Variable`로 정의

~~~python
#가설 공식
hypothesis = x1*w1 + x2*w2 + x3*w3 +b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - Y))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate= 1e-5)
train = optimizer.minimize(cost)
~~~
- 여러 값을 받는 hypothesis식 정의
- `1e-5` == 0.00001 와 값음

### 2. Run & Udapte graph an get resluts
~~~python
#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(2001):
    cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                          feed_dict={x1: x1_data, x2: x2_data, x3: x3_data, Y: y_data}) 
    if(step % 10 ==0):
        print(step, "Cost :", cost_val, "\nPrediction:\n", hy_val)

~~~
- 2000번 학습 시키고 10번마다 cost와 hypothesisr값 출력
- train에 필요한 x값들과 y값 넣어주기

## 2. 결과값
~~~bash
0 Cost : 85044.164 
Prediction:
 [-110.81506  -129.87576  -113.758125 -138.99664   -99.838295]
10 Cost : 12.711104 
Prediction:
 [153.14961 184.43903 172.86333 198.46986 139.78937]
20 Cost : 11.337999 
Prediction:
 [154.1517  185.61557 174.01117 199.7503  140.67699]
30 Cost : 11.203392 
Prediction:
 [154.15158 185.5989  174.0703  199.74948 140.65501]
...............
1970 Cost : 5.5551157
Prediction:
 [153.58856 184.18779 178.07129 199.35437 138.89828]
1980 Cost : 5.5516033
Prediction:
 [153.58627 184.18678 178.0764  199.35399 138.89662]
1990 Cost : 5.5481853
Prediction:
 [153.58395 184.18578 178.08144 199.35364 138.89496]
2000 Cost : 5.5447683
Prediction:
 [153.58163 184.18477 178.08641 199.35326 138.89331]
~~~
- cost 값은 계속 줄어들고 있고 hypothesis값 또한 y_data값과 비슷해 지고 있다.

---
# 2. 매트릭스로 구현


## 1. 전체 코드
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정

#매트릭스 방법
x_data = [[73., 80., 75.], 
          [93., 88., 93.],
          [89., 91., 90.], 
          [96., 98., 100.], 
          [73., 66., 70.]]
y_data = [[152.],[185.],[180.],[196.],[142.]]

# placeholders
X = tf.placeholder(tf.float32, shape=[None, 3])
Y = tf.placeholder(tf.float32, shape=[None, 1])

W = tf.Variable(tf.random_normal([3,1]),name= 'weight')
b= tf.Variable(tf.random_normal([1]), name='bias')

hypothesis = tf.matmul(X,W) + b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - Y))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate= 1e-5)
train = optimizer.minimize(cost)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(2001):
    cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                          feed_dict={X: x_data, Y: y_data}) 
    if(step % 10 ==0):
        print(step, "Cost :", cost_val, "\nPrediction:\n", hy_val)

~~~
- 기존 그래프 빌드 과정에서 입력값들을 매트릭스로 변경

## 2. 코드분석
### 1. Build graph using TF operatios by placeholder
~~~python
#매트릭스 방법
x_data = [[73., 80., 75.], 
          [93., 88., 93.],
          [89., 91., 90.], 
          [96., 98., 100.], 
          [73., 66., 70.]]
y_data = [[152.],[185.],[180.],[196.],[142.]]
~~~
- x_data : [5,3], y_dat : [5,1] 매트릭스로 정의

~~~python
# placeholders
X = tf.placeholder(tf.float32, shape=[None, 3])
Y = tf.placeholder(tf.float32, shape=[None, 1])

W = tf.Variable(tf.random_normal([3,1]),name= 'weight')
b= tf.Variable(tf.random_normal([1]), name='bias')

hypothesis = tf.matmul(X,W) + b
~~~
- X : 기본적으로 x값이 3개이고 데이터 갯수가 N개(현재 주어진 x_data는 5개) 이므로 shape는 `shape=[None, 3]`로 정의(데이터가 늘어날 경우 대비 None으로 정의)
- Y : y_data값은 데이터 갯수당 1개 이므로 `shape=[None, 1]`로 구현
- `tf.matmul(X,W)`
 - 텐서플로우의 매트릭스 곱셈 함수로 매트릭스 곱 구현

### 2. Run & Udapte graph an get resluts
~~~python

for step in range(2001):
    cost_val, hy_val, _ = sess.run([cost, hypothesis, train],
                          feed_dict={X: x_data, Y: y_data}) 
    if(step % 10 ==0):
        print(step, "Cost :", cost_val, "\nPrediction:\n", hy_val)

~~~
- X에 x_data, Y에 y_data 매트릭스 넣기
## 2. 결과값
~~~bash
0 Cost : 183810.95 
Prediction:
 [[-225.85709]
 [-275.002  ]
 [-269.20688]
 [-290.7459 ]
 [-212.92038]]
20 Cost : 13.655874 
Prediction:
 [[154.51501]
 [182.22302]
 [181.28087]
 [199.82495]
 [135.83789]]
40 Cost : 13.541206 
Prediction:
 [[154.49428]
 [182.24399]
 [181.27779]
 [199.82256]
 [135.8645 ]]
............
1960 Cost : 6.6363306
Prediction:
 [[152.70035]
 [183.48975]
 [180.74835]
 [199.28217]
 [137.6322 ]]
1980 Cost : 6.5946045
Prediction:
 [[152.6863 ]
 [183.49953]
 [180.74426]
 [199.27762]
 [137.6464 ]]
2000 Cost : 6.553218
Prediction:
 [[152.67233]
 [183.50928]
 [180.74019]
 [199.27307]
 [137.66054]]
~~~

마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.