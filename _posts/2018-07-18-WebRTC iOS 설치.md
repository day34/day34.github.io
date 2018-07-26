---
classes: wide
title: WebRTC - iOS 설치
date: 2018-07-18 00:00:00 +0900
type: post
comments: true
categories: []
tags: [WebRTC, iOS]
---

# 참고 사이트
[http://clover.chat/dev/webrtc-on-android-and-ios](http://clover.chat/dev/webrtc-on-android-and-ios)

[http://qiita.com/nakadoribooks](http://qiita.com/nakadoribooks)

[https://github.com/nakadoribooks?tab=repositories](https://github.com/nakadoribooks?tab=repositories)

[https://gist.github.com/noiges/7781364](https://gist.github.com/noiges/7781364)

[https://html5experts.jp/mganeko/5181/](https://html5experts.jp/mganeko/5181/)


## 설치준비
약 1시간 정도의 시간이 걸린다.

실수하면, 무한 재반복!!!

맥용 기준으로 정리했음.

### WebRTC 작업 폴더 생성
```
$ mkdir webrtc-ios
$ cd webrtc-ios
```

### depot_tools 설치
```
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ export PATH=`pwd`/depot_tools:"$PATH"
```
## 설치
### WebRTC ios 소스 다운로드
```
$ fetch --nohooks webrtc_ios
$ gclient sync
```

### gn gen 빌드 1
```
$ cd src
$ gn gen out/ios_arm --args='target_os="ios" target_cpu="arm" is_debug=false ios_enable_code_signing=false'
$ gn gen out/ios_arm_64 --args='target_os="ios" target_cpu="arm64" is_debug=false ios_enable_code_signing=false'
$ gn gen out/ios_sim --args='target_os="ios" target_cpu="x64" is_debug=false ios_enable_code_signing=false'
```

### ninja 빌드 2
```
$ ninja -C out/ios_arm/
$ ninja -C out/ios_arm_64/
$ ninja -C out/ios_sim/
```

### WebRTC 복사
```
$ cp -R out/ios_arm_64/WebRTC.framework/ out/WebRTC.framework
```

### WebRTC 통합 후 복사
```
$ lipo -create -output out/WebRTC.framework/WebRTC out/ios_arm/WebRTC.framework/WebRTC out/ios_arm_64/WebRTC.framework/WebRTC out/ios_sim/WebRTC.framework/WebRTC
```

## XCode 프로젝트에 적용
### WebRTC 교체
```
WebRTC.framework 안에 WebRTC 교체
```

### xCode에 추가 방법
```
TARGETS -> General -> Embedded Binaries -> WebRTC.framework 추가
TARGETS -> Build Settings -> Enable Bitcode -> No 수정
TARGETS -> Build Settings -> Valid Architectures -> Debug -> Any SDK -> arm64 armv7 armv7s
TARGETS -> Build Settings -> Valid Architectures -> Debug -> Any iOS Simulator SDK -> x64 (시뮬레이션 빌드가 필요함)
info.plist -> Privacy - Camera Usage Description -> 설명
```

## 에러 대응
### Xcode 에러 1 (Undefined symbols for architecture armv7)
```
TARGETS -> General -> Linked Frameworks and Libraries -> libstdc++.6.dylib 추가
```