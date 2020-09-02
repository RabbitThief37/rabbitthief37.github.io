---
layout: post
title:  "STM32 Discovery Day Online Track 2020 Day 2"
description: "STM32 Discovery Day Online Track 2020 둘째날 세미나"
author: RabbitThief
date: 2020-09-03 00:50:00 +0900
tags: seminar stm32 mcu 
category: Seminar
comments: false
published: true
---



## [Microsoft IoT solutions overview - 강희권](https://rabbitthief37.github.io/assets/article_images/2020-09-02/(Day2-1)STM32MP1_Microprocessor.pdf)


![/assets/article_images/2020-09-02/_2020-09-03__12.12.02.png](/assets/article_images/2020-09-02/1.png)

- 별도의 OS에서 동작하지만 통신 경로는 연결되어 있음

    ![/assets/article_images/2020-09-02/_2020-09-03__12.15.20.png](/assets/article_images/2020-09-02/2.png)

- 따로 국밥인거 같은 느낌인데.

    ![/assets/article_images/2020-09-02/_2020-09-03__12.17.56.png](/assets/article_images/2020-09-02/3.png)

- Basic boot chain

    ![/assets/article_images/2020-09-02/_2020-09-03__12.18.48.png](/assets/article_images/2020-09-02/4.png)

- 개발이 쉽지 않고 많은 부분을 고려해야 하는 구조다.  사용할려면 공부 빡시게 하지 않으면 자원 낭비각.

    ![/assets/article_images/2020-09-02/_2020-09-03__12.21.06.png](/assets/article_images/2020-09-02/5.png)

- 옆에 있던 OS팀에서 했던 작업이 Distribution Package의 위치로군.

    ![/assets/article_images/2020-09-02/_2020-09-03__12.23.03.png](/assets/article_images/2020-09-02/6.png)

- 그래서 얼만데?


## [STM32CubeMonitor를 이용한 런타임 변수 모니터링 - 김희수](https://rabbitthief37.github.io/assets/article_images/2020-09-02/(Day2-2)STM32_CubeMonitor.pdf)


## [iOS 기기를 통한 도어락 컨트롤 - 양태영](https://rabbitthief37.github.io/assets/article_images/2020-09-02/(Day2-3)NFC_solutions_ST25_product_portfolio_for_Door-Lock.pdf)


- 사전 제작인데 음질이 개판. ㅡ.ㅡ;

![/assets/article_images/2020-09-02/_2020-09-03__12.31.44.png](/assets/article_images/2020-09-02/7.png)

![/assets/article_images/2020-09-02/_2020-09-03__12.37.11.png](/assets/article_images/2020-09-02/8.png)

- 집중해서 듣지 못했다. 하지만 별루였던 발표인건 알겠다.

## Q & A

- **Linux 로 MP1 이 동작되면 Wifi, BLE 를 USB로 연결하여 동작 가능한가요?**

    해당 주변장치를 USB로 연결하시면 동작 가능합니다만, 드라이버 및 USB Class는 직접 포팅하여야 합니다.

- **RETRAM이 어떤것인지요?**

    RETRAM은 STM32MP1의 SRAM중 하나로 인터럽트 벡터와 배터리 장착시 전원 OFF상태에서도 일부의 데이터를 유지하는데 사용됩니다.

- **리눅스상의 어플과 MCU 상의 기능을 어떤 식으로 개발해야 하는지 궁금합니다. 단순히 리눅스 어플리케이션 코딩중에 M4 코어로 지정을 하는것인지 아니면 별도 F/W를 개발하여 각각 바이너리를 별도 탑재해야하는것인지요?**

    리눅스 User application은 기존 cross compiler (예: GCC)등으로 컴파일된 바이너리를 실행하는 것으로 개발되며, M4코어는 .elf로 생성된 파일을 리눅스의 file system에 올려 리눅스에 의해 실행되는 방식으로 개발됩니다.

- **그러면 M4 코어를 이용한 MCU 단의 F/W 개발은 기존처럼 CUBEIDE를 활용하면 되는 것인가요?**

    네 기존 STM32 개발환경과 같은 환경을 사용하시면 됩니다.

- **TF-A도 무료로 제공이 되는 것인가요?**

    ARM사에서 제공되는 TF-A는 무료로 제공되며 OpenSTLinux에 포함되어 있습니다.

- **리눅스 시스템의 경우, 항상 부팅 시간이 오래걸린다는 단점이 있는데요. 이를 해결할 수 있는 방안이 조금 있는지 궁금합니다.**

    각 부트단계를 최적화한 방법을 제공하는 아래 URL을 참조하시기 바랍니다. [https://wiki.st.com/stm32mpu/wiki/How_to_optimize_the_boot_time](https://wiki.st.com/stm32mpu/wiki/How_to_optimize_the_boot_time)

- **직접 사용해 보려면 가장 쉬은 방법이 STM32MP157C-DK2 을 구입하는건가요?**

    네 MP1 Discovery board 를 사용하시면 빠르게 적용해 보실수 있습니다.

- **TF-A 관련 기술문서 링크 공유 좀 부탁드리겠습니다**

    [https://wiki.st.com/stm32mpu/wiki/TF-A_overview](https://wiki.st.com/stm32mpu/wiki/TF-A_overview) 를 참조하세요.

- **더불어 M4의 디버깅은 STLink 을 그대로 사용하여 할수 있나요?**

    네, ST link로 가능합니다.

- **M4 코어가 elf 파일로 리눅스 파일시스템에 별도로 파일형태로 올라간다고 했는데 그럼 elf 파일을 리눅스 부팅시에 M4의 sram에 올라가서 실행되는 형태인가요?**

    리눅스 부팅시에 올릴 수도 있고 부팅완료시점에 올릴 수도 있습니다.

- **무조건 Yocto 방식으로 진행할 수 밖에 없나요? 과거에 방식처럼, 개발 환경을 구축할 수 있을까요? (시간이 너무 오래 걸려서 초기에 구성이 많이 힘듭니다.)**

    Developer package를 이용하시면 됩니다. [https://wiki.st.com/stm32mpu/wiki/STM32MP1_Developer_Package](https://wiki.st.com/stm32mpu/wiki/STM32MP1_Developer_Package)

- **AI 딥러닝 적용 예제가 있나요?**

    네 ST홈페이지를 방문하시면 X-LINUX-AI-CV 라는 MP1에 적용가능한 AI 예제가 있습니다. 다운받으셔서 적용해보시면 됩니다.

- **실시간 영상전송을 위한 MCU 솔루션은 어떤게 있을까요?**

    현재 솔루션으로 제공되는 예제는 아쉽게도 없습니다. 보통 흔히 리눅스에서 사용하는 TCP/IP 방식은 속도가 느리지만 사용가능할 것으로 보입니다. 비디오 라이브러리는 Gstream도 포팅되어 있습니다. [https://wiki.st.com/stm32mpu/wiki/GStreamer_overview](https://wiki.st.com/stm32mpu/wiki/GStreamer_overview)

- **데모에서 보여주셨던 LCD 컨트롤 UI는 어떠걸 사용하는건가요? 리눅스 베이스의 UI인가요>? 아니면 TOUCH FX 를 사용해서 구동되는 건가요?**

    데모의 UI 는 QT 기반으로 되어 있습니다. 언급하신 TouchGFX는 현재 STM32MP1 을 지원하지 않습니다.

- **특정 변수에 값을 모니터링 할때의 Access 주기를 설정 할 수 있나요? 없다면 주기가 어떻게 되나요?**

    네 Variables node에서 Sampling frequency를 변경하여 주기 설정을 하실 수 있습니다.

- **probe 디바이스는 st-link만 되는지요?**

    네, Probe node에서 선택하실 수 있습니다.

- **SWO 연결 없이도 모니터링이 가능한가요?**

    네, PC의 IP 주소를 가지고 웹 브라우저로 호환하여 Remote access가 가능합니다.

- **레지스터 값들도 확인할수 있나요?**

    Peripheral 레지스터는 가능하나, Core 레지스터는 불가 합니다.

- **CUBE MONITOR는 디버깅용 펌웨어를 따로 개발하지 않아도 펌웨어 내부에 있는 변수를 사용해서 모니터가 된다는 의미인가요?**

    네 맞습니다. MCU 펌웨어의 변수 모니터링이 가능합니다.

- **큐브모니터는 별도의 하드웨어 구성없이 사용가능한가요?**

    네 별도의 하드웨어 구성은 필요로 하지 않습니다.

- **SWD 핀만으로도 모니터 가능한가요?**

    네 SWD 핀으로 사용이 가능합니다.

- **stm32h743 에 keil 5 컴파일러를 사용하는데 elf 값만 있으면 stm32cubemonitor를 사용가능한가요?**

    네 elf 파일을 적용하면 사용이 가능합니다.

- **샘플링 속도를 최대로해도 MCU의 퍼포먼스에는 영향이 전혀없나요?**

    네 Variables node의 Sampling frequency에서 Sequential loop로 설정하면 가장 빠른 주파수 설정이 됩니다.

- **CUBE MONITOR 에서 다른 외부 프로그램으로 측정값을 넘겨 줄 수 있나요?**

    네 STM32CubeMonitor에서 Export 기능을 이용하여 Library로 만들 수 있습니다.

- **STANDARD LIBRARY로 개발된 펌웨어도 모니터가 가능한가요?**

    네 가능합니다.

- **breakpoint 활용하는건가요? 값을 불러올때마다 mcu가 아주잠시 멈추는건가요?**

    MCU는 동작을 멈추지 않고 디버거 모듈을 통해 ST-Link에서 데이터를 불러갑니다.

- **큐브모니터에서 현재 구동중인 펌웨어를 Pause 시키고 다시 시작할고 리셋할수 있나요?**

    Dashboard에서 Acquisition중일때 Pause 시키시는 걸 의미하신거라면, Clear 버튼 클릭 후 다시 시작 하실 수 있지만, 리셋을 하는 기능은 없습니다.

- **elf를 사용하는거 봐선 MCU에서 debug 모드로 수행이 된 상태에서만 가능한건가요?**

    ELF 파일을 불러와서 지정해주셔야 합니다. 하지만 MCU는 Debuf 모드가 종료 된 상태에서 진행 되어야 합니다.

- **그럼 큐브IDE 에서 디버깅중에 큐브모니터를 실행시켜 값들을 모니터 가능한가요?**

    디버깅을 끝내고, 디버깅 모드가 종료된 상태에서 프로그램 사용을 하시면 됩니다.

- **컴파일러에서 Release mode 둔 상태에서 build한 파일을 사용 중인 MCU에서도 사용이 가능한지 궁금합니다**

    네 Release mode로 빌드후에 elf파일을 적용하시면 됩니다.

- **CubeMonitor - Power는 MCU에서만 소비되는 전류를 모니터링 가능한 것인가요?**

    소비전류는 Power monitor 툴을 사용하시면 됩니다. [https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-performance-and-debuggers/stm32cubemonpwr.html](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-performance-and-debuggers/stm32cubemonpwr.html)

- **인터넷으로 원격 구동도 가능한가요?**

    STM32CubeMonitor는 Local access모드와 Remote access모드가 있습니다. Local access의 경우에는 STM32와 호스트 PC를 연결하여 사용하실 수 있고, Remote access의 경우에는 호스트 PC의 IP주소를 가지는 웹브라우저로 호환하여 사용하실 수 있습니다.

- **STM32CubeMonitor Power 통해 소모전류측정시 실제 측정 오차(uA)는 어느 정도 되나요?**

    ST의 Discovery 보드를 사용했을때 2%정도의 오차를 가지고 있습니다.

- **ULINK2로도 probe가 가능하지요?**

    ULINK는 지원하지 않습니다. ST Link로만 가능합니다.

- **시리얼통신이 포트가 최대 존재하는 MCU는 어떤게 있나요?**

    STM32F091이 가장 많은 8개 UART 를 지원합니다.

- **Cubemonitor 와 ST Link 사용시 MCU의 SWO 핀도 연결을 해줘야 하나요? 아니면 2wire만 연결 해줘야 하나요?**

    별도 연결은 필요하지 않습니다.

- **NFC 관련 패턴 안테나 설계(?)에 대해서 레퍼런스가 있는지요?**

    네, ST에서 reference 문서를 제공해 드립니다.

- **제가 가지고 있는 테스트 보드가 Open 407-C / ST-Link/V2 를 가지고 있습니다. 이 환경하에서 CubeMonitor 테스트가 가능할까요?**

    네 가능합니다. ST의 모든 MCU는 STM32CubeMonitor에서 테스트가 가능하기 때문입니다.