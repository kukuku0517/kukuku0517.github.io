---
layout: post
title: Dialog isWind
sidebar_link: true
categories:
  - Android
  - Extra
tags:
  - android
  - daily
---

## ISSUES
- [x] Dialog에서 width가 예상한대로 설정되지 않는 이슈가 있었다.
- [x] MATCH_PARENT vs WRAP_CONTENT
- [x] View Hierarchy

---
## DP vs PX
## Dialog View Properties
## WRAP_CONTENT vs MATCH_PARENT
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
