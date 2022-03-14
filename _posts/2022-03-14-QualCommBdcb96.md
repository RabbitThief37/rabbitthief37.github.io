---
layout: post
title:  "BlueTest3 USER GUIDE"
description: "QualComm CSR8675 BlueTest3 USER GUIDE 번역"
author: RabbitThief
date: 2022-03-14 15:00:00 +0900
tags: qualcomm csr8675 translatoin 
category: bluetooth
comments: false
---	



# QualComm BlueSuite Components

### BlueTest3

BlueTest3는 RF 테스트를 위해 BlueCore의 BIST(Built In Self Test) 기능을 실행할 수 있는 프로그램입니다. BIST 기능은 주로 저수준 무선 테스트(규정된 주파수에서 연속파 전송 또는 의사 랜덤 데이터 수신 및 비트 오류율 계산)로 구성됩니다. PCM 포트 및 기타 내부 블록에 대한 테스트가 포함됩니다.

### PSTool and PSCli

PSTool을 사용하면 IC의 영구 저장소를 구성할 수 있습니다.
PSCli는 명령줄을 사용하여 유사한 기능 세트를 제공합니다.

### BlueFlash and FlueFlashCmd

BlueFlash는 IC의 플래시 메모리에 펌웨어를 쓰기 위한 GUI를 제공합니다.
BlueFlashCmd는 명령줄 응용 프로그램에서 유사한 기능을 제공합니다.

### BTCli

Bluetooth 명령줄 인터페이스(BTCli)는 BlueCore 장치에 HCI(호스트 컨트롤러 인터페이스) 명령을 보낼 수 있는 명령줄 프로그램입니다. 호스트 컴퓨터는 HCI 인터페이스(Bluetooth 사양의 일부)를 사용하여 Bluetooth 컨트롤러(예: BlueCore 칩)와 통신합니다.

### E2Cmd

EEPROM의 내용을 구성, 덤프, 작성 및 확인하는 명령줄 응용 프로그램입니다.
E2Cmd는 또한 CSR 갱 프로그래머 하드웨어를 사용하여 최대 16개의 장치에 브로드캐스트 쓰기를 지원합니다.

### SpiUnlock

SPI 포트가 잠겨 있는 IC의 잠금을 해제하는 명령줄 도구입니다.

### NVSCmd

SQI 및 SPI 플래시에서 읽기/쓰기를 위한 명령줄 도구입니다.

### USBPurger

Windows 레지스트리에서 USB 장치에 대한 참조를 제거하는 명령줄 도구입니다.

### DFU Tools

CSR DFU 도구는 펌웨어 및 영구 저장소 파일을 서명하고 결합하여 BlueCore DFU 파일을 형성할 수 있도록 하는 프로그램 모음입니다. 

### DFUWizard

DFUWizard는 CSR IC에 대한 장치 펌웨어 업그레이드를 수행합니다.

### DFUToolTips

Windows의 DFU 파일에 대한 도구 설명을 표시하는 셸 확장입니다.

### DFUBabel

USB-SPI 변환기의 펌웨어를 업그레이드하는 명령줄 응용 프로그램입니다.

### TrueTest

TrueTest 툴킷은 BlueCore 지원 장치의 프로덕션 프로그래밍 및 테스트를 위해 다양한 언어로 애플리케이션을 개발하는 데 필요한 라이브러리와 문서로 구성되어 있습니다. 이 툴킷은 프로덕션 테스트 시스템에서 사용하도록 설계되었습니다. 다른 BlueSuite 프로그램을 호출하는 테스트 스크립트를 작성하는 대신 테스트 API(Application Programming Interface)에 직접 액세스하는 테스트 프로그램을 작성할 수 있습니다.

### HidDfu and HidDfuCmd

HidDfu DLL API는 CSR USB 드라이버 대신 Windows HID 드라이버를 사용하여 CSR BlueCore 장치용 PC 기반 장치 펌웨어 업그레이드 개발을 위한 인터페이스를 제공합니다. 설치된 BlueSuite 도움말을 참조하십시오. HidDfuCmd 애플리케이션은 HidDfu API를 사용하여 장치 업그레이드 및 백업 작업을 수행합니다.

### Architecture

BlueCore에는 테스트, 플래시 및 디버깅 목적을 위한 디버그(SPI) 인터페이스가 있습니다. 사용자 모드 애플리케이션은 LPT 포트에 연결하거나 USB 포트에 연결된 CSR USB-SPI 장치를 통해 SPI 프로토콜을 사용하여 BlueCore IC 디버그 인터페이스와 통신합니다.
BlueCore에는 제어 및 데이터 전송을 위한 호스트 인터페이스도 있습니다. 사용자 모드 응용 프로그램은 USB 연결을 통해 또는 UART 연결을 통해 BCSP, H4, H4DS 또는 H5를 사용하여 BlueCore의 호스트 인터페이스와 통신합니다.

![Untitled](/assets/images/2022/0314/Untitled.png)

## Firmware

CSR의 BlueCore IC에는 제어 소프트웨어(펌웨어라고 함)가 포함되어 있습니다. BlueSuite의 도구를 사용하여 플래시가 있는 BlueCores의 펌웨어를 업데이트할 수 있습니다.

- BlueFlash(섹션 8.2 참조)는 개발 중에 SPI 디버깅 인터페이스를 사용하여 BlueCore의 펌웨어를 업데이트하는 데 사용됩니다. TestFlash(TrueTest의 일부)는 생산 라인에서 동일한 목적으로 사용할 수 있습니다.
- DFUWizard(섹션 8.4 참조)를 사용하여 개발 및 현장에서 펌웨어를 업그레이드할 수 있습니다. DFU는 최종 사용자가 USB 또는 UART 인터페이스를 통해 BlueCore의 펌웨어를 업그레이드할 수 있도록 하는 USB 표준입니다. DFUWizard를 사용하려면 장치에 이미 일부 펌웨어(로더)가 있어야 합니다. 로더는 BlueFlash에서만 업데이트할 수 있습니다.
- HidDfuCmd(또는 HidDfu API를 사용하는 자체 애플리케이션)를 사용하여 개발 및 현장에서 펌웨어를 업그레이드할 수 있습니다. 섹션 3을 참조하십시오.

펌웨어 버전 번호는 중요한 정보입니다. BlueFlash를 사용하여 현재 펌웨어 버전을 식별할 수 있습니다(섹션 8.2 참조).

### Firmware File Formats

- .xpv / .xdv - 표준 BlueCore 펌웨어 파일 형식은 각각 데이터의 일부를 보유하는 두 개의 개별 파일에 펌웨어 릴리스에 대한 이진 데이터를 보유합니다.
- .xuv - 이 펌웨어 파일 형식은 .xpv 파일과 .xdv 파일의 내용을 모두 포함하는 하나의 파일입니다.
- dfu - DFUWizard 또는 HidDfu 응용 프로그램과 함께 사용하기 위한 파일 형식입니다. 참고: DFUTools는 .xpv 및 .xdv 파일을 .dfu파일로 변환할 수 있습니다.

.dfu 파일 형식은 여러 펌웨어 이미지와 여러 버전의 영구 저장소를 저장할 수 있는 유연한 컨테이너 형식입니다. 이는 여러 하드웨어 장치에서 작동하는 범용 .dfu 파일을 가질 수 있음을 의미합니다. DFU에 대한 자세한 내용은 BlueCore 장치 펌웨어 업그레이드 개요를 참조하십시오.

### Persistent Store

펌웨어 이미지(즉, 제어 소프트웨어)를 포함할 뿐만 아니라 펌웨어 파일에는 영구 저장소(PS)로 알려진 구성 정보도 포함될 수 있습니다. DFU 파일에는 PS의 부분 또는 전체 업데이트에 대한 정보가 포함될 수 있습니다. DFU 파일에 없는 PS 영역은 업데이트 전 그대로 유지됩니다. 다른 펌웨어 파일은 일부 정보를 포함할 수 없습니다. 그들은 PS를 보존하거나 대체합니다.
CSR 지원 웹사이트의 펌웨어 업그레이드에는 PS 설정이 포함되어 있지 않으므로 현재 설정이 유지됩니다. 만든 펌웨어 덤프에는 PS 설정이 포함되어 있으므로 이전에 덤프한 파일을 사용하여 업그레이드하는 경우 기존 설정을 덮어씁니다.

PS가 없는 펌웨어를 빈 플래시에 다운로드하면 Casira 모듈에 적합한 기본값을 사용하여 플래시 메모리에 새로운 PS가 생성됩니다. 그러나 일부 키는 최적의 성능을 위해 모듈당 보정이 필요합니다. 개별 PS 키 사용에 대한 자세한 내용은 펌웨어 릴리스의 pskeys.html을 참조하십시오. PSTool에서 PS 키 이름을 클릭하면 동일한 정보 중 일부를 사용할 수 있습니다. PSTool 사용 설명서를 참조하십시오.

### Firmware Build Types

Bluecore의 Bluetooth 스택 펌웨어는 HCI 계층까지 Bluetooth 스택 계층을 포함하거나 RFCOMM 계층까지 모든 스택 계층을 포함하는 두 가지 형태로 제공됩니다.
펌웨어 버전 18부터 펌웨어 빌드를 통합이라고 합니다. 여기에는 RFCOMM까지의 모든 Bluetooth 스택 계층이 포함되지만 HCI 인터페이스 또는 RFCOMM 인터페이스를 제공하도록 구성할 수 있습니다. 동작은 PSKEY_ONCHIP_HCI_CLIENT에 의해 제어됩니다. 자세한 내용은 펌웨어 릴리스 파일을 참조하십시오.
모든 유형의 펌웨어 빌드는 모든 BlueCore 펌웨어 파일 형식으로 보관할 수 있습니다.

### Upgrading Firmware

최신 BlueCore 펌웨어 파일을 받으려면 CSR 지원 웹사이트([www.csrsupport.com/BluetoothFirmware](http://www.csrsupport.com/BluetoothFirmware))의 Bluetooth 펌웨어 섹션으로 이동하십시오. BlueFlash, DFUWizard 또는 HidDfuCmd를 사용하여 BlueCore 장치에 펌웨어를 다운로드합니다.
Casira 사용자는 펌웨어 업그레이드 지침에 대해 Casira 사용 설명서를 참조해야 합니다.

## BlueFlash

BlueFlash는 Casira 모듈 또는 자체 BlueCore 설계의 플래시 메모리에서 펌웨어를 다운로드 및 업로드할 수 있는 유틸리티입니다. 섹션 6에 설명된 대로 여러 다른 펌웨어 파일 형식이 있으며 펌웨어 파일에는 영구 저장소 설정이 포함될 수 있습니다. 영구 저장 설정을 실수로 변경하지 않았는지 확인하고 BlueFlash에 의해 업데이트된 BlueCore 펌웨어의 부분을 확인하려면 섹션 6.2를 읽으십시오.

완전히 확신하지 않는 한 플래시 지우기로 영구 저장소를 지우지 마십시오(선택 또는 전체 지우기를 통해). 플래시를 완전히 지우면 모든 영구 저장 설정이 제거됩니다. 영구 저장소에 문제가 있고 이전의 알려진 작동 버전으로 다시 로드하려는 경우에만 전체 지우기를 사용하십시오.

- Choose File - BlueCore 모듈의 플래시에 다운로드할 펌웨어 파일을 선택하기 위한 대화 상자를 엽니다. .xpv, .xuv 또는 .xbv 펌웨어 파일을 열 수 있습니다. .xpv 파일을 열면 .xdv 파일이 같은 폴더에 있어야 합니다.
- Edit - 누르면 더 이상 지원되지 않는다는 메시지가 표시됩니다.
- Download - 선택한 파일을 BlueCore 모듈의 플래시에 복사합니다. 이 프로세스는 필요에 따라 각 플래시 블록을 업데이트하고 차례로 확인합니다.
- Verify - 선택한 파일을 BlueCore 모듈의 플래시에 프로그래밍된 코드와 비교합니다. 확인은 파일에 포함된 구성 요소만 비교합니다. 상태 영역에는 보고된 차이점이 표시됩니다.
- Status - 현재 작업의 상태에 대한 텍스트 보고서를 제공합니다.
- Progress Bar - 현재 작업의 진행 상황을 시각적으로 표시합니다.
- Start Processor - 프로세서 상태에 따라 변경됩니다. 프로세서 중지가 표시되면 프로세서가 실행 중인 것입니다. 코드 다운로드를 시도하기 전에 중지해야 합니다. 새 코드가 로드되면 프로세서 시작을 클릭하여 프로세서를 다시 시작할 수 있습니다.
- Firmware ID - 모듈에 현재 로드된 펌웨어 버전을 식별합니다. BlueFlash가 펌웨어 버전을 식별할 수 없는 경우에도 이 유틸리티를 사용하여 펌웨어를 업그레이드할 수 있습니다.
- Dump - 사용자가 BlueCore 모듈의 플래시 내용을 파일로 덤프할 수 있습니다. 데이터는 .xpv/.xdv 파일 쌍, 결합된 .xuv 파일 또는 원시 바이너리 파일로 저장할 수 있습니다. 덤프된 파일에는 모든 영구 저장소 설정도 포함되어 있어 알려진 펌웨어 빌드 및 영구 저장소 설정으로 복원할 수 있습니다.
- Flash Erase - 영구 저장 영역을 포함하여 플래시 메모리의 일부 또는 전체를 지우는 옵션을 제공합니다.
- File ID - 모듈에서 다운로드하기 위해 선택한 파일의 펌웨어 이름과 버전을 표시합니다.
- File Selection - 다운로드를 위해 선택한 파일의 이름과 위치를 표시합니다.
- Flash Type - BlueCore 모듈에서 식별된 플래시 메모리 유형을 표시합니다. 프로세서를 중지한 후 플래시 유형이 식별됩니다. 다른 유형의 플래시 메모리는 BlueCore의 메모리 맵을 조정해야 하는 다른 메모리 블록 구조를 가질 수 있습니다. 플래시 유형이 인식할 수 없는 플래시를 나타내는 경우 BlueCore 모듈의 플래시가 지원되지 않거나 SPI 연결에 문제가 있는 것입니다. 섹션 8.2.2를 참조하십시오.
- SPI Selection - 사용자가 SPI 케이블이 연결된 LPT 또는 USB 포트를 선택할 수 있습니다.