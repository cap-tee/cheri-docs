# Installing the Morello edition of the Arm Development Studio
 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

 ## Overview

 This guide covers
 * Download the Morello edition of the Development Studio
 * Download the supporting documents
 * Install the Development Studio
 * Start the Development Studio
 * Set up the prebuilt Morello supported LLVM binaries for bare metal with the Development Studio
 * Set up the Morello FVP with the Development Studio

 ## Assumptions
* It is assumed the prebuilt Morello supported LLVM binaries for bare metal have been installed.
* It is assumed the Morello FVP has been installed.

## Download the Morello edition of the Development Studio
The *Morello Arm Development Studio* `armds-morello-edition-2020-1m0.tgz` can be downloaded, and then extracted into a named directory `projects/armDevelopmentStudio`

https://developer.arm.com/architectures/cpu-architecture/a-profile/morello/development-tools/downloads 

## Download the supporting documents
Supporting documentation can be downloaded from here:

*Getting Started Guide:*

armds_morello_getting_started_102233_2020-1m0_00_en.pdf

https://developer.arm.com/documentation/102233/2020-1m0 

*User Guide:*

armds_morello_user_guide_102234_2020-1m0_00_en.pdf

https://developer.arm.com/documentation/102234/2020-1m0 

*Reference Guide:*

armds_morello_reference_guide_102272_2020-1m0_00_en.pdf

https://developer.arm.com/documentation/102272/2020-1m0 

## Install the Development Studio
1. Install as root (using sudo) because the post install stage needs root access. Navigate to the extracted directory `projects/armDevelopmentStudio/armds-morello-edition-2020-1m0` and run the installation script.
```
sudo ./armds-2020.1M0.sh
```
2. When prompted of where to install, select a directory
```
~/projects/armDevelopmentStudio/developmentstudio_morello-2020.1M0
```
## Start the Development Studio

To start the Arm Development Studio Morello Edition 2020.1M0 launch the GUI from the desktop shortcut.

The Release notes for the product can be found here: `projects/armDevelopmentStudio/developmentstudio_morello-2020.1M0/sw/info/readme.html`

## Set up the prebuilt Morello supported LLVM binaries for bare metal with the Development Studio

From within the Development Studio, goto the menus and select `Window/preferences/ArmDS/toolchains` and add the LLVM tool chain binaries from the downloaded git repository by pointing to the bin directory `projects/baremetalsources/llvm-morello-releases/bin`. It should recognise the binaries and label the toolchains as LLVM 11.0.0. Apply the changes and let the tool restart.

The tool chain set-up is now ready to be used for bare metal coding.

## Set up the Morello FVP with the Development Studio

To allow the Development Studio to know where the Morello FVP is launched from, it needs to be added to the path.

1. Locate your .bashrc file. This could be under `home/.bashrc`, and open it with an editor.

2. Add the full path to the Morello FVP binary to your PATH environment variable, for example add to the bottom of the script
```
export PATH=/home/osboxes/projects/morello_workspace/model/FVP_Morello/models/Linux64_GCC-6.4:$PATH
```
Restart the Development Studio for the changes to take effect.

You are now ready to start running bare metal code with the Development Studio.  [Go back to the Morello Getting Started Guide.](./../../../morello-getting-started.md)