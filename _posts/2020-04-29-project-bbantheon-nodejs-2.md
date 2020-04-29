---
layout: post
title:  "프로젝트-빵테온 node.js - express module"
description: "토이 프로젝트인 프로젝트-node.js에서 사용하는 express module에 대한 설명"
author: RabbitThief
date: 2020-04-29 15:55:00 +0900
tags: raspberrypi nodejs 
category: Raspberrypi
comments: false
---	



## express module 설치

[이전 글](https://rabbitthief37.github.io/post/project-bbantheon-nodejs-1) 에서 설치한 모듈에 대한 설명을 따로 하지 않아서 여기에 추가한다.

우선 설치한 모듈은 아래와 같다. 

```
npm i express cookie-parser express-session morgan connect-falsh 
npm i -g nodemon
npm i -D nodemon
```

![1](/assets/article_images/2020-04-27/1.png)



## morgan

node.js 콘솔에 로그를 출력한다.  

```html
GET / 200 51.267 ms - 1539
```

**HTTP요청**(GET)  **주소**(/)  **HTTP상태코드**(200)  **응답속도**(51.267ms)  **응답바이트**(1539)

코드에는 

```javascript
...
var logger = require('morgan');
...
app.use(logger('dev'));
...
```

'dev' 대신에 short, common, combined 등을 줄 수 있다.  개발시에는 short, dev를 많이 사용하고, 배포 시에는 common, combined를 많이 사용한다고 한다.



## cookie-parser

요청에 동봉된 쿠키를 해석한다.  해석된 쿠키들은 req.cookies 객체에 들어간다.



## express-session

 로그인 등의 이유로 세션을 구현할 때 유용하다.  

```javascript
...
var session = require('express-session')
...
app.use(session({
	reesave: false,
	saveUninitialized: false,
	secret: 'secret code',
	cookie: {
		httpOnly: true,
		secure: false,
	},
}));
```

1.5 버전 이전에는 내부적으로 cookie-parser를 사용하고 있어서 cookie-parser 보다 뒤에 위치해야 했다.  하지만 그 이후로 사용하지 않도록 되어서 순서가 상관은 없어졌지만 그냥 뒤에 위치하도록 하는 것이 안전하다.

- resave - 요청이 왔을 때 세션에 수정사항이 생기지 않더라도 세션을 다시 저장할지에 대한 설정
- saveUninitialized - 세션에 저장할 내역이 없더라도 세션을 저장할지에 대한 설정.  보통은 방문자를 추적할 때 사용.
- secret - cookie-parser의 비밀키와 동일하게 지정.  세션 쿠키를 안전하게 전송하려면 쿠키에 서명을 추가해야해서 secret 값이 필요.
- cookie 옵션
  - httpOnly - 클라이언트에서 쿠키를 확인하지 못하도록 설정
  - secure - http가 아닌 환경에서도 사용할 수 있도록 설정. 배포시에는 https를 적용하고 secure: true 설정하는 것을 추천



## connect-flash

중요도는 떨어지는 모듈.  일회성 메시지들을 웹브라이저에 나타낼 때는 편하다.

```javascript
router.get('/flash', function(req, res) {
  req.session.message = '세션 메시지';
  req.flash('message', 'flash message');
  res.redirect('/users/flash/result');
});

router.get('/flash/result', function(req, res) {
  res.send(`${req.session.message} ${req.flash('message')}`);
});
```

