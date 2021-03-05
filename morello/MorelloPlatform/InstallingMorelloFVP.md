# Installing the Morello Fixed Virtual Platform.
 [Go back to Morello Getting Started Guide.](./../../morello-getting-started.md)

The *Morello Platform* is a prototype development board with Arm cores supporting *Cheri capabilities*.
A hardware emulator version of the Morello Platform is available to download and install called the *Morello Fixed Virtual Platform* (Morello FVP). This can be used in place of the physical hardware.

The rest of the documentation builds upon the Morello Platform. Software stacks such as Android Nano, and CheriBSD can be built on top of this. Bare metal coding is also supported.

## Assumptions:

It is recommended that Ubuntu 18.04 (Ubuntu Bionic Beaver) is installed with at least 300Gb of free space. If needed you can do this using a VM (e.g https://www.osboxes.org/ubuntu/#ubuntu-1804-vbox).

If you have a problem with the installation refer to the section on *Discovered Issues and Work-arounds* below. 

## Morello FVP Installation

This Follows the first part of the video guide at the following link:

https://www.morello-project.org/resources/morello-platform-model-and-android-stack-walkthrough/ 

1. Within Ubuntu 18.04 make a new directory called ```morello_workspace``` under your ```projects``` directory.

```bash
mkdir morello_workspace
```
2. Create a directory under ```morello_workspace``` called ```model```.

```
cd morello_workspace
mkdir model
cd model
```
3. Open a web page and go to the *Ecosystem FVP Developer* page at  https://developer.arm.com/tools-and-software/open-source-software/arm-platforms-software/arm-ecosystem-fvps. Alternatively follow the link from the main Morello Landing page at https://www.morello-project.org 

4. Scroll down to *Morello Platform FVP* and select *Morello Platform FVP* and save to the ```downloads``` directory.

```
Download Linux
```
5. Make sure you are in the ```model``` directory and then move the file to the ```model``` directory. Check the version number as it may change from what is shown here. *(Don't forget to include the dot at the end of the command)*

```
mv ~/Downloads/FVP_Morello_0.11_9.tgz .
```

6. Extract the tgz file.
```
tar -xzf FVP_Morello_0.11_9.tgz 
```
The following is now present under the `model` directory:
`FVP_Morello_0.11_9.tgz  FVP_Morello.sh  license_terms`. 

7. Run the installation.

```
./FVP_Morello.sh
```
and enter `yes` when asked to proceed. Press `ENTER` to scroll down the license text.
Enter `yes` to agree to the terms and conditions. When asked `Where would you like to install to? [default: …..]` type the following
```
/<path>/projects/morello_workspace/model/FVP_Morello
```
The FVP_Morello directory will be created. Say `yes` when asked to create.

8. Change to the new FVP_Morello directory.

```
cd FVP_Morello
```

9. You can check the tree structure if you wish. Use `sudo apt install tree` if it is not already installed. The document *morello_platform_model_rg_102225_0101_00_en.pdf* contains more information about the FVP.

```
tree .
```
10. Check the dependencies are correctly set up. Under the `FVP_Morello` directory type
```
ldd ./models/Linux64_GCC-6.4/FVP_Morello
```
The dependencies should be set as follows:

```
	linux-vdso.so.1 (0x00007fffdff9a000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f12dad52000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f12dab4a000)
	libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f12da7c1000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f12da423000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f12da20b000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f12d9fec000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f12d9bfb000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f12daf56000)
```
11. Check that you have the correct revision by typing

```
cat sw/ARM_Fast_Models_FVP_Morello/rev
```
or
```
./models/Linux64_GCC-6.4/FVP_Morello –version
```

## Running the Morello FVP 

Run the model in GUI mode without any parameters to perform an intial check.
```
./models/Linux64_GCC-6.4/FVP_Morello
```
If everything is working correctly two terminals will be loaded: The GUI display (*Fast Models – Morello HDLCD*) and the dashboard (*Fast Models – Morello*).

12. To stop the model use `CTRL & C`.

13. To obtain a list of the FVP model parameters use the `-l` option or `--list-params`
```
./models/Linux64_GCC-6.4/FVP_Morello -l
./models/Linux64_GCC-6.4/FVP_Morello --list-params
```
You are now ready to install a software stack, or follow the bare metal guide. [Go back to Morello Getting Started Guide.](./../../morello-getting-started.md)


## Discovered Issues and Work-arounds

The following issues were noted when installing FVP_Morello_0.11_9.

1. Version FVP_Morello_0.11_9.tgz installer file installs to incorrect directory when the *(A) Choose another directory location* is invoked.
    * *Description:* If an error is made on the install directory and the (A) option is chosen to choose another installation directory, the installer installs to .../morel_workspace/… instead of .../morello_workspace/…
    * *Work-around:* Quit out of the installation and start again.

2. Incorrect version is displayed.
    * *Description:* When asking for the installed version of FVP_Morello_0.11_9 using `FVP_Morello --version`, the incorrect version (0.11.8) is returned. 