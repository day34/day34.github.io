---
classes: wide
title: WebRTC - TURN Server 설치
date: 2018-07-19 00:00:00 +0900
type: post
comments: true
categories: []
tags: [WebRTC, TURN, CentOS]
---

# 참고 사이트
[http://qiita.com/okyk/items/2d7db6b148a43bc3b405](http://qiita.com/okyk/items/2d7db6b148a43bc3b405)

[http://stackoverflow.com/questions/22233980/implementing-our-own-stun-turn-server-for-webrtc-application](http://stackoverflow.com/questions/22233980/implementing-our-own-stun-turn-server-for-webrtc-application)

[http://blog.wnotes.net/blog/article/stun-server-on-aws-ec2](http://blog.wnotes.net/blog/article/stun-server-on-aws-ec2)

## 설치
CentOS에서 설치했다.

### TURN server 최신버전 다운로드
```
http://turnserver.open-sys.org/downloads
```

### TURN server 인스톨
```
$ sudo apt-get install coturn -y
$ sudo apt-get update && apt-get install libssl-dev libevent-dev sqlite3 build-essential make -y
$ sudo wget -O turnserver.tar.gz http://turnserver.open-sys.org/downloads/v4.5.0.6/turnserver-4.5.0.6.tar.gz
$ tar -zxvf turnserver.tar.gz
$ cd turnserver-*
$ ./configure
$ make && sudo make install
```

### TURN server.conf 수정
```
$ sudo cp /usr/local/etc/turnserver.conf.default /usr/local/etc/turnserver.conf
$ sudo vi /usr/local/etc/turnserver.conf
```

### 주석 해제
```
verbose
fingerprint
lt-cred-mech
realm=webrtc
no-tls
no-dtls
#stun-only
log-file=/var/log/turnserver.log
```

### 서버 시작
```
-o : 데몬
-c : /usr/local/etc/turnserver.conf (설정 안해도 기본)
$ sudo turnserver -X realserver-ip
```

### 서버 검증
```
$ sudo turnutils_uclient -t -u 'username' -w 'password’ realserver-ip
```