---
layout: post
title:  "Introduction to Device Trees - 2"
description: "Introduction to Device Trees - 2"
author: RabbitThief
date: 2020-01-31 19:23:00 +0900
tags: embedded linux pdf 
category: Translation
comments: false
---


원본은 [NXP 사이트](https://www.nxp.com/docs/en/application-note/AN5125.pdf)에서 바로 받을 수 있다.


[Introduction to Device Trees - 1](https://rabbitthief37.github.io/2020/01/30/trans-el-introduction-to-device-trees-01.html)

[Introduction to Device Trees - 3](https://rabbitthief37.github.io/2020/02/03/trans-el-introduction-to-device-trees-03.html)

[Introduction to Device Trees - 4](https://rabbitthief37.github.io/2020/02/04/trans-el-introduction-to-device-trees-04.html)

[Introduction to Device Trees - 5](https://rabbitthief37.github.io/2020/02/06/trans-el-introduction-to-device-trees-05.html)

[Introduction to Device Trees - 6](https://rabbitthief37.github.io/2020/02/07/trans-el-introduction-to-device-trees-06.html)

[Introduction to Device Trees - 7](https://rabbitthief37.github.io/2020/02/13/trans-el-introduction-to-device-trees-07.html)




# 4. Memory mapping and addressing

주소는 다음 세 가지 속성을 사용하여 지정할 수 있습니다.

- reg
- #address-cells
- #size-cells

주소 지정 가능 장치에는 reg 속성이 있습니다.   이 속성에는 하나 이상의 32비트 정수로 이루어진 cells라고 하는 list를 이용해서 장치에서 사용하는 주소 범위를 표시합니다.  주소와 길이 모두 크기가 가변적이므로, 부모 노드의 #address-cells 및 #size-cells 속성은 자식 노드의 cell의 개수를 정의합니다.

CPU 노드는 간단한 주소 지정 예제를 나타냅니다.  각 CPU에는 고유 ID가 할당되며, CPU ID 크기는 없습니다.

```shell
cpus {
	#address-cells = <1>;
	#size-cells = <0>;	
cpu0: PowerPC,e6500@0 {
	device_type = "cpu";
	reg = <0 1>;
	next-level-cache = <&L2>;
};
cpu1: PowerPC,e65000@2 {
	device_type = "cpu";
	reg = <2 3>;
	next-level-cache = <&L2>;
};
cpu2: PowerPC,e6500@4 {
	device_type = "cpu";
	reg = <4 5>;
	next-level-cache = <&L2>;
};
cpu3: PowerPC,e65000@6 {
	device_type = "cpu";
	reg = <6 7>;
	next-level-cache = <&L2>;
};
};
```

메모리 매핑된 장치에는 CPU 노드에서와 같이 단일 주소 값이 아닌 범위의 주소가 할당됩니다. 부모 노드의 #size-cells는 자식노드의 길이 필드가 얼마나 큰지(32비트)를 나타냅니다.  #address-cells은 자식 노드가 사용하는 32비트 주소 셀의 개수를 나타냅니다.

```shell
{
	#address-cells = <0x1>;
	#size-cells = <0x1>;
	compatible = "fsl,p1022-immr", "simple-bus";
	i2c@3100 {
		reg = <0x3100 0x100>;
	};
}
```

위의 예제에서는 I2C 자식 노드의 reg 속성에 두 개의 셀이 표시됩니다. 첫 번째 셀은 0x3100의 기본 주소에 해당합니다.  두 번째 셀은 크기입니다. 따라서 이 특정 I2C 컨트롤러의 레지스터 맵은 0x3100에서 0x31ff까지입니다.

메모리 매핑된 디바이스는 부모 노드에서 부터 자식 노드의 범위 주소를 지정하기 위해 **ranges** 속성을 포함할 수도 있다.

루트 노드는 CPU의 주소 공간을 설명합니다. 루트의 자식 노드는 CPU의 주소 도메인을 사용하고 명시적 매핑이 필요하지 않습니다. 그러나 루트 노드의 직접적인 자식 노드가 아닌(간접적인) 것들은 CPU의 주소 도메인을 사용하지 않습니다.  Device Tree는 한 도메인에서 다른 도메인으로 주소를 변환하는 방법을 지정해야 합니다.  **ranges** 속성을 통해 이러한 변환이 수행되고, 루트 노드의 간접적인 자식노드가 메모리 매핑된 주소를 얻을 수 있습니다.

```shell
/ {
	#address-cells = <0x2>;
	#size-cells = <0x2>;
	soc@fffe00000 {
		ranges = <0x0 0xf 0xffe00000 0x100000>;
		#address-cells = <0x1>;
		#size-cells = <0x1>;
		compatible = "fsl,p1022-immr", "simple-bus";
		i2c@3100 {
			#address-cells = <0x1>;
			#size-cells = <0x0>;
			cell-index = <0x1>;
			compatible = "fsl-i2c";
			reg = <0x3100 0x100>;
			codec@1a {
				compatible = "wlf,wm8776";
				reg = <0x1a>;
			};
		};
	...
```

ranges 속성은 다음과 같은 형식으로 주소의 범위를 정의합니다. 

< bus-address parent-bus-address size >

- **bus-address** - bus base address, using **#address-size** of this bus node
- **parent-bus-address** - base address in the parent's address space, using **#address-size** of the parent node
- **size** - size of mapping, using **#address-size** of this node

빈 **ranges** 속성은 부모 노드에서 자식 노드로 주소의 변환이 Identity Mapping 뿐임을 나타내며, 이것은 부모노드의 버스 주소 공간이 자식 노드의  버스 주소 공간과 동일 함을 의미합니다.  **ranges** 속성이 없는 것은 빈 **ranges** 속성과 다릅니다.  **ranges** 속성이 없다는 것은 변환이 불가능하다는 것을 의미합니다 (예 : CPU 노드에서).

위의 예제에서 SoC는 다음과 같이 범위를 설정합니다.

- Bus address = 0x0 (using the **#address-size** of the SoC node)

- Parent address = 0x0F_FFE0_0000

  **Number는 Device Tree에서 32bit bit-endian으로 표시됩니다. 그러나 부모 노드의 #address-size가 2로 설정되므로 두 cell을 64비트 0x0000_000F_FFE0_0000 주소로 연결합니다.**

  이 예제에서는 SoC 노드가 이 주소에서 정의됩니다.  이는 QorIQ P1022 장치의 CCSR 기본 주소(또는 내부 레지스터 맵 기본 주소)에 해당합니다.

- Size = 0x10_0000(using **#address-size** of the child node)

이것들은 본질적으로 자식 노드의 주소 0x0을 SoC의 기본 주소인 0xF_FFE0_0000에 매핑합니다.  예를 들어, 정의된 I2C 컨트롤러는 주소 0x3100에 있으며, 이는 기본 주소에서 0x3100의 오프셋 또는 0xF_FFE0_3100의 절대 SoC 주소에 해당합니다.

마지막으로 프로세서 버스에 메모리 매핑되지 않은 장치가 있습니다.  CPU에서 직접 액세스 할 수 없는 간접 주소가 있을 수 있습니다.  이때는 대신 부모 장치의 디바이스 드라이버가 버스 액세스를 담당합니다.

```shell
i2c@3000 {
	gpio3: gpio@21 {
		compatible = "nxp,pca9555";
		reg = <0x21>;
		#gpio-cells = <2>;
		gpio-controller;
		polarity = <0x00>;
	};
...
```

예를 들어, PSC9131rdb.dts에서 가져온 위의 I2C 컨트롤러는 주소 0x21이 할당된 I2C 장치를 보여 주지만, 길이나 범위는 없습니다.

PCI 주소 공간은 CPU 주소 공간과 완전히 분리되어 있으며 주소 변환은 약간 다르게 처리됩니다.  이것은 여전히 **range**, **# address-cells** 및 **# size-cells** 속성을 사용하여 수행됩니다.

```shell
pci1: pcie@ffe09000 {
	reg = <0 0xffe09000 0 0x1000>;
	ranges = <0x2000000 0x0 0xa0000000 0 0xa0000000 0x0 0x20000000
		0x1000000 0x0 0x00000000 0 0xffc10000 0x0 0x10000>;
```

PCI 자식 노드 주소는 phys.hi, phys.mid 및 phys.low로 레이블 된 세 개의 셀을 사용합니다.  이 중 첫 번째 phys.hi는 공간에 대한 정보를 인코딩합니다.  설정 공간, I/O 공간 또는 32/64-bit 메모리를 나타내는 공간 코딩이 가장 흥미로울 수 있습니다.

PCI 자식 노드 주소는 CPU 주소 위치와 크기를 따라서 설정됩니다.  이것들의 크기는 부모 노드의 **#address-cell** 과 **#size-cell** 속성 정의에 따라 결정됩니다.

위의 예제에서, 두 개의 주소가 정의되어 있습니다.

- 32-bit memory region : CPU address  0xa00_0000 --- PCI address 0xa000_0000 , size = 0x2000_0000
- I/O region : CPU address 0xffc1_0000 --- PCI address 0x0 , size = 0x1_0000