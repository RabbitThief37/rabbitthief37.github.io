---
layout: post
title:  "MacOS에서 Raspberry Pi 이미지 만들기"
description: "예전에는 shell command로 했던 것을 GUI program을 이용해서 처리"
author: RabbitThief
date: 2020-09-15 10:39:00 +0900
tags: c# winui 
category: Dotnet
comments: false
---	




# WinUI 3.0

갓MS께서 .NET UI 쪽을 WinUI 라는 것으로 통합하시겠다고 하는 위대한 🥳 계획을 발표하셨다. 

[Windows UI 라이브러리(WinUI)](https://docs.microsoft.com/ko-kr/windows/apps/winui/)

아직 Preview 상태라서 여러가지를 희생하면서 실행을 해 보려고 했는데, 사전 조건이 꽤나 까다롭다.  거기에 작업PC가 폐급 🥴이면 여러가지 어려움이 밀물처럼 들어온다.

![/assets/article_images/2020-09-15/Untitled.png](/assets/article_images/2020-09-15/Untitled.png)

![/assets/article_images/2020-09-15/Untitled1.png](/assets/article_images/2020-09-15/Untitled1.png)

최근에 간신히 2004가 올라갔다.  그래서 그 나마 이런 흔적이라도 남길 수 있는 기회 🤣 라도 얻었다.

Visual Studio Preview 도 설치하고

[Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview/)

.Net SDK 최종도 설치하고

[Download .NET (Linux, macOS, and Windows)](https://dotnet.microsoft.com/download)

.Net 5 Preview 마지막도 설치하고 했는데

[Download .NET 5.0 (Linux, macOS, and Windows)](https://dotnet.microsoft.com/download/dotnet/5.0)

New Project에서 WinUI in Desktop 만 선택하면, 프로젝트 추가 하다가 에러가 발생한다. SDK Version도 조절해 보고 해도 변화가 없었다. (딥빡~~~ 🥵)

![/assets/article_images/2020-09-15/Untitled2.png](/assets/article_images/2020-09-15/Untitled2.png)

바로 구글님에게 문의해 보니 GitHub에 등록이 되어 있었다.  ( 밑에 있는 WinUI in UWP는 잘 실행된다는 건 함정 😼 )

[Error while creating a WinUI 3, C# on Desktop project. · Issue #2949 · microsoft/microsoft-ui-xaml](https://github.com/microsoft/microsoft-ui-xaml/issues/2949)

글을 대충 읽어 보니 Preview 5 설치하는 거고, 자동으로 설치하는 Powershell Script도 친절하게도 같이 올려 주었다. 

그러나 세상이 그리 녹녹하지는 않다는거.

 Powershell을 관리자 권한으로 열었는데도 해당 스크립트는 실행이 되지 않았다. 😊

![/assets/article_images/2020-09-15/Untitled3.png](/assets/article_images/2020-09-15/Untitled3.png)

네??? 머라구요??? 권한이요??? 관리자로 실행하고 있는건데요??? 여보세요???

머..별수 있나... 다시 구글님에게 문의...

[about_Execution_Policies - PowerShell](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7)

착실히 시키는대로 하니깐 실행 성공.

![/assets/article_images/2020-09-15/Untitled4.png](/assets/article_images/2020-09-15/Untitled4.png)

그래서 다시 큰 꿈을 가지고 다시 실행 하면

![/assets/article_images/2020-09-15/Untitled5.png](/assets/article_images/2020-09-15/Untitled5.png)

@#%@$^#$%^#$%@#$@#$!@332%ㄲ$#%^

저기요... 똑같이 안되는데요.  UWP로 하던가 다음 SDK Release되면 다시 시도해 보는 걸로...쳇...

이슈 코멘트가 더 있길래, 미련을 버리고 못하고 읽어 보니 최신 .Dot Net 5 preview를 지우고 설치를  해야 한다고 하네.

![/assets/article_images/2020-09-15/Untitled6.png](/assets/article_images/2020-09-15/Untitled6.png)

![/assets/article_images/2020-09-15/Untitled7.png](/assets/article_images/2020-09-15/Untitled7.png)

이 친구의 조언대로 하면 추가는 잘 된다.  하지만 실행을 하면 Error가 발생하면서 실행이 되지 않는다.  본인도 딱히 여기까지는 대안이 없는 모양.

![/assets/article_images/2020-09-15/Untitled8.png](/assets/article_images/2020-09-15/Untitled8.png)

그래서 결론은 아직은 하지마!!! 기다려!!!