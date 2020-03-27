---
layout: post
title:  "Introduction to Device Trees - 7"
description: "Introduction to Device Trees - 7"
author: RabbitThief
date: 2020-02-13 10:00:00 +0900
tags: embedded linux pdf 
category: Translation
comments: false
---	


원본은 [NXP 사이트](https://www.nxp.com/docs/en/application-note/AN5125.pdf)에서 바로 받을 수 있다.

[Introduction to Device Trees - 1](https://rabbitthief37.github.io/2020/01/30/trans-el-introduction-to-device-trees-01.html)

[Introduction to Device Trees - 2](https://rabbitthief37.github.io/2020/01/31/trans-el-introduction-to-device-trees-02.html)

[Introduction to Device Trees - 3](https://rabbitthief37.github.io/2020/02/03/trans-el-introduction-to-device-trees-03.html)

[Introduction to Device Trees - 4](https://rabbitthief37.github.io/2020/02/04/trans-el-introduction-to-device-trees-04.html)

[Introduction to Device Trees - 5](https://rabbitthief37.github.io/2020/02/06/trans-el-introduction-to-device-trees-05.html)

[Introduction to Device Trees - 6](https://rabbitthief37.github.io/2020/02/07/trans-el-introduction-to-device-trees-06.html)



### 11.2.2 ls1021a.dtsi

다음 섹션에서는 LS1021A SoC 하드웨어를 설명하고, 타워 보드 장치 트리에 포함된 ls1021a.dtsi 파일의 정보를 사용합니다.  이 파일에는 두 개의 추가 파일이 포함되어 있으며, 이 파일에는 주석이 없습니다.

| DTS file                                                     | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| \#include "skeleton64.dtsi"                                  |                                                              |
| \#include <dt-bindings/interrupt-controller/arm-gic.h>       | Interrupt controller 정의                                    |
| / {                                                          |                                                              |
| compatible = "fsl,ls1021a";                                  |                                                              |
| interrupt-parent = <&gic>;                                   | Interrupt controller - phandle GIC(arm-gic.h)                |
| \#address-cells = <2>;                                       |                                                              |
| \#size-cells = <2>;                                          |                                                              |
| aliases {                                                    | aliases node                                                 |
| serial0 = &lpuart0;                                          |                                                              |
| serial1 = &lpuart1;                                          |                                                              |
| serial2 = &lpuart2;                                          |                                                              |
| serial3 = &lpuart3;                                          |                                                              |
| serial4 = &lpuart4;                                          |                                                              |
| serial5 = &lpuart5;                                          |                                                              |
| ethernet0 = &enet0;                                          |                                                              |
| ethernet1 = &enet1;                                          |                                                              |
| ethernet2 = &enet2;                                          |                                                              |
| sysclk = &sysclk;                                            |                                                              |
| gpio0 = &gpio1;                                              |                                                              |
| gpio1 = &gpio2;                                              |                                                              |
| gpio2 = &gpio3;                                              |                                                              |
| gpio3 = &gpio4;                                              |                                                              |
| crypto = &crypto;                                            |                                                              |
| };                                                           |                                                              |
| memory@80000000 {                                            | memory address = 0x8000_0000                                 |
| device_type = "memory";                                      |                                                              |
| reg = <0x0 0x80000000 0x0 0x20000000>;                       | address = 0x8000_0000, size = 0x2000_0000                    |
| };                                                           |                                                              |
| cpus {                                                       |                                                              |
| \#address-cells = <1>;                                       |                                                              |
| \#size-cells = <0>;                                          |                                                              |
| cpu@f00 {                                                    | ARM, Cortex®-A7, address = 0xf00                             |
| compatible = "arm,cortex-a7";                                |                                                              |
| device_type = "cpu";                                         |                                                              |
| reg = <0xf00>;                                               | **reg** 속성은 CPU ID 정의. CPU 노드의 주소와 동일해야 함.   |
| clocks = <&cluster1_clk>;                                    |                                                              |
| };                                                           |                                                              |
| cpu@f01 {                                                    | ARM, Cortex®-A7, address = 0xf01                             |
| compatible = "arm,cortex-a7";                                |                                                              |
| device_type = "cpu";                                         |                                                              |
| reg = <0xf01>;                                               |                                                              |
| clocks = <&cluster1_clk>;                                    |                                                              |
| };                                                           |                                                              |
| };                                                           |                                                              |
| soc {                                                        | SoC를 정의하는 상위 레벨 노드                                |
| compatible = "simple-bus";                                   | 특정 드라이버가 없는 메모리 매핑. 자식 노드는 플랫폼 장치로 등록됩니다. |
| \#address-cells = <2>;                                       |                                                              |
| \#size-cells = <2>;                                          |                                                              |
| device_type = "soc";                                         |                                                              |
| interrupt-parent = <&gic>;                                   | interrput를 phandle GIC 전달되도록 설정                      |
| ranges;                                                      | 빈 **ranges** 속성은 아이디 매핑이 사용됨을 나타냅니다.      |
| gic: interrupt-controller@1400000 {                          | phandle GIC, interrupt-controller address = 0x140_0000       |
| compatible = "arm,cortex-a15-gic";                           |                                                              |
| \#interrupt-cells = <3>;                                     | interrupt 지정하는데 필요한 cell의 개수                      |
| interrupt-controller;                                        | interrupt-controller 노드로써 정의                           |
| reg = <0x0 0x1401000 0x0 0x1000>,                            | location 0x140_1000, size 0x1000                             |
| <0x0 0x1402000 0x0 0x1000>,                                  | location 0x1402000, size 0x1000                              |
| <0x0 0x1404000 0x0 0x2000>,                                  | location 0x1404000, size 0x2000                              |
| <0x0 0x1406000 0x0 0x2000>;                                  | location0x1406000, size 0x2000                               |
| interrupts = <GIC_PPI 9 (GIC_CPU_MASK_SIMPLE(2) \| IRQ_TYPE_LEVEL_HIGH)>; | GIC interrupt 정의                                           |
| };                                                           |                                                              |
| ifc: ifc@1530000 {                                           | IFC 노드, address = 0x1530000                                |
| compatible = "fsl,ifc", "simple-bus";                        | "fsl,ifc" driver                                             |
| reg = <0x0 0x1530000 0x0 0x10000>;                           | location 0x1530000, size 0x10000                             |
| interrupts = <GIC_SPI 75 IRQ_TYPE_LEVEL_HIGH>;               | IFC interrupt 정의                                           |
| };                                                           |                                                              |
| qspi: quadspi@1550000 {                                      | QSPI노드, address = 0x1550000                                |
| compatible = "fsl,ls1-qspi";                                 |                                                              |
| \#address-cells = <1>;                                       | 자식노드에 대한 ...                                          |
| \#size-cells = <0>;                                          |                                                              |
| reg = <0x0 0x1550000 0x0 0x10000>,                           | 0x155_0000, size 0x10000                                     |
| <0x0 0x40000000 0x0 0x4000000>;                              | 0x4000_0000 size 0x4000_0000                                 |
| reg-names = "QuadSPI", "QuadSPI-memory";                     | 첫번째 영역 이름 QuadSPI,  두번째 영역 QuadSPI-memory        |
| interrupts = <GIC_SPI 131 IRQ_TYPE_LEVEL_HIGH>;              | QSPI interrupt                                               |
| clock-names = "qspi_en", "qspi";                             | QSPI clock name                                              |
| clocks = <&platform_clk 1>, <&platform_clk 1>;               | QSPI에 해당하는 clock                                        |
| big-endian;                                                  | 주변 장치가 big-endian                                       |
| amba-base = <0x40000000>;                                    | AMBA® base address = 0x4000_0000                             |
| status = "disabled";                                         | DISABLE                                                      |
| };                                                           |                                                              |
| i2c0: i2c@2180000 {                                          | i2c 노드 address = 0x218_0000                                |
| compatible = "fsl,vf610-i2c";                                |                                                              |
| \#address-cells = <1>;                                       |                                                              |
| \#size-cells = <0>;                                          |                                                              |
| reg = <0x0 0x2180000 0x0 0x10000>;                           | 0x2180000, size 0x10000                                      |
| interrupts = <GIC_SPI 88 IRQ_TYPE_LEVEL_HIGH>;               | interrupt for I2C0                                           |
| clock-names = "i2c";                                         | label                                                        |
| clocks = <&platform_clk 1>;                                  | Clocks = phandle platform_clk                                |
| status = "disabled";                                         | 장치가 현재 작동하지 않지만 나중에 작동 할 수 있음을 나타냅니다. |
| };                                                           |                                                              |

