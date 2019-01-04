---
layout: post
title: Handle Input Method Visibility
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
EditText가 있는 View에서 StatusBar영역 이상으로 크기가 조정되는 현상

![오류화면](/assets/181231_edittext_error.jpeg){:width="250px"}

## android:windowSoftInputMode
___

보통 `state mode | adjust mode` 의 조합으로 설정됨

`state mode` 는 키보드를 보여줄지 안보여줄지,
`adjust mode` 는 UI 사이즈가 어떻게 조정될지 결정한다.

```
// 진입시 키보드를 보여주면서, 키보드의 영역만큼 UI의 크기가 조정된다.
<activity android:windowSoftInputMode="stateVisible|adjustResize">
```


| Value |  Description |
|---|---|
|stateUnspecified |설정값 없음 ( undefined ). System에서 알아서 적절히 설정하거나 Theme에 영향을 받는다.  |
|stateUnchanged |지난 setting 값을 유지한다. |
|stateHidden |activity "진입" 시에 keyboard 를 숨긴다. ( resume으로 돌아오는 경우는 적용되지 않는다. )|
|stateVisible| activity "진입" 시에 특이 사항이 없다면 keyboard 를 보여준다. |
|stateAlwaysVisible| activity "진입" 시에 항상 keyboard 를 보여준다.( resume으로 돌아오는 경우는 적용되지 않는다. )|
|adjustUnspecified |설정값 없음( undefined ). System에서 알아서 mode 설정.여기서 System 설정은 Scroll 가능한 View 를 가지고 있다면 Resize 함.
|adjustResize|  Soft keyboard 공간을 위해 activity 를 resize 한다. |
|adjustPan| Window의 focus를 input focus에 위치하도록 이동하여 보여준다.Typing 하는 동안에는 해당 view 를 볼 수 있지만, 다른 UI 의 시야를 방해할 수 있기 때문에 추천되지는 않는다.|



## Resize를 하려면 Resize할수있는 View가 있어야 한다 (당연하지만...)
---

기존엔 NestedScrollView없이 , `adjustPan` 옵션만 설정되어 있었음.

그럼 전체 View는 조정되지 않고, EditText에 focus되게끔만 view가 이동이 되어서 statusbar를 가렸던것.

또한 그냥 `adjustResize` 만 설정 했을경우, match_parent로 설정되어있는 EditText에  크기가 줄어든다.

EditText에  NestedScrollView안에 배치하여, view가 조정될때 NestedScrollView의 위치가 조정되도록 변경 하였다.

![오류화면](/assets/181231_edittext_error_fixed.jpg){:width="250px"}



