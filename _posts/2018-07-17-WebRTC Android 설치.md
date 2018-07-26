---
classes: wide
title: WebRTC - Android 설치
date: 2018-07-17 00:00:00 +0900
type: post
comments: true
categories: []
tags: [WebRTC, Android]
---

# 참고 사이트
[http://qiita.com/nakadoribooks/items/7950e29ad3b751ddab12](http://qiita.com/nakadoribooks/items/7950e29ad3b751ddab12)

[http://qiita.com/nakadoribooks](http://qiita.com/nakadoribooks)

[https://github.com/nakadoribooks?tab=repositories](https://github.com/nakadoribooks?tab=repositories)

## 설치 준비
약 1~2시간 정도의 시간이 걸린다.

실수하면, 무한 재반복!!!

맥용 기준으로 정리했음.

### WebRTC 작업 폴더 생성
```
$ mkdir webrtc-build
$ cd webrtc-build
```

### vagrant 설치
```
$ vagrant init hashicorp/precise64
```

### 내용 교체
```
$ vi Vagrantfile


# 이 내용을 입력한다.
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "hashicorp/precise64"

  config.ssh.forward_agent = TRUE

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end

  config.vm.network "private_network", ip: "192.168.50.5"
  config.vm.synced_folder ".", "/vagrant", type: :nfs

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update
     sudo apt-get -y install wget git gnupg flex bison gperf build-essential zip curl subversion pkg-config libglib2.0-dev libgtk2.0-dev libxtst-dev libxss-dev libpci-dev libdbus-1-dev libgconf2-dev libgnome-keyring-dev libnss3-dev
     # Download the latest script to install the android dependencies for ubuntu
     curl -o install-build-deps-android.sh https://src.chromium.org/svn/trunk/src/build/install-build-deps-android.sh
     # Use bash (not dash which is default) to run the script
     sudo /bin/bash ./install-build-deps-android.sh
     # Delete the file we just downloaded... not needed anymore
     rm install-build-deps-android.sh
   SHELL
end
```

### VM 설치 
```
$ vagrant up
$ 비밀번호를 물어보면 pc의 비밀번호를 입력
```

### VM 접속
```
$ vagrant ssh
```

## 설치
### git 설치
```
$ sudo apt-get install git -y
```

### depot_tools 설치
```
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ export PATH=`pwd`/depot_tools:"$PATH"
```

### WebRTC 작업 폴더 생성
```
$ mkdir webrtc-checkout
$ cd webrtc-checkout
```

### WebRTC android 소스 다운로드
```
$ fetch --nohooks webrtc_android
$ gclient sync
Do you accept the license for version 10.2.0 of the Google Play services client library? -> y
```


### git 확인 (업데이트에서 에러나면 한번 해보기)
```
$ cd src
$ git log
$ git status
```

### 업데이트
```
$ src/build/android/play_services/update.py download
Do you accept the license for version 10.2.0 of the Google Play services client library? -> y
$ gclient sync
```

### libstdc++6 설치
```
$ cd src
$ sudo apt-get install libstdc++6 -y
```

### GLIBCXX 설치 확인
```
$ sudo strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

### 패키지 인덱스 인덱스 정보를 업데이트 (GLIBCXX_3.4.18 에러가 나서 설치를 해야함)
```
$ sudo apt-get install python-software-properties -y
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
$ sudo apt-get update
```

### 의존성 검사하며 설치하기
```
$ sudo apt-get dist-upgrade
```

### GLIBCXX_3.4.18 설치 확인
```
$ sudo strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

### 자바7 설치
```
$ sudo apt-get install openjdk-7-jdk -y
$ sudo update-alternatives --config java
$ java -version
```

### aar 빌드
```
$ tools_webrtc/android/build_aar.py
```

### 파일 복사
```
$ ls -l
$ cp libwebrtc.aar /vagrant/
$ exit
$ ls -hl
```

## Android Studio 프로젝트에 적용
### Android Studio 라이브러리 추가
```
#build.gradle (Module:app)

dependencies {
    compile(name:'libwebrtc', ext:'aar')
}

repositories{
    flatDir{
        dirs 'libs'
    }
}
```