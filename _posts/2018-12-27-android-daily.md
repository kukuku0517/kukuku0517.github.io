---
layout: post
title: 181227 개발일지
sidebar_link: true
categories:
  - Android
  -  Extra
tags:
  - android
  - daily
---

#Intent Scheme

##궁금한점
Q1. Intent Scheme 에 Convention이 정해져있는가?

Q2. 그렇다면 플랫폼(Android ,IOS, Web ... ) 공통인가

Q3. 아니면 제공 플랫폼(Facebook, Kakao ... ) 마다 Extra값이 명시되어 있는가

##Convention

[Intent 공식 문서](https://developer.android.com/reference/android/content/Intent?hl=ko#standard-extra-data)

Standard Extras 가 정해져 있음

![Standard Extras](/assets/181227_standard_intents.JPG)
- EXTRA_TEXT : 텍스트 (URL도 여기 포함)
- EXTRA_STREAM : 이미지, 영상 등

##Client Platform (Android, IOS)

결국 `&{key}={value};` 형태라 공통으로 사용될 수도 있음

안드로이드 특정 키:값을 사용하면 플랫폼 고유의 기능을 사용할 수 있음
`ex)action=android.intent.action.VIEW;`

##Service Platform (Facebook etc.)
플랫폼에서 제공해주는 공유 기능을 사용할 경우엔
플랫폼에서 정해놓은 키:값이 넘어오기 때문에, Receiver가 키값을 다 알고있다.
클라이언트를 거쳐 다시 전달만 될 뿐인 것.

`kakaolink://send?appkey=APIKEY&appver=1.0&apiver=3.0&linkver=3.5&objs=%5B%7B%22objtype%22%3A%22label%22%2C%22text%22%3A%22%EC%8A%A4%ED%83%80%EC%99%80%20%EB%94%94%EC%9E%90%EC%9D%B4%EB%84%88%EC%9D%98%20%EC%8D%B8%26%EC%8C%88%20%EC%BD%9C%EB%9D%BC%EB%B3%B4%EB%A0%88%EC%9D%B4%EC%85%98%20%ED%8C%A8%EC%85%98%EC%99%95%20%EC%BD%94%EB%A6%AC%EC%95%84%EC%97%90%20%EC%B4%88%EB%8C%80%20%ED%95%A9%EB%8B%88%EB%8B%A4.%20%22%7D%2C%7B%22objtype%22%3A%22image%22%2C%22src%22%3A%22http%3A%2F%2Fbnsfolio.com%2Ffk%2Fimages%2Ffk_icon_100.png%22%2C%22width%22%3A100%2C%22height%22%3A100%7D%2C%7B%22objtype%22%3A%22button%22%2C%22text%22%3A%22%ED%8C%A8%EC%85%98%EC%99%95%20%EC%BD%94%EB%A6%AC%EC%95%84%20(SBS%EA%B3%B5%EC%8B%9D%EC%95%B1)%22%2C%22action%22%3A%7B%22type%22%3A%22web%22%2C%22url%22%3A%22https%3A%2F%2Fplay.google.com%2Fstore%2Fapps%2Fdetails%3Fid%3Dcom.xxx.XXXXXX%22%7D%7D%5D`

