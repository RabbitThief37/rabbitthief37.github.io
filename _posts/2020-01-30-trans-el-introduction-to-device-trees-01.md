---
layout: post
title:  "Introduction to Device Trees - 1"
description: "Introduction to Device Trees - 1"
author: RabbitThief
date: 2020-01-30 17:43:00 +0900
tags: embedded linux pdf 
category: Translation
comments: false
---


원본은 [NXP 사이트](https://www.nxp.com/docs/en/application-note/AN5125.pdf)에서 바로 받을 수 있다.

[Introduction to Device Trees - 2](https://rabbitthief37.github.io/2020/01/31/trans-el-introduction-to-device-trees-02.html)

[Introduction to Device Trees - 3](https://rabbitthief37.github.io/2020/02/03/trans-el-introduction-to-device-trees-03.html)

[Introduction to Device Trees - 4](https://rabbitthief37.github.io/2020/02/04/trans-el-introduction-to-device-trees-04.html)

[Introduction to Device Trees - 5](https://rabbitthief37.github.io/2020/02/06/trans-el-introduction-to-device-trees-05.html)

[Introduction to Device Trees - 6](https://rabbitthief37.github.io/2020/02/07/trans-el-introduction-to-device-trees-06.html)

[Introduction to Device Trees - 7](https://rabbitthief37.github.io/2020/02/13/trans-el-introduction-to-device-trees-07.html)



# 1. Introduction

Device Tree는 시스템의 물리적 하드웨어를 설명하는데 사용되는 트리 구조입니다.  트리의 각 노드는 표시되는 장치의 특성을 설명합니다.  Device Tree의 목적은 클라이언트 프로그램이 동적으로 감지하거나 찾을 수 없는 시스템의 장치 정보를 설명하는 것입니다.  예를 들어, PCI 호스트는 연결된 장치를 probe하고 감지할 수 있습니다.  따라서 PCI 장치를 설명하는 Device Tree 노드가 필요하지 않을 수 있습니다. 그러나 probing으로 감지할 수 없는 경우, 시스템의 PCI host brige를 설명하기 위한 장치 노드가 필요합니다.

Device Tree가 등장하기 전에 커널에는 장치 별 코드가 포함되어 있었습니다. I2C 주변 장치의 주소를 수정하는 것과 같은 작은 변경으로 인해 커널 이미지를 실행하기 위해서 다시 컴파일을 해야만 했습니다.

부트 로더 (예 : U-Boot)는 단일 바이너리(커널 이미지)를 로드하고 실행합니다.

Device Tree 이전에는 복잡성을 줄이고 소량의 정보를 커널에 전달하기 위한 여러 가지 시도가 있었습니다. 부트 로더는 일반적으로 몇 가지 추가 정보를 준비하여, 미리 정의된 레지스터가 가리키는 위치의 메모리에 배치합니다. 이 정보에는 메모리의 크기와 위치같은 기본 정보와 IP 주소와 같은 커널 명령어 정보가 포함됩니다. 커널이 하드 코딩된 초기화 기능이 아닌 하드웨어에 대한 파싱 가능한 정보를 기반으로 하드웨어를 구성할 수 있도록 하는 것이 목표였습니다 (예 : 하드 코딩 된 IP 주소).

Device Tree를 사용하면 커널 자체가 더 이상 각 하드웨어 버전에 대한 특정 코드가 필요하지 않습니다.  대신 그 코드는 Device Tree Blob이라는 별도의 바이너리에 저장합니다. 이를 통해 훨씬 더 간단하고, 훨씬 작은 Device Tree 바이너리만을 변경하여, 동일한 커널 이미지로 다른 하드웨어를 타겟팅할 수 있습니다.

Device Tree는 커널 이미지에 추가하거나 부트 로더를 통해 커널에 전달할 수 있습니다.  장치 유형은 이제 Device Tree 자체에서 정의됩니다. 부트 로더는 일부 정보 (예 : 클럭 주파수)를 Device Tree에 동적으로 추가한 다음 r2 (ARM® 아키텍처의 경우) 또는 r3 (Power Architecture®의 경우)을 통해 시스템 메모리에 있는 트리에 대한 포인터를 전달합니다.  그런 다음 커널은 Device Tree를 읽고 구문 분석을 합니다.



# 2. Basic device tree

Device Tree는  [Power.org (Standard Embedded Power Architecture Platform Requirements)](https://www.power.org/documentation/epapr-version-1-1/) 에 잘 설명되어 있습니다. ePAPR은 시스템 하드웨어를 설명하고, 이 설명을 커널 이미지와 분리하기 위한 개념으로 Device Tree를 정의합니다.

Device Tree는 소프트웨어에서 동적으로 확인할 수 없는 시스템 장치를 설명하는 노드들로 이루어진 트리입니다. 노드는 계층적 부모-자식 관계로 구성됩니다.

<img src="/assets/article_images/2020-01-30/1.png" alt="1" style="zoom:50%;" />

이 그림은 플랫폼 유형, CPU 및 메모리를 설명하는 간단한 Device Tree를 나타냅니다.  노드는 속성 및 가치로 이루어진 토큰들의  집합으로 계층 구조 형식으로 구성됩니다.  하위 노드는 계층 내에서 장치들의 상호 관계를 정의합니다. (예 : I2C 장치는 I2C 컨트롤러 노드의 자식입니다.)

P1010 기반 시스템의 정의를 봅니다. 호환 가능한 키워드는 <manufacturer>, <model> 형식으로 시스템 이름을 지정합니다. 이것은 운영 체제가 장치에서 어떻게 실행할지 결정하는데 사용될 수 있습니다.

또한 트리에서 cpus라는 노드가 단위 주소가 0 인 하나의 CPU를 정의하는 것을 볼 수 있습니다.  이는 노드의 reg 속성값에 의해 단일 CPU를 사용할 수 있음을 나타냅니다.

마찬가지로 Ethernet이라는 노드의 단위 주소 값은 FE001000입니다.

이 예는 Device Tree의 일부에 대한 간단한 예입니다. 다음 섹션에서는 트리에서 노드를 정의하는데 사용되는 구문의 세부 사항뿐만 아니라 고급 예제를 자세히 설명합니다.



# 3. Syntax

Device Tree는 노드와 속성으로 이루어진 단순한 트리 구조입니다. 특성은 키-값 쌍이며, 특성과 하위 노드를 모두 포함 할 수 있습니다. 다음 섹션에서는 Device Tree 노드의 기본 문법과 부모-자식 노드 관계를 살펴봅니다.



## 3.1. Node names

노드 이름은 노드를 식별하는 데 사용되는 레이블입니다.  노드의 구성 요소인 Unit-Address는 노드가 있는 버스의 기본 주소를 알려줍니다.  장치에 접근하는데 사용되는 기본 주소입니다.

자식 노드의 이름은 고유해야 하지만, 동일한 이름으로 노드를 구별하는데 사용되는 "단위 이름"(예: 동일한 SoC의 여러 I2C 장치)으로 해결할 수도 있습니다.  단위 이름은 노드 이름, “@”, 장치 주소 (예 : i2c @ 3000, i2c @ 4000 등등)로 구성됩니다.

동일한 노드에 대한 여러 정의가 Device Tree 컴파일러에 의해 하나로 합쳐집니다.



## 3.2. Properties

노드는 *name = value* 형식으로 구성된 속성들을 포함할 수 있습니다.  *name* 은 문자열만으로, 반면에 *value*는 string, byte, number, phandle 또는 모든 것이 조합된 형식으로 구성됩니다.  

- compatible = "fsl,mpc8610-msi", "fsl,mpic-msi";
- reg = <0 0 0x8000000>;
-  interrupt-parent = <&mpic>;

**NOTE**

> Device Tree에서 숫자( number)는 항상 32-bit big-endian이어야 합니다.  때때로 다수의 32-bit big-endian은 큰 수를 표현하는데 사용합니다.(예를 들어 64-bit)
>



## 3.3. Phandle

phandle(포인터 핸들)은  노드가 다른 노드의 속성에서 참조될 수 있도록 해당 노드의 고유 식별 (32bit) 값입니다. 간단히 말해서, 다른 노드에 대한 포인터가 포함된 노드의 속성입니다. phandle은 각 레이블에 대한 Device Tree 컴파일러 또는 U-Boot에 의해 만들어집니다.

다음 예제에서 <&amp;label>는 DTC에 의해 레이블이 지정된 노드의 phandle로 변환됩니다.

> name@address {
>
> <key> = <&label>;
>
> };



> label: name@adresss {
>
> }

인터럽트에 가장 일반적으로 사용됩니다.  

> mpic: pic@40000 {
>
> ​	interrupt-controller;
>
> ​	#address-cells = <0>;
>
> ​	#interrupt-cells = <4>;
>
> ​	reg = <0x40000 0x40000>;
>
> ​	compatible = "fsl,mpic";
>
> ​	device_type = "open-pic";
>
> };

interrupt-parent는 레이블 mpic을 가진 노드에 phandle을 할당합니다.



## 3.4. Aliases

별칭 노드는 다른 노드의 인덱스입니다.  노드의 속성은 phandles가 아니라 Device Tree 내의 경로입니다.

> aliases {
>
> ​	ethernet0 = &enet0;
>
> ​	ethernet1 = &enet1;
>
> ​	ethernet2 = &enet2;
>
> ​	serial0 = &serial0;
>
> ​	serial1 = &serial1;
>
> ​	pci0 = &pci0;
>
> };