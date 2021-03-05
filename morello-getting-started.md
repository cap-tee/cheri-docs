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
2. [Creating and running a "hello world" application on Android-Morello.](./morello/AndroidOnMorello/HelloWorldOnAndroid/helloWorldOnAndroid.md)
3. Understanding the Repo Tool and the Morello Android Manifest Hierarchy.

## CheriBSD on Morello
### CheriBSD on Morello Overview
Content to be added.
### CheriBSD on Morello Documents
Content to be added.

## Bare Metal on Morello
### Bare Metal Overview
In order to run bare metal code on the Morello Platform, a small modification is needed to the SCP source code, and then re-built. The Yocto bitbake recipe installed with android is a convienient method to re-build the firmware as it is already set up with the correct build options for the FVP Morello Platform.
### Bare Metal Documents
1. Bare metal build options. 
2. Using Yocto bitbake to generate bare metal firmware.
3. Creating and running a bare metal "hello world" application on the command line.
4. Creating and running a bare metal "hello world" through the Arm Development Studio.