# FVP/Hardware: Create a TF-A baremetal payload running at EL2

 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

 ## Overview

This describes how to generate a baremetal program to run on the Morello FVP or hardware in the Normal World (untrusted) at EL2 as a TF-A (Trusted Firmware-A) payload. The baremetal code will be compiled from the command line, and then either run directly by the FVP or the binaries will be transfered to the hardware platform for execution. To run baremetal code at EL2 from Development Studio see [FVP/Hardware: Create a TF-A "loop" payload running at EL2, and download an EL2 baremetal program via Development Studio](./TFAloop.md)

This document largely follows https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/standalone-baremetal-readme.rst 

## Assumptions

* The LLVM toolchain for Morello baremetal has been downloaded. See [Getting the prebuilt Morello supported LLVM binaries for bare metal.](./../LlvmBinaries/LlvmBinaries.md) These are needed in order to compile application code for bare metal. It is recommended to download the latest version.
* The firmware binaries to boot to UEFI have been downloaded and built. See [Using cmake to generate firmware binaries for fvp or hardware (soc).](./../CmakeFirmwareBinaries/CmakeFirmwareBinaries.md)
* The Morello hardware is available. See [Turning on the Morello Hardware Platform for the first time.](./../../MorelloPlatform/SettingUpMorelloSoc.md) or the Morello FVP has been installed. See [Installing the Morello Fixed Virtual Platform.](./../../MorelloPlatform/InstallingMorelloFVP.md)


## Create baremetal source code

**C File**

First create a directory for your example. 'helloworld/src'. Create a C file with the source code. You may follow the example code from the link to output 'hello' to the AP console: https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/standalone-baremetal-readme.rst , section **1. Create standalone application code**


It is assumed the file has been called `helloworld.c` in this example.

**Linker Script**

Create a linker script in the same path. Ensure the memory and sections start from the address 0xE0000000, as this is where the EL2 baremetal program will be placed and start from. You may follow the example script from the link: https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/standalone-baremetal-readme.rst , section **2. Create linker script**

It is assumed the file has been called `link_scripts.ld.S` in this example.

## Build baremetal 

You may wish to create bash scripts to Build the code: 'helloworld/scripts`
* /createprogbinNoCap.sh`
* /createprogbinWithCap.sh`

The script commands below are non-specific to Morello, but will run on Morello and build for non capability mode. 

```bash
<toolchain path>/bin/clang -target aarch64-none-elf -c ../src/helloworld.c -o ../progbinaries/noCapabilities/helloworld.o -O3
<toolchain path>/bin/ld.lld -o ../progbinaries/noCapabilities/helloworld -T ../src/link_scripts.ld.S ../progbinaries/noCapabilities/helloworld.o -s
<toolchain path>/bin/llvm-objcopy -O binary ../progbinaries/noCapabilities/helloworld
```

The script commands below are to build with capabilities. Note that the example given at the link: https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/standalone-baremetal-readme.rst , section **1. Create standalone application code** will not work with capabilities because a specific address value is defined for the UART base address. With capabilities, the UART pointer needs to be derived from a valid capability.

```bash
<toolchain path>/bin/clang -target aarch64-none-elf  -march=morello+c64 -mabi=purecap -c ../src/helloworld.c -o ../progbinaries/withCapabilities/helloworld.o -O3
<toolchain path>/bin/ld.lld -o ../progbinaries/withCapabilities/helloworld -T ../src/link_scripts.ld.S ../progbinaries/withCapabilities/helloworld.o -s
<toolchain path>/bin/llvm-objcopy -O binary ../progbinaries/withCapabilities/helloworld
```

Run the script from the script directory:
```
./createprogbinNoCap.sh
```

This will generate a binary called helloworld to run as a non-secure payload on the Morello FVP or hardware platform.

## Package baremetal binary for fvp using fip tool

You may wish to create bash scripts to package the program: 'helloworld/scripts`
* /createfvpbinNoCap.sh`
* /createfvpbinWithCap.sh`

The following commands /script needs to be run from the <morello_workspace> directory. Copy the commands into the script or run from the console.

```
make -C "bsp/arm-tf" PLAT=morello TARGET_PLATFORM=fvp clean

MBEDTLS_DIR="<morello_workspace>/bsp/deps/mbedtls" \
CROSS_COMPILE="<morello_workspace>/tools/clang/bin/llvm-" \
make -C "bsp/arm-tf" \
CC="<morello_workspace>/tools/clang/bin/clang" \
LD="<morello_workspace>/tools/clang/bin/ld.lld" \
PLAT=morello ARCH=aarch64 TARGET_PLATFORM=fvp ENABLE_MORELLO_CAP=0 \
E=0 TRUSTED_BOARD_BOOT=1 GENERATE_COT=1 ARM_ROTPK_LOCATION="devel_rsa" \
ROT_KEY="plat/arm/board/common/rotpk/arm_rotprivk_rsa.pem" \
BL33=<helloworld>/progbinaries/noCapabilities/helloworld
all fip
```

The commands above will generate `fip.bin` and `bl1.bin` under the `<morello_workspace>/bsp/arm-tf/build/morello/release/` directory. You may wish to copy these files to a `helloworld/fvpbinaries/NoCap/` directory.


## Package baremetal binary for hardware (soc) using fip tool

Note that `TARGET_PLATFORM` takes 'fvp' for FVP and 'soc' for Development Board. `ENABLE_MORELLO_CAP` can take either 0 or 1.

You may wish to create bash scripts to package the program for soc: 'helloworld/scripts`
* /createsocbinNoCap.sh`
* /createsocbinWithCap.sh`

The following commands /script needs to be run from the <morello_workspace> directory. Copy the commands into the script or run from the console.

```
make -C "bsp/arm-tf" PLAT=morello TARGET_PLATFORM=soc clean

MBEDTLS_DIR="<morello_workspace>/bsp/deps/mbedtls" \
CROSS_COMPILE="<morello_workspace>/tools/clang/bin/llvm-" \
make -C "bsp/arm-tf" \
CC="<morello_workspace>/tools/clang/bin/clang" \
LD="<morello_workspace>/tools/clang/bin/ld.lld" \
PLAT=morello ARCH=aarch64 TARGET_PLATFORM=soc ENABLE_MORELLO_CAP=0 \
E=0 TRUSTED_BOARD_BOOT=1 GENERATE_COT=1 ARM_ROTPK_LOCATION="devel_rsa" \
ROT_KEY="plat/arm/board/common/rotpk/arm_rotprivk_rsa.pem" \
BL33=<helloworld>/progbinaries/noCapabilities/helloworld
all fip
```
The commands above will generate `fip.bin` and `bl1.bin` under the same `<morello_workspace>/bsp/arm-tf/build/morello/release/` directory. **You only need the `fip.bin` file for the soc.** You may wish to copy these files to a `helloworld/socbinaries/NoCap` directory.

## Run the EL2 baremetal program directly with the FVP

You may wish to create bash scripts to run the fvp for the different configurations: 'helloworld/scripts`

/runfvpNoCap.sh`
/runfvpWithCap.sh`

```
<modelpath>/FVP_Morello/models/Linux64_GCC-6.4/FVP_Morello \
--data Morello_Top.css.scp.armcortexm7ct=<morello_workspace>/output/fvp/firmware/scp_romfw.bin@0x0 \
--data Morello_Top.css.mcp.armcortexm7ct=<morello_workspace>/output/fvp/firmware/mcp_romfw.bin@0x0 \
-C Morello_Top.soc.scp_qspi_loader.fname=<morello_workspace>/output/fvp/firmware/scp_fw.bin \
-C Morello_Top.soc.mcp_qspi_loader.fname=<morello_workspace>/output/fvp/firmware/mcp_fw.bin \
-C css.scp.armcortexm7ct.INITVTOR=0x0 \
-C css.mcp.armcortexm7ct.INITVTOR=0x0 \
-C css.trustedBootROMloader.fname=<helloworld>/fvpbinaries/NoCap/bl1.bin \
-C board.ap_qspi_loader.fname=<helloworld>/fvpbinaries/NoCap/fip.bin \
-C css.pl011_uart_ap.out_file=uart0.log \
-C css.scp.pl011_uart_scp.out_file=scp.log \
-C css.mcp.pl011_uart0_mcp.out_file=mcp.log \
-C css.pl011_uart_ap.unbuffered_output=1
```

## Run the EL2 baremetal program on the hardware

1. Connect the USB cable from the Morello machine to the host computer, and turn on the Morello machine via the switch on the back. If this is the first time the Morello hardware has been turned on follow this first: [Turning on the Morello Hardware Platform for the first time.](./../../MorelloPlatform/SettingUpMorelloSoc.md)

2. If you have a **Windows** machine running a Ubuntu VM with the firmware build, open a **PuTTY** window for each COM port from Windows. The COM ports appear in the following order: **Motherboard Configuration Controller(MCC), Platform Controller Chip(PCC), Application Processor(AP), System Control Processor(SCP)**. See [Turning on the Morello Hardware Platform for the first time.](./../../MorelloPlatform/SettingUpMorelloSoc.md) on how to set up PuTTY. Alternatively you can set up a connection with something like Minicom from Ubuntu.

3. On the **Windows** machine the SD card containing the current Morello firmware files will be automatically opened, or you can browse to it. 

Under the `SOFTWARE` directory you should see the following files:

* fip.bin
* mcp_fw.bin
* scp_fw.bin

4. Replace the `fip.bin` file with your newly built version containing the baremetal program. You may also replace `mcp_fw.bin` and `scp_fw.bin` from the `<morello_workspace>/output/soc/firmware/` directory if you have recently rebuilt the firmware files. The `fip.bin` in this directory will boot to the UEFI screen.

5. From the MCC console reboot the Morello machine

```
Cmd>REBOOT
```

6. If you are running the hello example, check for the following AP console message:
```
hello
```


To shutdown type the following into the MCC console. Then turn off the machine.
```
Cmd>SHUTDOWN
```