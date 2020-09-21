---
layout: post
title:  "WPF Samples"
description: "WPF Samples...순서는 너맘대로..."
author: RabbitThief
date: 2020-09-21 10:09:00 +0900
tags: c# wpf 
category: Dotnet
comments: false
---	



매번 마다 급하게 만들려고 하니 그냥 WinForm으로 하는 경우가 잦았다.  해서 이번에 WPF로 변경을 시도해 보려고 한다.  ( 최신으로 WinUI를 할려고 했으나, 환경이 지원되지 않아서 포기 )

책 같은 것을 차근 차근 넘겨 보며 진행할 여유가 없으므로 바로 샘플로 고고

[microsoft/WPF-Samples](https://github.com/microsoft/WPF-Samples)

.Net 관련 예제는 Docker로 배포하고 해서 의외로 귀찮았는데, 이건 통~ 샘플로 받을 수 있어서 기분 좋게 출발할 수 있었다.

전체를 빌드할 수 있는 Solution 파일도 있고, 각각의 파트별로 Solution 파일들이 따로 존재해서 구성은 나쁘지 않다.

그러나, 벗드, 😓

Getting Started 라고 보이는 아주 출발하기 좋은 폴더가 보이길래 '요놈부터다! ' 하고 열었는데... 진행 가이드 따위는 없다.  그냥 너가 보고 싶은데로 봐~ 라고 하는 이 황당함.

![/assets/article_images/2020-09-21/Untitled.png](/assets/article_images/2020-09-21/Untitled.png)

이 세상의 모든 코드의 시작은 HelloWorld 아니겠나.  

예가 1번

![/assets/article_images/2020-09-21/Untitled1.png](/assets/article_images/2020-09-21/Untitled1.png)

나중을 위해서 프로젝트명에 번호를 붙이기로 했다. 

```csharp
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml"/>
```

app.xaml 파일에는 가장 중요한 내용은 StartupUri.  시작하는 화면을 알 수 있다. 

```csharp
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525" Icon="app.ico">
    <Grid>
        <TextBlock VerticalAlignment="Center" HorizontalAlignment="Center">
            Hello, World!
        </TextBlock>
    </Grid>
</Window>
```

XML 스러운 복잡함으로 여러가지 요소가 있지만, 난 초보니깐 패스.  텍스트 박스 하나 있고, 정렬 기준 정한 다음에 "Hello, World!" 출력하는거로군.

![/assets/article_images/2020-09-21/Untitled2.png](/assets/article_images/2020-09-21/Untitled2.png)

Simple 이라는 단어가 들어가 있으니, 너가 2번

app.xaml 은 동일한거 같고

```csharp
<StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
        <Button HorizontalAlignment="Left"
                Width="100"
                Margin="10,10,10,10">
            Button 1
        </Button>
        <Button HorizontalAlignment="Left"
                Width="100"
                Margin="10,10,10,10">
            Button 2
        </Button>
        <Button HorizontalAlignment="Left"
                Width="100"
                Margin="10,10,10,10">
            Button 3
        </Button>
        <Button HorizontalAlignment="Left"
                Width="100"
                Margin="10,10,10,10">
            Button 4
        </Button>
    </StackPanel>
```

StackPanel에 Button 4개를 추가하는 거군.

![/assets/article_images/2020-09-21/Untitled3.png](/assets/article_images/2020-09-21/Untitled3.png)

먼가 위에 달려는 있는데 무슨 기능인지를 아직 절대 궁금하지 않다.

![/assets/article_images/2020-09-21/Untitled4.png](/assets/article_images/2020-09-21/Untitled4.png)

이름은 난이도가 있어 보이지만, 파일 구성이 간단해 보여서 3번.

```csharp
<StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
        <Button HorizontalAlignment="Left"
				Width="100"
				Margin="10,10,10,10"
				Click="HandleClick"
				Name="Button1">Click Me</Button>
    </StackPanel>
```

Dynamic 이라는 단어는 코드를 도입한 거라는 의미를 뻥티기 한것으로 이해된다.

F7 누르면 연결된 코드가 나온다.

![/assets/article_images/2020-09-21/Untitled5.png](/assets/article_images/2020-09-21/Untitled5.png)

원래 코드는 한번 누르면 Hello World로 button text가 변경되고 끝인데, 심심한거 같아서 toggle 되도록 변경해 보았다.

복잡한 것을 해 볼까.

![/assets/article_images/2020-09-21/Untitled6.png](/assets/article_images/2020-09-21/Untitled6.png)

```csharp
<DockPanel>
        <TextBlock Background="LightBlue"
                DockPanel.Dock="Top">Some Text</TextBlock>
        <TextBlock DockPanel.Dock="Bottom"
                Background="LightYellow">Some text at the bottom of the page.</TextBlock>
        <TextBlock DockPanel.Dock="Left"
                Background="Lavender">Some More Text</TextBlock>
        <DockPanel Background="Bisque">
            <StackPanel DockPanel.Dock="Top">
                <Button HorizontalAlignment="Left" 
                Height="30px"
                Width="100px"
                Margin="10,10,10,10">Button1</Button>
                <Button HorizontalAlignment="Left"
                Height="30px"
                Width="100px"
                Margin="10,10,10,10">Button2</Button>
            </StackPanel>
            <TextBlock Background="LightGreen">Some Text Below the Buttons</TextBlock>
        </DockPanel>
    </DockPanel>
```

DockPanel를 이용해서 배치를 시켜 보는것. DockPanel.Doc 이라는 항목이 중요한 기준.

![/assets/article_images/2020-09-21/Untitled7.png](/assets/article_images/2020-09-21/Untitled7.png)

![/assets/article_images/2020-09-21/Untitled8.png](/assets/article_images/2020-09-21/Untitled8.png)

실행 중 구성을 보여 주는 재미난 툴도 추가되었네.

![/assets/article_images/2020-09-21/Untitled9.png](/assets/article_images/2020-09-21/Untitled9.png)

여기서 부터 시작 xaml 이름이 변경되기 시작한다.  슬슬 본색이 나올려고 하는 모양.

화면을 추가를 한다고 하면 

![/assets/article_images/2020-09-21/Untitled10.png](/assets/article_images/2020-09-21/Untitled10.png)

Window 와 Page가 있다.  User Control 이야 당연히 무엇인지 대충 감은 오니깐 빼고.

> **Window** is the root control that must be used to hold/host other controls (e.g. Button) as container. **Page** is a control which can be hosted in other container controls like NavigationWindow or Frame. Page control has its own goal to serve like other controls (e.g. Button). Page is to create browser like applications.

Link 나 Navigation 에서 이용하려면 Page 를 쓰는 것으로 이해하면 되겠다.  다 예전 설명이라서 지금이랑은 먼가 다른 듯 싶다.

### Page1.xaml

```csharp
<Page
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <StackPanel Background="LightBlue">
    <TextBlock Margin="10,10,10,10">Start Page</TextBlock>	
    <TextBlock  HorizontalAlignment="Left" Margin="10,10,10,10">
      <Hyperlink  NavigateUri="Page2.xaml">Go To Page 2</Hyperlink>
    </TextBlock>
  </StackPanel>
</Page>
```

### Page2.xaml

```csharp
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <StackPanel Background="LightGreen">
    <TextBlock DockPanel.Dock="Top" Margin="10,10,10,10">Page 2</TextBlock>
    <TextBlock  HorizontalAlignment="Left" Margin="10,10,10,10">
      <Hyperlink  NavigateUri="Page1.xaml">Go To The Start Page</Hyperlink>
    </TextBlock>
  </StackPanel>
</Page>
```

이해되는 수준은 여기까지 이고, 이 다음부터는 복잡도가 너무 갑자기 올라가서 완전 당황 중...

![/assets/article_images/2020-09-21/Untitled11.png](/assets/article_images/2020-09-21/Untitled11.png)