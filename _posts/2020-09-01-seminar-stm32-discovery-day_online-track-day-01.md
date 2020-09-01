---
layout: post
title:  "STM32 Discovery Day Online Track 2020 Day 1"
description: "STM32 Discovery Day Online Track 2020 첫째 날 세미나"
author: RabbitThief
date: 2020-09-01 23:23:00 +0900
tags: seminar stm32 mcu 
category: Seminar
comments: false
published: true
---



## [STM32 2020 라인업 및 STM32 Discovery Day Online Track 2020 행사 소개 - 최경화](https://rabbitthief37.github.io/assets/article_images/2020-09-01/(Day1-1)STM32_introduction.pdf)

![_2020-09-01__6.00.46](/assets/article_images/2020-09-01/_2020-09-01__6.00.46.png)

* 이것 한장으로 요약 가능
* 발표는 어색함의 극치



## [STM32WL, 세계 최초의 LoRa 원-칩 솔루션 - 이준호](https://rabbitthief37.github.io/assets/article_images/2020-09-01/(Day1-2)STM32WL_series_MCU_long_range_wireless_system_on_chip.pdf)

![2-1](/assets/article_images/2020-09-01/2-1.png)



![2-2](/assets/article_images/2020-09-01/2-2.png)

![2-3](/assets/article_images/2020-09-01/2-3.png)

* sigfox는 아직 개발 중...

![2-4](/assets/article_images/2020-09-01/2-4.png)

* 솔직히 회사 안 망하고 어느 정도 매출이 있다면, 이 동네는 계속 유지하는게 답.
* 반은 졸아서 내용이 잘 생각 나지는 않고, 아래 Q&A 질문에 대부분의 내용이 있다.



## [STM32 2020 라인업 및 STM32 Discovery Day Online Track 2020 행사 소개 - 최경화](https://rabbitthief37.github.io/assets/article_images/2020-09-01/(Day1-3)Microsoft_Azure_IoT_solutions.pdf)

![3-1](/assets/article_images/2020-09-01/3-1.png)

* 서비스 개~많음 ㅜㅜ

![3-2](/assets/article_images/2020-09-01/3-2.png)

* 전체 흐름도는 이것이 다 설명하고 있음

![3-3](/assets/article_images/2020-09-01/3-3.png)

*  IoT Hub에서 지원하는 프로토콜이 아닌 경우에는 할일이 많이 늘어날 것 같다.

![3-4](/assets/article_images/2020-09-01/3-4.png)

* MS는 언제 ThreadX 인수 한겨? 모르고 있다고 깜놀했음

![3-5](/assets/article_images/2020-09-01/3-5.png)

* MS만큼 깔끔하게 샘플을 준비하는 곳은 없더라... 갓엠에스



## Q & A

- **F4시리즈와 G4시리즈의 차이점은 무엇인가요?**

    동일한 Cortex-M4 코어를 사용하며 SPI, I2C 등에서 업그레이드된 IP 를 내장하고 있습니다.
    추가로 FMAC (하드웨어 필터) 와 CORIC (수학연산) 을 하드웨어로 지원하는 IP 등이 추가 되었습니다

    STM32G0는 STM32F0 차세대 제품으로 좀더 미세 공정을 채택했고, CPU performance도 개선된 제품입니다. G는 F다음의 제품이라고 보시면 됩니다.

- **무선 프로토콜 스택은 Source Code 형태로 제공이 되는지요? 특히 Private Network 구성을 위해서 관련 기능까지 포함되어 있는지요?(Gateway)**

    Lora 는 무선통신 이름이며 LoraWAN 은 소프트웨어 스택을 의미합니다. 소스코드 형태로 제공되며 LoraWAN 은 엔드노드와 게이트웨이 사이의 프로토콜이 구현되어 있습니다

- **NXP MCU 중에 clock이 800Mhz 까지 올린 넘이 있던데 ST에서는 이런 계열은 안만들까요? 로직 없이 1D CCD 같이 타이밍 제어을 빨리 해야 하는 프로젝트 들이 있어서요**

    아직 STM32 의 가장 높은 동작 주파수를 가지는 계열은 STM32H7 이며 최대 550 MHz 입니다. 추후에 더 높은 동작 주파수를 가지는 MCU를 계획하고 있으나 현재까지는 STM32H7 이 가장 높을 동작 주파수를 가지고 있습니다. 또한 TIM 의 입력 clock 또한 고려되어야합니다.

- **stm32wl 로 제품을 개발하면 전파인증을 별도로 받아야 하나요?**

    모듈인증과 세트인증등으로 구분할수 있는데 만드시는 제품이 모듈이면 모듈인증 세트면 세트인증을 받아야 합니다

- **RF 출력단에 삽입되는 LC 매칭에 대한 기술적 자료가 제공되나요?**

    AN5407 어플리케이션 노트 문서에서 관련한 설명을 찾아보실수 있습니다

- **시간성이 필요한 기능은 함께 내장된 M4 코어를 사용해서 처리가 가능합니다. =>S/W적 처리에 대한 질의 사항이였습니다. 별도의 API를 제공하는지 아니면 코어를 지정하도록 하는 옵션을 사용하는지 등등에 대한 자세한 내용이 궁급합니다**

    M4코어에서 돌아가는 별도의 FW를 작성하여 리눅스와 IPCC (Inter-Processor Communication Controller) 통해서 상호 전달 할 수 있습니다.

- **PCB패턴 or 칩안테나 형태의 구성으로도 Node간 500m 이내에서 사용이 가능할지 궁금합니다. (공장/건물 사이 환경**)

    안테나는 다양한 형태로 구성할수 있습니다. LoRa 는 최소 수 미터의 이격 거리에서 RF 성능이 보장됩니다

- **시연에서 소개하는 보드에서 보면, Gateway/Node의 보드(H/W)가 다른가요? STM32WL을 가지고 Gateway 구현도 가능한 것인지요?**

    시연은 STM32WL뉴크레오보드를 Node 로 사용하고, STM32F7 과 Lora 모듈을 gateway 로 사용하였습니다. STM32WL 로도 gateway 구현은 가능하지만 gateway 는 여러개의 Node 가 동시에 다른 채널로 보내는 데이터를 동시에 수신하고 디코딩하는 고성능이 필요합니다

- **nucleo gateway 보드를 Loriot이 아닌 semtech 클라우드로 접속할려고 하는데 어떻게 해야하나요?**

    Gateway 와 네트워크 서버 사이의 프로토콜은 LoRa 에서 표준으로 지정하지 않기 때문에 네트워크 서버 제조사마다 고유의 프로토콜이 있습니다 (UDP, MQTT 등). 따라서 semtech 클라우드 서버에서 지원하는 프로토콜 포팅이 필요합니다

- **프로토콜 포팅이라고 하면 Gateway을 변경해야하는거죠?**

    semtech 서버와 통신하는 메시지 프로토콜에 대해 구체적인 정보가 없지만 일반적으로 외부에 공개해서 노드를 만들수 있도록 메시지 API 와 예제코드가 제공됩니다. 또한 semtech 회사에서 예제코드를 실제 MCU 기반이나 AP 기반의 보드에 포팅해놓은 사례가 있는지 또는 이미 구현해서 판매하는 사용 gateway 제품이 있는지 등도 찾아보아야 합니다

- **STM32MP1에 Azure RTOS 또는 Windows IoT 접목이 가능한지 궁금하네요.**

    Azure RTOS 는 접목이 가능합니다만, Windows IoT 는 아직 지원되지 않습니다.

- **Azure RTOS의 경우, 무료로 사용이 가능한 것인지요? (Azure IoT와 연동하지 않고)**

    개발 시에는 무료로 사용이 가능하고 양산 시는 많이 사용되는 STM32 시리즈에서 무료로 사용이 가능합니다.(ST에 문의) 아래 링크에서 소스코드 및 예제 확인이 가능합니다. [https://github.com/azure-rtos](https://github.com/azure-rtos)

- **Azure RTOS의 fileX같은것도 무료로 소스코드로 제공되나요?**

    아래의 GitHub 링크에서 fileX 소스코드 확인이 가능합니다.개발용도로는 무료이고 양산 시 MCU 제조사와 확인이 필요합니다. [https://github.com/azure-rtos/filex](https://github.com/azure-rtos/filex)

- **Embedded C SDK의 경우, ST에서 별도로 진행하는 부분인가요? STM32 전용인 것인지요?**

    Azure IoT Embedded C SDK 는 Microsoft 에서 구현합니다. STM32 뿐 아니라 어느 MCU에도 포팅이 가능합니다.

- **Azure RTOS의 기본 코드용량은 어느정도인가요?**

    커널만 적용할 경우 최소 2KB 정도입니다.

- **Azure RTOS 는 상업용으로 사용할시 유료이네요.**

    많이 사용되는 MCU 에서는 무료로 사용할 수 있도록 지원될 예정입니다.