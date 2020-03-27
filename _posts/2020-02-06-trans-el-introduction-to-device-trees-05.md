---
layout: post
title:  "Introduction to Device Trees - 5"
description: "Introduction to Device Trees - 5"
author: RabbitThief
date: 2020-02-06 10:00:00 +0900
tags: embedded linux pdf 
category: Translation
comments: false
---	


원본은 [NXP 사이트](https://www.nxp.com/docs/en/application-note/AN5125.pdf)에서 바로 받을 수 있다.

[Introduction to Device Trees - 1](https://rabbitthief37.github.io/2020/01/30/trans-el-introduction-to-device-trees-01.html)

[Introduction to Device Trees - 2](https://rabbitthief37.github.io/2020/01/31/trans-el-introduction-to-device-trees-02.html)

[Introduction to Device Trees - 3](https://rabbitthief37.github.io/2020/02/03/trans-el-introduction-to-device-trees-03.html)

[Introduction to Device Trees - 4](https://rabbitthief37.github.io/2020/02/04/trans-el-introduction-to-device-trees-04.html)

[Introduction to Device Trees - 6](https://rabbitthief37.github.io/2020/02/07/trans-el-introduction-to-device-trees-06.html)

[Introduction to Device Trees - 7](https://rabbitthief37.github.io/2020/02/13/trans-el-introduction-to-device-trees-07.html)



# 11. Examples

예를 들어 Power Architecture Device Tree는 arch / powerpc / boot / dts에 있고, ARM Architecture Device Tree는 arch / arm / boot / dts에 있습니다.

다음은 P2020과 LS1021A-TWR 제품에 대한 DTS, DTSI 설명 예제 입니다.

**NOTE**

>  간결성을 위해 일부 섹션만 아래에 설명되어 있습니다.



## 11.1. P2020 example

다음은 P2020 RDB에 대한 장치 트리의 섹션 예입니다.  이 DTS 파일은 여러 DTSI 파일을 사용합니다.



### 11.1.1. P2020rdb.dts

아래 표는 P2020 보드를 설명하는 P2020rdb.dts 파일입니다.

| DTS file                                                     | Comment                                                      |
|: ----------------------------------------------------------- |: ----------------------------------------------------------- |
| `/include/ "fsl/p2020si-pre.dtsi"`                             | INCLUDE fsl/p2020si-pre.dtsi                                 |
| `/ {`                                                          | 루트노드는 '/'로 구분합니다.                                 |
| model = "fsl,P2020RDB";                                      | 장치의 제조사(fsl)와 모델번호(P2020RDB)를 정의               |
| compatible = "fsl,P2020RDB";                                 | 특정 보드를 설명                                             |
| aliases {                                                    | aliases 노드의 각 속성은 다른 노드의 인덱스로 정의           |
| ethernet0 = &enet0;                                          |                                                              |
| ethernet1 = &enet1;                                          |                                                              |
| ethernet2 = &enet2;                                          |                                                              |
| serial0 = &serial0;                                          |                                                              |
| pci0 = &pci0;                                                |                                                              |
| pci1 = &pci1;                                                |                                                              |
| };                                                           |                                                              |
| memory {                                                     | 메모리 노드                                                  |
| device_type = "memory";                                      | 메모리로 장치 종류를 정의                                    |
| };                                                           |                                                              |
| lbc: localbus@ffe05000 {                                     | localbus노드는 주소가 0xFFE0_5000에서 시작                   |
| reg = <0 0xffe05000 0 0x1000>;                               | reg의 첫번째 항목은 반드시 localbus노드와 주소가 같아야 한다.<br>  address-cells = 2 ( DTSI 파일에서 정의)이기 때문에 <br> "Address (64-bit) = 0x0_FFE0_5000 , Size (루트노드에서) = 2", <br> 따라서 크기는 0x1000과 동일한 64 비트 (두 개의 <u32> 값으로 표시)입니다. |
| /* NOR and NAND Flashes */                                   | ranges 속성은 버스(자식) 주소 공간과 부모 주소 공간 간의 변환을 매핑합니다. 첫 번째 셀은 chip-select를 나타내고, 그 뒤에 chip-select 주소 및 크기의 오프셋이 됩니다. |
| ranges = <0x0 0x0 0x0 0xef000000 0x01000000                  | CS0, offset 0xef000000, size 0x1000000                       |
| 0x1 0x0 0x0 0xffa00000 0x00040000                            | CS1, offset 0xffa00000, size 0x40000                         |
| 0x2 0x0 0x0 0xffb00000 0x00020000>;                          | CS2, offset 0xffb00000, size 0x20000                         |
| nor@0,0 {                                                    | local bus의 첫번째 자식 노드. CS = 0, address offset = 0x0   |
| \#address-cells = <1>;                                       | Addresses of children (for example, partitions) are 32 bits  |
| \#size-cells = <1>;                                          | Size of children are 32 bits                                 |
| compatible = "chi-flash"                                     | NOR 하드웨어는 cfi-flash로 표시                              |
| reg = <0x0 0x0 0x1000000>;                                   | CS = 0, address offset 0x0000_0000, size = 0x1000000         |
| bank-width = <2>;                                            | 16-bit device on the local bus                               |
| device-width = <1>;                                          | cfi-flash에 특정한 바인딩                                    |
| partition@0 {                                                | NOR의 첫번째 자식 노드                                       |
| /* This location must not be altered */                      |                                                              |
| /* 256KB for Vitesse 7385 Switch firmware */                 |                                                              |
| reg = <0x0 0x00040000>;                                      | reg = 0, 이는 NOR의 상단에서 시작하고 (부모 = 0x0_EF00_0000으로 정의) 크기 = 0x40000 |
| label = "NOR (RO) Vitesse-7385 Firmware";                    | 레이블 및 읽기 전용은 드라이버와 관련된 바인딩입니다.  라벨은 장치를 정의하는 사람이 읽을 수있는 문자열입니다. |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| partition@40000 {                                            | NOR의 두번째 자식 노드 0x40000에서 시작                      |
| /* 256KB for DTB Image */                                    |                                                              |
| reg = <0x00040000 0x00040000>;                               | address = 0x40000 , size = 0x40000                           |
| label = "NOR (RO) DTB Image";                                |                                                              |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| ...                                                          | ...                                                          |
| nand@1,0 {                                                   | local bus의 NAND 자식 노드, chip-select = 1, 부모로부터 address offset = 0x0 |
| \#address-cells = <1>;                                       |                                                              |
| \#size-cells = <1>;                                          |                                                              |
| compatible = "fsl,p2020-fcm-nand",                           | NAND 노드는 두 개의 다른 드라이버로 compatible 속성을 정의   |
| "fsl,elbc-fcm-nand";                                         |                                                              |
| reg = <0x1 0x0 0x40000>;                                     | CS = 1, address offset 0x0000_0000, size = 0x4_0000          |
| partition@0 {                                                |                                                              |
| /* This location must not be altered */                      |                                                              |
| /* 1MB for u-boot Boot loader Image */                       |                                                              |
| reg = <0x0 0x00100000>;                                      | NAND의 첫번째 자식노드, NAND 주소 범위의 상단에 상주         |
| label = "NAND (RO) U-Boot Image";                            |                                                              |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| partition@100000 {                                           | NAND의 두번째 자식노드, address offset = 0x10_0000           |
| /* 1MB for DTB Image */                                      |                                                              |
| reg = <0x00100000 0x00100000>;                               | NAND상단으로부터 address offset = 0x10_0000                  |
| label = "NAND (RO) DTB Image";                               |                                                              |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| ...                                                          | ...                                                          |
| soc: soc@ffe00000 {                                          | SoC address 0xFE00_0000의 레이블.  NOTE> 루트노드의 #address-cells 속성이 2로 설정되었기 때문에 32bit보다 크다.(실제로 #address-cells는 dtsi파일 안에서) 이 노드는 내부 SoC 레지스터를 0x0_FFE0_0000 주소에 연결한다. |
| ranges = <0x0 0x0 0xffe00000 0x100000>;                      | 버스의 주소 공간과 부모의 주소 공간 사이의 변환을 매핑합니다.  자식 노드의 주소 크기는 1이고이 노드의 주소 크기는 2이므로, 자식 노드의 주소 0x0을 0x0_FFE0_0000 (0x100000 크기)에 매핑합니다. |
| ...                                                          | ...                                                          |
| i2c@3000{                                                    | i2c 노드, address = 0x3000                                   |
| rtc@68{                                                      | 자식 RTC 노드, 부모노드로 부터 offset = 0x68                 |
| compatible = "dallas,ds1339";                                | dallas, ds1339                                               |
| reg = <0x68>;                                                | Address = 0x68                                               |
| };                                                           |                                                              |
| };                                                           |                                                              |
| ...                                                          | ...                                                          |
| spi@7000 {                                                   | SPI 노드, address = 0x7000                                   |
| flash@0 {                                                    | SPI 자식 노드 offset = 0x0, 레이블은 flash                   |
| #address-cells = <1>;                                        | 자식 노드는 하나의 <u32> address 사용                        |
| #size-cells = <1>;                                           | 자식도는는 하나의 <u32> size 사용                            |
| compatible = "spansion,s25sl12801";                          | Spansion, s25sl12801                                         |
| reg = <0>;                                                   |                                                              |
| spi-max-frequency = <40000000>;                              | SPI 포트가 허용하는 최대 주파수에 대해 SPI 드라이버가 구문 분석 할 수있는 추가 정보 |
| partition@0 {                                                | flash의 자식노드, offset = 0x0                               |
| /* 512KB for u-boot Boot loader Image */                     |                                                              |
| reg = <0x0 0x00080000>;                                      | offset = 0x0, size = 0x80000                                 |
| label = "SPI (RO) U-Boot Image";                             | 파티션에서 사용하는 레이블                                   |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| partition@80000 {                                            | flash의 두번째 자식노드(파티션)                              |
| /* 512KB for DTB Image */                                    |                                                              |
| reg = <0x00080000 0x00080000>;                               | offset = 0x80000, size = 0x80000                             |
| label = "SPI (RO) DTB Image";                                | 파티션에서 사용하는 레이블                                   |
| read-only;                                                   | 이 속성은 flash 드라이버에 의해 분석 가능                    |
| };                                                           |                                                              |
| partition@100000 {                                           | flash의 파티션 자식 노드, offset = 0x100000                  |
| /* 4MB for Linux Kernel Image */                             |                                                              |
| reg = <0x00100000 0x00400000>;                               | offset = 0x100000, size = 0x400000                           |
| label = "SPI (RO) Linux Kernel Image";                       | 파티션에서 사용하는 레이블                                   |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| partition@500000 {                                           | flash의 파티션 자식 노드, offset = 0x50_0000                 |
| /* 4MB for Compressed RFS Image */                           |                                                              |
| reg = <0x00500000 0x00400000>;                               | offset = 0x500000, size = 0x400000                           |
| label = "SPI (RO) Compressed RFS Image";                     |                                                              |
| read-only;                                                   |                                                              |
| };                                                           |                                                              |
| partition@900000 {                                           | flash의 파티션 자식 노드, offset = 0x90_0000                 |
| /* 7MB for JFFS2 based RFS */                                |                                                              |
| reg = <0x00900000 0x00700000>;                               | offset = 0x900000, size = 0x700000                           |
| label = "SPI (RW) JFFS2 RFS";                                |                                                              |
| };                                                           |                                                              |
| };                                                           |                                                              |
| };                                                           |                                                              |
| usb@22000 {                                                  | USB, SoC로 부터 offset = 0x22000                             |
| phy_type = "ulpi";                                           | ULPI interface, 드라이버에 의해 분석 가능                    |
| dr_mode = "host";                                            | USB host, 드라이버에 의해 분석 가능                          |
| };                                                           |                                                              |
| mdio@24520 {                                                 | MDIO port, SoC로 부터 offset = 0x24520                       |
| phy0: ethernet-phy@0 {                                       | PHY0 자식노드, MDIO로부터 offset = 0x0                       |
| interrupts = <3 1 0 0>;                                      | xIVPR=3, active-low, normal                                  |
| reg = <0x0>;                                                 |                                                              |
| };                                                           |                                                              |
| enet0: ethernet@24000 {                                      | Ethernet 0, SoC로 부터 offset = 0x24000                      |
| fixed-link = <1 1 1000 0 0>;                                 | 드라이버에 의해 분석 가능, fixed link                        |
| phy-connection-type = "rgmii-id";                            | 드라이버에 의해 분석 가능, PHY connection = RGMII            |
| };                                                           |                                                              |
| enet1: ethernet@25000 {                                      | Ethernet 1, SoC로 부터 offset = 0x25000                      |
| tbi-handle = <&tbi0>;                                        | TBI0의 주소                                                  |
| phy-handle = <&phy0>;                                        | Ethernet PHY0의 주소, phandle                                |
| phy-connection-type = "sgmii";                               | 드라이버에 의해 분석 가능, PHY connection = SGMII            |
| };                                                           |                                                              |
| pci0: pcie@ffe08000 {                                        | PCIe, 루트노드로 부터 offset 0xFFE0_8000                     |
| reg = <0 0xffe08000 0 0x1000>;                               | PCI0 레지스터의 address 0xFFE0_8000                          |
| status = "disabled";                                         | 현재는 사용하지 않음                                         |
| };                                                           |                                                              |
| pci1: pcie@ffe09000 {                                        | PCIe, 루트노드로 부터 offset 0xFFE0_9000                     |
| reg = <0 0xffe09000 0 0x1000>;                               | PCI1 레지스터의 address 0xFFE0_9000.  #address-cells 속성이 2로 설정되었기 때문에 (루트노드에서 dtsi파일), Address(64bit) = 0x0_FFE0_9000, Size = 0x1000 |
| ranges = <0x2000000 0x0 0xa0000000 0 0xa0000000 0x0 0x20000000 | PCI 자식노드는 3개의 셀을 사용한다.(phys.hi , phys.mid , phys.low)  phys.hi = 0x2000000은 프리 페치 가능, 구성 공간, 메모리 공간 등과 같은 것들에 대한 바인딩에 정의 된 필드입니다. 이 경우 32 비트 메모리 공간에 매핑됩니다. PCI Address = 0x0_a0000000. 루트 노드로 부터 0x0_a0000000으로 공간 변환. (루트 공간으로부터 address-size를 사용) 윈도우의 크기 = 0x20000000 |
| 0x1000000 0x0 0x00000000 0 0xffc10000 0x0 0x10000>;          | I/O 공간으로 매핑됩니다. PCI Address = 0x0_00000000. 루트 노드로 부터 0x0_ffc10000으로 공간 변환. (루트 공간으로부터 address-size를 사용) 윈도우의 크기 = 0x10000 |
| pcie@0 {                                                     | PCIe 자식노드, 부모 노드로 부터 offset = 0x0 (루트노드로 부터 0xffe09000) |
| ranges = <0x2000000 0x0 0xa0000000                           | PCIe(p2020rdb-post.si)는 3개의 <u32> 주소셀로 정의한다. size = 2 <u32>. phys.hi=0x2000000 = 32-bit memory space. PCI address = 0x0_a0000000. 0x2000000 (phys.hi) , 0x0 (phys.mid) , 0xa0000000 (phys.low). 윈도우의 크기 = 0x20000000 |
| 0x2000000 0x0 0xa0000000                                     |                                                              |
| 0x0 0x20000000                                               |                                                              |
| 0x1000000 0x0 0x0                                            | phys.hi=0x1000000 = I/O space.  PCI address = 0x0_00000000.  0x1000000 (phys.hi) , 0x0 (phys.mid) , 0x0(phys.low).  윈도우의 크기 = 0x1000000 |
| 0x1000000 0x0 0x0                                            |                                                              |
| 0x0 0x100000>;                                               |                                                              |
| };                                                           |                                                              |
| };                                                           |                                                              |
| /include/ "fsl/p2020si-post.dtsi"                            | Include file fsl/p2020si-post.dtsi                           |

