---
layout: post
title: "AN986: BLUETOOTH® A2DP AND AVRCP PROFILES GETTING STARTED GUIDE - 1"
description: "SILICON LABS BLUETOOTH® A2DP AND AVRCP PROFILES 번역"
author: RabbitThief
date: 2022-05-30 18:00:00 +0900
tags: bluetooth a2dp avrcp siliconlabs translatoin 
category: bluetooth
comments: false
---	




# BLUETOOTH® A2DP AND AVRCP PROFILES - 1

GETTING STARTED GUIDE - SILICON LABS

## Introduction

이 애플리케이션 노트에서는 Bluetooth A2DP(Advanced Audio Distribution Profile)와 Bluetooth AVRCP(Audio/Video Remote Control Profile)의 장점과 이러한 프로필을 사용하는 방법에 대해 설명합니다. 또한 A2DP 및 AVRCP 프로파일이 iWRAP 펌웨어와 함께 사용되는 방법에 대한 실제 예가 제공됩니다.

### Advanced Audio Distribution Profile

A2DP는 스테레오 품질의 오디오를 미디어 소스에서 싱크로 스트리밍하는 방법을 설명합니다. 오디오 소스는 음악 플레이어이고 오디오 싱크는 무선 헤드셋 또는 무선 스테레오 스피커입니다.
프로필은 오디오 장치의 두 가지 역할인 소스와 싱크를 정의합니다.

- A2DP Source – 피코넷의 SINK로 전달되는 디지털 오디오 스트림의 소스 역할을 하는 장치는 소스입니다.
- A2DP Sink – 동일한 피코넷의 SOURCE에서 전달되는 디지털 오디오 스트림의 싱크 역할을 하는 장치는 싱크입니다.

A2DP는 ACL 채널에서 모노 또는 스테레오로 고품질 오디오 콘텐츠 배포를 실현하는 프로토콜 및 절차를 정의합니다. 따라서 "고급 오디오"라는 용어는 베이스밴드 사양에 정의된 대로 SCO 채널에서 협대역 음성 분포를 나타내는 "블루투스 오디오"와 구별되어야 합니다.
A2DP 프로파일은 저복잡도 서브밴드 코덱(SBC)에 대한 필수 지원을 포함하고 선택적으로 MPEG-1,2 오디오, MPEG-2,4 AAC, ATRAC 또는 기타 코덱을 지원합니다.
제한된 대역폭을 효율적으로 사용할 수 있도록 오디오 데이터를 적절한 형식으로 압축합니다. 서라운드 사운드 분포는 이 프로필의 범위에 포함되지 않습니다.

### Audio/Video Remote Control Profile

AVRCP는 사용자가 액세스할 수 있는 모든 A/V 장비를 단일 리모컨(또는 기타 장치)으로 제어할 수 있도록 TV, Hi-Fi 장비 또는 기타를 제어하는 표준 인터페이스를 제공하도록 설계되었습니다. A2DP 또는 VDP와 함께 사용할 수 있습니다.
기본적으로 당신의 행동은 컨트롤을 조작합니다. TV의 밝기나 색조, VCR 타이머 등 이미 많이 사용되는 메뉴 기능과 소리 조정, 재생, 일시 정지, 건너뛰기 등의 오디오 기능을 조정할 수 있습니다.
AVRCP는 컨트롤러와 대상 장치의 두 가지 역할을 정의합니다.

- Controller - 컨트롤러는 일반적으로 원격 제어 장치로 간주됩니다.
- Target - 대상 장치는 특성이 변경되는 장치입니다.

"워크맨" 유형의 미디어 플레이어 시나리오에서 제어 장치는 트랙을 건너뛸 수 있는 헤드셋일 수 있으며 대상 장치는 실제 미디어 플레이어가 됩니다.
AVRCP에서 컨트롤러는 감지된 사용자 동작을 A/V 제어 신호로 변환한 다음 원격 Bluetooth 지원 장치로 전송합니다. 기존 적외선 리모컨에서 제공되던 기능을 이 프로필에서 구현할 수 있습니다. AVRCP 브라우징을 사용하면 미디어 라이브러리 인식 플레이어에서 미디어 콘텐츠를 검색할 수 있습니다.

## Using A2DP and AVRCP with iWRAP

이 장에서는 iWRAP 펌웨어를 사용한 A2DP 및 AVRCP 사용 및 구성에 대해 설명합니다.

### Configuring A2DP

A2DP 프로필에는 싱크인지 소스인지에 상관없이 단 하나의 구성만 있습니다. Sink는 오디오를 수신 및 디코딩하고 모듈의 오디오 인터페이스 중 하나를 통해 렌더링합니다. 소스는 일반적으로 ADC 또는 디지털 오디오 인터페이스에서 수신되는 오디오를 인코딩하여 Bluetooth 링크를 통해 보냅니다.
iWRAP를 싱크로 구성하려면:

- SET PROFILE A2DP SINK

iWRAP를 소스로 구성하려면:

- SET PROFILE A2DP SOURCE

### Configuring AVRCP

AVRCP 프로필에는 첫 번째 장에서 설명한 대로 컨트롤러와 대상이라는 두 가지 역할이 있습니다. 컨트롤러는 항상 모든 명령을 시작하고 대상은 응답합니다. Target은 일부 변수가 Target에서 변경될 때 컨트롤러에 알림 이벤트를 보낼 수 있지만 컨트롤러가 이전에 수신하도록 등록한 이벤트에 대해서만 가능합니다.
이 두 가지 역할 외에도 AVRCP는 네 가지 범주의 A/V 제어 명령을 정의합니다. 컨트롤러 역할 지원은 명령 전송 지원을 의미합니다. Target 역할에서는 수신 지원을 의미합니다.
iWRAP6에서 SET PROFILE AVRCP 설정에는 추가 필드(이전 버전의 iWRAP와 비교하여) 지원 범주가 있습니다. 각각의 필수 AVRCP 명령과 함께 4개의 고유한 범주의 장치:

- Category 1: Player/Recorder - must support PLAY and PAUSE
- Category 2: Monitor/Amplifier - must support VOLUP and VOLDN (volume up and down)
- Category 3: Tuner - must support CHUP and CHDN (channel up and down)
- Category 4: Menu - must support MSELECT, MUP, MDN, MLEFT, MRIGHT (menu select and
directions)

어떤 경우에는 두 장치가 모두 컨트롤러 및 대상으로 작동할 수도 있습니다. iWRAP는 SDP 레코드에서 둘 이상의 AVRCP 역할 인스턴스를 브로드캐스트할 수 있습니다. 동시에 대상과 컨트롤러가 될 수 있습니다. 또한 iWRAP는 대상으로 구성된 경우에도 컨트롤러 명령을 성공적으로 보낼 수 있습니다.

예를 들어, iWRAP가 절대 볼륨 제어를 지원하는 A2DP 싱크 + AVRCP 대상으로 구성되고 iOS 장치가 A2DP 소스로 작동하는 경우 iOS 장치는 미디어 플레이어의 볼륨 슬라이더로 iWRAP의 볼륨을 제어하고 여전히 AVRCP 명령을 수락합니다. iWRAP에서 재생, 일시 중지 및 중지와 같은. 이 시나리오에서 iWRAP은 출력 게인을 로컬로 조정할 수 있는 증폭기 또는 헤드셋으로 생각할 수 있지만 재생, 일시 중지 등의 명령을 보내기 위한 패널이나 버튼도 있습니다. **iOS 장치는 볼륨 레벨을 최대로 조정하고(즉, 디지털 감쇠 없음) A2DP 싱크가 게인을 로컬로 조정하도록 합니다. 디지털 게인이 오디오 신호를 약간 저하시키므로 최적의 음질을 얻을 수 있습니다.** iWrap 장치가 볼륨 레벨과 재생 상태(재생, 일시 중지 등)를 제어할 수 있는 경우 AVRCP 대상 및 컨트롤러 역할을 모두 구성하는 것이 좋습니다.

### Example configurations

**A2DP Sink and AVRCP Controller**

A2DP 및 AVRCP의 가장 일반적인 사용 사례는 재생, 일시 중지 및 볼륨 제어와 같은 간단한 원격 제어 기능을 갖춘 무선 헤드셋일 것입니다. A2DP 싱크는 오디오 소스에서 오디오 스트리밍을 가능하게 하는 반면 AVRCP 컨트롤러는 오디오 스트림의 무선 제어를 용이하게 합니다.
A2DP 싱크는 iWRAP 명령으로 활성화됩니다.

- SET PROFILE A2DP SINK

플레이어/앰프 애플리케이션 제어를 지원하는 AVRCP 컨트롤러는 다음 명령으로 활성화됩니다.

- SET PROFILE AVRCP CONTROLLER 3

휴대폰, 태블릿 및 PC에서 장치를 무선 헤드셋으로 인식하려면 장치의 기능을 반영하도록 Bluetooth CoD(Class of Device)를 구성해야 합니다. 일부 장치는 CoD가 올바르게 설정되어 있지 않으면 A2DP를 검색 및 연결하지 못할 수 있습니다.
A2DP 사양은 A2DP 싱크에 대해 렌더링 서비스 비트(0x040000)가 설정되도록 지정합니다. 또한 오디오 서비스 비트(0x200000)를 설정하고 주요 장치 클래스를 오디오/비디오(0x000400)로 설정하는 것이 좋습니다.
부 장치 클래스는 문제의 장치 유형에 따라 설정해야 합니다(예: 헤드폰(0x000018) 및 Hi-Fi 오디오 장치(0x000028)). 결합된 CoD는 0x240438입니다.
장치 클래스는 iWRAP 명령으로 구성됩니다.

- SET BT CLASS 240438

마지막으로 프로필을 활성화하려면 을(를) 재설정해야 합니다.

```bash
SET PROFILE A2DP SINK
SET PROFILE AVRCP CONTROLLER 3
SET BT CLASS 240438
RESET
```

**A2DP Source and AVRCP Target**

또 다른 매우 일반적인 시나리오는 홈 스테레오 시스템과 같이 원격으로 제어할 수 있는 오디오 소스입니다.
이전 예에서와 같이 프로파일을 활성화하려면 "SET PROFILE A2DP SOURCE" 및 "SET PROFILE AVRCP TARGET 7"을 발행해야 합니다. 이 경우 카테고리 1~3이 지원됩니다. 라디오 튜너도 스테레오 시스템에 포함되어 있습니다.
앞의 예와 같이 Class of Device를 올바르게 설정해야 합니다. A2DP 소스의 경우 사양은 캡처 서비스 비트(0x080000) 사용을 의무화합니다. 오디오 서비스(0x200000) 및 오디오/비디오 주요 장치 클래스(0x000400)를 사용해야 하지만 필수 사항은 아닙니다.
일반적인 예로는 보조 장치 클래스가 Hi-Fi 오디오 장치(0x000028)입니다. 결합된 CoD는 0x280428입니다.

마지막으로 A2DP 프로필이 활성화되려면 재설정이 필요합니다.

```bash
SET PROFILE A2DP SOURCE
SET PROFILE AVRCP TARGET 7
SET BT CLASS 280428
RESET
```

**A2DP Sink and AVRCP Target**

이 경우 iWRAP은 오디오를 출력하는 장치이며 볼륨 조절 기능이 있으며 로컬에서 오디오 게인을 조정할 수 있지만 재생, 일시 중지 또는 기타 제어 버튼이 있는 패널은 없습니다. 예를 들어 전화기에서 오디오를 수신하고 전화기에서 오디오 스트림을 완전히 제어하는 스피커 시스템이 있습니다.

```bash
SET PROFILE A2DP SINK
SET PROFILE AVRCP TARGET 2
SET BT CLASS 240428
RESET
```

**A2DP Sink and AVRCP Target and Controller**

이 경우 iWRAP은 오디오를 출력하는 장치이며 볼륨 조절이 가능하며 로컬에서 오디오 게인을 조정할 수 있지만 재생, 일시 중지 또는 기타 제어 버튼이 있는 패널도 있습니다. 예를 들어 전화기에서 오디오를 수신하는 스피커 시스템이 있으며 두 장치 모두 오디오 스트림을 완전히 제어할 수 있습니다.

```bash
SET PROFILE A2DP SINK
SET PROFILE AVRCP CONTROLLER 3 TARGET 2
SET BT CLASS 240428
RESET
```

**Security configuration**

다른 Bluetooth 지원 장치와 페어링하려면 Bluetooth 보안을 올바르게 구성해야 합니다. iWRAP는 Bluetooth 2.1 + EDR 사양에 정의된 SSP(Secure Simple Pairing)를 지원하며 이를 사용하는 것은 필수이지만 기존 장치와 페어링이 가능하도록 PIN 코드 페어링도 지원합니다.
SSP 및 PIN 코드 페어링을 활성화하려면 다음 구성 명령이 필요합니다.

- SET BT SSP 3 0 - 이렇게 하면 PIN 코드 입력이나 암호 키 확인이 필요하지 않은 SSP "그냥 작동" 모드가 활성화됩니다. 중간자 보호(man-in-the-middle protection) 옵션을 활성화하려면 iWRAP 사용 설명서를 참조하십시오.
- SET BT AUTH * <pin> - 이 명령은 PIN 코드 페어링을 활성화하여 레거시 정의와의 페어링을 지원합니다. <pin>은 원하는 PIN 코드이며 1-16자의 영숫자 문자일 수 있습니다.

마지막으로 보안 설정 프로필이 활성화되려면 재설정이 필요합니다.

다음은 iWRAP에서 SSP가 작동하고 PIN 코드 페어링을 활성화하는 방법의 예입니다.

```bash
SET BT SSP 3 0
SET BT AUTH * 0000
RESET
```

### Service discovery

Bluetooth 기술은 무선 서비스 검색을 가능하게 하므로 원격 장치가 지원하는 기능을 찾을 수 있습니다. 무선 서비스 검색은 Bluetooth SDP(서비스 검색 프로필)를 사용합니다.
iWRAP를 사용하면 서비스 검색이 "SDP {bd_addr} {uuid}" 명령으로 수행됩니다.

- bd_addr - 원격 장치의 Bluetooth 장치 주소입니다.
- uuid - 보편적으로 고유한 식별자입니다. 찾고자 하는 블루투스 프로파일을 말합니다. A2DP 싱크의 경우 uuid는 110B이고 A2DP 소스의 경우 AVRCP 타겟(110C) 및 AVRCP 컨트롤러(110E)의 경우 110A입니다.

다음은 A2DP 싱크에 대한 서비스 검색을 수행하는 방법의 예입니다.

```bash
SDP 00:07:80:81:66:6f 110B
SDP 00:07:80:81:66:6f < I SERVICENAME S "Stereo Headset" > < I PROTOCOLDESCRIPTORLIST < <
U L2CAP I 19 > < U 0019 I 100 > > >
SDP
```

- Stereo Headset = Service Name
- 19 = L2CAP psm for A2DP sink

다음은 A2DP 소스에 대한 서비스 검색을 수행하는 방법의 예입니다.

```bash
SDP 00:07:80:93:0c:aa 110A
SDP 00:07:80:93:0c:aa < I SERVICENAME S "Stereo Audio" > < I PROTOCOLDESCRIPTORLIST < < U
L2CAP I 19 > < U 0019 I 100 > > >
SDP
```

다음은 AVRCP 컨트롤러에 대한 서비스 검색을 수행하는 방법의 예입니다.

```bash
SDP 00:07:80:93:0c:aa 110C
SDP 00:07:80:81:66:6f < I SERVICENAME S "A/V Controller" > < I PROTOCOLDESCRIPTORLIST < < U
L2CAP I 17 > < U 0017 I 104 > > >
SDP
```

- A/V Controller = Service name
- 17 = L2CAP psm for A2DP source

다음은 AVRCP 대상에 대한 서비스 검색을 수행하는 방법의 예입니다.

```bash
SDP 00:07:80:93:0c:aa 110E
SDP 00:07:80:93:0c:aa < I SERVICENAME S "A/V Target" > < I PROTOCOLDESCRIPTORLIST < < U
L2CAP I 17 > < U 0017 I 103 > > >
SDP
```

### Connection establishment

**A2DP Connection**

A2DP 프로필에는 두 개의 연결이 필요합니다. 하나는 제어 채널용이고 다른 하나는 데이터 채널용입니다. 그러나 iWRAP는 자동으로 두 연결을 모두 설정하므로 사용자는 두 채널 설정에 대해 걱정할 필요가 없습니다.
A2DP 연결은 CALL 명령으로 iWRAP에 일반적으로 열립니다.

```bash
“CALL {bd_addr} 19 A2DP”
```

- bd_addr - 원격 장치의 Bluetooth 장치 주소입니다.

다음은 A2DP 연결을 설정하는 방법의 예입니다.

```bash
CALL 00:07:80:81:66:6f 19 A2DP
CALL 0
CONNECT 0 A2DP 19
CONNECT 1 A2DP 19
A2DP STREAMING START 0
```

발신 전화 및 성공적인 연결의 일반적인 표시가 수신됩니다(CALL 및 CONNECT). 제어 및 데이터 채널의 설정을 나타내는 두 개의 연결 이벤트가 일반적으로 수신됩니다.
또한 일반적으로 A2DP 스트리밍이 시작되었음을 나타내는 세 번째 이벤트가 수신됩니다.

```bash
“A2DP STREAMING START {link_id}”
```

- link_id - 스트리밍을 시작한 숫자 연결 식별자입니다.

(주의) iWRAP를 사용하면 여러 개의 동시 A2DP 연결을 설정할 수 있습니다. 그러나 한 번에 하나의 연결만 오디오를 스트리밍할 수 있습니다.

**AVRCP connection**

CALL 명령을 사용하여 iWRAP에 일반적으로 AVRCP 연결이 열립니다.

```bash
“CALL {bd_addr} 17 AVRCP”
```

- bd_addr - 원격 장치의 Bluetooth 장치 주소입니다.

다음은 AVRCP 연결을 설정하는 방법의 예입니다.

```bash
CALL 00:07:80:81:66:6f 17 AVRCP
CALL 2
CONNECT 2 AVRCP 17
```

발신 호출 및 연결 성공에 대한 일반적인 표시(CALL 및 CONNECT)가 수신되고 연결 설정이 성공하지 못한 경우 CALL 및 NO CARRIER 이벤트가 수신됩니다.
컨트롤러가 브라우징을 지원하기 때문에 일부 장치는 iWRAP에서 요청하지 않더라도 브라우징 채널을 자동으로 연결할 수 있습니다. 탐색 기능이 필요하지 않은 경우 탐색 채널을 무시하거나 닫을 수 있습니다.

### Connection termination

**A2DP connection**

A2DP 연결은 iWRAP 명령 "CLOSE {link_id}"로 간단히 닫힙니다. 항상 첫 번째 A2DP 연결(제어 채널)을 종료해야 하며 두 번째 연결(데이터 채널)은 종료하지 않아야 합니다. 제어 채널을 종료하면 iWRAP도 데이터 채널을 종료합니다.
다음은 A2DP 연결 종료의 예입니다.

```bash
CLOSE 0
A2DP STREAMING STOP 0
NO CARRIER 0 ERROR 0
NO CARRIER 1 ERROR 0
```

일반적으로 A2DP 스트리밍 중지 표시가 먼저 수신됩니다.

```bash
“A2DP STREAMING STOP {link_id}”
```

- link_id - 스트리밍을 시작한 링크의 숫자 연결 식별자입니다.

**AVRCP connection**

AVRCP 연결은 iWRAP 명령 "CLOSE {link_id}"로 간단히 닫힙니다. 브라우징 연결이 열려 있으면 iWRAP는 자동으로 먼저 브라우징 연결을 닫은 다음 기본 AVRCP 연결을 닫습니다.
다음은 AVRCP 연결 종료의 예입니다.

```bash
CLOSE 2
NO CARRIER 2 ERROR 0
```

그러면 정상적인 NO CARRIER 이벤트가 수신됩니다.

## AVRCP notifications and absolute volume control

iWRAP5부터 컨트롤러는 알림을 지원합니다. iWRAP6부터 컨트롤러와 타겟 모두 알림과 절대적인 볼륨 제어를 지원합니다.

### Notifications general concepts

AVRCP에서 컨트롤러는 항상 모든 트랜잭션을 시작하고 대상은 응답합니다. Target의 내부 이벤트 또는 사용자 인터페이스로 인해 발생하는 이벤트의 경우 Target은 알림을 보낼 수 있습니다. 컨트롤러는 초기에 알림을 요청해야 하며 내부 이벤트 또는 사용자 상호 작용으로 인해 대상에서 변경 사항이 있는 경우 컨트롤러에 알림을 보냅니다.
**컨트롤러는 먼저 대상이 지원하는 이벤트의 종류를 물어봐야 합니다.** 그런 다음 지원되는 이벤트 중 하나 이상에 대한 알림을 수신하도록 등록할 수 있습니다.

### Absolute volume control general concepts

AVRCP 사양 버전 1.4에서는 절대 볼륨의 개념이 도입되었습니다. 대상이 이미 최대 또는 최소 볼륨 수준에 있는지 여부를 고려할 수 없는 상대적 볼륨 명령인 볼륨 증가 및 볼륨 감소 대신 이제 대상 장치의 볼륨 수준을 백분율로 설정하고 읽을 수도 있습니다. 알림과 절대 볼륨 제어는 다음 하위 섹션에서 설명하는 것처럼 함께 작동합니다.

### Notifications and absolute volume control workflow

절대 볼륨 제어를 위해 사양에서는 A2DP 싱크가 AVRCP 대상이라고 가정합니다. 두 iWRAP 사이. 하나는 Sink+Target으로, 다른 하나는 Source+Controller로 구성된 경우 교환은 다음과 같이 보일 수 있습니다.

```bash
장치 A, 소스 + 컨트롤러, 지원되는 이벤트 0x03을 요청하는 Get Capabilities PDU 0x10을 연결하고 보냅니다.

CALL 00:07:80:bb:bb:bb 17 AVRCP
CALL 0
CONNECT 0 AVRCP 17
CALL 00:07:80:bb:bb:bb 19 A2DP
CALL 1
CONNECT 1 A2DP 19
CONNECT 2 A2DP 19
A2DP STREAMING START 1
@0 AVRCP PDU 10 3
```

```bash
장치 B, 싱크 + 대상은 연결 및 Get Capabilities PDU를 수신합니다. 트랙 변경 0x01, 재생 상태 변경 0x02, (절대) 볼륨 변경 0x0d의 세 가지 이벤트로 응답합니다.

RING 0 00:07:80:aa:aa:aa 17 AVRCP
RING 1 00:07:80:aa:aa:aa 19 A2DP
RING 2 00:07:80:aa:aa:aa 19 A2DP
A2DP STREAMING START 1
AVRCP 0 PDU_GET_CAPABILITIES 3
@0 AVRCP RSP 1 2 d
```

```bash
장치 A는 기능 가져오기에 대한 응답을 받습니다. 다른 장치가 지원하는 것을 확인하고 볼륨 변경 알림을 등록합니다.

AVRCP 0 PDU_GET_CAPABILITIES_RSP EVENT COUNT 3 TRACK_CHANGED
PLAYBACK_STATUS_CHANGED VOLUME_CHANGED
@0 AVRCP PDU 31 d
```

```bash
이제 장치 B는 컨트롤러에 대상 볼륨의 현재 값(최대값의 55%)을 알려주는 중간 응답으로 즉시 응답해야 합니다.

AVRCP 0 PDU_REGISTER_NOTIFICATION 1 VOLUME_CHANGED 0
@0 AVRCP NFY INTERIM 1 d 55
```

```bash
장치 A는 중간 응답을 수신하고 이제 볼륨이 55%임을 알고 있습니다. 볼륨을 61%로 변경하려고 합니다.

AVRCP 0 PDU_REGISTER_NOTIFICATION_RSP INTERIM VOLUME_CHANGED 55
@0 AVRCP PDU 50 61
```

```bash
장치 B는 명령을 수신하지만 볼륨을 5%씩만 변경할 수 있으므로 볼륨을 가장 가까운 숫자인 60%로 조정하고 컨트롤러에 다시 보고합니다. 컨트롤러가 변경을 요청했기 때문에 이것은 볼륨 변경 알림을 트리거하지 않습니다.

AVRCP 0 PDU_SET_ABSOLUTE_VOLUME 61
@0 AVRCP RSP 60
```

```bash
장치 B는 명령을 수신하지만 볼륨을 5%씩만 변경할 수 있으므로 볼륨을 가장 가까운 숫자인 60%로 조정하고 컨트롤러에 다시 보고합니다. 컨트롤러가 변경을 요청했기 때문에 이것은 볼륨 변경 알림을 트리거하지 않습니다.

AVRCP 0 PDU_SET_ABSOLUTE_VOLUME 61
@0 AVRCP RSP 60
```

```bash
이제 장치 A는 볼륨이 60%로 설정되었음을 확인하고 그에 따라 자체 볼륨 디스플레이를 조정합니다.

AVRCP 0 PDU_SET_ABSOLUTE_VOLUME_RSP 60
```

```bash
장치 B의 사용자는 볼륨을 한 단계 되돌려 55%로 낮춥니다. 이것은 컨트롤러에 알려야 하고 변경됨 알림을 트리거합니다. 트랜잭션 레이블(INTERIM 문자열 뒤의 첫 번째 매개변수)은 원래 요청과 동일합니다.

@0 AVRCP NFY CHANGED 1 d 55

이제 장치 A가 여전히 장치 B의 볼륨 수준을 모니터링하려는 경우 AVRCP 사양에 따라 모든 알림이 한 번만 트리거되기 때문에 다른 알림을 등록해야 합니다.
```