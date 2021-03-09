---
layout: post
title:  "JetsonNano 설치를 위한 준비"
description: "Nvidia JetsonNano 이미지 파일을 설치하기 위한 준비"
author: RabbitThief
date: 2021-03-09 16:21:00 +0900
tags: jetsonnano linux microsd 
category: jetsonnano
comments: false
---	



# 설치를 위한 준비

당연하지만 microSD가 필요하다.  이전에 회사에서 사용하고 남은 MINIBOSS 16GB를 사용하여 먼저 시도를 해 보았다.  결과는 망~ 이다.  

![/assets/images/2021/0309/Untitled.png](/assets/images/2021/0309/Untitled.png)

요즘은 XC 모델로 다 변경되어서 괜찮나 모르겠지만 HC1은 절대 사용하면 안되겠더라.  OS가 도중에 Pause 걸리는 현상을 자주 구경할 수 있다.  그냥 삼성 사라.

그런데 삼성 이것들도 장난질이 심하더라.

그다지 많은 프로그램을 설치 하지 않을 것이기에 32GB정도를 생각하고 있었는데

![/assets/images/2021/0309/Untitled201.png](/assets/images/2021/0309/Untitled201.png)

와~ 쓰기 속도 보소.

가격이 더럽게 비싼 것도 아니라서 정신 건강을 위해 128GB를 선택했다.

무사히 물건이 왔으니 포멧부터

![/assets/images/2021/0309/__2021-01-25_082835.png](/assets/images/2021/0309/__2021-01-25_082835.png)

![/assets/images/2021/0309/__2021-01-25_082938.png](/assets/images/2021/0309/__2021-01-25_082938.png)

![/assets/images/2021/0309/__2021-01-25_083009.png](/assets/images/2021/0309/__2021-01-25_083009.png)

정확한 용량이 잘 나오는 것을 확인할 수 있다. 

다음은 flash tool 인데 요즘은 balenaEtcher가 가장 많이 쓰이는 거 같다.  편리하기도 하고.

쓰기를 시작하니 miniboss와 한 10배 이상 차이가 나는 것으로 느껴졌다.  

금방 끝남.

![/assets/images/2021/0309/__2021-01-25_083046.png](/assets/images/2021/0309/__2021-01-25_083046.png)

![/assets/images/2021/0309/__2021-01-25_083140.png](/assets/images/2021/0309/__2021-01-25_083140.png)

![/assets/images/2021/0309/__2021-01-25_083156.png](/assets/images/2021/0309/__2021-01-25_083156.png)

![/assets/images/2021/0309/__2021-01-25_083209.png](/assets/images/2021/0309/__2021-01-25_083209.png)

![/assets/images/2021/0309/__2021-01-25_083225.png](/assets/images/2021/0309/__2021-01-25_083225.png)

![/assets/images/2021/0309/__2021-01-25_083246.png](/assets/images/2021/0309/__2021-01-25_083246.png)

앗! 

물론 모든 작업이 마친 뒤에 아래와 같은 상황이 발생하지만 당황하지 마라. 그냥 포커스를 위해 한번 클릭을 해 주고 ESC 무한 누르기를 하면 없어진다.  너~~~~~~~~무 친절한 윈도우씨.

![/assets/images/2021/0309/__2021-01-25_084844.png](/assets/images/2021/0309/__2021-01-25_084844.png)

![/assets/images/2021/0309/__2021-01-25_085314.png](/assets/images/2021/0309/__2021-01-25_085314.png)