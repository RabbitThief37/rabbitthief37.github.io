---
layout: post
title:  "STM32 Discovery Day Online Track 2020 Day 3"
description: "STM32 Discovery Day Online Track 2020 세째날 세미나"
author: RabbitThief
date: 2020-09-03 21:45:00 +0900
tags: seminar stm32 mcu 
category: Seminar
comments: false
published: true
---



# 하...할말이 없다. 아까운 내 시간... 지금까지 이런 웹비나는 없었다.



## [Certified IC solution for security, STSAFETM Family & Ecosystem - 조경원](https://rabbitthief37.github.io/assets/article_images/2020-09-03/(Day3-1)Certified_IC_solution_for_security_STSAFE(tm)_Family_Ecosystem.pdf)



## [AWS IoT Services running on STM32 - 이세현](https://rabbitthief37.github.io/assets/article_images/2020-09-03/(Day3-2)AWS_IoT_Services_running_on_STM32.pdf)



## [iOS 기기를 통한 도어락 컨트롤 - 양태영](https://rabbitthief37.github.io/assets/article_images/2020-09-03/(Day3-3)_STM32_기반_IoT_모듈.pdf)



## Q & A

* 현재 L475-IoT 보드를 가지고 있는데, 현재 진행하는 예제를 접목해 볼 수 있을까요?**

  네 Arduino 확장팩 연결이 가능하기 때문에 실습이 가능합니다.

* 스마트 가로등을 전원 제어가 아닌 On/Off제어가 되어야 하지 않은가요?

  네, 스마트 가로등 데모는 가로등 제어에 대한 인증 use case 를 소개하기 위해 준비한 것으로 On/Off 제어로도 가능합니다.

* STSAFE 인증 시스템을 구현하려면 인증된 ST칩외에 어떤것이 필요한가요?

  STSAFE 제품별로 이에 맞는 Host platform 이 필요합니다. 손쉽게 검증하기 위해서는 STSAFE-A Host 로 STM32L4, STSAFE-TPM Host 로는 라즈베리파이 보드 또는 STM32MP1 을 권장합니다.

* STPM4RasPI 활용 예제 사이트가 있으면 공유 부타 드립니다. 

  https://www.st.com/resource/en/data_brief/stpm4raspi.pdf 에 Linux kernel 포팅 예제 및 기본 테스트 예제가 포함되어 있습니다. 추가로 Host 용 SW stack (TSS 및 Test tool) 이 필요할 경우, https://github.com/tpm2-software 에서 다운로드 가능합니다.

* 전체적인 윤곽 및 서비스 이용에 대하서는 AWS 사이트에서 습득해야 하는가요? 혹시 별도 제공 자료가 있는지요?

  AWS에 연락해 주시면 AWS IoT 서비스에 대한 소개 및 교육 등의 지원이 가능합니다. 감사합니다.

* AWS IoT Greengrass의 기능 중, AWS 서비스에 빠르게 연결해주는 타이밍은 어떻게 제어하나요?

  AWS IoT Greengrass와 AWS cloud상의 IoT Core라는 서비스와의 통신은 MQTT로 하실 수 있습니다. 타이밍 등 필요하신 비즈니스 로직은 직접 코딩 가능하시고 AWS lambda라는 AWS 서비스를 통해서 배포 가능하십니다.

* ST 마이크로콘트롤러에 ALEXA 구현하는 업데이트된 자료가 있나요, 2018년에 나온 자료만 본적이 있어서요

  문의하신 ST의 ALEXA service 자료는 아래의 링크에서 확인해보시기 바랍니다. https://www.st.com/en/embedded-software/x-cube-vs4a.html

* 디바이스의 OS 업데이트를 위한 FOTA 기술지원은 어떻게 지원 되는지 궁금합니다. 참고 할 수 있는 자료 링크 부탁 드립니다.

  하기 링크를 참고하실 수 있을것으로 보입니다. https://docs.aws.amazon.com/freertos/latest/userguide/freertos-ota-dev.html https://www.st.com/en/embedded-software/x-cube-aws.html

* SDT에서 제공하는 솔루션에서 네트워크 연결에 대한 보장을 한다고 하는데, 예로 TCP/IP Stack 같은 경우, 어떤걸 주로 이용하고 있는지요?

  lwIP 사용하고 있습니다.

* 하나의 단말 장치가 요구하는 전원 스펙은 어떻게 되나요?

  최종 제품 사용 목적에 따라 전원 스펙이 달라집니다. 다만 컴퓨팅 수준과 통신 거리 등에 비례합니다. 즉, 저희 Cortex-M0 모듈에 비해 Cortex-A7 모듈에서 사용되는 전력이 훨씬 더 높습니다.

* SDT에서 생각하는 IoT적용에 있어, 문제를 해결하는 비용측면에서 IoT가 가성비가 있다고 생각하시는지요? 의견을 듣고 싶습니다.

  IoT 디바이스를 개발하는 것만을 생각할 때는 가성비가 낮을 수 있습니다. 하지만 SDT는 IoT 디바이스와 다양한 솔루션들을 통해 전체 문제를 해결하는 것을 도와드리기 때문에 고객의 입장에서 ROI를 높이는데 도움을 드린다고 말씀드릴 수 있습니다.