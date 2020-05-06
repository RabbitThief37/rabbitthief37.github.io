---
layout: post
title:  "프로젝트-빵테온 재-시작하기"
description: "토이 프로젝트인 프로젝트-빵테온에 대해 다시 시작한다"
author: RabbitThief
date: 2020-05-06 17:29:00 +0900
tags: raspberrypi nodejs sqlite3 android 
category: Raspberrypi
comments: false
published: true
---



## 망했다 ㅡㅡ

[프로젝트-빵테온 시작하기](https://rabbitthief37.github.io/post/project-bbantheon) 를 시작하면서 정말 오래된 Raspberry Pi 1 Model B를 가지고 이것 저것을 설치도 해서 어느 정도 안정화 시켰다고 생각하고 3일 정도 외부 일이 있어서 신경을 쓰지 못했다. 

오늘 접속을 시도하니 SSH Key Error 내면서 계속 접속이 되지 않는 것이다.  구글님이 알려 주신대로 해도 되지 않아서 강제 리부팅을 시도 했으나 부팅이 되지 않는다. 앙? 😱

온갖시도를 다 해 보고 결국에는 포기하고 남아 있던 Raspberry Pi 2 Model B로 시도를 해 봤다.

어덥터로 하면 전원이 안들어고 맥북 USB에 혹시 물리니 화면이 나오고 부팅도 잘 되고...😥



## 전원 문제

무엇인가 전원의 문제인가 싶었는데, 부분에 대한 지식이 0에 수렴하니 몸으로 때우는 수 뿐이... 너무 높은 전류인가 싶어서 가운데 남아 도는 외부 베터리는 연결했더니... 잘된다... 😑 

<img src="/assets/article_images/2020-05-06/1.png" alt="1" style="zoom:50%;" />



라즈베리파이 포럼에 보면 이거에 대한 상세한 답변이 있다.

![2](/assets/article_images/2020-05-06/2.png)



어쨌든 Arm7이라서 이전의 불편함을 조금은 상쇄할 수 있을꺼 같다.

<img src="/assets/article_images/2020-05-06/3.png" alt="3" style="zoom:50%;" />



라고 생각했으나, 결과는 [오래된 Raspberry Pi 1(armv6l)에 node.js 설치하기](https://rabbitthief37.github.io/post/raspberrypi-nodejs) 와 동일하다. 🤬

![4](/assets/article_images/2020-05-06/4.png)



머...별루 있나.  이번에는 이미 삽질을 한 경험이 있어서 시간을 많이 낭비하지 않고 바로 공홈에서 binary 다운 받아서 수동 설치를 완료 했다.

![5](/assets/article_images/2020-05-06/5.png)



또 다시  [sqlite3 작업](https://rabbitthief37.github.io/post/project-bbantheon-db) 과 [node module](https://rabbitthief37.github.io/post/project-bbantheon-nodejs-2) 설치 작업을 해야겠다. 

귀찮...귀찮...귀찮...

