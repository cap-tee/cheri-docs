# Morello Getting Started Guide
This Document provides an overview of Cheri with the Morello Platform and links to other documents in this repository. This is a work in progress, and new content will be added on a continuous basis throughout the project. The examples documented here are intended for research purposes associated with the Morello Platform.

## Morello Platform
### Morello Platform Overview
The morello Platform is a prototype development board with Arm cores supporting Cheri capabilities.
A hardware emulator version of the Morello Platform is available to download and install called the Morello Fixed Virtual Platform (Morello FVP). This can be used in place of the physical hardware.

The rest of the documentation builds upon the Morello Platform. Software stacks such as Android, and CheriBSD can be built on top of this. Bare metal coding is also supported.

### Morello Platform Documents
1. [Installing the Morello Fixed Virtual Platform.](./morello/MorelloPlatform/InstallingMorelloFVP.md)

## Android on Morello
### Android on Morello Overview
Android can be installed to run on top of the Morello FVP.
### Android on Morello Documents
1. [Downloading and building Android on Morello FVP.](./morello/AndroidOnMorello/BuildingMorelloAndroid/BuildingAndroidOnMorello.md)
2. [Understanding the Repo Tool and the Morello Android Manifest Hierarchy.](./morello/AndroidOnMorello/RepoTool/RepoTool.md)

3. [Creating and running a "hello world" application on Android-Morello.](./morello/AndroidOnMorello/HelloWorldOnAndroid/helloWorldOnAndroid.md)


## CheriBSD on Morello
### CheriBSD on Morello Overview
Content to be added.
### CheriBSD on Morello Documents
Content to be added.

## Bare Metal on Morello
### Bare Metal Overview
In order to run bare metal code (code starting at EL3 without the trusted firmware) on the Morello Platform for `version 0.11_9 - Feb 21`, a small modification is needed to the SCP source code, and then re-built. The Yocto bitbake recipe installed with android is a convienient method to re-build the firmware as it is already set up with the correct build options for the FVP Morello Platform. The latest version of the FVP can be downloaded from Arm here: https://developer.arm.com/tools-and-software/open-source-software/arm-platforms-software/arm-ecosystem-fvps 

### Bare Metal Getting Started
This section is a gentle introduction to get you setup for bare metal coding on Morello.
1. [Bare metal build options.](./morello/BareMetalOnMorello/BuildOptions/BuildOptions.md) `fvp version 0.11_9`
2. [Using Yocto bitbake to generate bare metal firmware.](./morello/BareMetalOnMorello/YoctoBitbake/YoctoBitbake.md) `fvp version 0.11_9`
3. Building bare metal firmware from cloning the SCP Firmware and Trusted Firmware-A Morello git projects.
4. [Getting the prebuilt Morello supported LLVM binaries for bare metal.](./morello/BareMetalOnMorello/LlvmBinaries/LlvmBinaries.md)
5. [Installing the Morello edition of the Arm Development Studio.](./morello/BareMetalOnMorello/InstallingArmDevStudio/InstallingArmDevStudio.md)
6. [Creating and running a bare metal "hello world" application on the command line.](./morello/BareMetalOnMorello/HelloWorldCommandLine/HelloWorldCommandLine.md)
7. [Creating and running a bare metal "hello world" through the Arm Development Studio.](./morello/BareMetalOnMorello/HelloWorldArmDevStudio/HelloWorldArmDevStudio.md)

### Bare Metal Default Configuration Setup for Morello
This section explains more about the set up of bare metal on Morello. It is worth reading if you wish to do more than just run the examples. 
1. [Understanding the default initialisation sequence for Morello.](./morello/BareMetalOnMorello/DefaultSetup/InitSequence/InitSequence.md)
2. [Understanding the Morello memory map.](./morello/BareMetalOnMorello/DefaultSetup/MemMap/MemMap.md)
3. [Understanding the default MMU set up at EL3](./morello/BareMetalOnMorello/DefaultSetup/MMU/MMU.md)
4. [Important information for writing programs for Morello - things that might catch you out](./morello/BareMetalOnMorello/DefaultSetup/Important/Important.md)

### Bare Metal Examples (Starting at EL3)
This section guides you through the bare metal example code which can be found in the [morello-baremetal-examples](https://github.com/cap-tee/morello-baremetal-examples) repository. The examples are used with Development Studio. 

Projects that can be compiled in both *Morello* and *Morello-purecap* have a `purecap` label, otherwise they can only be compiled under *Morello* at present.

Note that these examples have been tested with the following combinations:
* Development Studio version `armds-morello-edition-2020-1m0.tgz`, FVP version `FVP_Morello_0.11_9` and LLVM version `bare-metal-release-1.0`.
* Development Studio version `armds-morello-edition-2020-1m0.tgz`, FVP version `FVP_Morello_0.11_19` and LLVM version `bare-metal-release-1.2`.


[Cloning and importing the examples into Development Studio.](./morello/BareMetalOnMorello/BareMetalExamples/DownloadingExamples/DownloadingExamples.md)

[Settings needed when compiling for *Morello*, *Morello-purecap*, or *armv8.2*](./morello/BareMetalOnMorello/BareMetalExamples/Compiling/Compiling.md)

1. [HelloWorld](./morello/BareMetalOnMorello/BareMetalExamples/HelloWorld/HelloWorld.md) - Outputs "Hello World" to the console at EL3. `purecap`
2. [EL3-purecap](./morello/BareMetalOnMorello/BareMetalExamples/EL3-purecap/EL3-purecap.md) - performs some low level capability tests in assembly to test and explore basic CHERI functionality. `purecap` 
3. [ChericExample](./morello/BareMetalOnMorello/BareMetalExamples/ChericExample/ChericExample.md) - Exploring the cheri.h API functions and the cheriintrin.h built-in functions. Setting up capability pointers, and retreiving and printing their properties. `purecap`
4. [MMUEL3](./morello/BareMetalOnMorello/BareMetalExamples/MMUEL3/MMUEL3.md) - Changes the MMU set up at EL3 for Morello. `purecap`
5. [EL3ToEL1](./morello/BareMetalOnMorello/BareMetalExamples/EL3ToEL1/EL3ToEL1.md) - Changes the exception level from EL3 to either EL1 secure or EL1 non-secure. `purecap`
6. [EL3MMUToEL1MMU](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUToEL1MMU/EL3MMUToEL1MMU.md) - Changes the exception level from EL3 to either EL1 secure or EL1 non-secure, and sets up the MMU at each level. `purecap`
7. [EL3MMUUART](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUUart/EL3MMUUart.md) - Changes the MMU set up at EL3 for Morello, sets up the UART and writes a message. `purecap`
8. [EL3MMUToEL1MMUUART](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUToEL1MMUUART/EL3MMUToEL1MMUUART.md) - Changes Exception level to either EL1S or EL1N and sets up the MMUs and uart, and writes a message. It also sets up secure & non-secure memory regions.
9. [EL3MMUTimerInterrupt
](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUTimerInterrupt/EL3MMUTimerInterrupt.md) -  sets up the mmu at EL3, installs the vector tables for exception handling, sets up the interrupt controller, and performs a timer interrupt.
10. [EL3MMUEL1MMUUARTtimerInterrupt](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUEL1MMUUARTtimerInterrupt/EL3MMUEL1MMUUARTtimerInterrupt.md) - sets up the mmu at EL3, installs the vector tables for exception handling, sets up the interrupt controller, and performs a secure timer interrupt. Changes Exception level to either EL1S or EL1N and sets up the MMUs and uart, and performs another timer interrupt (secure timer in EL1S and non secure timer in EL1N). It also sets up secure & non-secure memory regions.
11. [EL1ToEL3SMC](./morello/BareMetalOnMorello/BareMetalExamples/EL1ToEL3SMC/EL1ToEL3SMC.md) - example showing how to use the SMC instruction to call into EL3 from either EL1N (normal world) or EL1S (secure world).
12. [EL1NToEL3ToEL1SSMC](./morello/BareMetalOnMorello/BareMetalExamples/EL1NToEL3ToEL1SSMC/EL1NToEL3ToEL1SSMC.md) - example showing how to pass messages between EL1N (normal world) and EL1S (secure world) using SMC.
13. [EL2](./morello/BareMetalOnMorello/BareMetalExamples/EL2/EL2.md) - example showing how to use the EL2N Hypervisor mode to perform a two stage memory translation for EL1N, and restrict EL1N from reading and writing to the EL1N MMU memory registers. 
14. [EL3UARTredirect](./morello/BareMetalOnMorello/BareMetalExamples/EL3UARTredirect/EL3UARTredirect.md) - example showing how to redirect an embedded printf function to the uart. 

### Boot Flow Examples (Stand-a-lone programs, e.g targeted for EL2N only)
This section guides you through some boot flow examples on Morello. Code can be found in the [morello-baremetal-examples](https://github.com/cap-tee/morello-baremetal-examples) repository under `commandLine/bootflow` unless specified otherwise. Some examples require **Development Studio**, and allow you to step through code and check memory contents. 

Note that these examples have been tested with Development Studio version `armds-morello-edition-2020-1m0.tgz`, FVP version `FVP_Morello_0.11_9` and LLVM version `bare-metal-release-1.0`. Also none of these examples have yet been tested with *Morello-purecap*.

[The trusted boot flow](./morello/BootFlowOnMorello/BootFlowOverview/BootFlowOverview.md)


**EL3**

These examples use the bare metal llvm compiler with the [default morello initialisation code](./morello/BareMetalOnMorello/DefaultSetup/InitSequence/InitSequence.md) by-passed, so that the **image entry point** is `main`. You would not wish to by-pass initialisation code normally, but the purpose is to test and prepare for EL2 code.


1. [EL3_s_loop - Loading an assembly program at EL3](./morello/BootFlowOnMorello/BootFlowExamples/EL3_s_loop/EL3_s_loop.md) - This example shows how to load a small assembly program which sits in a loop at EL3. The boot flow process is stopped at EL3 by the AP reset address.

2. [EL3_c_loop - Loading programs at EL3](./morello/BootFlowOnMorello/BootFlowExamples/EL3_c_loop/EL3_c_loop.md) - This example shows how to load a small c program which sits in a loop at EL3. The boot flow process is stopped at EL3 by the AP reset address.

**EL2N**

These examples use the bare metal llvm compiler with the [default morello initialisation code](./morello/BareMetalOnMorello/DefaultSetup/InitSequence/InitSequence.md) by-passed, so that the **image entry point** is `main`.


1. [EL2N_s_loop - Loading an assembly program at EL2](./morello/BootFlowOnMorello/BootFlowExamples/EL2N_s_loop/EL2N_s_loop.md) - This example shows how to load a small assembly program at EL2N (BL33) in place of the UEFI boot as part of the Android Morello boot flow process.


2. [EL2N_c_loop - Loading programs at EL2N](./morello/BootFlowOnMorello/BootFlowExamples/EL2N_c_loop/EL2N_c_loop.md) - This example shows how to load a small program at EL2N (BL33) in place of the UEFI boot as part of the Android Morello boot flow process.



**EL2N - Trusted Firmware Tests**

These examples use the bare metal llvm compiler with a basic **morello initialisation code for EL2**, so that the **image entry point** is `_startel2`.

1. [developmentStudio/trustedFW_EL2](./morello/BootFlowOnMorello/BootFlowExamples/TrustedFirmware/TrustedFirmwareDS.md) - Trusted Firmware Tests. Basic tests to interface to the trusted firmware - this example follows the boot flow to EL2, and then loads an EL2 program from **Development Studio** to interact with the trusted firmware (at EL3).

2. [commandLine/bootflow/trustedFW_EL2](./morello/BootFlowOnMorello/BootFlowExamples/TrustedFirmware/TrustedfirmwareBF.md) - Trusted Firmware Tests. Basic tests to interface to the trusted firmware -  this example loads an EL2 program from the command line as part of the boot flow to interact with the trusted firmware (at EL3).





