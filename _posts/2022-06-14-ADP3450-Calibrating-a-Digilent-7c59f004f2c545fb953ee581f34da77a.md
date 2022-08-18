---
layout: post
title: "ADP3450 - Calibrating a Digilent Test and Measurement Device"
description: "ADP3450 - Calibrating a Digilent Test and Measurement Device 번역"
author: RabbitThief
date: 2022-06-14 18:00:00 +0900
tags: adp3450 digilent calibration translation 
category: adp3450
comments: false
---	




# ADP3450 - Calibrating a Digilent Test and Measurement Device

[Calibrating a Digilent Test and Measurement Device](https://digilent.com/reference/test-and-measurement/guides/waveforms-calibration)

모든 Digilent 테스트 및 측정 장치는 공장에서 보정되지만 가장 정확한 측정을 수행할 수 있도록 다시 보정할 수 있습니다.

준비물

- A Digilent Test and Measurement device to be calibrated.
- A Mac, Windows, or Linux computer with [WaveForms](https://digilent.com/reference/software/waveforms/waveforms-3/start) installed
- A reliable [Digital Multimeter (DMM)](https://digilent.com/shop/ms8217-autorange-digital-multimeter/)
- DMM probes with grabber ends
- A [Breadboard](https://digilent.com/shop/solderless-breadboard-kit-small/)
- [Male pin headers](https://digilent.com/shop/6-pin-header-gender-changer-5-pack/)

참고: 모든 Digilent 테스트 및 측정 장치는 공장에서 보정되고 사용자가 WaveForms의 보정 마법사에서 보정할 수 있지만 Digilent는 사내 또는 타사 보정 서비스를 제공하지 않습니다.

****1. Device Manager****

WaveForms 창 상단 바의 설정 메뉴에서 장치 관리자를 클릭하여 장치 관리자 창을 엽니다. 장치 관리자 창이 열리면 장치 관리자의 모든 기능을 탐색할 수 있습니다. 창의 위쪽 절반에 있는 테이블에는 WaveForms와 함께 사용할 수 있는 장치가 포함되어 있습니다. 장치가 연결되어 있으면 여기에 표시됩니다. 장치 관리자에 표시되도록 장치를 연결합니다. 연결된 유일한 장치인 경우 일련 번호가 있는 테이블의 유일한 항목이 됩니다.

![/assets/article_images/2022-06-14/Untitled](/assets/article_images/2022-06-14/Untitled.png)

****2. Rename Tool****

탐색할 첫 번째 도구는 이름 바꾸기 도구입니다. 장치 관리자에서 화면 왼쪽 상단의 "이름 바꾸기"를 클릭합니다. 여기에서 장치의 새 이름을 입력할 수 있습니다. 대문자 또는 소문자 영숫자를 사용할 수 있습니다.

![Untitled](/assets/article_images/2022-06-14/Untitled1.png)

예를 들어 "MyDevice"라는 이름을 지정했습니다. 이름 바꾸기 도구는 장치가 두 개 이상이거나 여러 장치가 있는 대규모 그룹에서 공동 작업하는 경우에 유용할 수 있습니다.

****3. Calibration Window****

다음으로 캡처된 데이터에 대해 가능한 최고의 정확도를 보장하기 위해 보정 기능을 살펴보겠습니다. 

장치 관리자 창에서 "보정"을 클릭합니다. 그러면 장치 보정 창이 열립니다. 보정 창에서 볼 수 있는 첫 번째 탭은 "보정" 탭입니다. 여기에서 보정할 수 있는 모든 도구를 볼 수 있습니다. 기존 캘리브레이션 파일을 저장하거나 불러올 수 있는 "파일" 메뉴와 나중에 설명할 "재설정" 메뉴도 있습니다. 매개변수 및 참조 탭도 볼 수 있습니다. 기존 보정을 정의하는 값입니다. 이 값을 수동으로 변경하지 않는 것이 좋습니다. 대신 Calibration Wizard를 사용하여 전체 교정을 수행합니다. 

참고: 오른쪽 스크린샷과 같이 수행할 수 있는 보정 목록이 모든 장치를 대표하는 것은 아닙니다.

![Untitled](/assets/article_images/2022-06-14/Untitled2.png)

****4. Device Calibration Wizard****

장치 보정 마법사를 열려면 장치 보정 창 상단의 "마법사"를 클릭하십시오. 마법사가 열리면 오른쪽 그림과 같이 장치를 보정하는 데 필요한 항목을 알려주는 시작 화면이 나타납니다. 다음을 클릭합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled3.png)

****5. Waveform Generator Linearity****

참고: 이 보정은 일부 장치에서만 사용할 수 있습니다. 보정 마법사에 이 표시되지 않으면 6단계로 건너뜁니다. 

일부 장치에서 사용할 수 있는 이 보정은 전적으로 장치 자체 내에서 발생하며 외부 연결이나 측정을 수행할 필요가 없습니다. 다음을 클릭하여 수행하고 계속하십시오.

![Untitled](/assets/article_images/2022-06-14/Untitled4.png)

****6. Calibrate Waveform Generator 1****

보정해야 할 첫 번째 작업은 DC의 Wavegen 채널 1입니다. 이렇게 하려면 Wavegen 채널 1 핀을 디지털 멀티미터에 연결합니다. 

장치의 Wavegen 채널 1 핀을 멀티미터의 양극에 연결합니다. MTE 케이블이 있는 장치의 경우 이것은 노란색 와이어입니다. BNC 케이블이 있는 장치의 경우 BNC 미니그래버 케이블 세트의 빨간색 면이 됩니다. 

멀티미터의 음극을 장치의 접지에 연결합니다. MTE 케이블이 있는 장치의 경우 검정색 와이어가 됩니다. BNC 케이블이 있는 장치의 경우 BNC 미니그래버 케이블 세트의 검은색 면이 됩니다.

![Untitled](/assets/article_images/2022-06-14/Untitled5.png)

멀티미터의 값을 읽습니다. 보정 마법사에 표시된 대로 약 0V여야 합니다. 멀티미터에서 읽은 값을 보정 마법사에 입력하고 다음을 클릭합니다. 교정 마법사가 파형 발생기 2(총 10개 측정)로 이동하도록 표시할 때까지 이 프로세스를 반복합니다. 

참고: "측정됨" 필드의 기본값은 측정의 예상 결과를 나타냅니다. 

참고: Analog Discovery Pro(ADP3450/ADP3250) 파형 발생기는 고/저 이득 대신 고/저 범위를 기반으로 보정됩니다. 교정은 다른 장치와 동일한 방식으로 수행되며 장치는 설명된 대로 연결해야 합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled6.png)

****7. Calibrate Waveform Generator 2****

파형 발생기 1 대신 파형 발생기 2를 사용하여 이전과 동일한 회로를 만듭니다. 이렇게 하려면 파형 발생기 아날로그 출력 1 핀을 파형 발생기 아날로그 출력 2 핀으로 교체합니다. DMM과 장치 접지 사이의 기존 연결을 유지하십시오. MTE 케이블이 있는 장치를 사용하는 경우 새 연결은 흰색 줄무늬가 있는 노란색 와이어에 연결됩니다.

![Untitled](/assets/article_images/2022-06-14/Untitled7.png)

DMM의 값을 읽으십시오. 약 0V여야 합니다. 이 값을 보정 마법사에 입력하고 다음을 누르십시오. 보정 마법사가 오실로스코프 보정(총 10개 측정)으로 이동하도록 표시할 때까지 이 절차를 반복합니다.

****8. Oscilloscope****

교정 프로세스의 다음 단계는 오실로스코프를 영점화하는 것입니다. 

보정 창에 표시된 대로 각 오실로스코프 채널을 접지에 연결합니다. 모든 채널이 동시에 보정됩니다. 즉, 장치가 BNC 케이블을 사용하는 경우 각 채널에 대해 BNC 오실로스코프 프로브가 필요합니다. 예를 들어, MTE 케이블이 있는 2채널 장치를 사용하는 경우 채널 1의 양극 끝(주황색 케이블), 채널 2의 양극 끝(파란색 케이블), 채널 1의 음극 끝(흰색 줄무늬가 있는 주황색 케이블), 음극을 연결합니다. 채널 2의 끝(흰색 줄무늬가 있는 파란색 케이블) 및 접지(검정색 케이블). BNC 케이블을 사용하는 경우 각 BNC 프로브의 감쇠를 1X로 설정합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled8.png)

다음을 클릭하여 보정을 시작합니다. 오실로스코프 영점 보정은 자동이므로 완료될 때까지 기다리기만 하면 됩니다(몇 분 정도 소요될 수 있음). 진행률 표시줄은 이 자동 프로세스가 얼마나 완료되었는지 나타냅니다. 완료되면 다음을 눌러 다음 단계로 진행합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled9.png)

다음으로, 자동으로 가져온 판독값을 파형 발생기에서 출력된 알려진 전압과 비교하여 오실로스코프를 보정합니다. 

첫번째 단계는 회로를 설정하는 것입니다. 각 오실로스코프 채널의 양극 끝을 Wavegen 채널 1에 연결하고 (채널이 차동인 경우) 오실로스코프의 두 채널의 음극 끝을 접지에 연결합니다. 아날로그 입력용 MTE 케이블은 파란색과 주황색으로 표시되며 음극 끝에 흰색 줄무늬가 있습니다.

![Untitled](/assets/article_images/2022-06-14/Untitled10.png)

****9. Digital Supply****

참고: 장치에 디지털 전원이 없으면 10단계로 건너뛰십시오. 

디지털 전원을 보정하려면 신뢰할 수 있는 DMM을 사용하여 전원 핀과 접지 사이의 전압을 측정해야 합니다.

마법사가 공급 핀에 1.2V 및 3.2V 신호를 적용할 때 측정을 수행해야 합니다. 이러한 측정값을 텍스트 필드에 입력하고 다음을 클릭하여 저장하고 계속합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled11.png)

![Untitled](/assets/article_images/2022-06-14/Untitled12.png)

****12. Frequency Compensation****

참고: 이 보정은 일부 장치에서만 사용할 수 있습니다. 

보정 마법사에 이 나타나지 않으면 13단계로 건너뜁니다. 주파수 보정은 완전히 자동으로 발생합니다. 마법사에 설명된 대로 모든 스코프 입력을 분리하거나 접지하고 다음을 눌러 교정 절차를 실행합니다.

![Untitled](/assets/article_images/2022-06-14/Untitled13.png)

****13. Done Calibrating!****

****14. Apply the Changes****

![Untitled](/assets/article_images/2022-06-14/Untitled14.png)

****15. Compare Accuracy****

0V DC에 대해 구성된 Wavegen 채널 1이 오실로스코프 채널 1에 연결되도록 장치를 연결했습니다. 아래 이미지에서 보정이 있거나 없는 0V 값의 캡처를 볼 수 있습니다. 보정을 통해 영점이 더 정확하다는 것을 알 수 있습니다.

![Untitled](/assets/article_images/2022-06-14/Untitled15.png)

![Untitled](/assets/article_images/2022-06-14/Untitled16.png)

****16. If Necessary, Reset Calibration****

장치 보정이 만족스럽지 않은 경우 보정을 공장 설정으로 재설정하거나 장치 보정 창의 재설정 메뉴를 사용하여 모든 값을 0으로 재설정할 수 있습니다.