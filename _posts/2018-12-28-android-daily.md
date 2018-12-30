---
layout: post
title: (작성중) Dialog windowIsFloating
sidebar_link: true
categories:
  - Android
  - Extra
tags:
  - android
  - daily
---

## ISSUES
---
Dialog에서 width가 예상한대로 설정되지 않는 이슈가 있었다.

![오류화면](/assets/181229_error_screenshot.png){:width="250px"}



## DP vs PX
---
이전부터 디자인팀에서도 제기되었던 노트8의 해상도 문제가 아닐까 싶어서,
아예 Pixel단위로 디버깅을 해보았다.

- px : 화면을 구성하는 최소 단위. 정사각형 점 하나.
- dp : 픽셀과 무관하게, 모든 화면에서 동일한 크기로 보이게하는 영역의 최소 단위.
    - dp와 px의 관계는, 160dpi인 기기를 기준 1dp = 1px이다.
- 해상도 : 단위 px. 몇개의 픽셀로 가로, 세로가 이루어졌는지 나타내는 수치
- ppi : pixel per inch. 1인치에 몇개의 픽셀이 포함되어있는지. 높을수록 선명.
- dpi : dots per inch. 1인치에 몇개의 dot이 포함되어있는지. 높을수록 선명.

- [ ] Note8은 ppi는 521, dpi는 560. 아니 dpi랑 ppi차이가 뭐임????

- 560dpi를 기준으로 정상적으로 계산되고 있음.

`1050px = 300dp * (560dpi / 160dpi)`

![dp게산](/assets/181229_pixel_screenshot.png)

## WRAP_CONTENT vs MATCH_PARENT
---

[Child never asks back for parent size](https://stackoverflow.com/questions/4606613/combining-wrap-content-on-parent-and-fill-parent-on-child_)

Child View에서 MATCH_PARENT가 먼저 전달되면
Parent에선 아마 길이 조정에 고려대상이 되지 않을것
Parent가 Child를 Wrap하는 최소한의 크기로 결정될듯함.

- [ ] 테스트해보기

## Theme

## Activity/Window/DecorView
---

[Window란?](https://stackoverflow.com/questions/9451755/what-is-an-android-window)

[DecorView란?](https://stackoverflow.com/questions/23276847/what-is-an-android-decorview)

Activity는 Window를 생성한다

Window는 말 그대로 데스크탑의 윈도우를 생각하면된다. Single Hierarchy View를 지니고 있다

DecorView는 Window의 Single Hierarchy의 RootView라고 볼 수 있다.
DecorView의 배경이 Window의 전체 배경이다.

## LayoutInspector
---

연결된 ADB의 View를 실시간으로 수치를 관찰할수 있다.

![LayoutInspector](/assets/181229_layout_inspector.png){:width="250px"}

LayoutInspector를 통해서 살펴보았을때, WRAP_CONTENT로 했을땐 DecorView수치가 더 커져있었고
300dp고정값을 했을땐 DecorView는 정상 수치였지만 더 작게 표현되고 있었음.
--> Dialog CustomView의 영역 밖의 이슈인가?


## LayoutParams
---

View의 property를 결정하는 클래스
XML상에서 추가할 수 있는 property들이라고 생각하면 될것같다.
`ViewGroup.getLayoutParams()`로 불러오거나
`new LayoutParams()`를 통해서 생성한 후
`requestLayout(), setContentView()`등 뷰를 업데이트 해줘야 한다.

- [ ] 정확히 불려야하는 함수가 어떤것?

```
<TextView
           android:layout_width="wrap_content"
           android:layout_height="wrap_content" />
```

이는 아래와 동일하다.

```
TextView tv = new TextView();
tv.setLayoutParams(new ViewGroup.LayoutParams(
    ViewGroup.LayoutParams.WRAP_CONTENT,
    ViewGroup.LayoutParams.WRAP_CONTENT));
```
