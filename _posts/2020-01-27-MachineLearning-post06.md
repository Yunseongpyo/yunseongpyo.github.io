---
title:  "MachineLearning - 06. Linear Regression의 cost최소화 구현(Python)"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
  - Python
last_modified_at: 2020-01-27
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# Linerar Regression의 Cost 최소화 파이썬 구현
  1. CostFunction의 그래프화(실제 그래프로 표시) 
  1. Gradient Descent 구현
  1. `optimizer`를 이용한 Cost최소화

---

# 1. CostFunction 그래프화


## 1. 전체 코드
~~~python
import tensorflow as tf
import matplotlib.pyplot as plt  # 그래프 출력 모듈

# 1.그래프 빌드 과정
# X(입력)에 관한 Y(출력)의 데이터(정답) 값 
x_train = [1,2,3]
y_train = [1,2,3]

W = tf.placeholder(tf.float32)

#H(x) = Wx
hypothesis = x_train * W

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

#그래프 표시를 위한 데이터 리스트 (W: x축, cost: y축)
W_val = []
cost_val = []

# 0.1단위로 증가할수 없어 -30 부터 시작, 그래프 에서는 -3~5 로 표시
for i in range(-30,50):
    feed_W = i * 0.1  # x좌표로 -3에서 5까지 0.1씩 증가
    curr_cost, curr_w = sess.run([cost, W], feed_dict={W: feed_W}) #x값따라 변하는 y값
    #그래프에 표시할 데이터 누적
    W_val.append(curr_w)
    cost_val.append(curr_cost)

#그래프 타입
plt.plot(W_val, cost_val, 'ro')
plt.show()
        
~~~
## 2. 코드분석
### 1. Build graph using TF operatios
~~~python
import tensorflow as tf
import matplotlib.pyplot as plt 
~~~
- `matplotlib` : 그래프 출력 모듈

### 2. Run & Udapte graph an get resluts
~~~python
#그래프 표시를 위한 데이터 리스트 (W: x축, cost: y축)
W_val = []
cost_val = []

# 0.1단위로 증가할수 없어 -30 부터 시작, 그래프 에서는 -3~5 로 표시
for i in range(-30,50):
    feed_W = i * 0.1  # x좌표로 -3에서 5까지 0.1씩 증가
    curr_cost, curr_w = sess.run([cost, W], feed_dict={W: feed_W}) #x값따라 변하는 y값
    #그래프에 표시할 데이터 누적
    W_val.append(curr_w)
    cost_val.append(curr_cost)

#그래프 타입
plt.plot(W_val, cost_val, 'ro')
plt.show()
~~~
- 그래프 표시를 위해 W(x축)과 Cost(y축) 리스트화
- `pli.plot(X축, Y축, 그래프 모양)`
  - 그래프 모양의 경우 r은 빨강(red), o는 동그라미 모양 의미(x일 경우 x 표시)
- `plt.show()` : 그래프 출력


## 2. 결과값
![CostFunctionGraph](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/06CostMinimize(pythone)/Figure_2.png?raw=true){: width="500" height="500"}
- CostFunction의 그래프가 2차함수로 표현된 것을 볼수 있다.


---
# 2. Gradient Descent 구현


## 1. 전체 코드
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정

x_data = [1,2,3]
y_data = [1,2,3]

w = tf.Variable(tf.random_normal([1], name='weight'))
x_train = tf.placeholder(tf.float32)
y_train = tf.placeholder(tf.float32)

#H(x) = Wx 정의
hypothesis = x_train*w

#Cost Function
cost = tf.reduce_sum(tf.square(hypothesis - y_train))

#GradientDescent - cost minimize
learning_rate = 0.1
gradient = tf.reduce_mean((w*x_train-y_train)*x_train)
descent = w - learning_rate*gradient
update = w.assign(descent)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(21):
    sess.run(update, feed_dict={x_train : x_data, y_train : y_data})
    cost_val = sess.run(cost, feed_dict={x_train:[1,2,3], y_train:[1,2,3]})
    w_val = sess.run(w)
    print(step, cost_val, w_val)
~~~
- GradientDescent로 cost mizimize 구현

## 2. 코드분석
### 1. Build graph using TF operatios by placeholder
~~~python
#H(x) = Wx 정의
hypothesis = x_train*w

#Cost Function
cost = tf.reduce_sum(tf.square(hypothesis - y_train))

#GradientDescent - cost minimize
learning_rate = 0.1
gradient = tf.reduce_mean((w*x_train-y_train)*x_train)
descent = w - learning_rate*gradient
update = w.assign(descent)
~~~
![CostFunction](https://github.com/Yunseongpyo/yunseongpyo.github.io/blob/master/assets/images/MachineLearning/05CostMinimize/Cost07.jpg?raw=true)
- Gradient Descent에서 평균값을 내기 때문에 CostFundtion은 sum으로 구현


### 2. Run & Udapte graph an get resluts
~~~python
for step in range(21):
    sess.run(update, feed_dict={x_train : x_data, y_train : y_data})
    cost_val = sess.run(cost, feed_dict={x_train:[1,2,3], y_train:[1,2,3]})
    w_val = sess.run(w)
    print(step, cost_val, w_val)
~~~
- 스텝별로 cost값, w값 출력 


## 2. 결과값
~~~bash
0 7.4831824 [0.26889604]
1 2.1285498 [0.61007786]
2 0.6054541 [0.79204154]
3 0.17221813 [0.8890888]
4 0.048986427 [0.9408474]
5 0.013933953 [0.9684519]
6 0.003963413 [0.9831744]
7 0.0011273748 [0.99102634]
8 0.0003206734 [0.99521405]
9 9.121426e-05 [0.9974475]
10 2.5943842e-05 [0.9986387]
11 7.380222e-06 [0.99927396]
12 2.0988446e-06 [0.9996128]
13 5.9723936e-07 [0.99979347]
14 1.6986041e-07 [0.99988985]
15 4.8397126e-08 [0.99994123]
16 1.3738898e-08 [0.99996865]
17 3.933355e-09 [0.99998325]
18 1.1127241e-09 [0.99999106]
19 3.1832315e-10 [0.99999523]
20 9.10525e-11 [0.99999744]
~~~
- cost 값이 최소값이 되기 위해 W는 점점 1에 가까워 지고 있다.

# 3. optimizer를 이용한 Cost최소화


## 1. 전체 코드
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정

x_data = [1,2,3]
y_data = [1,2,3]

w = tf.Variable(5.0)

#H(x) = Wx 정의
hypothesis = x_data*w

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_data))

#cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate= 0.1)
train = optimizer.minimize(cost)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(21):
    print(step, sess.run(w))
    sess.run(train)
~~~
- 텐서플로우 내부의 함수로 cost mizimize 구현

## 2. 코드분석
### 1. Build graph using TF operatios by placeholder
~~~python
w = tf.Variable(5.0)
~~~
- w에 맨 처음 값을 5.0으로 완전히 먼 값을 주고 1를 찾아가는지 확인

~~~python
#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_data))

#cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate= 0.1)
train = optimizer.minimize(cost)
~~~
- CostFunction의 경우 내부함수 이용시 평균값으로 계산
- 텐서플로우의 `GradientDescentOptimizer`로 미적분 계산
- `minimize`를 이용해 optimizer에서 경사도를 계산해 변수에 적용해 최소비용 찾아준다

## 2. 결과값
~~~bash
0 5.0
1 1.2666664
2 1.0177778
3 1.0011852
4 1.000079
5 1.0000052
6 1.0000004
7 1.0
8 1.0
9 1.0
10 1.0
11 1.0
12 1.0
13 1.0
14 1.0
15 1.0
16 1.0
17 1.0
18 1.0
19 1.0
20 1.0
~~~
- W 값이 1.0으로 수렴한다. 즉 `GradientDescentOptimizer`가 잘 적용이 되었다.

마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.