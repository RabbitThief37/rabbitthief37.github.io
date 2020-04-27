---
layout: post
title:  "오래된 Raspberry Pi 1(armv6l)에 node.js 설치하기"
description: "공식 사이트 링크도 사라진 라즈베리파이 모델에 node.js 설치하여 웹서버를 구축해 보자"
author: RabbitThief
date: 2020-04-14 20:55:00 +0900
tags: raspberrypi nodejs 
category: Raspberrypi
comments: false
---	



[이전 글](https://rabbitthief37.github.io/post/raspberrypi-macos) 에서 언급했듯이 나에게는 오래된 - 정말 오래된 라즈베리파이 모델이 뿐이 없다.  새로 구매하고 싶은 욕구는 넘치나 구매욕뿐이 없어서 문제이다. ^^

그래서 무엇인가 만들어 봐야겠다 하는 생각에 백수가 되어서 시간도 있겠다~ 공부도 할 수 있겠다~ 라는 허튼 생각이 하나 하나씩 구성을 해 보려고 한다.



### deno 설치 - 실패

node.js 만들었던 개발자가 새로이 시작하는 프로젝트라는 말을 들은 기억이 있어서 이걸로 해 보자~ 라는 생각에 설치를 도전해 보았으나

```sh
curl -fsSL https://deno.land/x/install/install.sh | sh
```

![1](/assets/article_images/2020-04-14/1.png)

아직 arm 계열은 개발 중인가 보다.  깔끔히 포기... 그래도 아쉽기는 하다.



### apt-get을 이용한 설치

apt-get으로 설치할 때 node.js만 설치하면 되는지 알았는데, npm도 같이 설치를 해야 한다고 한다.  놀란 것은 node.js 설치할때 표시되는 패키지 개수보다 npm을 (이걸 따로 설치해야 하더라고??) 설치할 때 나온 것이 훨씬 많아서다.  왠지 느낌은 배보다 배꼽? 이라고나 할까.

[라즈베리파이 node.js 서버구축](http://www.hardcopyworld.com/ngine/aduino/index.php/archives/3310) 글을 참고해서 진행을 했다만은...

![2](/assets/article_images/2020-04-14/2.png)

어... 저기요... 이게 머죠??? 먼가 이상다라는 WARN 메세지가... 숨을 고르고 읽어 보면 현재 설치된 npm이 node.js 10.15.2는 지원하지 않는다는 건데... 왜??? 같이 apt-get으로 설치했는데... 이상하다... 라고 생각하고 version을 확인해 보았다.

- node.js -v  : 10.15.2
- npm -v : 5.8.0 (시간이 엄청 오래 걸림)

이것 저것 검색을 해 보니 npm을 최신 버젼으로 설치하는 방법이 있어서 그 명령을 실행해 보았다.

![3](/assets/article_images/2020-04-14/3.png)

무엇인가 설치되는 모양새였는데 여전히 npm version은 변경되지 않았다.  다 지우고, update도 다시 하고, clean도 해 보고.  그러나 다 실패.  이렇게 하면 안되는구나 ㅜㅜ



### Source를 받아서 설치

[Install Node.js and Npm on Raspberry Pi](https://www.instructables.com/id/Install-Nodejs-and-Npm-on-Raspberry-Pi/) 의 글을 참조해서 직접 소스를 다운 받아서 설치를 해 보는 것으로 결정했다.  이 글을 보니 node.js와 npm이 같이 포함되어 있는 것으로 나온다. (당연...apt-get은 왜?)  그래서 node.js 공식 사이트에 갔더니 armv6l 은 없더라.  아하... 직접 디렉토리를 찾아 보았다.

<img src="/assets/article_images/2020-04-14/4.png" alt="4" style="zoom:50%;" />

armv6l을 지원하는 node.js 마지막 version은 11.15.0 였다.  해서 이것으로 다운로드를 받아서 위 링크의 글 대로 진행을 하고 version을 확인해 보니

![5](/assets/article_images/2020-04-14/5.png)

짜잔~ 만세... 예제 코드도 잘 동작했다.  

한 가지 PATH에서 '.' 이 포함되지 않으면 이상하게 node 명령어를 찾지 못했다.  /usr/local/bin이 PATH에 추가된 것을 확인 했는데도 무슨 문제인지는 정확하게 모르겠지만.  