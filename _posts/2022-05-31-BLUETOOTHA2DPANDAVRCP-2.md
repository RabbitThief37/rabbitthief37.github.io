---
layout: post
title: "AN986: BLUETOOTH® A2DP AND AVRCP PROFILES GETTING STARTED GUIDE - 2"
description: "SILICON LABS BLUETOOTH® A2DP AND AVRCP PROFILES 번역"
author: RabbitThief
date: 2022-05-31 18:00:00 +0900
tags: bluetooth a2dp avrcp siliconlabs translatoin 
category: bluetooth
comments: false
---	




# BLUETOOTH® A2DP AND AVRCP PROFILES - 2

## AVRCP browsing

찾아보기는 AVRCP 사양 버전 1.4에 도입된 기능입니다. iWRAP6은 AVRCP 버전 1.5 컨트롤러를 구현하므로 컨트롤러 측에서 브라우징이 지원됩니다.
AVRCP 탐색은 탐색이 지원되는 대상의 모든 미디어 플레이어에서 가상 파일 시스템을 탐색하는 데 사용됩니다. AVRCP 브라우징은 미디어 플레이어 나열 및 선택, 폴더 위아래 탐색 및 폴더 내용 나열, 플레이어 항목에 대한 정보 검색, 특정 항목 검색을 용이하게 합니다.

### Browsing general concepts

**Browsing channel**

일반 AVRCP 채널이 설정된 후에 열리는 별도의 L2CAP 채널을 통해 찾아보기가 수행됩니다. 모든 일반 AVRCP 명령은 여전히 일반 AVRCP 채널을 통해 전송됩니다. 탐색 채널은 대상 장치의 미디어에 대한 정보를 검색하는 데만 사용됩니다. 찾은 항목을 현재 재생 목록에 추가하는 것과 같은 다른 모든 작업은 여전히 일반 AVRCP 채널을 통해 실행됩니다.
컨트롤러 또는 대상은 탐색 채널을 열 수 있으며 요청 시 채널을 열고 닫을 수 있습니다. 더 일반적으로 일반 AVRCP 링크가 열려 있는 한 열린 상태로 유지할 수 있습니다. 일반 AVRCP 링크를 닫기 전에 탐색 채널을 닫아야 합니다.
iWRAP LIST 출력에서 브라우징 채널의 연결 유형은 AVRCP이지만 L2CAP PSM 번호는 0x001b인 반면 일반 AVRCP 연결의 PSM은 0x0017입니다.

**UIDs and UID counter**

탐색 가능 플레이어의 모든 항목에는 두 AVRCP 채널 모두에서 유효한 고유한 128비트 UID가 있습니다. 탐색 채널을 통해 발견된 모든 항목은 발견된 UID를 사용하여 일반 AVRCP PDU(예: PDU 0x74, PLAY_ITEM)로 처리할 수 있습니다.
플레이어의 가상 파일 시스템에 더 많은 미디어 항목이 추가되거나 제거되면 미디어 항목의 UID가 변경될 수 있습니다. UID 카운터는 UID 변경 사항을 추적하는 메커니즘입니다. 컨트롤러가 AVRCP BROWSE LIST 또는 AVRCP BROWSE SEARCH 탐색 명령을 보낼 때마다 대상은 다른 데이터와 함께 UID 카운터의 현재 값을 반환합니다. UID로 미디어 항목을 지정하는 모든 컨트롤러 명령에는 컨트롤러가 받은 마지막 UID 카운터 값도 포함되어야 합니다.
UID 카운터 값이 일치하지 않는 경우(즉, 플레이어의 가상 파일 시스템에서 무언가가 추가, 제거 또는 변경됨) 대상은 상태 0x05, UID가 변경된 명령을 거부합니다. 그런 다음 컨트롤러는 항목의 UID를 검색하는 데 사용한 LIST 명령을 반복해야 합니다.

### Browsing workflow

대상에는 탐색을 지원하는 여러 미디어 플레이어가 있을 수 있으므로 첫 번째 단계는 미디어 플레이어 목록을 가져오고 탐색할 플레이어를 선택하는 것입니다.
예: AVRCP 및 AVRCP 검색 링크를 열고 미디어 플레이어 목록에서 처음 16개 항목을 요청한 다음 ID가 0x0001인 플레이어를 검색된 플레이어로 설정합니다.

```bash
CALL 00:ff:00:bb:aa:cc 17 AVRCP
CALL 0
CONNECT 0 AVRCP 17
@0 AVRCP BROWSE OPEN
CONNECT 1 AVRCP 1b
@0 AVRCP BROWSE LIST 0 0 f ff
AVRCP_BROWSE 1 GET_FOLDER_ITEMS UIDC 0000 ITEMS 0001 < PLAYER ID 0001 TYPE 01
SUBTYPE 00000000 PLAY_STATUS STOPPED FEATURES 0000000000b7011c0200000000000000
NAME 0005 Music >
@0 AVRCP BROWSE SETPLAYER 1
AVRCP_BROWSE 1 SET_BROWSED_PLAYER UIDC 0000 COUNT 00000005 FOLDERS 00
```

(주의) AVRCP 탐색 채널이 이미 열려 있는 경우(예: 전화로 연 경우) AVRCP BROWSE OPEN은 구문 오류를 반환합니다.
탐색을 위해 플레이어를 선택한 후에는 폴더 목록을 검색하고 재생할 미디어를 찾을 차례입니다.

```bash
@0 AVRCP BROWSE LIST 1 0 f ff
AVRCP_BROWSE 1 GET_FOLDER_ITEMS UIDC 0000 ITEMS 0005 < FOLDER < UID
0400000000000000 TYPE ARTISTS PLAYABLE NO NAME 0007 Artists > FOLDER < UID
0500000000000000 TYPE TITLES PLAYABLE NO NAME 0005 Songs > FOLDER < UID
0600000000000000 TYPE ALBUMS PLAYABLE NO NAME 0006 Albums > FOLDER < UID
0900000000000000 TYPE ARTISTS PLAYABLE NO NAME 0009 Composers > FOLDER < UID
0a00000000000000 TYPE GENRES PLAYABLE NO NAME 0006 Genres > >
```

노래 폴더로 직접 이동하여 재생 대기열에 넣을 일부 항목을 선택한다고 가정합니다. 노래 폴더에는 1509개의 항목이 있습니다.

```bash
@0 AVRCP BROWSE PATH 0 1 0500000000000000
AVRCP_BROWSE 1 CHANGE_PATH COUNT 000005e5
```

다음으로 2번째 항목부터 3개의 미디어 항목을 나열하고 모든 속성을 요청합니다.

```bash
@0 AVRCP BROWSE LIST 1 1 3 ff
AVRCP_BROWSE 3 GET_FOLDER_ITEMS UIDC 0000 ITEMS 0003 < < MEDIA UID 9b1afa9f4c12ce59
TYPE AUDIO NAME 0003 ABC ATTRIBUTES 01 < < TITLE NAME 0003 ABC > > > < MEDIA UID
2bf8243352cebe67 TYPE AUDIO NAME 000a About Life ATTRIBUTES 01 < < TITLE NAME 000a About
Life > > > < MEDIA UID b0bdea9e7ecb5224 TYPE AUDIO NAME 0008 Absorbed ATTRIBUTES 01 < <
TITLE NAME 0008 Absorbed > > > >
```

이제 "ABC"라는 노래를 재생해 보겠습니다.

```bash
@0 AVRCP PDU 74 1 9b1afa9f4c12ce59 0
AVRCP 0 PLAY_ITEM_RSP OK
```

재생 대기열에 넣을 "Rain Dogs"라는 앨범을 검색한 다음 10개의 첫 번째 검색 결과를 검색해 보겠습니다.

```bash
@0 AVRCP BROWSE SEARCH “rain dogs”
AVRCP_BROWSE 1 SEARCH UIDC 0000 COUNT 0000001b
@0 AVRCP BROWSE LIST 3 0 9 0
AVRCP_BROWSE 1 GET_FOLDER_ITEMS UIDC 0000 ITEMS 000a < < MEDIA UID 0e24d548196f8e70
TYPE AUDIO NAME 0016 Anywhere I Lay My Head ATTRIBUTES 01 < < TITLE NAME 0016 Anywhere I
Lay My Head > > > < MEDIA UID 8942c6d007e09367 TYPE AUDIO NAME 0010 Big Black Mariah
ATTRIBUTES 01 < < TITLE NAME 0010 Big Black Mariah > > > < MEDIA UID 884472822381fb9b TYPE
AUDIO NAME 000a Blind Love ATTRIBUTES 01 < < TITLE NAME 000a Blind Love > > > < MEDIA UID
06eec7217eed9522 TYPE AUDIO NAME 0011 Bride of Rain Dog ATTRIBUTES 01 < < TITLE NAME 0011
Bride of Rain Dog > > > < MEDIA UID bab8b9091baf783a TYPE AUDIO NAME 000e Cemetery Polka
ATTRIBUTES 01 < < TITLE NAME 000e Cemetery Polka > > > < MEDIA UID 04bb90d8c8c54d00 TYPE
AUDIO NAME 000a Clap Hands ATTRIBUTES 01 < < TITLE NAME 000a Clap Hands > > > < MEDIA UID
163f19ccd93ee837 TYPE AUDIO NAME 000f Diamonds & Gold ATTRIBUTES 01 < < TITLE NAME 000f
Diamonds & Gold > > > < MEDIA UID f73b53fa79c152b0 TYPE AUDIO NAME 0016 Do You Close Your
Eyes ATTRIBUTES 01 < < TITLE NAME 0016 Do You Close Your Eyes > > > < MEDIA UID
97526ba94b21557c TYPE AUDIO NAME 000e Downtown Train ATTRIBUTES 01 < < TITLE NAME 000e
Downtown Train > > > < MEDIA UID db0b9bfc5abaa800 TYPE AUDIO NAME 000f Gun Street Girl
ATTRIBUTES 01 < < TITLE NAME 000f Gun Street Girl > > > >
```

검색 결과의 첫 번째 노래를 재생 대기열에 추가해 보겠습니다.

```bash
@0 AVRCP PDU 90 2 0e24d548196f8e70 0
AVRCP 0 ADD_TO_NOW_PLAYING_RSP OK
```

## Power saving

iWRAP은 두 가지 절전 옵션을 제공합니다. 활성 Bluetooth 연결을 위한 전력을 절약하고 내부 프로세서를 감소된 듀티 사이클 모드로 전환하는 더 깊은 절전에 사용할 수 있는 스니프 모드. sniff 및 deep sleep 모드에 대한 자세한 내용은 iWRAP 사용 설명서를 참조하십시오. 활성 A2DP 연결과 함께 스니프 모드를 사용할 때는 매우 주의해야 합니다. 스니프 모드는 데이터 전송 속도를 줄여 오디오 링크의 견고성에 영향을 줍니다. 스니프 모드를 A2DP 오디오와 함께 사용하면 절전 없이 "클리핑"이 발생할 가능성이 더 큽니다.
또한 Bluetooth 연결이 활성 모드에 있을 때 즉, 사용 중인 절전 모드가 없을 때 마스터 장치는 슬레이브 장치보다 3-4배 적은 전력을 사용합니다. 따라서 배터리 구동 애플리케이션의 경우 장치를 슬레이브가 아닌 마스터로 구성하는 것이 유용할 수 있습니다.

## Audio quality improvement

A2DP 오디오를 인코딩하거나 디코딩하는 데 사용되는 서브밴드 코딩(SBC)은 손실 오디오 코덱이며 블루투스 링크를 통해 전송될 때 오디오 데이터의 일부가 손실됩니다. 고급 헤드셋 및 헤드폰 또는 오디오 증폭기와 같은 일부 응용 프로그램의 경우 허용되지 않을 수 있습니다.
대체 오디오 코덱을 사용하여 오디오 스트림을 인코딩 또는 디코딩하고 더 나은 품질 또는 더 낮은 대기 시간의 Bluetooth 오디오를 제공할 수 있습니다. 이러한 코드는 예를 들어 MP3, AAC, aptX 또는 aptX Low Latency입니다. 그러나 MP3 코덱은 일반적으로 Bluetooth 지원 휴대폰, 태블릿 및 PC에서 지원되지 않습니다. AAC 코덱은 최신 세대의 Apple 장치에서 지원됩니다. 반면에 aptX 코덱은 시장에서 가장 널리 채택되었으며 많은 휴대폰, 태블릿, PC 및 헤드셋에서 지원됩니다.
AAC 코덱 지원은 기본적으로 모든 펌웨어 빌드에서 iWRAP6부터 사용할 수 있습니다(iWRAP5 오디오 펌웨어 빌드 요청 시). 단, 최종 제품에서 코덱을 사용하기 위해서는 별도의 라이선스가 필요하며 별도의 라이선스 비용이 발생합니다. 자세한 내용은 [http://www.vialicensing.com/licensing/aac-fees.aspx를](http://www.vialicensing.com/licensing/aac-fees.aspx%EB%A5%BC) 참조하십시오.
aptX 코덱은 거의 무손실 오디오 품질을 제공하므로 Hi-Fi 오디오 응용 프로그램 및 고품질 Bluetooth 오디오가 필요한 제품에 사용하기에 이상적입니다. 블루투스의 최고 품질
오디오는 Bluetooth 대기 시간을 크게 줄이는 aptX Low Latency 코덱으로 제공됩니다. iWRAP는 aptX 및 aptX Low Latency 코덱을 지원하지만 타사 코덱이고 추가 라이선스 비용이 있기 때문에 일반 펌웨어 빌드에 포함되지 않습니다. aptX 및 aptX Low Latency 코덱에 대한 자세한 내용은 [https://www.silabs.com/about-us/contact-sales에](https://www.silabs.com/about-us/contact-sales%EC%97%90) 문의하십시오.

### Enabling AAC Audio Codec for A2DP

Bluetooth A2DP가 있는 옵션 오디오 코덱 중 하나는 AAC입니다. AAC는 현재 Apple iOS 장치에서 지원되며 일반 SBC 코덱에 비해 더 나은 오디오 품질을 제공합니다. WT32i는 AAC 오디오 코덱을 지원하며 다음 iWRAP 명령으로 활성화할 수 있습니다.

```bash
SET CONTROL CODEC AAC JOINT_STEREO 44100 0
SET CONTROL CODEC SBC JOINT_STEREO 44100 1
```

첫 번째 명령은 스테레오 모드 및 44.1kHz 샘플링 속도 및 우선 순위 0(가장 높음)에서 AAC 코덱을 활성화합니다. 두 번째 명령은 스테레오 모드에서 필수 SBC 코덱과 44.1kHz 샘플링 속도 및 우선 순위 1(낮음)을 활성화합니다. 이 구성에서는 AAC가 기본 코덱이지만 원격 장치가 지원하지 않는 경우 SBC가 대신 사용됩니다.
(참고) iWRAP에는 수많은 제3자가 소유한 지적 재산을 통합하는 AAC 기술이 포함되어 있습니다. 이 제품의 공급은 해당 제3자의 관련 지적 재산권에 따른 라이선스를 전달하지 않으며 최종 사용자 또는 바로 사용할 수 있는 최종 제품에서 이 제품을 사용할 권리를 의미하지 않습니다. 이러한 사용을 위해서는 독립 라이선스가 필요합니다. 자세한 내용은 [http://www.vialicensing.com을](http://www.vialicensing.xn--com-of0o/) 참조하십시오.

### Enabling aptX Audio Codec for A2DP

aptX는 Bluetooth A2DP를 위한 또 다른 선택적 오디오 코덱입니다. aptX는 최신 Android 기기에서 지원되며 일반 SBC 코덱에 비해 더 나은 품질의 오디오를 제공합니다. 이 기능은 WT32i 모듈에서만 사용할 수 있습니다. aptX 오디오 코덱은 다음 iWRAP 명령을 사용하여 특수 빌드에서 활성화할 수 있습니다.

```bash
SET CONTROL CODEC APT-X JOINT_STEREO 44100 0
SET CONTROL CODEC SBC JOINT_STEREO 44100 1
```

(참고) aptX는 특수 라이선스가 필요하며 별도의 iWRAP 빌드에만 포함됩니다. 활성화하려면 aptX 빌드를 플래시하고 유효한 라이선스 키를 추가해야 합니다. 라이선스 키 획득 방법에 대한 자세한 내용은 Bluegiga 기술 지원([https://www.silabs.com/support](https://www.silabs.com/support))에 문의하십시오.

### Enabling aptX Low Latency Audio Codec for A2DP

aptX Low Latency는 Bluetooth A2DP를 위한 또 다른 옵션 오디오 코덱입니다. 약 40ms(표준 Bluetooth 대기 시간보다 훨씬 낮음)의 Bluetooth 대기 시간을 제공하고 오디오 애플리케이션에 권장되는 대기 시간을 충족합니다. aptX Low Latency 기능은 A2DP 소스 모드를 지원하며 WT32i 모듈에서만 사용할 수 있습니다.
aptX Low Latency 오디오 코덱은 다음 iWRAP 명령을 사용하여 특수 빌드에서 활성화할 수 있습니다.

```bash
SET CONTROL CODEC APT-X_LL JOINT_STEREO 44100 0
SET CONTROL CODEC APT-X JOINT_STEREO 44100 1
SET CONTROL CODEC SBC JOINT_STEREO 44100 2
```

### Routing the A2DP Audio to I2S (External Codec)

iWRAP6은 외부 MCU 없이 외부 I2S 오디오 코덱을 구성하기 위한 기본 지원을 추가합니다. 이를 제어하는 명령어는 “SET CONTROL AUDIO”와 “SET CONTROL EXTCODEC”이다.
WT32i 개발 키트에는 WT32i의 I2S 인터페이스 및 I2C 인터페이스에 연결하는 외부 I2S 오디오 코덱(Texas Instruments TLV320AIC32IRHB)이 포함되어 있습니다. 외부 코덱을 시도하려면 다음 iWRAP 구성이 필요합니다.

```bash
SET CONTROL AUDIO INTERNAL I2S EVENT 32
SET CONTROL EXTCODEC PRE 30 18 0200918000008A1037000100008000000FF07C7C787C7C78 30 04
25C01400 30 0a 28C00000000000008000 30 03 330F00 30 05 3F00800F00 30 04 6500A200
RESET
```