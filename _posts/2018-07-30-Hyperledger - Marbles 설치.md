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

### 종속된 프로그램 설치
```
$ npm install
```

### chaincode 설치
throw new Error('Cannot find org.', orgName); [에러 대응](#에러1)
```
$ cd secipts
$ node install_chaincode.js
---------------------------------------
info: Install done. Errors: nope
---------------------------------------

$ node instantiate_chaincode.js
---------------------------------------
info: Instantiate done. Errors: nope
---------------------------------------
```

### gulp 설치
```
$ npm install gulp -g
```

### marbles 실행
error: Looks like you are using an old version of marbles chaincode...[에러 대응](#에러2)

```
$ gulp marbles_local
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
info: Detected that we have NOT launched successfully yet
debug: Open your browser to http://localhost:3001 and login as "admin" to initiate startup
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

### UI 확인
http://localhost:3001
![스크린샷 2018-07-30 오후 4.39.35.png](../../assets/images/968F808D7B9EF395299DD08D22CF0CF3.png)


## 에러 대응
### 에러1
/Users/kmi/fabric/marbles/utils/connection_profile_lib/parts/org.js:102
		throw new Error('Cannot find org.', orgName);
		
`기본 폴더명이 fabric-sample이어서 에러가 발생한것임` 폴더를 수정해주면 된다.
```
"client": {
        "organization": "Org1MSP",
        "credentialStore": {
            "path": "/$HOME/fabric-samples/fabcar/hfc-key-store"
        }
    },
"organizations": {
        "Org1MSP": {
            "mspid": "Org1MSP",
            "peers": [
                "fabric-peer-org1"
            ],
            "certificateAuthorities": [
                "fabric-ca"
            ],
            "x-adminCert": {
                "path": "/$HOME/fabric-samples/basic-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/admincerts/Admin@org1.example.com-cert.pem"
            },
            "x-adminKeyStore": {
                "path": "/$HOME/fabric-samples/basic-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore/"
            }
        }
    },
```

### 에러2
```
warn: ---------------------------------------------------------------
warn: ----------------------------- Ah! -----------------------------
warn: ---------------------------------------------------------------
error: Looks like you are using an old version of marbles chaincode...
warn: The INTERNAL version of the chaincode found is: v0.x.x
warn: But this UI is expecting INTERNAL chaincode version: v4.x.x
warn: This mismatch won't work =(
warn: Install and instantiate the chaincode found in the ./chaincode folder on your channel mychannel
warn: ----------------------------------------------------------------------
```

```
$ node install_chaincode.js
$ node instantiate_chaincode.js
```
