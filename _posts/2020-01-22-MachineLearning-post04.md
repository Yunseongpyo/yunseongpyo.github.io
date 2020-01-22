---
title:  "MachineLearning - 04. Linear Regression 구현(Python)"
excerpt: "MachineLearning 정리."

toc: true
toc_sticky: true

categories:
  - MachineLearning
tags:
  - MachineLearning
  - Python
last_modified_at: 2020-01-22
---
모두를 위한 머신러닝/딥러닝 강의 기반으로 공부한 내용입니다.

---
# Linerar Regression을 파이썬으로 구현
  1. 그래프 빌드 : H(x) = Wx+b (Hypothesis), CostFunction, Cost Minimize 구현
  1. 학습 실행 및 학습 결과 프린트
- 첫번째는 `placeholder` 없이 직접 데이터를 입력해 구현
- 두번째는 `placeholder` 로 이용한 구현



# 1. PlaceHolder없이 구현


## 1. 전체 코드
~~~python
import tensorflow as tf

# 1.그래프 빌드 과정
# X(입력)에 관한 Y(출력)의 데이터(정답) 값 
x_train = [1,2,3]
y_train = [1,2,3]

#텐서플로우가 자체적으로 변경할 변수들
w = tf.Variable(tf.random_normal([1], name='weight'))
b = tf.Variable(tf.random_normal([1], name='bias'))

#H(x) = Wx+b 정의
hypothesis = x_train*w +b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(2001):
    sess.run(train)
    if step % 20 == 0:
        print(step, sess.run(cost), sess.run(w), sess.run(b))
        
~~~
## 2. 코드분석
### 1. Build graph using TF operatios
~~~python
import tensorflow as tf

# 1.그래프 빌드 과정
# X(입력)에 관한 Y(출력)의 데이터(정답) 값 
x_train = [1,2,3]
y_train = [1,2,3]
~~~
- `placeholder`없이 직접 정답인 데이터 입력

~~~python
#텐서플로우가 자체적으로 변경할 변수들
w = tf.Variable(tf.random_normal([1], name='weight'))
b = tf.Variable(tf.random_normal([1], name='bias'))
~~~
- `Variable`
  - 텐서플로우가 사용하는 변수(텐서플로우가 자체적으로 변경함)
  - 즉 학습 시키는데 사용될 변수를 텐스플로우가 변경
- `random_normal(shpae[], name = '')`
  - 랜덤값을 발생 시킴
  - shpaer값이 [1]이기 때문에 1개의 랜덤값 발생

~~~python
#H(x) = Wx+b 정의
hypothesis = x_train*w +b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)
~~~
- `reduece_mean` : 평균값 구하기
- `square` : 제곱
- GradientDescent는 다음번 포스트에서 설명, 우선은 cost의 최소값을 찾아내 준다고 생각

### 2. Run & Udapte graph an get resluts
~~~python
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())
~~~
- `global_variables_initalizer()` : `Variable`를 실행시키기 위한 초기화(꼭 해주어야 함)

~~~python
for step in range(2001):
    sess.run(train)
    if step % 20 == 0:
        print(step, sess.run(cost), sess.run(w), sess.run(b))
~~~
- 2000번을 학습 시키고 20번 마다 학습 결과를 보여줌
- train노드 밑의 cost, hypothesis, w, b 값 등이 있기 때문에 train만 실행

## 2. 결과값
~~~bash
0 12.358636 [-0.9513019] [0.7688759]
20 0.35182998 [0.2997254] [1.2427298]
40 0.2209599 [0.4430257] [1.2328869]
60 0.19978558 [0.47971272] [1.1795696]
80 0.18144037 [0.5051651] [1.1245747]
100 0.16478693 [0.5285159] [1.0717657]
...............
1900 2.8441185e-05 [0.993806] [0.01408037]
1920 2.5831172e-05 [0.9940971] [0.01341865]
1940 2.3460028e-05 [0.9943745] [0.01278804]
1960 2.1307174e-05 [0.99463886] [0.01218706]
1980 1.9350911e-05 [0.99489087] [0.01161431]
2000 1.7575063e-05 [0.995131] [0.01106845]
~~~
- cost 값은 계속 줄어들고, W는 1에 가까워 지고 b는 0에 가까워 진다.(h(x) = 1*x+0)
- 즉 우리가 구하고자 한 y = x 에 가까워 진다.


---
# 2. PlaceHolder로 구현


## 1. 전체 코드
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정
# placeholder로 정의 
x_train = tf.placeholder(tf.float32)
y_train = tf.placeholder(tf.float32)

#텐서플로우가 자체적으로 변경할 변수들
w = tf.Variable(tf.random_normal([1], name='weight'))
b = tf.Variable(tf.random_normal([1], name='bias'))

#H(x) = Wx+b 정의
hypothesis = x_train*w +b

#Cost Function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

#GradientDescent - cost minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

#2. Run & Udapte graph an get resluts
sess = tf.Session()
#variables에 관한 초기화
sess.run(tf.global_variables_initializer())

for step in range(2001):
    cost_val, W_val, b_val, _ = sess.run([cost,w,b,train],
                                feed_dict={x_train:[1,2,3], y_train:[1,2,3]})
    if step % 20 == 0:
        print(step, cost_val, W_val, b_val)
~~~
- 기본 코드에서 데이터 입력 부분과 출력 부분만 변경

## 2. 코드분석
### 1. Build graph using TF operatios by placeholder
~~~python
import tensorflow as tf
# 1.그래프 빌드 과정
# placeholder로 정의 
x_train = tf.placeholder(tf.float32)
y_train = tf.placeholder(tf.float32)
~~~
- `placeholder`로 입력값 형식은 float32비트로 선언

### 2. Run & Udapte graph an get resluts
~~~python
for step in range(2001):
    cost_val, W_val, b_val, _ = sess.run([cost,w,b,train],
                                feed_dict={x_train:[1,2,3], y_train:[1,2,3]})
    if step % 20 == 0:
        print(step, cost_val, W_val, b_val)
~~~
- `feed_dict=`를 통해 유연하게 데이터값 입력

## 2. 결과값
- 학습 결과는 기존과 비슷하기 때문에 생략
- 학습 후 다른 사람이 직접 입력 값을 주었을 경우
~~~python
print(sess.run(hypothesis, feed_dict= {x_train:[363.6]}))
print(sess.run(hypothesis, feed_dict= {x_train:[1,2,3,4,5]}))
~~~
~~~bash
[362.63654]
[1.0033951 2.0007286 2.9980621 3.9953957 4.992729 ]
~~~
- y=x에 매우 가까운 결과값을 보여준다



마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.