# Morello Getting Started Guide
This Document provides an overview of Cheri with the Morello Platform and links to other documents in this repository.

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
2. Understanding the Repo Tool and the Morello Android Manifest Hierarchy.
3. [Creating and running a "hello world" application on Android-Morello.](./morello/AndroidOnMorello/HelloWorldOnAndroid/helloWorldOnAndroid.md)
3. Buffer overflow exercise 

## CheriBSD on Morello
### CheriBSD on Morello Overview
Content to be added.
### CheriBSD on Morello Documents
Content to be added.

## Bare Metal on Morello
### Bare Metal Overview
In order to run bare metal code on the Morello Platform, a small modification is needed to the SCP source code, and then re-built. The Yocto bitbake recipe installed with android is a convienient method to re-build the firmware as it is already set up with the correct build options for the FVP Morello Platform. Alternatively, the necessary files can be cloned from the [morello-baremetal-examples](https://github.com/cap-tee/morello-baremetal-examples) repository for convenience.
### Bare Metal Getting Started
This section is a gentle introduction to get you setup for bare metal coding on Morello.
1. [Bare metal build options.](./morello/BareMetalOnMorello/BuildOptions/BuildOptions.md) 
2. [Using Yocto bitbake to generate bare metal firmware.](./morello/BareMetalOnMorello/YoctoBitbake/YoctoBitbake.md)
3. Building bare metal firmware from cloning the SCP Firmware and Trusted Firmware-A Morello git projects.
4. [Getting the prebuilt Morello supported LLVM binaries for bare metal.](./morello/BareMetalOnMorello/LlvmBinaries/LlvmBinaries.md)
5. [Installing the Morello edition of the Arm Development Studio.](./morello/BareMetalOnMorello/InstallingArmDevStudio/InstallingArmDevStudio.md)
6. [Creating and running a bare metal "hello world" application on the command line.](./morello/BareMetalOnMorello/HelloWorldCommandLine/HelloWorldCommandLine.md)
7. [Creating and running a bare metal "hello world" through the Arm Development Studio.](./morello/BareMetalOnMorello/HelloWorldArmDevStudio/HelloWorldArmDevStudio.md)
8. Buffer overflow exercise.
9. Interfacing to the trusted firmware.
### Bare Metal Default Configuration Setup for Morello
This section explains more about the set up of bare metal on Morello. It is worth reading if you wish to do more than just run the examples. 
1. [Understanding the default initialisation sequence for Morello.](./morello/BareMetalOnMorello/DefaultSetup/InitSequence/InitSequence.md)
2. [Understanding the Morello memory map.](./morello/BareMetalOnMorello/DefaultSetup/MemMap/MemMap.md)
3. [Understanding the default MMU set up at EL3](./morello/BareMetalOnMorello/DefaultSetup/MMU/MMU.md)
### Bare Metal Examples
This section guides you through the bare metal example code which can be found in the [morello-baremetal-examples](https://github.com/cap-tee/morello-baremetal-examples) repository.

[Cloning and Importing the examples into Development Studio.](./morello/BareMetalOnMorello/BareMetalExamples/DownloadingExamples/DownloadingExamples.md)

1. [HelloWorld - Outputs "Hello World" to the console at EL3.](./morello/BareMetalOnMorello/BareMetalExamples/HelloWorld/HelloWorld.md)
2. [MMUEL3 - Changes the MMU set up at EL3 for Morello.](./morello/BareMetalOnMorello/BareMetalExamples/MMUEL3/MMUEL3.md)
3. [EL3ToEL1 - Changes the exception level from EL3 to either EL1 secure or EL1 non-secure](./morello/BareMetalOnMorello/BareMetalExamples/EL3ToEL1/EL3ToEL1.md)
4. [EL3MMUToEL1MMU - Changes the exception level from EL3 to either EL1 secure or EL1 non-secure, and sets up the MMU at each level.](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUToEL1MMU/EL3MMUToEL1MMU.md)
5. [EL3MMUUART - Changes the MMU set up at EL3 for Morello, sets up the UART and writes a message.](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUUart/EL3MMUUart.md)
6. [EL3MMUToEL1MMUUART - Changes Exception level to either EL1S or EL1N and sets up the MMUs and uart, and writes a message. It also sets up secure & non-secure memory regions.](./morello/BareMetalOnMorello/BareMetalExamples/EL3MMUToEL1MMUUART/EL3MMUToEL1MMUUART.md)