---
layout: post
title:  "프로젝트-빵테온 node.js - express module 재설치"
description: "토이 프로젝트인 프로젝트-node.js에서 사용하는 express module에 대한 설명"
author: RabbitThief
date: 2020-05-06 20:34:00 +0900
tags: raspberrypi nodejs 
category: Raspberrypi
comments: false
---



## express module 설치

[프로젝트-빵테온 재-시작하기](https://rabbitthief37.github.io/post/project-bbantheon-restart)를 하는 바람에 모듈을 다시 설치를 시도해야 한다.

우선 설치한 모듈은 아래와 같다. 

```shell
npm i express cookie-parser express-session morgan connect-flash
```

![7](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-05-06/7.png)

![8](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-05-06/8.png)



```shell
npm i -g nodemon
```

이렇게 하면 된다고 [책](https://rabbitthief37.github.io/post/br-nodejs-textbook) 에는 있지만 

![9](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-05-06/9.png) 

접근 권한 때문이라는 전형적인 문제 발생.  따라서 sudo를 추가해서 설치를 진행했다.



```shell
sudo npm i -g nodemon
```

![10](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-05-06/10.png)



```shell
sudo npm i -D nodemon
```

![11](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-05-06/11.png)



