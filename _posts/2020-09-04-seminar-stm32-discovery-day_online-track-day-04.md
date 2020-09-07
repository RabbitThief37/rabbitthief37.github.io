---
layout: post
title:  "STM32 Discovery Day Online Track 2020 Day 4"
description: "STM32 Discovery Day Online Track 2020 네째날 세미나"
author: RabbitThief
date: 2020-09-07 23:30:00 +0900
tags: seminar stm32 mcu 
category: Seminar
comments: false
published: true
---


- # **9월 4일**

    ## [TouchGFX, STM32 전용 무료 그래픽 솔루션 - 김두형](https://rabbitthief37.github.io/assets/article_images/2020-09-04/(Day4-1)TouchGFX.pdf)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled.png](/assets/article_images/2020-09-04/Untitled.png)

    - Save 되는 메모리의 크기가 생각보다 크다.

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%201.png](/assets/article_images/2020-09-04/Untitled_1.png)

    - H/W 가속이 꽤 매력있다.

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%202.png](/assets/article_images/2020-09-04/Untitled_2.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%203.png](/assets/article_images/2020-09-04/Untitled_3.png)

    - 아직까지 Max 1074 * 768 이군.  임베이드니깐.

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%204.png](/assets/article_images/2020-09-04/Untitled_4.png)

    - 재미 있는 기능

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%205.png](/assets/article_images/2020-09-04/Untitled_5.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%206.png](/assets/article_images/2020-09-04/Untitled_6.png)

    ## [STM32Cube.AI](http://stm32cube.ai/), [Neural Networks on STM32 - 문현수](https://rabbitthief37.github.io/assets/article_images/2020-09-04/(Day4-2)STM32CubeAI_Neural_Networks_on_STM32.pdf)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%207.png](/assets/article_images/2020-09-04/Untitled_7.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%208.png](/assets/article_images/2020-09-04/Untitled_8.png)

    - 머신러닝의 모든 것은 데이터 수집 및 가공

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%209.png](/assets/article_images/2020-09-04/Untitled_9.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%2010.png](/assets/article_images/2020-09-04/Untitled_10.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%2011.png](/assets/article_images/2020-09-04/Untitled_11.png)

    - 용량 줄이기는 좋은 기능이기는 하나, 아직까지는 속도와 크기가 깡패인 시즌이라

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%2012.png](/assets/article_images/2020-09-04/Untitled_12.png)

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%2013.png](/assets/article_images/2020-09-04/Untitled_13.png)

    - 너가 적용할 프로젝트에서는 절대 이렇게 안 나옴

    ![STM32%20Discovery%20Day%20Online%20Track%202020%20be08e47a70b64201b718043bb29145f3/Untitled%2014.png](/assets/article_images/2020-09-04/Untitled_14.png)

    - 깔끔한 정리 페이지. 인정!

    ## Q & A

    - **COLOR 포멧이 4bit 인경우 흑백 그래이스케일 만 지원되는되는 데 3bit color 4bit color 포멧을 사용할 수있는 방법이 있는지요**

        4bit color 포멧을 지원하지만 3 bit color 의 경우 LCD spec 과 사용 인터페이스에 따라 달라질 수 있습니다.

    - **4bit, 8bit 컬러모드에서는 흑백 그레이 모드로 동작하는데 3비트 4비트 에서도( 8컬러,16컬러) 컬러를 사용 할수는 없는지요**

        LCD의 spec 에 따라 LCD가 gray 모드만 지원하는 경우 color 모드가 지원되지는 않습니다.

    - **4비트인경우 그레이스케일만 지원 하는것 같습니다.**

        LCD가 4bit gray 의 경우 color 로 format converting 되지는 않습니다.

    - **CubeIDE에서 LTDC 부터 DMA2D FreeRTOS 및 TouchGFX를 설정하는 메뉴얼 같은게 있나요?**

        [https://support.touchgfx.com/docs/introduction/welcome/](https://support.touchgfx.com/docs/introduction/welcome/) 위 링크에서 touchGFX 사용 방법에 대해 확인 하실 수 있습니다.

    - **다국어 지원 방법에 대해 자세히 나와 있는 문서가 있을까요?**

        [https://support.touchgfx.com/docs/introduction/welcome/](https://support.touchgfx.com/docs/introduction/welcome/) 위 링크에서 다국어 지원에 대한 내용을 찾으실 수 있습니다.

    - **toughGFX관련 user교육도 있었으면 좋겠습니다.**

        네, 매년 2차례 정도 실습을 포함한 세미나를 진행 하고 있습니다.[http://www.st.com/stm32korea](http://www.st.com/stm32korea) 에서 세미나에 대한 일정을 확인 하실 수 있습니다.

    - **lcd는 컬러입니다. 하지만 gfx에서는 4비트 컬러모드로 하면 흑백으로 시뮬레이션이되고 gfx 자료에서도 4bit 컬러모드인경우 그레이스케일 모드로 된다고 되어 있습니다.**

        TouchGFX 에서는 4 bit gray 모드로 지원합니다. 다만 DMA2D의 4bit color 모드를 확인해볼 필요가 있을 듯합니다.

    - **Touch GFX Simulator 에서 실행한 화면과 실제 board 에서 실행한 화면에서 image 위치는 문제 없지만 string 의 경우 위치가 달라지는 경우가 있는데 혹시 특정 font 의 문제일까요?**

        해당 문제는 LCD 또는 DMA2D offset 을 확인해볼 필요가 있을 듯 합니다.

    - **외장메모리 없이 이미지를 데이터화하여 내부 메모리에 저장하고 display하는것도 가능한가요?**

        내부 메모리에 여우가 충분한 MCU의 경우 가능합니다.

    - **touchgfx가 적용된 상용제품 많이 있나요? 상용제품에 적용할 경우 무료로 사용 가능한가요**

        네, 모든 STM32시리즈에서 무료로 사용할 수 있습니다.

    - **touchGFX에서 사용되는 폰트는 윈도우에서 지원하는 폰트를 그대로 사용할수 있는지요?**

        네, 윈도우 폰트를 그대로 사용할 수 있지만 제품 상용화때 해당 폰트의 라이센스 정책을 확인해볼 필요가 있습니다. 윈도우 폰트라도 라이센스를 가지는 경우가 있습니다.

    - **STM32CubeIDE update 시 ioc file 이 호환되지 않는 경우가 있는데 해결 방법이 있을까요?**

        STM32CubeMX 최신버전에서 ioc 파일을 Migration을 진행한뒤에 테스트 해보시길 바랍니다. CubeMX 와 CubeIDE를 최신버전으로 둘다 업데이트 하시는것을 권장합니다.

    - **그래픽데이터를 외부 QSPI Flash에 저장하는데 디버그확인시 매번 해당 메모리에 데이터를 로드하는 시긴이 길게 걸리는데 초기에 한번만 로드하는 방법이 없는지요?**

        외부메모리 접근을 번지로 접근하는 memory mapped 를 사용하시거나, 외부에 SDRAM, SRAM을 달아 미리 이미지를 덤프하는 방법이 있습니다.

    - **벽걸이형 디지털 시계를 Touch GFX 솔루션으로 개발해보려 합니다. 램누수 걱정 없을까요?**

        네, C 사용 시 heap leakage 에 대한 예외 처리를 고려하여 개발할 필요가 있습니다.

    - **cache 및 MMU 설정 관련 자세한 자료는 어디서 확인할수 있을까요?**

        STM32H, F7 시리즈의 L1 캐시는 아래 URL을 참조하세요. STM32 MCU에는 MMU가 없습니다. [https://www.st.com/resource/en/application_note/dm00272913-level-1-cache-on-stm32f7-series-and-stm32h7-series-stmicroelectronics.pdf](https://www.st.com/resource/en/application_note/dm00272913-level-1-cache-on-stm32f7-series-and-stm32h7-series-stmicroelectronics.pdf)

    - **LDTC설정이 매우 중요한데 각 조건에 맞는 환경설정에 대한 설명이 부족합니다.**

        아래의 링크는 LTDC 어플리케이션 노트입니다. 자세한 사항은 아래의 문서를 참고해주시기 바랍니다. [https://www.st.com/content/ccc/resource/technical/document/application_note/group0/25/ca/f9/b4/ae/fc/4e/1e/DM00287603/files/DM00287603.pdf/jcr:content/translations/en.DM00287603.pdf](https://www.st.com/content/ccc/resource/technical/document/application_note/group0/25/ca/f9/b4/ae/fc/4e/1e/DM00287603/files/DM00287603.pdf/jcr:content/translations/en.DM00287603.pdf)

    - **프레임 버퍼와 실행 코드를 SDRAM에 적재 시키면 MCU 동작 스피드가 느려지나요?**

        SDRAM 은 XIP (execute in place) 를 지원하지 않기 때문에 SDRAM 에 실행코드를 올려놓고 SDRAM 에서 직접 코드 실행은 불가능합니다.

    - **TouchGFX는 PPT 9페이지에 표기된 MCU외에 추가적으로 지원 MCU 확인 유무는 어떻게 확인 할 수 있는지요?**

        TouchGFX 는 STM32의 모든 제품을 지원합니다. LCD의 인터페이스가 어떤 것을 사용하는지에 따라 STM32 MCU를 선정하시면 됩니다.

    - **CubeIDE에서 LTDC설정 중 Layer 1개만 이용하는데도, 자꾸만 초기화 코드에는 layer 2개를 생성하면서 오류를 유발하네요.**

        해당 문제를 저도 확인한 문제 입니다. 해당 문제가 해결 될 수 있도록 해당 부서에 문의하겠습니다.

    - **다국어 Font를 지원한다는데 라이센스는 무료인가요?그리고 다국어 Font가 내장되어있다면 기본 라이브러리가 커질것같은데 기본 필요한 램이나 ROM 용량은 얼마나 되나요?**

        폰트는 윈도우에 내장되거나 윈도우에 추가한 custom 폰트를 사용합니다. 해당 폰트들에 대한 라이센스는 사용자가 확인해야합니다. 다국어 폰트가 기본 내장은 아닙니다. 다국어 폰트를 TouchGFX 에 적용하여 사용할 수 있다는 의미로 이해하시면 될 것같습니다.

    - **세미나 영상에서 사용하는 샘플코드들을 함께 배포하시면 도움이 될 것같은데요. 이번 온라인세미나에서는 실습이 처음 나오는 듯한데 코드는 함께 오픈하지 않으시나봐요.**

        TouchGFXDesigner 에는 STM32CubeMX 프로젝트를 포함하는 다양한 예제를 받아서 확인해보실 수 있습니다.

    - **그래프 위젯 추가 계획은 없나요?**

        TouchGFXDesigner 에 circular, bar 그래프 위젯을 포함하고 있습니다.

    - **stm32l4r9i_eval 보드를 이용해여 LCD 480 272에 touchgfx를 사용하여 코드를 작성하였는데 처음 10초정도는 touchgfx에서 그린 UI가 나오지만 이후 화면이 이상하게 나옵니다. 프레임버퍼의 메모리를 보면 데이터는 바뀌지 않는데 LCD설정 문제일까요?**

        여러 경우가 있을 수 있습니다. LCD 에서 지원하는 clock 을 MCU에서 생성하고 있는지, LTDC 에서 overrun error 가 발생하는 것은 아닌지 등 여러가지 가능성이 있습니다. 대리점 또는 ST 한국 사이트에서 Q&A를 해보시기 바랍니다.

    - **io expander mfxstm32l152의 데이터시트가 없던데 내부 i2c커멘드에 대한 정보를 얻을 수 있는 방법은 없을까요? 보드의 메뉴얼에는 i2c를 이용하여 통신한다고만 나오고 다른정보는 나오지 않습니다.**

        별도의 메뉴얼은 없으며, 해당 보드의 예제파일을 확인하시면 command 가 표시되어 있으니 참조하세요.

    - **이렇게 통합해서 쓰다면 Compile툴은IAR하나만 사용할수있느건지요?**

        IAR과 STM32CubeIDE 도 가능합니다.

    - **STM32H7B3 Discovery kit 를 가지고 있는데, 여기도 적용이 가능한가요?**

        네, 해당 보드에서도 지원가능합니다. 특히 STM32H7B3 칩은 내장 SRAM size가 매우 크게 들어있어 외부메모리 없이 프레임버퍼를 내부메모리에서 구동할 수 있습니다.

    - **TouchGFX에서는 화면 전환에 대해서 LCD에 touch 기능이 있어야만 동작하도록 되어 있는데요, 외부 스위치와 연동할 수 없는지요?**

        TouchGFX는 GUI 프레임워크 입니다 LCD touch를 적용하지 않더라도 외부 하드웨어 버튼, 조그 셔틀 과 같은 외부 입력과 연동될 수 있습니다.

    - **TouchGFX가 RTOS기반이면 기존의 C로만 프로젝트를 구현한던 개발자는 RTOS와 C++에 대한 어느정도의 지식이 필요한가요?**

        코드를 확인하고 이해하는 수준이면 됩니다. 기존 코드 기반으로 새로 코드를 만들기 위해서는 코드의 동작 및 문법 이해도가 수반되기 때문입니다.

    - **무료제공하는 그래픽에 문자 또는 숫자를 수정 할 수 있나요?**

        Desinger 에서 제공하는 기본위젯에 string 을 적용할 수 있으나 기본 위젯 자체를 수정하기 위해서는 TouchGFX framework 수정이 필요합니다. 결과적으로는 불가능하지는 않습니다.

    - **TouchGFX를 구돋하는데 있어 STM32F M0+ 칩으로도 구동이 가능한 것으로 이해가 되는데요.**

        네 TouchGFX는 STM32 MCU 전 제품을 지원합니다. 다만, LCD의 사용 인터페이스에 따라 드라이버 코드 구현 작업이 필요합니다.

    - **STM3224x9I-EVL 보드를 가지고 있는데 여기에도 TouchGFX 적용 가능한가요?**

        네 가능합니다.

    - **5단계 설명에서 3 -> NN을 MCU에 최적화된 코드로 만드는데는 컨버팅 수준인가요? 여기에 시간이 많이 소요되지는 않는지요?**

        네 컨버팅과정을 거칩니다. 시간은 Nueural Network크기에 따라 소요시간이 달라집니다.

    - **OpenMV에 이미 Cube.AI가 적용되어 있다는 것인가요?**

        STM32 MCU를 사용한 HW 보드에 CubeAI가 적용되어 있고 OpenMV 환경에 적용하여 테스트 하실 수 있습니다.

    - **칩 내부적으로 반드시 필요로 하는 부분은 어떤 부분인가요?**

        Display에 따라 다릅니다. 먼저 Display에서 지원하는 I/F와 이에 맞는 MCU를 선택하는 것이 좋습니다. 보통 Display 은 FMC, DSI, SPI 등을 사용합니다.

    - **예를 들어, Swipe Container를 이용하여 화면 전환을 구현했는데, 외부 스위치와 연동해서 화면 전환을 하고 싶습니다.**

        Model 에서 외부 입력을 Tick 주기에 맞춰 message 로 받아서 presenter->View 로 message를 전달하여 swipe container 의 Page 를 전환하는 방법으로 가능합니다.

    - **MEMS 가속도/자이로/마그네트 센서를 이용한 AI 적용을 해 보고 싶은데, 이 때 학습을 어떻게 해야 하는지에 대한 정보가 필요합니다.**

        STM32CubeAI 예제에는 tensorflow 기반으로 딥러닝 학습이 가능한 python code를 제공하고 있습니다. 참조 부탁드립니다.

    - **TouchGFX를 사용하기 위해 H/W 설계시 LCD I/F mapping 시 주의해야 할 사항들이 있다면 무엇이 있을까요?**

        LCD 의 spec 그리고 interface 에 따라 h/w 구성이 달라질 수 있습니다. TouchGFX는 SPI, MIPI-DSI, RGB parallel 인터페이스를 지원하기 때문에 해당 인터페이스에 맞는 LCD를 선정해야합니다.

    - **vision example 이미지 클래스 관련해서, 카메라와 연결하여 실시간 영상을 stm32h7에서 처리할 수 있나요?**

        네 STM32H7의 DCMI 인터페이스를 사용하여 이미지센서의 데이터를 실시간으로 처리 할수 있으며 자세한 사항안 st홈페이지에서 fp-ai-vision예제를 참조 하시면 됩니다.

    - **STM32769G-EVAL 에서 DSI 방식이 아닌 DPI 방식으로 사용하는 예제는 없나요?**

        STM32769G-EVAL 에는 DSI 인터페이스 LCD가 연결되어 있어서 DPI 방식으로 동작되지 않을 뿐만 아니라 예제를 제공하지 않습니다.

    - **H750 에서 SDRAM 에 프레임 버퍼와 실행 코드를 적재할 경우 실행 속도에 영향이 없을까요? 코드실행이 많이 느려지지는 않을까요?**

        FMC 의 SDRAM 에서 코드 실행 (XIP) 이 가능한 것으로 확인하였습니다. 잘못된 답변을 정정 드립니다. TouchGFX 가 프레임 버퍼로 그리기 동작을 자주 하고 LTDC 에서 읽어서 화면에 출력하는 오버헤드도 많기 때문에 코드도 SDRAM 에 놓을 경우 병목 현상이 발생하기 때문에 코드는 SDRAM 이 아닌 내부 플래시 메모리나 다른 외부 메모리를 권장합니다

    - **CUBE.AI를 사용하기 위한 최소 MCU Spec이 있는지요? 가령 간단한 On/Off 음성인식을 할경우.**

        사용하시려는 Network의 사이즈에 따라 Flash/Ram Size 등이 고려되어야 합니다. 이 부분의 경우 저희 CubeMX에서 적용하시려는 Neural Network에 대한 분석이 가능하며 가능한 MCU를 추천해 줍니다.

    - [**STM32Cube.AI](http://stm32cube.ai/) 는 Linux 플랫폼 에서만 사용이 가능한가요?**

        아닙니다. STM32CubeMX 안에 Plug-in 형태로 포함되어 있기때문에 윈도우즈 환경에서도 사용이 가능합니다.

    - **양자화 툴 사이트 알 수 있을까요?**

        [STM32Cuba.AI](http://stm32cuba.ai/) 툴에도 양자화 진행툴이 포함되어 있습니다. 그이외에는 범용 딥러닝 머신에서 해당 기능을 찾아 보시면 됩니다. ( tensorflow .. )

    - **STM32769G-EVAL 회로에서 DPI 방식의 커넥터가 존재 하는데 사용하지 못하는건가요?**

        STM32769G-EVAL에 포함되어 있는 LCD의 인터페이스가 DSI 인터페이스 LCD 입니다. 가령 DPI LCD를 별도로 연결하여 사용하는 경우에는 구현 가능합니다. 다면 예제를 지원하지는 않습니다.