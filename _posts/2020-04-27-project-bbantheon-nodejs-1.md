---
layout: post
title:  "프로젝트-빵테온 node.js - sqlite3 진행 중 #1"
description: "토이 프로젝트인 프로젝트-node.js와 sqlite3를 연결하기 시작"
author: RabbitThief
date: 2020-04-27 23:55:00 +0900
tags: raspberrypi nodejs sqlite3 
category: Raspberrypi
comments: false
---	



## express module 설치

[Do It! Node.js 프로그래밍](https://rabbitthief37.github.io/post/br-do-it-nodejs) 을 속독으로 훝어 보고, 지금은 다음 책을 보고 있는 중이다.  정독을 할 생각은 1도 없었는데, 자꾸 이해 안되는 부분이 나오면 집중해서 보고 있는 나를 시간이 좀 흐른 후에 파악해서 생각보다는 느리게 진행되고 있다.

express를 비롯하여 책에서 필요하다고 추천하는 부분을 같이 설치를 했다.

 view module은 필요 없을 것이라서 pug만 제외했다.

```
npm i express cookie-parser express-session morgan connect-falsh 
npm i -g nodemon
npm i -D nodemon
```

![1](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-04-27/1.png)

대충 이런 version들이 설치가 된다.

이렇게 여러 개를 설치하는데 시간이(**75sec**) 별루 오래 걸리지 않았는데...문제는...



## sqlite3 module 설치

구글님에게 문의를 해서 설치를 진행했는데, 역시 시간이(**2217sec**) 꽤나 걸린다.  소스를 받아서 컴파일을 해서 설치를 하는 거 같은데, 도중에 Warning이 꽤나 많이 표시된다.

동일한 함수에서 나오는거 같기는 한데, 내가 고칠 만한 능력은... 😝

![2](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-04-27/2.png)

무엇인가 깔끔하게 서로 맞는 않는 분위기이다. 

걱정이 되어서 급하게 샘플코드를 찾아서 동작을 시켜 보았다.

```javascript
const sqlite3 = require('sqlite3').verbose();

// open the database
let db = new sqlite3.Database('./db/bbantheon.db', sqlite3.OPEN_READWRITE, (err) => {
  if (err) {
    console.error(err.message);
  }
  console.log('Connected to the bbantheon database.');
});

db.serialize(() => {
  db.each(`SELECT IDX, NAME, LEVEL FROM USER_INFO`, (err, row) => {
    if (err) {
      console.error(err.message);
    }
    console.log(`${row.IDX} , ${row.NAME} , ${row.LEVEL}`);
  });
});

db.close((err) => {
  if (err) {
    console.error(err.message);
  }
  console.log('Close the database connection.');
});
```

![3](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-04-27/3.png)

문제 없이 모듈이 동작하는 것으로 보인다. 

이제 세부적으로 SQL문장과 코드를 추가해 나가야겠다.