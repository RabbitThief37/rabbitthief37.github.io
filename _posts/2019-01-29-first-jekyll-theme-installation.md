---
layout: post
title:  "Jekyll Minimal Mistake Theme 적용하기"
description: "github.io에 Jekyll Minimal Mistake Theme 적용을 하면서 실패한 이야기"
author: RabbitThief
date:   2020-01-29 19:40:00 +0900
tags: jekyll githubio gossip
category: Gossip
comments: false
---


글을 써 보겠다고 결정을 했으면 바로 시작해야 한다는 것을 조언으로 많이 들었다.

하지만 이것이 어떤 플렛폼을 선택할지에서 부터 문제가 시작되었다.

- 나도 매일 커밋이라는 것을 하고 싶다 - 잔디 심기.
- 마크다운을 함 경험해 보고 싶다.
- 다른 분들 보니 이쁜 테마 많더라.

해서 결정한 것이 github.io 에서 만들어 보자는 것이었다.



# 첫번째 시도 - 검색해서 이쁜 걸로 적용해 보자.



구글님께 질문을 했더니 많은 문서 중에서 아래를 참고해서 작업을 시작했다.

- [Jekyll을 이용해 GitHub에 블로그 만들기(1)](https://jetalog.net/86?category=808871)
- [Jekyll을 이용해 GitHub에 블로그 만들기(2)](https://jetalog.net/87?category=808871)

테마를 지정해 주지 않아서 어느 블러그 인지 기억이 안나지만, 괜찮다고 추천한 "Clean Blog"를 적용해 보기 시작했는데...

FAILED!!!



# 두번째 시도 - 설명이 잘 되어 있는 걸로 다시.



[쉽고 빠르게 수준 급의 GitHub 블로그 만들기 - jekyll remote theme으로](https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html)

Repository를 몇 번이 지우면서 도전을 해 보았는데, 그래도 나름 최신 글이라고 생각을 했는데 오픈 소스의 성격자체 워낙 빠르게 변하니 내용이 먼가 잘 맞지 않은거 같다. 

어째든 FAILED!!!



# 세번째 시도 - Wordpress로 가자



[Rabbit Thief's LOG](https://tvrabbitthief.video.blog/)

옛날 옛적 사진을 뒤져서 몇 개 배경으로 구성은 잘 되었는데, 웹에디터를 써야 하는 불편함이 있고, 이걸 타파하려면 MarsEdit 같은 유료 툴을 써야 하는 장벽이 있다.  그리고 잔디를 심을 수가 없쟎아 !

그래서 포기!!!



# 네번째 시도 - Original로 하자



[평범한 텍스트 파일을 정적 웹사이트 또는 블로그로 변신시켜 보세요.](https://jekyllrb-ko.github.io/)

테마가 자시고 패스하고 원본으로 해보자 하고 했다.  단번에 깔끔히 publish까지 완료 했다.

하지만 역시 많이 허전하다.  하~ 그냥 할까 하다가 괜시리 github 설정에 있는 Theme에 손을 대었다가

![repository - setting](/assets/article_images/2020-01-29/1.png)



화면이 표시가 안된다. What!  

FAILED!!!



# 마지막 시도 - Fork



두번째 시도 했던 블러그를 포크 하셨다는 코멘트가 있어서, 나도 시도해 봤다.

_config.yml를 변경하고 ~ image를 변경하고 ~ 기존 글을 보면서 테스트 페이지를 만들어 보고 ~

진작에 이럴껄... 시간 & 노력을 많이 낭비했다네~

Fork의 위대함을 처음으로 경험해 보았다.

