---
layout: post
title:  "Yocto 프로젝트를 활용한 임베디드 리눅스 개발 2/e"
description: "Yocto 프로젝트를 활용한 임베디드 리눅스 개발 2/e"
author: RabbitThief
date:   2020-01-30 17:24:00 +0900
tags: development book 
category: Books
comments: false
---


# EMBEDDED LINUX DEVELOPMENT USING YOCTO PROJECTS SECOND EDITION

![COVER](http://image.kyobobook.co.kr/images/book/xlarge/894/x9791161751894.jpg)

지음 : 오타비우 살바로드 , 다이앤 앤골리니

옮김 : 배창혁

[교보문고 상세](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791161751894&orderClick=LAG&Kc=#N)

한줄평 : 초보책은 맞는데 YOCTO 초보를 위한 책이니 Embedded Linux 초보를 위한 책은 아니다. Poky v2.4

제 점수는요 : 3.5 / 5



## 들어가며

Yocto 프로젝트 전체를 이해하기 위해 깃 저장소에 있는 [Heading for the Yocto Project(PDF)](https://github.com/CollaborativeWritersHub/heading-for-the-yocto-project/releases) 를 읽어 볼 것을 추천한다.  정말 개요다.



## 1장. Yocto 프로젝트 소개

어디에서나 기술 초보책에 있는 개괄적인 역사. 

난 SKIP



## 2장. 포키 시스템

빌드 환경 설정하는 법



## 3장. Toaster를 사용한 이미지 생성

빌드를 웹에서 할 수 있도록 지원하는 솔류션을 소개.  좋다~



## 4장. 비트베이크

- 환경설정 (.conf 파일)
- 클래스 (.bbclass 파일)
- 레시피(.bb와 .bbappend 파일)

build/conf/bblayers.conf 부터 시작



```bash
$ bitable <recipe>

$ bitbake <recipe> -c <task>

$ bitbake <recipe> -c listtasks  (레시피에 정의된 태스크를 알아보기)
```



## 5장. 임시 빌드 디렉토리

생성되는 디렉토리에 대한 개괄적 설명.



## 6장. 패키지 지원 고찰

처음부터 빌드가 필요할 경우 build/tmp 디렉토리를 지워 sstate-cache를 사용해 빌드 속도를 높이거나, build/tmp 와 state-cache를 모두 지워 빌드를 깨끗하게 진행할 수도 있다. 

> if exported a non-exist TEMPLATECONF path by typos, **remove** **build** **folder and export again** 



> if want a clean build, **remove** **build** **and** **sstate-cache** **folder, and start from beginning** 



## 7장. 비트베이크 메타데이터

- .conf - 환경설정 파일은 전역으로 영향을 미치는 파일로, 클래스와 레시피의 동작을 위한 정보를 제공
- .bbclass - 전체 시스템에서 이용할 수 있고, 쉬운 유지 보수와 코드 중복을 피하기 위해서 레시피에서 상속
- .bb or .bbappend - 태스크가 실행되도록 정의하고 비트베이크가 필요한 태스트 체인을 생성할 수 있게 필요한 정보를 제공.  레시피는 가장 많이 사용하는 메타데이터 종류고, 모든 작업에 필요.



비트베이크의 환경 변수 옵션을 사용하면 각 변수의 값을 확인할 수 있다.

```bash
$ bitbake -e <recipe> | grep <variable>
```



비트베이크에서 사용하는 문법 설명이 간략하게 나오는데, 잘 읽어볼 필요가 있는 chapter이다.



## 8장. Yocto 프로젝트를 이용한 개발

eclipse와 통합에 대해서 가장 흥미롭지만 

> [4.15. Moving to the Yocto Project 2.7 Release](https://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#moving-to-the-yocto-project-2.7-release)
>
> - [4.15.1. BitBake Changes](https://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#migration-2.7-bitbake-changes)
> - [4.15.2. Eclipse™ Support Removed](https://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#migration-2.7-eclipse-support-dropped)

그렇다고 한다. ㅡ.ㅡ



## 9장. Yocto 프로젝트 디버깅

이미지에 디버그 심폴과 디버그 도구를 포함한 디버그 패키지를 설치하려면 IMAGE_FEATURES += "deg-pkgs tools-debug"를 build/conf/local.conf 파일에 추가



## 10장. 외부 레이어

## 11장. 사용자 레이어 생성

IMAGE_FEATURES에 추가할 수 있는 특징들을 설명해 놓았다. 

이거 말고는 그다지...



## 12장. 레시피 커스터마이즈

하드코딩한 경로를 사용하지 말고 poky/meta/conf/bitbake.conf에 정의된 변수를 사용해야 한다.



## 13장. GPL 규정 준수

좋은 이야기.



## 14장. 커스텀 임베디드 리눅스 부팅

비글본 블랙, 라즈베리파이 3, 완드보드에 layer 만들어서 빌드하고 복사하는거 까지 설명했는데, 허접하기 이를때 없다.





