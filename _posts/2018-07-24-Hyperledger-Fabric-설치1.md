---
classes: wide
title: Hyperledger-Fabric 설치1
date: 2018-07-24 00:00:00 +0900
type: post
comments: true
categories: [BlockChain]
tags: [Docker, BlockChain, Hyperledger, Fabric]
---

# 참고 사이트
[https://hyperledger-fabric.readthedocs.io/en/release-1.2/](https://hyperledger-fabric.readthedocs.io/en/release-1.2/)

[http://tmtm-yagi-007.hatenablog.com/entry/2018/01/29/111404](http://tmtm-yagi-007.hatenablog.com/entry/2018/01/29/111404)


## 사전 설치
### Homebrew Install
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew —version
```

### Homebrew Update
```
$ brew update
```

### Go 설치
```
$ brew install go
$ go version
$ vi ~/.bash_profile
export PATH=$PATH:/usr/local/go/bin
```

### Node.js 설치
```
$ brew install node
$ node -v
$ npm -v
https://gist.github.com/rcugut/c7abd2a425bb65da3c61d8341cd4b02d
$ brew install node --without-npm (왜 npm을 빼고 설치하는지 이유를 모르겠음)
```

### Python (우분투 사용자만)
```
$ sudo apt-get install python
$ python --version
```

## 설치
### git 가져오기
```
$ git clone https://github.com/hyperledger/fabric-samples.git fabric
$ cd fabric
```

### 1.2.0 배치 실행
```
$ curl -sSL http://bit.ly/2ysbOFE | bash -s 1.2.0
Installing Hyperledger Fabric binaries
fabric binaries
fabric-ca-client binary

Installing Hyperledger Fabric docker images
hyperledger/fabric-peer
hyperledger/fabric-orderer
hyperledger/fabric-ccenv
hyperledger/fabric-tools
hyperledger/fabric-ca
hyperledger/fabric-couchdb
hyperledger/fabric-kafka
hyperledger/fabric-zookeeper
```

### 폴더 이동
```
$ cd first-network
```

### 키 생성
```
$ ./byfn.sh -m generate
```

### fabric 실행
```
$ ./byfn.sh -m up
start, end가 나와야함
```

### fabric 중지
```
$ ./byfn.sh -m down
```

##명령어
### 명령어 모음 폴더
```
$ cd fabcar
```

### 관리자 등록
```
$ node enrollAdmin.js
```

### 사용자 등록
```
$ node registerUser.js 
```

### ledger에 접속&데이터 가져오기
```
$ node query.js
```

### ledger 새로운 데이터 등록
```
$ vi invoke.js (편집)
$ node invoke.js (등록)
$ node query.js (확인)
```