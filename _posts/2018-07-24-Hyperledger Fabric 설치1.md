---
classes: wide
title: Hyperledger Fabric 설치1
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

### Homebrew Uninstall
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

### Homebrew Update
```
$ brew update
```

### Go 설치 (1.10.x)
```
$ brew install go
$ go version
$ echo 'export PATH=$PATH:/usr/local/opt/go/libexec/bin' >> ~/.bash_profile
```

### Node.js 설치 (8.9.x)
[https://hyperledger-fabric.readthedocs.io/en/release-1.2/prereqs.html](https://hyperledger-fabric.readthedocs.io/en/release-1.2/prereqs.html)

`Node.js version 9.x is not supported at this time. Node.js - version 8.9.x or greater`

```
$ brew install node@8
$ echo 'export PATH="/usr/local/opt/node@8/bin:$PATH"' >> ~/.bash_profile
$ brew unlink node
$ brew link node@8
$ node -v
$ npm -v
```

### Python (우분투 사용자만)
```
$ sudo apt-get install python
$ python --version
```

## 설치
### 샘플 다운로드
```
$ git clone https://github.com/hyperledger/fabric-samples.git fabric
$ cd fabric
```

### 바이너리 파일 설치
`could not read CA certificate` [에러 대응](#에러1)

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

## First Network 샘플
### 폴더 이동
```
$ cd first-network
```

### 키 생성
`cryptogen tool not found. exiting` [에러 대응](#에러2)

```
$ ./byfn.sh -m generate
```

### 네트워크 시작
`ERROR !!! FAILED to execute End-2-End Scenario` [에러 대응](#에러3)
```
$ ./byfn.sh -m up
start, end가 나와야함
```

### 네트워크 중지
```
$ ./byfn.sh -m down
```

## 명령어
### 명령어 모음 폴더
```
$ cd fabcar
```

### fabric 시작
[https://hyperledger-fabric.readthedocs.io/en/release-1.2/write_first_app.html](https://hyperledger-fabric.readthedocs.io/en/release-1.2/write_first_app.html)

node-pre-gyp ERR! [에러 대응](#에러4)

```
실행된 도커 프로세스 중지
$ docker rm -f $(docker ps -aq)

캐시된 네트워크 삭제
$ docker network prune

fabric에 종속된 프로그램 설치
$ npm install

$ ./startFabric.sh
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


---
## Marbles 설치
[Hyperledger-Marbles-설치로 이동하기](http://127.0.0.1:4000/blockchain/Hyperledger-Marbles-설치/)

---


## 에러 대응
### 에러1

could not read CA certificate "/Users/kmi/.docker/machine/machines/blockchain/ca.pem": open /Users/kmi/.docker/machine/machines/blockchain/ca.pem: no such file or directory
could not read CA certificate "/Users/kmi/.docker/machine/machines/blockchain/ca.pem": open /Users/kmi/.docker/machine/machines/blockchain/ca.pem: no such file or directory

```
$ docker-machine create --driver virtualbox blockchain
$ docker-machine env blockchain
$ eval $(docker-machine env blockchain)
```

### 에러2
cryptogen tool not found. exiting

`바이너리 파일 설치`를 먼저 진행하고, `샘플 다운로드`을 받는 순서대로 진행하면 `cryptogen tool not found. exiting`에러가 나온다.

`샘플 다운로드`하고 샘플 폴더에서 `바이너리 파일 설치`를 실행하고, `키 생성`을 하면 에러가 없어진다.

혹, 순서를 바꿔도, 해당 샘플 폴더에서 `바이너리 파일 설치`를 다시 설치하면 된다.


### 에러3
ERROR !!! FAILED to execute End-2-End Scenario

`바이너리 파일 설치`후 `네트워크 시작`를 바로 실행하면 에러가 발생. `docker`를 완전히 종료하고 `네트워크 시작`를 다시 시작하면 문제가 해결됨


### 에러4
```
node-pre-gyp ERR! Tried to download(403): https://storage.googleapis.com/grpc-precompiled-binaries/node/grpc/v1.10.1/node-v64-darwin-x64-unknown.tar.gz 
node-pre-gyp ERR! Pre-built binaries not found for grpc@1.10.1 and node@10.7.0 (node-v64 ABI, unknown) (falling back to source compile with node-gyp) 
node-pre-gyp ERR! Pre-built binaries not installable for grpc@1.10.1 and node@10.7.0 (node-v64 ABI, unknown) (falling back to source compile with node-gyp) 
node-pre-gyp ERR! Hit error Connection closed while downloading tarball file 
```

와 같은 에러 발생시 `node -v`를 확인한다. `현재 Hyperledger Fabric 1.2버전은 Node.js 8.9.x이상만 지원한다.` `Node.js 9.x 이상은 지원 안한다` 꼭 확인한다.