---
layout: post
title: Expansion Files
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
Google Play Store에 Apk크기가 100MB가 넘을수 없음.

App Bundle도 동일하게, App Bundle로 생성된 Apk의 크기가 100MB를 넘을수 없음.

*그러고 보니 App Bundle은 API level 20이상부터 지원하는데, 해상도 ldpi에서 오류가 나는것을 통해 알수 있다.*

## Expansion Files
___


Google Play Store에 APK를 올릴때, ![추가버튼](https://lh3.googleusercontent.com/4lFbzJhIhmrWnT-ZLNLx07iSFCVL4Qhyp9tRZ4jgjV1dV6ahIuUhNNTUyPQUGNedHd4=w18)를 눌러서 추가 리소스를 추가할수있다.

그러면 게임에서 추가다운로드 받는것 처럼 최초 다운로드시 추가리소스를 다운로드 할수 있다.

[Expansion Files 설정](https://developer.android.com/google/play/expansion-files)

이전에는 라이브러리 모듈 추가와 압축 해제등을 직접 구현해야 했지만, (downloader_library, zip_file 등)

이제는 SDK package를 다운로드하고 Manifest에 권한 설정만 하면 자동으로 .obb파일을 생성해주는 듯 하다.

### .OBB 파일
---

Android 추가파일에 사용되는 확장자. ZIP, MP4, PDF등 일반적으로 사용하는 파일형식도 물론 추가파일로 업로드 가능하다.
OBB파일은 Jobb툴을 이용해 파일들을 압축 및 암호화를 한다.

`[main|patch].<expansion-version>.<package-name>.obb`


[Expansion Files 추가](https://support.google.com/googleplay/android-developer/answer/2481797?hl=en)

 ![추가버튼](https://lh3.googleusercontent.com/4lFbzJhIhmrWnT-ZLNLx07iSFCVL4Qhyp9tRZ4jgjV1dV6ahIuUhNNTUyPQUGNedHd4=w18)버튼을 찾으려고 5조5억년 걸렸다.
 알고보니 아직 App Bundle방식에는 지원하지 않고 있고 known issue에 게재되어있다.

[StackOverflow 질문](https://stackoverflow.com/questions/54001690/how-can-i-use-android-expansion-files-using-app-bundle/54002159?noredirect=1#comment94853518_54002159)

[App Bundle Know Issue](https://developer.android.com/guide/app-bundle/#known_issues)

> Android App Bundles do not support APK expansion files. However, Google Play still requires that app downloads be 100MB or less. So, for example, when first installing your app, the total size of your base APK and its configuration APKs must equal 100 MB or less. Similarly, the total size of any dynamic feature APK and its configuration APKs must be 100 MB or less. After uploading your app bundle, the Play Console warns you if your app bundle results in APKs that violate this restriction.


## Expansion Files 직접구현 (진행중)
---
사실 Apk가 100MB가 넘은 이유는 이번 릴리즈에 추가한 영상이 무지막지하게(90MB...) 컸기때문이라,
그냥 영상 축소를 하여 해결하였다...

다만 이후에 리소스 추가할 일이 필요하고 그때도 Expansion Files 지원이 되지 않으면
직접 구현해야하기때문에 간단히 조사/작업 해보았다

#### Retrofit의 @Streaming 어노테이션

```
    @GET
    @Streaming
    Observable<ResponseBody> getVideoFromUrl(@Url String fileUrl);
```


>The @Streaming declaration doesn't mean you're watching a Netflix file. It means that instead of moving the entire file into memory, it'll pass along the bytes right away. But be careful, if you're adding the @Streaming declaration and continue to use the code above, Android will trigger a android.os.NetworkOnMainThreadException.

대용량 파일을 다운로드하면서 과한 메모리의 사용을 방지하기위해 사용

#### getExternalFilesDir() / getFilesDir

> - getFilesDir() - creates an app-specific directory; hidden from users; deleted with the app
> - getExternalFilesDir() - creates an app-specific directory; accessible to users; deleted with the app
> - getExternalStoragePublicDirectory() - uses a shared directory (e.g., Music); accessible to users; remains when the app is delete

추가리소스의 경우, 유저가 접근할수없는 getFilesDir()에 저장하면 된다.