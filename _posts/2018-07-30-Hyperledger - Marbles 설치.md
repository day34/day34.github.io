---
classes: wide
title: Hyperledger - Marbles 설치
date: 2018-07-30 00:00:00 +0900
type: post
comments: true
categories: [BlockChain]
tags: [Docker, BlockChain, Hyperledger, Fabric, Marbles]
---

# 참고 사이트
[https://github.com/IBM-Blockchain/marbles](https://github.com/IBM-Blockchain/marbles)

## 설치
### marbles 가져오기
```
$ git clone https://github.com/IBM-Blockchain/marbles.git --depth 1
$ cd marbles
```

## 종속된 프로그램 설치
```
$ npm install
```

## chaincode 설치
```
$ node install_chaincode.js
---------------------------------------
info: Install done. Errors: nope
---------------------------------------

$ node instantiate_chaincode.js.js
---------------------------------------
info: Instantiate done. Errors: nope
---------------------------------------

을 확인
```

## gulp 설치
```
$ npm install gulp -g
$ gulp marbles_local

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
info: Detected that we have NOT launched successfully yet
debug: Open your browser to http://localhost:3001 and login as "admin" to initiate startup
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

## UI 확인
http://localhost:3001
![스크린샷 2018-07-30 오후 4.39.35.png](../../assets/images/968F808D7B9EF395299DD08D22CF0CF3.png)
