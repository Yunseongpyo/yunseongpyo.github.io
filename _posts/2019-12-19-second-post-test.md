---
title:  "Unity를 이용한 OpenCV"
excerpt: "그날그날 공부한거 정리-01."

toc: true

toc_sticky: true

categories:
  - Unity
tags:
  - Unity
  - OpenCV
last_modified_at: 2019-12-19
---

## 1. opencv for unity 사용시

```c#
using OpenCVForUnity.CoreModule;
using OpenCVForUnity.UtilsModule;
using OpenCVForUnity.UnityUtils;
using OpenCVForUnity.ImgprocModule;
```

위에 꼭 선언을 해줘야지 됨. 

```c#
Mat mainMat = new Mat(baseTexture.height, baseTexture.width, CvType.CV_8UC3);
```
-> 기본적으로 매트릭스를 만들어주는 클래스, opencv같은 경우 행렬 이나 영상을 생성할때는 mat으로 사용해서 생성한다.  
-> 사용방법은 기존 opencv하고 같다.  
-> CvTpye같은 경우 행렬이 어떤 자료형을 사용하는지 대한 정보를 담고 있다.  
   - 기본 구조 = ```CV_<bit-depth>{U|S|F}C(<number_of_channels>)```
   - bit-depth : 원소값 하나의 비트수(8, 16, 32, 64)
   - ```{U|S|F}``` : U:정수형(Unsigned), S:부호있는 정수형(Signed), F:부동소수형(float)
   - ```C(<number_of_channels>)``` : C1, C3 같이 쓰면서 뒤의 숫자는 채널의 갯수 이다. 채널이 1가인 경우 C1은 생략 가능, C3의 같은 경우는 B,G,R 채널을 가진다.

```c#
Utils.texture2DToMat(baseTexture, mainMat);
Utils.matToTexture2D(grayMat, finaltexture);
```
->unity 같은 경우 기본 영상(이미지+동영상)은 texture로 받기 때문에 openCv를 하기 위해서는 Mat 클래스 형식으로 바꿔줘야한다.   
->Utils 의 'texture2DToMat' 와 'matToTexture2D'으로 사용해야 한다.  
->using OpenCVForUnity.UnityUtils; 를 위에 선언해줘야 한다.  

마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.