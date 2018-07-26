---
classes: wide
title: Hyperledger Fabric 설치2
date: 2018-07-25 00:00:00 +0900
type: post
comments: true
categories: [BlockChain]
tags: [Docker, BlockChain, Hyperledger, Fabric]
---

# 참고 사이트
[http://blog.idcf.jp/entry/hyperledger-fabric](http://blog.idcf.jp/entry/hyperledger-fabric)

## 설치
### 소스 가져오기
```
$ mkdir fabric
$ cd fabric
$ curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip
$ unzip fabric-dev-servers.zip
```

### fabric 설치
```
$ ./downloadFabric.sh
```

## 명령어
### fabric 시작
```
$ ./startFabric.sh
$ docker ps (확인)
hyperledger/fabric-peer
hyperledger/fabric-ca
hyperledger/fabric-orderer
hyperledger/fabric-couchdb:
```

### PeerAdminCard 생성
```
$ ./createPeerAdminCard.sh
```

### 프로필 작성
```
$ ./createComposerProfile.sh (deprecated from v0.15.0)
$ ./createPeerAdminCard.sh
```

### fabric 중지
```
$ ./stopFabric.sh
$ docker ps
```

### fabric  해체
```
$ ./teardownFabric.sh
```