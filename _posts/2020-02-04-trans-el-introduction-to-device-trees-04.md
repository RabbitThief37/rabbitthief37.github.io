---
layout: post
title:  "Introduction to Device Trees - 4"
description: "Introduction to Device Trees - 4"
author: RabbitThief
date: 2020-02-04 15:27:00 +0900
tags: embedded linux pdf 
category: Translation
comments: false
---	


원본은 [NXP 사이트](https://www.nxp.com/docs/en/application-note/AN5125.pdf)에서 바로 받을 수 있다.

[Introduction to Device Trees - 1](https://rabbitthief37.github.io/2020/01/30/trans-el-introduction-to-device-trees-01.html)

[Introduction to Device Trees - 2](https://rabbitthief37.github.io/2020/01/31/trans-el-introduction-to-device-trees-02.html)

[Introduction to Device Trees - 3](https://rabbitthief37.github.io/2020/02/03/trans-el-introduction-to-device-trees-03.html)

[Introduction to Device Trees - 5](https://rabbitthief37.github.io/2020/02/06/trans-el-introduction-to-device-trees-05.html)

[Introduction to Device Trees - 6](https://rabbitthief37.github.io/2020/02/07/trans-el-introduction-to-device-trees-06.html)

[Introduction to Device Trees - 7](https://rabbitthief37.github.io/2020/02/13/trans-el-introduction-to-device-trees-07.html)



# 8. Device tree compiler

Device Tree Compiler(DTC)는 소스를 바이너리 형식으로 컴파일하는 도구이다.  DTC에서 사용하는 소스 코드는 scripts/dtc 안에 있다.

Device Tree Compiler 결과은 부트 로더에 의해 로드되고, 부팅시 Linux 커널에 의해 구문 분석되는 이진 형식인 Device Tree Blob(DTB)입니다.

ARM® 및 ARM® 64-bit 아키텍처에서 빌드시 생성 될 DTB는 arch /../ boot / dts / Makefile에 나열되지만, 언제든지 DTC에서 수동으로 컴파일 할 수 있습니다.

DTC 명령어 사용 형식은 아래와 같습니다.

```shell
dtc [option] <input filename>
```

주요 option은 아래와 같습니다.

- -I <input format>
- -O <output format>
- -b <boot CPU> : set the physical boot cpu

입력 형식은 .dts, .dtb 또는 .fs 일 수 있습니다 (.fs는 현재 파일 시스템 /proc/device-tree 에서 읽습니다).  출력 형식은 .dts, .dtb 또는 .asm 일 수 있습니다.  바이트 채우는 옵션등과 같이 다른 옵션이 많이 있습니다 (-R, -S, -P).  예를 들어, 위에서 언급 한 bsc9131rdb.dts 파일을 컴파일하려면 다음을 수행하십시오. 

```shell
dtc –I dts –O dtb bsc9131rdb.dts > bsc9131rdb.dtb
```

DTC는 DTB를 역컴파일을 하여 쉽게 읽을 수 있게 변경할 수 있습니다.

```shell
dtc –I dtb –O dts bsc9131rdb.dtb > bsc9131rdb_output.dts
```



# 9. U-Boot

U-Boot는 RCW (Reset Configuration Word), 환경 변수 및 하드웨어 구성에서 파생된 정보와 같은 플랫폼별 정보로 FDT (flattened device tree)를 업데이트합니다.  U-Boot의 가장 일반적인 영역은 주파수, MAC 주소, LIODN (Peripheral MMU settings) 및 메모리 크기와 관련이 있습니다.   실제 수정은 보드별로 다르며, U-Boot 코드 이외의 다른 곳에서는 문서화되어 있지 않습니다.  U-Boot 내에서 이 모든 것이 발생하는 핵심 함수는 ft_board_setup() 입니다.

U-Boot 자체는 현재 Freescale 플랫폼에서 Device Tree를 사용하지 않지만, FDT 자체를 보고 조작 할수 있는 몇 가지 명령이 있습니다.

- *bottom*은 FDT 관련 하위 명령어가 있습니다.
  - bootm fdt — relocates the flattened device tree
  - bootm go — performs fix-up actions and boots the operating system
- fdt는 FDT를 조작합니다.
  - fdt addr <addr> [<length>] — sets the FDT location to <addr>
  - fdt boardsetup — performs board-specific setup
  - fdt move <fdt> <newaddr> <length> — copies the FDT to <addr> and makes it active
  - fdt resize — resizes the FDT to size + padding to 4 K address
  - fdt print <path> [<prop>] — recursive print starting at <path>
  - fdt set <path> <prop> [<val>] — sets <property> [to <val>]
  - fdt mknode <path> <node> — creates a new node after <path>
  - fdt rm <path> [<prop>] — deletes the node or <property>
  - fdt header — displays header information
  - fdt chosen [<start> <end>] — adds/updates the /chosen branch in the tree , <start>/<end> — initrd the start/end address



# 10. Linux



## 10.1. Reading the flattened device tree (FDT)

커널 구성 옵션에 **CONFIG_PROC_DEVICETREE**가 설정되어 있으면, 부팅 후 /proc 파일 시스템 내에서 커널이 파싱한 실제 Device Tree를 볼 수 있습니다.

예를 들어 /proc/device-tree 에서 모든 노드를 찾을 수 있습니다.

```shell
[root@p4080ds]# cd /proc/device-tree
[root@p4080ds]# find
.
./name
[...]
./model
./fsl,dpaa/ethernet@0/fsl,qman-channel
[...]
./soc@ffe000000/fman@500000/ethernet@f0000/phy-connection-type
[...]
./soc@ffe000000/dma@100300/dma-channel@100/interrupts
[...]
./chosen/linux,initrd-start
```

dtc 도구를 사용하여 /proc/device-tree를 DTS 파일로 컴파일 할 수도 있습니다.

```shell
[root@p4080DS]# dtc -I fs -O dts /proc/device-tree/
[...]
chosen {
bootargs = "root=/dev/ram rw console=ttyS0,115200 ramdisk_size=128000";
linux,stdout-path = "/soc@ffe000000/serial@11c500";
linux,initrd-start = <0x2f320000>;
linux,initrd-end = <0x2ffffd15>;
};
```



## 10.2. Device tree bindings

Device Tree 바인딩은 특정 유형 및 장치 클래스를 설명하는데 사용되는 구문을 설명합니다.  장치 노드의 **compatible** 속성은 노드가 따르는 특정 바인딩 또는 바인딩을 설명합니다.  커널이 인식하는 Device Tree 바인딩은 Documentation / devicetree / bindings에 문서화되어 있습니다.

각 문서에는 허용되는 속성, 값, 필수 속성 또는 선택적 속성이 설명되어 있습니다.  최신 Devie Tree 바인딩은 upstream에서 찾을 수 있습니다.

예를 들어 아래는 Documentation/devicetree/bindings/powerpc/fsl/ifc.txt에 있는 IFC 바인딩 문서입니다.

![IFC binding document](/assets/article_images/2020-02-04/1.png)



### 10.2.1. Manually parsing bindings

때때로 더 자주 모듈의 경우 Device Tree 바인딩이 문서화되어 있지 않습니다.  커널 소스가 열려 있기 때문에, 코드를 살펴보고 노드 사용 방법과 드라이버 코드를 정확하게 식별할 수 있습니다.

**compatible** 속성 문자열은 장치를 드라이버와 바인딩하는데 사용됩니다.  아래는 Freescale Wireless SDK Release 1.5의 bsc9131rdb.dts 파일에있는 SPI 노드의 예입니다.

```shell
spi@6000 {
	rfphy0: ad9361_phy@0{
		compatible = "fsl,espi-ad_phy","ad9361";
		reg = <0>;
		spi-max-frequency = <20000000>;
		spi-cpha;
		band_group1 = <1 7>;
		band_group2 = <41>;
		band_group3 = <>;
		band_group4 = <13 40>;
		band_group_sniff1 = <>;
		band_group_sniff2 = <13>;
		band_group_sniff3 = <1 7>;
		band_group_sniff4 = <>;
		band_group_config1 = <&pa_en 0 &lna_en 0>;
		band_group_config2 = <&pa_en 0 &lna_en 1>;
		band_group_config3 = <&pa_en 1 &lna_en 0>;
		band_group_config4 = <&pa_en 1 &lna_en 1>;
		reset: reset {
			label = "reset";
			gpios = <&gpio1 2 1>;
		};
		pa_en: pa_en {
			#num-val = <1>;
			label = "pa_en";
			gpios = <&gpio 18 1>;
		};
		lna_en: lna_en {
			#num-val = <1>;
			label = "lna_en";
			gpios = <&gpio 17 1>;
		};
	};
);	
```

노드의 바인딩에서 하드웨어가 fsl, espi-ad_phy 및 ad9361과 호환됨을 알 수 있습니다.  **compatible** 속성은 커널에서 하드웨어를 식별하고 커널에 등록된 드라이버와 일치시키는 데 사용됩니다.

소스를 살펴보면 espi-ad_phy가 ad9361_phy (drivers / of / base.c)에 별칭으로 표시되어 있음을 알 수 있습니다.  추가적인 검색에서 ad9361_phy의 드라이버는 drivers / rf / phy / ad9361.c 있는 것을 찾을 수 있습니다.

```c
#define DRV_NAME "ad9361_phy"
static struct spi_driver ad_phy_driver = {
			.driver = {
					.name = DRV_NAME,
					.bus = &spi_bus_type,
					.owner = THIS_MODULE,
			},
			.probe = ad_phy_probe,
			.remove = __devexit_p(ad_phy_remove),
};
```

드라이버 이름은 ad9361_phy로 커널에 등록 되므로, 이 특정 드라이버가 사용됩니다.

**Probe**는 Device Tree를 구문 분석하는데 사용되는 함수인 ad_phy_probe로 정의됩니다. 이 RF 모듈에 대한 Device Tree 노드의 속성이 사용되는 위치와 방법을 정확히 파악하기 위해서 이 함수를 쓸 수 있습니다.

또 다른 예제로 QorIQ SDK 1.6에 있는 T1040 Device Tree를 살펴 볼 수 있습니다.  다음은 t1040rdb.dts의 내용입니다.

```shell
ucc@2200{
	compatible = "fsl,ucc_hdlc";
	rx-clock-name = "brg2";
	tx-clock-name = "brg2";
	fsl,rx-sync-clock = "none";
	fsl,tx-sync-clock = "none";
	fsl,tx-timeslot = <0xfffffffe>;
	fsl,rx-timeslot = <0xfffffffe>;
	fsl,tdm-framer-type = "e1";
	fsl,tdm-mode = "normal";
	fsl,tdm-id = <1>;
	fsl,siram-entry-id = <2>;
	fsl,inter-loopback;
};
```

이 예제에서 하드웨어는 fsl,ucc_hdlc와 호환이 가능합니다.  하드웨어 디바이스 드라이버는 drivers / net / wan / fsl_ucc.hdlc.c에 있습니다.

```c
#define DRV_DESC "Freescale QE UCC HDLC Driver"
#define DRV_NAME "ucc_hdlc"
static struct platform_driver_ucc_hdlc_driver = {
		.probe = ucc_hdlc_probe,
		.remove = ucc_hdlc_remove,
		.driver = {
				.owner = THIS_MODULE,
				.name = DRV_NAME,
				.pm = HDLC_PM_OPS,
				.of_match_table = fsl_ucc_hdlc_of_match,
		},
};
```

이 경우 **probe** 는 ucc_hdlc_probe로 정의 되고, Device Tree를 구문 분석하는데 사용되는 함수입니다.

