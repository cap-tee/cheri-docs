# Cloning and Importing the Examples into Development Studio

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

 ## Prerequisites

It is assumed that you have already installed the Morello FVP (Morello hardware Emulator). See https://github.com/cap-tee/cheri-docs/blob/main/morello/MorelloPlatform/InstallingMorelloFVP.md 

It is assumed that you have already installed the Development Studio (GUI and command line debugger). You will need this to run and debug the example code. See https://github.com/cap-tee/cheri-docs/blob/main/morello/BareMetalOnMorello/InstallingArmDevStudio/InstallingArmDevStudio.md

## Clone examples

Go to the [morello-baremetal-examples](./https://github.com/cap-tee/morello-baremetal-examples) repository, and clone using ssh.

```
clone git@github.com:cap-tee/morello-baremetal-examples.git
```

## Import Examples into Development Studio

1. Open Development Studio and go to **File -> Switch Workspace -> Other**, and select `morello-baremetal-examples/DevelopmentStudio` directory.
2. Go to **File -> Import -> Existing Projects into Workspace - > Select root directory**, and then select `morello-baremetal-examples/DevelopmentStudio`. Import all the projects.
3. Select **HelloWorld -> includes** in the Project Explorer window and check they are pointing to the correct location. If you have cloned the llvm-morello-releases to a different directory you will need to change where they are pointing to. Go to **Window -> preferences -> ArmDS -> toolchains** and add the LLVM tool chain binaries by pointing to the correct bin directory. Apply the changes and let the tool restart.

## Check the FVP model run script

1. In a `File` window go to the scripts directory and open the `runfvp.sh` file in an editor. 
2. Check the following:
* The FVP model is pointing to the directory in which it was downloaded.
* For fvp `version 0.11_9`, the binary files scp_romfw.bin, mcp_romfw.bin, scp_fw.bin and the bts.bin are pointing to the `morello-baremetal-examples/bare-metal-example-scpmcp-binaries/SCPMCPBuiltWithAPreset0x14000000/` directory.

## Run the Model
Run the FVP model from the `morello-baremetal-examples/scripts` directory.

```
./runfvp.sh
```

## Run the examples
You are now ready to run the examples from Development Studio.