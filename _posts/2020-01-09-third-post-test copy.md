---
title:  "AR Foundation no.1"
excerpt: "AR Foundation 정리."

toc: true
toc_sticky: true


categories:
  - Unity
tags:
  - Unity
  - AR
last_modified_at: 2019-12-19
---
우선 수정 할꺼지만... 간단히 내용 정리

## 1. 기본 오브젝트
- AR Session 오브젝트 : 현재 ar의 모든 상태를 확인하는 부분, 하나만 존재(정적 필드)
- AR Session Origin 오브젝트 : ar 중심축을 잡아 준다. 하위 오브젝트로 ar 카메라를 가지고 있다. 
- AR Raycast Manger는 터치를 위해 추가하는데 기본적으로 ARseesion origin 오븍젝트에 추
  (ar origin 없는 곳에 추가시 자동으로 origin 컴퍼너트가 추가됨)

## 2. 타입 정리

- Touch 타입 : 다양한 터치 부가정보가 들어감
               getmousebuttuondown 같은 경우는 bool값 같은 단편적인 정보만 들어가므로 터치가 좀더 편함(마우스 터치 다운도 가능은 함)
- Pose 타입 : 터치한 포지션의 다양한 정보가 있음

- arSessionOrigin.MakeContentAppearAt : 유니티 상의 모든 중심점을 이동 시킬 때 사용(터치로 이동 가능), 즉 기준점을 이동 시킨다. 단 유니티 씬 상의 개별 오븍제트의 위치는 그대로

- TrackableType의 종류   
TrackableType.PlaneWithinPolygon(지정된 평면 모양대로) < TrackableType.PlaneWithinBounds(지정된 평면모양기준으로 직사각형) 
< TrackableType.PlaneEstimated(불분명한 경계) < TrackableType.PlaneWithinInfinity(지정된 면으로부터 끝이없는 경계선, 안드로이드에서는작동이 안됨)

- ARplane 타입 종류  
ARPlane a;  
a.alignment a= PlaneAlignment.HorizontalUp; // 수평 수직 확인  
a.center -> 유니티 월드 스페이스 상에서 중심  
a.boundary[0] - > 경계선 위치(배열로 저장)  
a.centerInPlaneSpace - > 평면 공간내에서 좌표   
a.extents -> 감지한 평면을 감쌀수 있는 직사각형을 저장(가로 및 세로 값의 반값 입력)  
a.infinitePlane -> 무한 평면  
a.normal -> 평면이 바로보는 방향  
a.size -> 현실세상 사이즈 (ARSession 오브젝트하고 1:1 대응 시켜야 함)  
a.boundaryChanged -> 감지한 평면이 확장될때 자동으로 실행되는 이벤트  




마지막으로 수정된 시간은 {{ page.last_modified_at }}이다.