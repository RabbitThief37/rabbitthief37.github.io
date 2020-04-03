---
layout: post
title:  "MacOS에 gcc 9.3 설치하기"
description: "MacOS에 기본적으로 설치된 gcc 말고 최신으로 설치를 해 보자"
author: RabbitThief
date: 2020-04-03 21:01:00 +0900
tags: gcc macos 
category: Gcc
comments: false
---	



## 왜 설치하려고?

c++14 나 그 이후를 컴파일을 해야겠다라고 생각했으나, 기존에 XCode랑 같이 설치되는 Clang은 어디까지 지원하는지를 정확하게 알 수 없고(찾기 싫고 ㅡ,.ㅡ) 이제는 업계 표준이라고 할 수 있는 gcc 최신 Release를 설치할 수 있는지가 궁금했다.  그래서 구글님을 열심히 검색한 결과, 내가 원하는 것을 비슷하게 시도할 글을 찾았다.

[Compiling GCC 9 on macOS Catalina](https://solarianprogrammer.com/2019/10/12/compiling-gcc-macos/)

아래의 순서를 모두 여기를 참고하여 진행하였다.  여기 글은 9.2.0을 기반으로 했지만, 최신 릴리즈는 9.3.0 이다.



## 1.Ready for compilation

![1](/assets/article_images/2020-04-03/1.png)

역시 맥북에 기본적으로 설치되어 있는 gcc 가 실행된다.

<img src="/assets/article_images/2020-04-03/2.png" alt="2" style="zoom:50%;" />

> xcode-select --install
>
> cd ~
>
> mkdir gcc_all && cd gcc_all



## 2.Download gcc



> curl -L https://ftpmirror.gnu.org/gcc/gcc-9.3.0/gcc-9.3.0.tar.xz | tar xf -



이 명령어으로 받으면 되는데 최종 버젼을 알고 싶어서 공식사이트 가서 Mirror를 연결해 보았다.  가까지만 먼 나라인 일본이 있길래 연결을 했더니

<img src="/assets/article_images/2020-04-03/3.png" alt="3" style="zoom:50%;" />

![4](/assets/article_images/2020-04-03/4.png)

이러다가 에러남.  어처구나가 없어서 그냥 http download를 해 봤더니, 이번에는 Total Size 설정이 안되서 막 올라가다가 실패... 이것들이 한국에서 받으면 잘 동작 안되도록 한 모양이라는 매우 현실적인 의심이 마구들어서 딴 곳에서 받으면 되지모.  크기  얼마 하지도 않는데 시간 낭비하게 만들고 있어. ㅡ.ㅡ+++



<img src="/assets/article_images/2020-04-03/5.png" alt="5" style="zoom:50%;" />

여기서 깔끔히 http download로 받았다.  그냥 official site에서 받으면 한방에 끝나는 것을 괜시리...흠...



## 3.Additional download

필요한 파일이 더 있나 보다.  따로 명령어를 준비를 해 놓았더라.

> cd gcc-9.3.0
>
> contrib/download_prerequistes



## 4.Make link

기존에 설치되어 있는 내용들을 build하는데 사용해야 해서 따로 만들어 놓는다.  

> sudo mkdir -p /usr/local/gcc_system_root
> cd /usr/local/gcc_system_root
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/Library .
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System .
>
> sudo mkdir usr && cd usr
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/bin .
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib .
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/libexec .
> sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/share .
>
> sudo cp -r /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include include

마지막은 copy 이다.  따라서 타이핑하지 말고 그냥 copy & paste 추천



## 5.Edit header file

원문에는 먼가 설명이 있는데, 굳이 해석해서 적고 싶지 않고 그냥 따라해라.

> sudo vim /usr/local/gcc_system_root/usr/include/Availability.h

300 번째 라인에 가서 아래 화면과 같이 사이에 코드를 넣는다.

![6](/assets/article_images/2020-04-03/6.png)



> #ifndef __OSX_AVAILABLE_STARTING
>     #define __OSX_AVAILABLE_STARTING(_osx, _ios)
>     #define __OSX_AVAILABLE_BUT_DEPRECATED(_osxIntro, _osxDep, _iosIntro, _iosDep)
>     #define __OSX_AVAILABLE_BUT_DEPRECATED_MSG(_osxIntro, _osxDep, _iosIntro, _iosDep, _msg)
> #endif



## 6.Build

자...이제 준비는 마쳤다.  이제는 환경 잡고 빌드를 하면 된다.

> cd ~/gcc_all/gcc-9.2.0
> mkdir build && cd build
>
>  ../configure --prefix=/usr/local/gcc-9.3 \
>               --enable-checking=release \
>               --enable-languages=c,c++ \
>               --disable-multilib \
>               --with-sysroot=/usr/local/gcc_system_root \
>               --program-suffix=-9.3

환경이 설정적으로 설정이 될 것이다. 이제는 빌드를 하면 되는데, 약 3,4분쯤에 한번 프린트를 하겠다라는 대화상자가 나오는데, 그걸 취소 또는 출력을 해 주어야 다음 단계로 넘어가니 주의 바란다.

대략 총 걸리는 시간은 1시간정도 족히 걸리니 넉넉하게 생각하시길...

> make -j 8



## 7.Install & add PATH

> sudo make install-strip

/usr/local/gcc-9.3 이라는 곳에 설치가 완료된다. 



> echo 'export PATH=/usr/local/gcc-9.2/bin:$PATH' >> ~/.zshrc
> source ~/.zshrc



## 8.C++14 compile

```c++
//Program to test the C++ lambda syntax and initializer lists
#include <iostream>
#include <vector>

int main()
{
  // Test lambda
  std::cout << [](int m, int n) { return m + n;} (2,4) << '\n';

  // Test initializer lists and range based for loop
  std::vector<int> V{1,2,3};

  std::cout << "V =\n";
  for(auto e : V) {
    std::cout << e << '\n';
  }

  return 0;
}
```

> g++-9.3 test.cpp -o test



완벽하게 이전 설치 gcc를 변경하는 것은 시스템에서 쓰는 것도 있어서 위험해 보인다. 

그냥 이렇게 각자 도생하자!

