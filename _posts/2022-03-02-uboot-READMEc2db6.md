---
layout: post
title:  "u-boot README"
description: "2022-03-02 uboot-2021.04-r0 README 번역"
author: RabbitThief
date: 2022-03-02 00:00:00 +0900
tags: linux uboot translation 
category: linux
comments: false
---	



# README

```bash
Directory Hierarchy:
====================

/arch                   Architecture specific files
  /arc                  Files generic to ARC architecture
  /arm                  Files generic to ARM architecture
  /m68k                 Files generic to m68k architecture
  /microblaze           Files generic to microblaze architecture
  /mips                 Files generic to MIPS architecture
  /nds32                Files generic to NDS32 architecture
  /nios2                Files generic to Altera NIOS2 architecture
  /powerpc              Files generic to PowerPC architecture
  /riscv                Files generic to RISC-V architecture
  /sandbox              Files generic to HW-independent "sandbox"
  /sh                   Files generic to SH architecture
  /x86                  Files generic to x86 architecture
  /xtensa               Files generic to Xtensa architecture
/api                    Machine/arch independent API for external apps
/board                  Board dependent files
/cmd                    U-Boot commands functions
/common                 Misc architecture independent functions
/configs                Board default configuration files
/disk                   Code for disk drive partition handling
/doc                    Documentation (don't expect too much)
/drivers                Commonly used device drivers
/dts                    Contains Makefile for building internal U-Boot fdt.
/env                    Environment files
/examples               Example code for standalone applications, etc.
/fs                     Filesystem code (cramfs, ext2, jffs2, etc.)
/include                Header Files
/lib                    Library routines generic to all architectures
/Licenses               Various license files
/net                    Networking code
/post                   Power On Self Test
/scripts                Various build scripts and Makefiles
/test                   Various unit test files
/tools                  Tools to build S-Record or U-Boot images, etc.
```

### Software Configuration

구성은 일반적으로 C 전처리기 정의를 사용하여 수행됩니다. 그 배후의 근거는 가능하면 데드 코드를 피하는 것입니다.

이전에는 기호 링크를 만들고 구성 파일을 수동으로 편집하는 모든 구성이 수작업으로 이루어졌습니다. 더 최근에 U-Boot는 Linux 커널에서 사용하는 Kbuild 인프라를 추가하여 "make menuconfig" 명령을 사용하여 빌드를 구성할 수 있도록 했습니다.

지원되는 모든 보드에는 바로 사용할 수 있는 기본 구성이 있습니다. "make <board_name>_defconfig"를 입력하기만 하면 됩니다.

```bash
cd u-boot
make TQM823L_defconfig
```

이전에는 분명히 있었지만 지금은 없는 보드의 기본 구성 파일을 찾고 있다면 doc/README.scrapyard 파일에서 더 이상 지원되지 않는 보드 목록을 확인하십시오.

U-Boot는 기본적으로 '샌드박스' 보드를 사용하여 Linux 호스트에서 실행되도록 구축할 수 있습니다.
이를 통해 보드 또는 아키텍처와 관련되지 않은 기능 개발을 네이티브 플랫폼에서 수행할 수 있습니다. 샌드박스는 또한 U-Boot의 일부 테스트를 실행하는 데 사용됩니다.

자세한 내용은 doc/arch/sandbox.rst를 참조하세요.

### Board Initialisation Flow

***(계열) Cortex - (아키텍처 버전) A53 - (코어) ARMv8-A  → (assembly code) ./arch/arm/cpu/armv8/start.S***  

이것은 보드의 의도된 시작 흐름입니다. 이것은 SPL과 U-Boot에 모두 적용되어야 합니다(즉, 둘 다 동일한 규칙을 따릅니다).

참고: "SPL"은 "Secondary Program Loader"를 나타내며 이 파일의 뒷부분에서 더 자세히 설명합니다.

현재 SPL은 대부분 별도의 코드 경로를 사용하고 있지만 각 기능의 기능 이름과 역할은 동일합니다. 일부 보드 또는 아키텍처는 이를 준수하지 않을 수 있습니다. 적어도 CONFIG_SPL_FRAMEWORK를 사용하는 대부분의 ARM 보드는 이것을 따릅니다.

실행은 일반적으로 다음과 같은 아키텍처별(및 CPU별) start.S 파일로 시작됩니다.

```bash
  - arch/arm/cpu/armv7/start.S
	- arch/powerpc/cpu/mpc83xx/start.S
	- arch/mips/cpu/start.S
```

거기에서 세 가지 함수가 호출됩니다. 이러한 각 기능의 목적과 제한 사항은 아래에 설명되어 있습니다.

**lowlevel_init():**

- 목적: 실행이 board_init_f()에 도달하도록 허용하는 필수 초기화
- global_data 또는 BSS 없음
- 스택이 없습니다(ARMv7에 스택이 있을 수 있지만 곧 제거됨)
- SDRAM을 설정하거나 콘솔을 사용해서는 안 됩니다.
- board_init_f()를 계속 실행하려면 최소한의 작업만 수행해야 합니다.
- 거의 필요하지 않습니다.
- 이 함수에서 정상적으로 반환

**board_init_f():**

- 목적: board_init_r()을 실행할 준비가 된 머신 설정: 즉 SDRAM 및 직렬 UART
- global_data를 사용할 수 있습니다.
- 스택이 SRAM에 있음
- BSS를 사용할 수 없으므로 전역/정적 변수를 사용할 수 없으며, 스택 변수와 global_data만 사용할 수 있습니다.
- Non-SPL-specific notes:
    - dram_init()는 DRAM을 설정하기 위해 호출됩니다. SPL에서 이미 수행된 경우 아무 것도 할 수 없습니다.
- SPL-specific notes:
    - 필요에 따라 전체 board_init_f() 함수를 자신의 버전으로 재정의할 수 있습니다.
    - preloader_console_init()는 극단에서 여기에서 호출될 수 있습니다.
    - SDRAM 및 UART 작동에 필요한 모든 것을 설정해야 합니다.
    - BSS를 지울 필요가 없습니다. crt0.S에 의해 완료됩니다.
    - 특정 아키텍처에 대한 특정 시나리오의 경우 초기 BSS를 *사용*할 수 있지만(board_init_f()를 입력하기 전에 BSS 지우기를 이동하여 CONFIG_SPL_EARLY_BSS를 통해) 그렇게 하는 것은 권장되지 않습니다. 대신 전체 코드 기반에서 호환성과 일관성을 유지하기 위해 이 README의 다른 섹션에 표시된 대로 board_init_f() 동안 BSS의 가용성에 의존하지 않도록 코드 변경 또는 추가를 설계하는 것이 좋습니다.
    - 이 함수에서 정상적으로 반환되어야 합니다(board_init_r()를 직접 호출하지 마십시오).

여기서 BSS가 지워집니다. SPL의 경우 CONFIG_SPL_STACK_R이 정의되어 있으면 이 시점에서 스택과 global_data가 CONFIG_SPL_STACK_R_ADDR 아래로 재배치됩니다. 비 SPL의 경우 U-Boot는 메모리 상단에서 실행되도록 재배치됩니다.

**board_init_r():**

- CONFIG_SPL_STACK_R이 정의되고 CONFIG_SPL_STACK_R_ADDR이 SDRAM을 가리키는 경우 스택은 선택적으로 SDRAM에 있습니다.
- preloader_console_init()를 여기에서 호출할 수 있습니다. - 일반적으로 CONFIG_SPL_BOARD_INIT를 선택한 다음 이 호출을 포함하는 spl_board_init() 함수를 제공하여 수행됩니다.
- U-Boot 또는 (팔콘 모드에서) Linux를 로드합니다.

### Configuration Options

CONFIG_SYS_BOOTM_LEN

일반적으로 압축된 uImage는 압축되지 않은 크기가 8MB로 제한됩니다. 이것으로 충분하지 않으면 보드 구성 파일에서 CONFIG_SYS_BOOTM_LEN을 정의하여 이 설정을 필요에 맞게 조정할 수 있습니다.

CONFIG_SPL_MAX_SIZE

SPL 이미지의 최대 크기(text, data, rodata, and linker lists sections), BSS 제외. 정의되면 링커는 실제 크기가 이를 초과하지 않는지 확인합니다.

CONFIG_SYS_MONITOR_LEN

모니터 코드용으로 예약된 메모리 크기로, 환경이 U-Boot 이미지 또는 별도의 플래시 섹터에 포함된 경우 *at_compile_time*(!)을 결정하는 데 사용됩니다.

CONFIG_SPL_BUILD

현재 실행 중인 컴파일이 SPL로 끝나는 아티팩트에 대한 것일 때 설정됩니다(TPL 또는 U-Boot의 적절한 반대). 단계별 동작이 필요한 코드는 이를 확인해야 합니다.

CONFIG_SPL_STACK

SPL이 사용할 스택 시작 주소

CONFIG_SPL_BSS_START_ADDR

SPL 바이너리 내의 BSS에 대한 링크 주소입니다.

CONFIG_SPL_BSS_MAX_SIZE

SPL BSS에 할당된 메모리의 최대 크기입니다. 정의되면 링커는 __bss_start에서 __bss_end까지 SPL에서 사용하는 실제 메모리가 초과하지 않는지 확인합니다. CONFIG_SPL_MAX_FOOTPRINT 및 CONFIG_SPL_BSS_MAX_SIZE를 동시에 정의하면 안 됩니다.

CONFIG_SYS_SPL_MALLOC_START

SPL에서 사용되는 malloc 풀의 시작 주소입니다. 이 옵션을 설정하면 SPL에서 전체 malloc을 사용하고 spl_init()으로 설정하고 그 이전에는 CONFIG_SYS_MALLOC_F가 정의되어 있으면 단순 malloc()을 사용할 수 있습니다.

CONFIG_SYS_SPL_MALLOC_SIZE

SPL에서 사용되는 malloc 풀의 크기입니다.

CONFIG_SPL_NAND_IDENT

SPL은 칩 ID 목록을 사용하여 NAND 플래시를 식별합니다. CONFIG_SPL_NAND_BASE가 필요합니다.

CONFIG_EXTRA_ENV_SETTINGS

부팅 이미지로 컴파일된 기본 환경의 일부가 될 null 종료 문자열(변수 = 값 쌍)을 원하는 수만큼 포함하도록 정의합니다.

경고: 이 방법은 U-Boot 코드에 의해 환경이 저장되는 내부 형식에 대한 지식을 기반으로 합니다. 이것은 공식적으로 내보낸 인터페이스가 아닙니다! 이 형식이 곧 변경될 것 같지는 않지만 보장도 없습니다. 당신이 여기서 무엇을 하고 있는지 더 잘 알 것입니다.

참고: 기본 환경을 과도하게 (ab) 사용하는 것은 권장되지 않습니다. "source" 명령이나 boot 명령과 같은 환경을 사전 설정하는 다른 방법을 먼저 확인하십시오.

CONFIG_SYS_INIT_RAM_ADDR

초기 데이터 및 스택에 사용할 수 있는 메모리 영역의 시작 주소입니다. 이것은 특별한 초기화 없이 작동하는 쓰기 가능한 메모리여야 합니다. 즉, 메모리 컨트롤러를 프로그래밍하고 특정 초기화 시퀀스를 실행한 후에만 사용할 수 있는 일반 RAM을 사용할 수 없습니다.

CONFIG_SYS_INIT_SP_OFFSET

초기 스택 포인터에 대한 CONFIG_SYS_SDRAM_BASE에 상대적인 오프셋입니다. 이것은 재배치 전 임시 스택에 필요합니다.

CONFIG_SYS_MALLOC_LEN

malloc() 사용을 위해 예약된 DRAM의 크기입니다.

CONFIG_SYS_SDRAM_BASE

SDRAM의 물리적 시작 주소입니다. *반드시*  0이어야 합니다.

CONFIG_SYS_CBSIZE

콘솔에서 입력을 위한 버퍼 크기

CONFIG_SYS_MAXARGS

최대 모니터 명령에 허용되는 인수 수

CONFIG_SYS_BARGSIZE

부팅될 때 응용 프로그램(일반적으로 Linux 커널)에 전달되는 부팅 인수의 버퍼 크기

CONFIG_SYS_PBSIZE

콘솔 출력의 버퍼 크기

CONFIG_SYS_NAND_5_ADDR_CYCLE

SPL이 U-Boot를 읽는 데 사용하는 NAND의 크기와 동작을 정의합니다.

CONFIG_SPL_BUILD

현재 실행 중인 컴파일이 SPL로 끝나는 아티팩트에 대한 것일 때 설정됩니다(TPL 또는 U-Boot의 적절한 반대). 단계별 동작이 필요한 코드는 이를 확인해야 합니다.

CONFIG_USBD_HS

USB 장치 및 usbtty에 대한 고속 지원을 활성화하려면 이것을 정의하십시오. 이 기능이 활성화된 경우 열거형이 고속 또는 최대 속도로 성공했는지 여부를 동적으로 폴링하려면 드라이버에서 루틴 "int is_usbd_high_speed(void)"도 정의해야 합니다.

### Board initialization settings

초기화하는 동안 u-boot는 보드 특정 전제 조건을 준비할 수 있도록 여러 보드 특정 기능을 호출합니다. 드라이버가 초기화되기 전에 핀 설정. 이러한 콜백을 활성화하려면 다음 구성 매크로를 정의해야 합니다. 현재 이것은 아키텍처에 따라 다르므로 일반적으로 board_init_f() 및 board_init_r()에서 arch/your_architecture/lib/board.c를 확인하십시오.

```makefile
- CONFIG_BOARD_EARLY_INIT_F: Call board_early_init_f()
- CONFIG_BOARD_EARLY_INIT_R: Call board_early_init_r()
- CONFIG_BOARD_LATE_INIT: Call board_late_init()
- CONFIG_BOARD_POSTCLK_INIT: Call board_postclk_init()
```

IN configs/imx8mn_ddr4_ab2_defconfig

```makefile
CONFIG_BOARD_LATE_INIT=y
CONFIG_BOARD_EARLY_INIT_F=y
```

### Reproducible builds

재현 가능한 빌드를 달성하려면 U-Boot 빌드 프로세스에서 사용되는 타임스탬프를 고정 값으로 설정해야 합니다.

이것은 SOURCE_DATE_EPOCH 환경 변수를 사용하여 수행됩니다.
SOURCE_DATE_EPOCH는 U-Boot의 구성 옵션이나 U-Boot의 환경 변수가 아닌 빌드 호스트의 셸에서 설정됩니다.

SOURCE_DATE_EPOCH는 UTC로 epoch 이후의 초 수로 설정되어야 합니다.

### Building the Software

U-Boot 빌드는 여러 기본 빌드 환경과 다양한 교차 환경에서 테스트되었습니다. 물론 모든(잠재적으로 사용되지 않는) 버전에서 교차 개발 도구의 기존 버전을 모두 지원할 수는 없습니다. 도구 체인 문제의 경우 U-Boot를 빌드하고 테스트하는 데 광범위하게 사용되는ELDK([https://www.denx.de/wiki/DULG/ELDK](https://www.denx.de/wiki/DULG/ELDK) 참조)를 사용하는 것이 좋습니다.

기본 환경을 사용하지 않는 경우 경로에서 GNU 크로스 컴파일 도구를 사용할 수 있다고 가정합니다. 이 경우 쉘에서 환경 변수 CROSS_COMPILE을 설정해야 합니다.
Makefile이나 다른 소스 파일을 변경할 필요는 없습니다. 예를 들어 4xx CPU에서 ELDK를 사용하려면 다음을 입력하십시오.

```makefile
$ CROSS_COMPILE=ppc_4xx-
$ export CROSS_COMPILE
```

U-Boot는 간단하게 구축할 수 있습니다. 소스를 설치한 후 하나의 특정 보드 유형에 대해 U-Boot를 구성해야 합니다. 다음을 입력하면 됩니다.

```makefile
make NAME_defconfig
```

여기서 "NAME_defconfig"는 기존 구성 중 하나의 이름입니다. 지원되는 이름은 configs/*_defconfig를 참조하십시오.

마지막으로 "make all"을 입력하면 시스템에 다운로드/설치할 준비가 된 작동 중인 U-Boot 이미지를 얻을 수 있습니다.

```makefile
- "u-boot.bin" is a raw binary image
- "u-boot" is an image in ELF binary format
- "u-boot.srec" is in Motorola S-Record format
```

기본적으로 빌드는 로컬에서 수행되고 개체는 소스 디렉터리에 저장됩니다. 두 가지 방법 중 하나를 사용하여 이 동작을 변경하고 U-Boot를 일부 외부 디렉터리에 빌드할 수 있습니다.

1. Add O= to the make command line invocations

```makefile
make O=/tmp/build distclean
make O=/tmp/build NAME_defconfig
make O=/tmp/build all
```

1. Set environment variable KBUILD_OUTPUT to point to the desired location

```makefile
export KBUILD_OUTPUT=/tmp/build
make distclean
make NAME_defconfig
make all
```

명령줄 "O=" 설정은 KBUILD_OUTPUT 환경 변수를 재정의합니다.

사용자별 CPPFLAGS, AFLAGS 및 CFLAGS는 환경 변수 KCPPFLAGS, KAFLAGS 및 KCFLAGS를 설정하여 컴파일러에 전달할 수 있습니다. 예를 들어 모든 컴파일러 경고를 오류로 처리하려면:

```makefile
make KCFLAGS=-Werror
```

Makefile은 GNU make를 사용한다고 가정하므로 예를 들어 NetBSD에서 기본 "make" 대신 "gmake"를 사용해야 할 수도 있습니다.

가지고 있는 시스템 보드가 목록에 없으면 U-Boot를 하드웨어 플랫폼으로 이식해야 합니다. 이렇게 하려면 다음 단계를 따르세요.

1. 보드 특정 코드를 저장할 새 디렉토리를 만듭니다. 필요한 파일을 추가하십시오. 보드 디렉토리에는 최소한 "Makefile"과 "<board>.c"가 필요합니다.
2. 보드에 대한 새 구성 파일 "include/configs/<board>.h"를 생성합니다.
3. U-Boot를 새 CPU로 이식하는 경우 CPU 특정 코드를 저장할 새 디렉토리도 만드십시오. 필요한 파일을 추가하십시오.
4. 새 이름으로 "make <board>_defconfig"를 실행합니다.
5. "make"를 입력하면 대상 시스템에 설치될 "u-boot.srec" 파일이 작동해야 합니다.
6. 발생할 수 있는 문제를 디버그하고 해결합니다.
물론 이 마지막 단계는 생각보다 훨씬 어렵습니다.

### Testing of U-Boot Modifications, Ports to New Hardware, etc.

U-Boot 소스를 수정한 경우(예: 새 보드를 추가하거나 새 장치, 새 CPU 등에 대한 지원을 추가한 경우) 다른 개발자에게 피드백을 제공해야 합니다. 피드백은 일반적으로 "패치"의 형태를 취합니다. 즉, U-Boot 소스의 특정(최신 공식 또는 최신 버전의 git 저장소) 버전에 대한 컨텍스트 차이입니다.

그러나 그러한 패치를 제출하기 전에 수정으로 인해 기존 코드가 손상되지 않았는지 확인하십시오. 최소한 지원되는 보드의 *ALL*이 컴파일러 경고 없이 컴파일되는지 확인하십시오. 이렇게 하려면 지원되는 모든 시스템에 대해 U-Boot를 구성하고 빌드하는 buildman 스크립트(tools/buildman/buildman)를 실행하기만 하면 됩니다. 시간이 좀 걸립니다. buildman README를 참조하거나 문서를 보려면 'buildman -H'를 실행하십시오.

### Monitor Commands

```makefile
go	- start application at address 'addr'
run	- run commands in an environment variable
bootm	- boot application image from memory
bootp	- boot image via network using BootP/TFTP protocol
bootz   - boot zImage from memory
tftpboot- boot image via network using TFTP protocol
	       and env variables "ipaddr" and "serverip"
	       (and eventually "gatewayip")
tftpput - upload a file via network using TFTP protocol
rarpboot- boot image via network using RARP/TFTP protocol
diskboot- boot from IDE devicebootd   - boot default, i.e., run 'bootcmd'
loads	- load S-Record file over serial line
loadb	- load binary file over serial line (kermit mode)
md	- memory display
mm	- memory modify (auto-incrementing)
nm	- memory modify (constant address)
mw	- memory write (fill)
ms	- memory search
cp	- memory copy
cmp	- memory compare
crc32	- checksum calculation
i2c	- I2C sub-system
sspi	- SPI utility commands
base	- print or set address offset
printenv- print environment variables
pwm	- control pwm channels
setenv	- set environment variables
saveenv - save environment variables to persistent storage
protect - enable or disable FLASH write protection
erase	- erase FLASH memory
flinfo	- print FLASH memory information
nand	- NAND memory operations (see doc/README.nand)
bdinfo	- print Board Info structure
iminfo	- print header information for application image
coninfo - print console devices and informations
ide	- IDE sub-system
loop	- infinite loop on address range
loopw	- infinite write loop on address range
mtest	- simple RAM test
icache	- enable or disable instruction cache
dcache	- enable or disable data cache
reset	- Perform RESET of the CPU
echo	- echo args to console
version - print monitor version
help	- print online help
?	- alias for 'help'
```

U-Boot는 플래시 메모리에 저장하여 영구적으로 만들 수 있는 환경 변수를 사용하여 사용자 구성을 지원합니다.

환경 변수는 "setenv"를 사용하여 설정하고 "printenv"를 사용하여 인쇄하고 "saveenv"를 사용하여 Flash에 저장합니다. 값 없이 "setenv"를 사용하면 환경에서 변수를 삭제할 수 있습니다. 환경을 저장하지 않는 한 메모리 내 복사본으로 작업하고 있습니다. 환경이 포함된 Flash 영역이 실수로 지워지는 경우 기본 환경이 제공됩니다.

일부 구성 옵션은 환경 변수를 사용하여 설정할 수 있습니다.

다음 이미지 위치 변수에는 부팅에 사용되는 이미지의 위치가 포함됩니다. "이미지" 열은 이미지의 역할을 제공하며 환경 변수 이름이 아닙니다. 다른 열은 환경 변수 이름입니다. "파일 이름"은 TFTP 서버의 파일 이름을 제공하고, "RAM 주소"는 이미지가 로드될 RAM의 위치를 제공하고, "플래시 위치"는 NOR 플래시의 이미지 주소 또는 NAND 플래시의 오프셋을 제공합니다.

```makefile
Image		    File Name	     RAM Address       Flash Location
-----		    ---------	     -----------       --------------
u-boot		    u-boot	     u-boot_addr_r     u-boot_addr
	Linux kernel	    bootfile	     kernel_addr_r     kernel_addr
device tree blob    fdtfile	     fdt_addr_r	       fdt_addr
ramdisk		    ramdiskfile	     ramdisk_addr_r    ramdisk_addr
```

다음 환경 변수는 부트 서버에서 제공하는 정보에 따라 네트워크 부트 명령("bootp" 및 "rarpboot")에 의해 사용되고 자동으로 업데이트될 수 있습니다.

```makefile
  bootfile	- see above
  dnsip		- IP address of your Domain Name Server
  dnsip2	- IP address of your secondary Domain Name Server
  gatewayip	- IP address of the Gateway (Router) to use
  hostname	- Target hostname
  ipaddr	- see above
  netmask	- Subnet Mask
  rootpath	- Pathname of the root filesystem on the NFS server
  serverip	- see above
```

### Callback functions for environment variables

일부 환경 변수의 경우 값이 변경되면 u-boot의 동작도 변경되어야 합니다. 이 기능을 사용하면 함수를 임의의 변수와 연결할 수 있습니다. 생성, 덮어쓰기 또는 삭제 시 콜백은 부작용이 발생하거나 변경이 거부될 수 있는 기회를 제공합니다.

콜백은 보드 또는 드라이버 코드에서 U_BOOT_ENV_CALLBACK 매크로를 사용하여 이름이 지정되고 함수와 연결됩니다.

이러한 콜백은 두 가지 방법 중 하나로 변수와 연결됩니다. 연결 목록을 정의하는 문자열에 보드 구성의 CONFIG_ENV_CALLBACK_LIST_STATIC을 정의하여 정적 목록을 추가할 수 있습니다. 목록은 다음 형식이어야 합니다.

```makefile
  entry = variable_name[:callback_name]
	list = entry[,list]
```

콜백 이름을 지정하지 않으면 콜백이 삭제됩니다.
목록의 모든 위치에 공백도 허용됩니다.

콜백은 위의 동일한 목록 형식으로 ".callbacks" 변수를 정의하여 연결할 수도 있습니다. ".callbacks"의 모든 연결은 정적 목록의 모든 연결을 재정의합니다. CONFIG_ENV_CALLBACK_LIST_DEFAULT를 목록(문자열)에 정의하여 기본 또는 포함된 환경에서 ".callbacks" 환경 변수를 정의할 수 있습니다.

CONFIG_REGEX가 정의되면 위의 variable_name은 정규식으로 평가됩니다. 이를 통해 여러 변수를 명시적으로 나열하지 않고도 동일한 콜백에 연결할 수 있습니다.

콜백 함수의 서명은

```makefile
int callback(const char *name, const char *value, enum env_op op, int flags)

* name - changed environment variable
* value - new value of the environment variable
* op - operation (create, overwrite, or delete)
* flags - attributes of the environment variable change, see flags H_* in include/search.h
```

반환 값은 변수 변경이 수락되면 0이고 그렇지 않으면 1입니다.

### Note for Redundant Ethernet Interfaces

일부 보드에는 중복 이더넷 인터페이스가 있습니다. U-Boot는 이러한 구성을 지원하며 필요할 때 "작동" 인터페이스를 자동으로 선택할 수 있습니다. MAC 할당은 다음과 같이 작동합니다.

네트워크 인터페이스의 번호는 eth0, eth1, eth2, ... 해당 MAC 주소는 환경에 "ethaddr"(=>eth0), "eth1addr"(=>eth1), "eth2addr", ...로 저장할 수 있습니다.

네트워크 인터페이스가 일부 유효한 MAC 주소(예: SROM에 저장)를 저장하는 경우 환경에 해당 설정이 없는 경우 이 주소가 기본 주소로 사용됩니다. 해당 환경 변수가 설정되면 카드의 설정보다 우선 적용됩니다. 그것의 의미는:

o SROM에 유효한 MAC 주소가 있고 환경에 주소가 없는 경우 SROM의 주소가 사용됩니다.

o SROM에 유효한 주소가 없고 환경에 정의가 있는 경우 환경 변수의 값이 사용됩니다.

o SROM과 환경 모두에 MAC 주소가 포함되어 있고 두 주소가 동일한 경우 이 MAC 주소가 사용됩니다.

o SROM과 환경 모두에 MAC 주소가 포함되어 있고 주소가 다른 경우 환경의 값이 사용되며 경고가 인쇄됩니다.

o SROM이나 환경에 MAC 주소가 없으면 오류가 발생합니다. CONFIG_NET_RANDOM_ETHADDR이 정의된 경우 이 경우 로컬에서 할당된 임의의 MAC이 사용됩니다.

이더넷 드라이버가 'write_hwaddr' 기능을 구현하면 유효한 MAC 주소가 초기화 프로세스의 일부로 하드웨어에 프로그래밍됩니다. 이는 적절한 'ethmacskip' 환경 변수를 설정하여 건너뛸 수 있습니다. 명명 규칙은 다음과 같습니다.
"ethmacskip"(=>eth0), "eth1macskip"(=>eth1) 등

### Image Formats

U-Boot는 두 가지 형식으로 이미지를 부팅(및 기타 보조 작업 수행)할 수 있습니다.

**New uImage format (FIT)**

Flattened Image Tree -- FIT(Flattened Device Tree와 유사)를 기반으로 하는 유연하고 강력한 형식. SHA1, MD5 또는 CRC32로 보호되는 콘텐츠와 함께 여러 구성 요소(여러 커널, 램디스크 등)가 있는 이미지를 사용할 수 있습니다. 자세한 내용은 doc/uImage.FIT 디렉토리에서 찾을 수 있습니다.

**Old uImage format**

이전 이미지 형식은 기본적으로 무엇이든 될 수 있는 바이너리 파일을 기반으로 하며 앞에 특수 헤더가 있습니다. 자세한 내용은 include/image.h의 정의를 참조하십시오. 기본적으로 헤더는 다음 이미지 속성을 정의합니다.

- Target Operating System (Provisions for OpenBSD, NetBSD, FreeBSD, 4.4BSD, Linux, SVR4, Esix, Solaris, Irix, SCO, Dell, NCR, VxWorks, LynxOS, pSOS, QNX, RTEMS, INTEGRITY;
Currently supported: Linux, NetBSD, VxWorks, QNX, RTEMS, LynxOS,INTEGRITY)
- Target CPU Architecture (Provisions for Alpha, ARM, Intel x86,IA64, MIPS, NDS32, Nios II, PowerPC, IBM S390, SuperH, Sparc, Sparc 64 Bit;
Currently supported: ARM, Intel x86, MIPS, NDS32, Nios II, PowerPC)
- Compression Type (uncompressed, gzip, bzip2)
- Load Address
- Entry Point
- Image Name
- Image Timestamp

헤더는 특수 매직 넘버로 표시되며 헤더와 이미지의 데이터 부분은 CRC32 체크섬에 의해 손상되지 않도록 보호됩니다.

### Linux Support

U-Boot는 모든 OS 또는 독립 실행형 응용 프로그램을 쉽게 지원해야 하지만 U-Boot를 설계하는 동안 주요 초점은 항상 Linux에 있었습니다.

U-Boot에는 지금까지 Linux 커널 내의 일부 특수 "부트 로더" 코드의 일부였던 많은 기능이 포함되어 있습니다. 또한 사용할 "initrd" 이미지는 더 이상 하나의 큰 Linux 이미지의 일부가 아닙니다. 대신 커널과 "initrd"는 별도의 이미지입니다. 이 구현은 다음과 같은 여러 용도로 사용됩니다.

- 동일한 기능을 다른 OS 또는 독립 실행형 애플리케이션에 사용할 수 있습니다(예: 압축된 이미지를 사용하여 플래시 메모리 사용 공간 줄이기).
- 많은 하위 수준의 하드웨어 종속 작업이 U-Boot에서 수행되기 때문에 새 Linux 커널 버전을 이식하는 것이 훨씬 쉬워집니다.
- 이제 동일한 Linux 커널 이미지를 다른 "initrd" 이미지와 함께 사용할 수 있습니다. 물론 이것은 다른 커널 이미지가 동일한 "initrd"로 실행될 수 있음을 의미합니다. 이것은 테스트를 더 쉽게 만듭니다("initrd"에서 파일을 변경할 때 새로운 "zImage.initrd" Linux 이미지를 빌드할 필요가 없습니다). 또한 소프트웨어의 현장 업그레이드가 더 쉬워졌습니다.

### Porting Linux to U-Boot based systems

U-Boot는 대상 하드웨어와 함께 사용하기 위해 Linux 장치 드라이버를 구성하는 데 필요한 모든 수정 작업을 수행하지 않아도 됩니다.

그러나 이제 모든 부트 로더 코드(arch/powerpc/mbxboot에 있음)를 무시할 수 있습니다.

시스템 특정 헤더 파일(예: include/asm-ppc/tqm8xx.h)이 include/asm-<arch>/u-boot.h에 정의한 것과 동일한 보드 정보 구조 정의를 포함하고 있는지 확인하십시오. IMAP_ADDR의 정의가 CONFIG_SYS_IMMR의 U-Boot 구성과 동일한 값을 사용하는지 확인하십시오.

U-Boot에는 이제 드라이버를 위한 통합 모델인 드라이버 모델이 있습니다. 새 드라이버를 추가하는 경우 드라이버 모델에 연결합니다. 사용 가능한 uclass가 없으면 하나 만드는 것이 좋습니다. "doc/driver-model"을 참조하십시오.

### Configuring the Linux kernel

U-Boot에 대한 특정 요구 사항은 없습니다. 대상 시스템에 대한 일부 루트 장치(초기 램디스크, NFS)가 있는지 확인하십시오.

### Building a Linux Image

U-Boot에서는 "zImage" 또는 "bzImage"와 같은 "일반" 빌드 대상이 사용되지 않습니다. 최신 커널 소스를 사용하는 경우 U-Boot에서 사용할 수 있는 이미지를 자동으로 빌드하는 새 빌드 대상 "uImage"가 존재합니다. 대부분의 오래된 커널은 이전 프로젝트 PPCBoot에 도입되었으며 100% 호환 가능한 형식을 사용하는 "pImage" 대상도 지원합니다.

```makefile
Example:

	make TQM850L_defconfig
	make oldconfig
	make dep
	make uImage
```

"uImage" 빌드 대상은 U-Boot와 함께 사용하기 위한 헤더 정보, CRC32 체크섬 등으로 압축된 Linux 커널 이미지를 캡슐화하는 특수 도구('tools/mkimage'에 있음)를 사용합니다. 이것이 우리가 하는 일입니다:

- 표준 "vmlinux" 커널 이미지 빌드(ELF 바이너리 형식):
- 커널을 원시 바이너리 이미지로 변환

```makefile
${CROSS_COMPILE}-objcopy -O binary \
				 -R .note -R .comment \
				 -S vmlinux linux.bin
```

- 바이너리 이미지 압축:

```makefile
gzip -9 linux.bin
```

- U-Boot용 패키지 압축 바이너리 이미지:

```makefile
mkimage -A ppc -O linux -T kernel -C gzip \
		-a 0 -e 0 -n "Linux Kernel Image" \
		-d linux.bin.gz uImage
```

"mkimage" 도구는 Linux 커널 이미지에서 분리하거나 하나의 파일로 결합하여 U-Boot와 함께 사용할 램디스크 이미지를 만드는 데 사용할 수도 있습니다. "mkimage"는 대상 아키텍처, 운영 체제, 이미지 유형, 압축 방법, 진입점, 타임스탬프, CRC32 체크섬 등에 대한 정보를 포함하는 64바이트 헤더로 이미지를 캡슐화합니다.

"mkimage"는 기존 이미지를 확인하고 헤더 정보를 인쇄하거나 새 이미지를 빌드하는 두 가지 방법으로 호출할 수 있습니다.

첫 번째 형식("-l" 옵션 사용)에서 mkimage는 기존 U-Boot 이미지의 헤더에 포함된 정보를 나열합니다. 여기에는 체크섬 확인이 포함됩니다.

```makefile
tools/mkimage -l image
	  -l ==> list image header information
```

두 번째 형식("-d" 옵션 포함)은 이미지 페이로드로 사용되는 "데이터 파일"에서 U-Boot 이미지를 빌드하는 데 사용됩니다.

```makefile
tools/mkimage -A arch -O os -T type -C comp -a addr -e ep \
		      -n name -d data_file image
	  -A ==> set architecture to 'arch'
	  -O ==> set operating system to 'os'
	  -T ==> set image type to 'type'
	  -C ==> set compression type 'comp'
	  -a ==> set load address to 'addr' (hex)
	  -e ==> set entry point to 'ep' (hex)
	  -n ==> set image name to 'name'
	  -d ==> use image data from 'datafile'
```

따라서 U-Boot 이미지를 빌드하기 위한 일반적인 호출은 다음과 같습니다.

```makefile
-> tools/mkimage -n '2.4.4 kernel for TQM850L' \
	> -A ppc -O linux -T kernel -C gzip -a 0 -e 0 \
	> -d /opt/elsk/ppc_8xx/usr/src/linux-2.4.4/arch/powerpc/coffboot/vmlinux.gz \
	> examples/uImage.TQM850L
	Image Name:   2.4.4 kernel for TQM850L
	Created:      Wed Jul 19 02:34:59 2000
	Image Type:   PowerPC Linux Kernel Image (gzip compressed)
	Data Size:    335725 Bytes = 327.86 kB = 0.32 MB
	Load Address: 0x00000000
	Entry Point:  0x00000000
```

이미지 내용 확인(또는 손상 확인):

```makefile
-> tools/mkimage -l examples/uImage.TQM850L
	Image Name:   2.4.4 kernel for TQM850L
	Created:      Wed Jul 19 02:34:59 2000
	Image Type:   PowerPC Linux Kernel Image (gzip compressed)
	Data Size:    335725 Bytes = 327.86 kB = 0.32 MB
	Load Address: 0x00000000
	Entry Point:  0x00000000
```

참고: 부팅 시간이 중요한 임베디드 시스템의 경우 메모리와 속도를 교환하고 압축되지 않은 이미지를 대신 설치할 수 있습니다. 플래시에 더 많은 공간이 필요하지만 압축을 풀 필요가 없기 때문에 훨씬 빠르게 부팅됩니다.

```makefile
-> gunzip /opt/elsk/ppc_8xx/usr/src/linux-2.4.4/arch/powerpc/coffboot/vmlinux.gz
	-> tools/mkimage -n '2.4.4 kernel for TQM850L' \
	> -A ppc -O linux -T kernel -C none -a 0 -e 0 \
	> -d /opt/elsk/ppc_8xx/usr/src/linux-2.4.4/arch/powerpc/coffboot/vmlinux \
	> examples/uImage.TQM850L-uncompressed
	Image Name:   2.4.4 kernel for TQM850L
	Created:      Wed Jul 19 02:34:59 2000
	Image Type:   PowerPC Linux Kernel Image (uncompressed)
	Data Size:    792160 Bytes = 773.59 kB = 0.76 MB
	Load Address: 0x00000000
	Entry Point:  0x00000000
```

커널이 초기 ramdisk를 사용하도록 의도된 경우 유사하게 'ramdisk.image.gz' 파일에서 U-Boot 이미지를 빌드할 수 있습니다.

```makefile
-> tools/mkimage -n 'Simple Ramdisk Image' \
	> -A ppc -O linux -T ramdisk -C gzip \
	> -d /LinuxPPC/images/SIMPLE-ramdisk.image.gz examples/simple-initrd
	Image Name:   Simple Ramdisk Image
	Created:      Wed Jan 12 14:01:50 2000
	Image Type:   PowerPC Linux RAMDisk Image (gzip compressed)
	Data Size:    566530 Bytes = 553.25 kB = 0.54 MB
	Load Address: 0x00000000
	Entry Point:  0x00000000
```

"dumpimage" 도구는 mkimage가 만든 이미지의 내용을 분해하거나 나열하는 데 사용할 수 있습니다. 자세한 내용은 dumpimage의 도움말 출력(-h)을 참조하십시오.

### Installing a Linux Image

직렬(콘솔) 인터페이스를 통해 U-Boot 이미지를 다운로드하려면 이미지를 S-Record 형식으로 변환해야 합니다.

```makefile
objcopy -I binary -O srec examples/image examples/image.srec
```

'objcopy'는 U-Boot 이미지 헤더의 정보를 이해하지 못하므로 결과 S-Record 파일은 주소 0x00000000을 기준으로 합니다. 지정된 주소로 로드하려면 'loads' 명령을 사용하여 대상 주소를 'offset' 매개변수로 지정해야 합니다.

```makefile
Example: install the image to address 0x40100000 
(which on the TQM8xxL is in the first Flash bank):

	=> erase 40100000 401FFFFF

	.......... done
	Erased 8 sectors

	=> loads 40100000
	## Ready for S-Record download ...
	~>examples/image.srec
	1 2 3 4 5 6 7 8 9 10 11 12 13 ...
	...
	15989 15990 15991 15992
	[file transfer complete]
	[connected]
	## Start Addr = 0x00000000
```

'iminfo' 명령을 사용하여 다운로드 성공 여부를 확인할 수 있습니다. 여기에는 체크섬 확인이 포함되므로 데이터 손상이 발생하지 않았는지 확인할 수 있습니다.

```makefile
=> imi 40100000

	## Checking Image at 40100000 ...
	   Image Name:	 2.2.13 for initrd on TQM850L
	   Image Type:	 PowerPC Linux Kernel Image (gzip compressed)
	   Data Size:	 335725 Bytes = 327 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 0000000c
	   Verifying Checksum ... OK
```

### Boot Linux

"bootm" 명령은 메모리(RAM 또는 플래시)에 저장된 응용 프로그램을 부팅하는 데 사용됩니다. Linux 커널 이미지의 경우 "bootargs" 환경 변수의 내용이 매개변수로 커널에 전달됩니다. "printenv" 및 "setenv" 명령을 사용하여 이 변수를 확인하고 수정할 수 있습니다.

```makefile
=> printenv bootargs
	bootargs=root=/dev/ram

	=> setenv bootargs root=/dev/nfs rw nfsroot=10.0.0.2:/LinuxPPC nfsaddrs=10.0.0.99:10.0.0.2

	=> printenv bootargs
	bootargs=root=/dev/nfs rw nfsroot=10.0.0.2:/LinuxPPC nfsaddrs=10.0.0.99:10.0.0.2

	=> bootm 40020000
	## Booting Linux kernel at 40020000 ...
	   Image Name:	 2.2.13 for NFS on TQM850L
	   Image Type:	 PowerPC Linux Kernel Image (gzip compressed)
	   Data Size:	 381681 Bytes = 372 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 0000000c
	   Verifying Checksum ... OK
	   Uncompressing Kernel Image ... OK
	Linux version 2.2.13 (wd@denx.local.net) (gcc version 2.95.2 19991024 (release)) #1 Wed Jul 19 02:35:17 MEST 2000
	Boot arguments: root=/dev/nfs rw nfsroot=10.0.0.2:/LinuxPPC nfsaddrs=10.0.0.99:10.0.0.2
	time_init: decrementer frequency = 187500000/60
	Calibrating delay loop... 49.77 BogoMIPS
	Memory: 15208k available (700k kernel code, 444k data, 32k init) [c0000000,c1000000]
	...
```

초기 RAM 디스크로 Linux 커널을 부팅하려면 커널과 initrd 이미지(PPBCOOT 형식!) 모두의 메모리 주소를 "bootm" 명령에 전달합니다.

```makefile
=> imi 40100000 40200000

	## Checking Image at 40100000 ...
	   Image Name:	 2.2.13 for initrd on TQM850L
	   Image Type:	 PowerPC Linux Kernel Image (gzip compressed)
	   Data Size:	 335725 Bytes = 327 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 0000000c
	   Verifying Checksum ... OK

	## Checking Image at 40200000 ...
	   Image Name:	 Simple Ramdisk Image
	   Image Type:	 PowerPC Linux RAMDisk Image (gzip compressed)
	   Data Size:	 566530 Bytes = 553 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 00000000
	   Verifying Checksum ... OK

	=> bootm 40100000 40200000
	## Booting Linux kernel at 40100000 ...
	   Image Name:	 2.2.13 for initrd on TQM850L
	   Image Type:	 PowerPC Linux Kernel Image (gzip compressed)
	   Data Size:	 335725 Bytes = 327 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 0000000c
	   Verifying Checksum ... OK
	   Uncompressing Kernel Image ... OK
	## Loading RAMDisk Image at 40200000 ...
	   Image Name:	 Simple Ramdisk Image
	   Image Type:	 PowerPC Linux RAMDisk Image (gzip compressed)
	   Data Size:	 566530 Bytes = 553 kB = 0 MB
	   Load Address: 00000000
	   Entry Point:	 00000000
	   Verifying Checksum ... OK
	   Loading Ramdisk ... OK
	Linux version 2.2.13 (wd@denx.local.net) (gcc version 2.95.2 19991024 (release)) #1 Wed Jul 19 02:32:08 MEST 2000
	Boot arguments: root=/dev/ram
	time_init: decrementer frequency = 187500000/60
	Calibrating delay loop... 49.77 BogoMIPS
	...
	RAMDISK: Compressed image found at block 0
	VFS: Mounted root (ext2 filesystem).

	bash#
```

### Boot Linux and pass a flat device tree

첫째, U-Boot는 적절한 정의로 컴파일되어야 합니다. 더 자세한 설명은 위의 "Linux 커널 인터페이스" 섹션을 참조하십시오. 다음은 커널을 시작하고 업데이트된 플랫 장치 트리를 전달하는 방법의 예입니다.

```makefile
=> print oftaddr
oftaddr=0x300000
=> print oft
oft=oftrees/mpc8540ads.dtb
=> tftp $oftaddr $oft
Speed: 1000, full duplex
Using TSEC0 device
TFTP from server 192.168.1.1; our IP address is 192.168.1.101
Filename 'oftrees/mpc8540ads.dtb'.
Load address: 0x300000
Loading: #
done
Bytes transferred = 4106 (100a hex)
=> tftp $loadaddr $bootfile
Speed: 1000, full duplex
Using TSEC0 device
TFTP from server 192.168.1.1; our IP address is 192.168.1.2
Filename 'uImage'.
Load address: 0x200000
Loading:############
done
Bytes transferred = 1029407 (fb51f hex)
=> print loadaddr
loadaddr=200000
=> print oftaddr
oftaddr=0x300000
=> bootm $loadaddr - $oftaddr
## Booting image at 00200000 ...
   Image Name:	 Linux-2.6.17-dirty
   Image Type:	 PowerPC Linux Kernel Image (gzip compressed)
   Data Size:	 1029343 Bytes = 1005.2 kB
   Load Address: 00000000
   Entry Point:	 00000000
   Verifying Checksum ... OK
   Uncompressing Kernel Image ... OK
Booting using flat device tree at 0x300000
Using MPC85xx ADS machine description
Memory CAM mapping: CAM0=256Mb, CAM1=256Mb, CAM2=0Mb residual: 0Mb
[snip]
```