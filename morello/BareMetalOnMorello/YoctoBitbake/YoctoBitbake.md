# Using Yocto bitbake to generate bare metal firmware

 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

 ## Overview

 This guide describes how to generate the necessary firmware files in order to run bare metal code on the Morello FVP using the Yocto bitbake recipes that come with the [Android on Morello installation.](./../../AndroidOnMorello/BuildingMorelloAndroid/BuildingAndroidOnMorello.md)

 ## Assumptions

 Android has already been installed for the Morello FVP.

 ## Set up the environment variables

 1. Firstly navigate to the `morello_workspace/bsp` directory and set up the environment variables.
 ```
 MACHINE=morello-fvp DISTRO=poky . ./conf/setup-environment-morello
 ```
You should see the following output
```
Your build environment has been configured with:

    MACHINE = morello-fvp
    SDKMACHINE = x86_64
    DISTRO = poky

You can now run 'bitbake <target>'
```
## Create a new workspace
You need to create a new workspace `morello_workspace/bsp/build-poky/workspace/sources/scp-firmware` using the Yocto `devtool` ready to modify the SCP source code. This allows a copy of the SCP source code to be modified without affecting the original source code. Note however that the output firmware binary files under `bsp/build-poky/tmp-poky/deploy/images/morello-fvp/` will be overwritten and if you wish to keep a copy of the old firmware files, back them up now.

1. Navigate to `morello_workspace/bsp/build-poky` and create a new SCP workspace.

```
devtool modify scp-firmware
```

## Checking the source code build chain works before making changes

You can check that the Morello Android set up still works using this new workspace before you make changes to the source code by building and then running the FVP.

2. Navigate to the workspace directory `/build-poky/workspace` and build the firmware using the bitbake recipe. Then check that the dates on the binary files have changed in the output directory `bsp/build-poky/tmp-poky/deploy/images/morello-fvp/`.

```bash
cd workspace
```
```
bitbake scp-firmware
```
3. Open a new terminal window and navigate to `morello_workspace`. Run the Morello FVP with Android to check that it still works as expected.

```
./run-scripts/run_model.sh -m model/FVP_Morello/models/Linux64_GCC-6.4/FVP_Morello -f android
```
## Changing the SCP source code
The SCP firmware is required to be changed in order to start the Rainier cores from address 0x14000000 (general purpose memory), instead of 0x80000000 (DDR memory). Application code can not be preloaded into the DDR from the command line while launching the FVP. This is because the DDR memory space can only be accessed by the Rainier cores after the CMN-Skeena interconnect and the DMC-Bing memory controller are set up.

1. Navigate to the directory `morello_workspace/bsp/build-poky/workspace/sources/scp-firmware/product/morello/module/morello_system/src/` and use an editor to open the `mod_morello_system.c` file. Find the function `morello_system_init_primary_core` and make changes by deleting or commenting out lines marked with `-` and adding lines marked with `+`.
```c
static int morello_system_init_primary_core(void)
     …..

     FWK_LOG_INFO(
-        "[MORELLO SYSTEM] Setting AP Reset Address to 0x%08" PRIX32,
-        (AP_CORE_RESET_ADDR - AP_SCP_SRAM_OFFSET));
+        "[MORELLO SYSTEM] Setting AP Reset Address to 0x14000000");

     cluster_count = morello_core_get_cluster_count();
     for (cluster_idx = 0; cluster_idx < cluster_count; cluster_idx++) {
...
             core_idx < morello_core_get_core_per_cluster_count(cluster_idx);
             core_idx++) {
             PIK_CLUSTER(cluster_idx)->STATIC_CONFIG[core_idx].RVBARADDR_LW
-                = (AP_CORE_RESET_ADDR - AP_SCP_SRAM_OFFSET);
+                = 0x14000000;
             PIK_CLUSTER(cluster_idx)->STATIC_CONFIG[core_idx].RVBARADDR_UP = 0;
         }
     }
```
## Rebuild the new firmware images

1. Navigate to the workspace directory `/build-poky/workspace` and build the modified firmware using the bitbake recipe. Check that the dates on the binary files have changed in the output directory `bsp/build-poky/tmp-poky/deploy/images/morello-fvp/`.

```
bitbake scp-firmware
```

2. You can prove to yourself that the firmware has changed by rerunning the Morello FVP with Android to check that the AP reset has changed. You can check the AP reset address information by running the FVP and observing the SCP terminal or by looking at the log file `morello_workspace/run-scripts/scp<uniqueID>.log`. Android doesn’t load, but the AP reset information is described as 0x14000000.

```
./run-scripts/run_model.sh -m model/FVP_Morello/models/Linux64_GCC-6.4/FVP_Morello -f android
```
You should see the following in the scp log file.
```
[    0.000000] [MORELLO SYSTEM] Setting AP Reset Address to 0x14000000
```
## Copy the new firmware files to a new project directory

It is recommended to copy the new firmware files to a project directory that you are going to use for your bare metal projects, and re-instate the old firmware files to enable Android to run.

1. Make a new project directory, for example under `projects` 
```
mkdir baremetalsources
```
You may wish to create sub directories for your firmware, for example

```bash
cd baremetalsources
mkdir bare-metal-example-scpmcp-binaries/ SCPMCPBuiltWithAPreset0x14000000
```

 2. Copy the following newly built firmware files into the new directory. You will only need to use those `highlighted` for the bare metal examples.

    * mcp_fw.bin
    * mcp_ramfw_fvp.bin
    * mcp_rom.bin *(Note this is a link to mcp_romfw.bin)*
    * `mcp_romfw.bin`
    * `scp_fw.bin`
    * scp_ramfw_fvp.bin
    * scp_rom.bin *(Note this is a link to scp_romfw.bin)*
    * `scp_romfw.bin`

You may also use the `mcp_fw.bin` file. Although this file is not strictly necessary for the bare metal set up, it is created as part of the build process. When it is *not* loaded into the FVP a `[FIP] Invalid FIP ToC header name:[0xE7FF0010]` is generated in the `FVP terminal_uart0` terminal window. This however does not effect the running of the bare metal examples.

You now have the bare metal firmware files built and ready.  [Go back to the Morello Getting Started Guide.](./../../../morello-getting-started.md)