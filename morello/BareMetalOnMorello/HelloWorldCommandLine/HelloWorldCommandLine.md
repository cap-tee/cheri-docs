# Creating and running a bare metal "hello world" application on the command line
 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

 ## Overview
 This guide covers
 * Locate the example C / C++ files
 * Using the make file to compile the example code into binary files
 * Format for compiling Morello code into binary files
 * Generating image files for the FVP
 * Run the Hello World Application on the FVP

## Assumptions

* The prebuilt Morello supported LLVM binaries for bare metal have been installed
* The Morello FVP has been installed
* The bare metal firmware has been built

## Locate the example C / C++ files
Navigate to the directory where you installed the LLVM binaries `projects/baremetalsources/llvm-morello-releases`. The example C / C++ files can be found under the `examples` directory.

* howdy.c - *C hello world*
* howdy++.cpp - *C++ hello world*
* ccall.c
* howdy-cap-call.c - *C hello from a capability call*

## Using the make file to compile the example code into binary files

Within the `examples` directory is a `MakeFile` which can be used to compile the example code and show the different build settings.

To use the make file you need to ensure the path to `CLANG` is set correctly. Open `MakeFile` in an editor and edit the line, for example 

*   From: CLANG?=/opt/morello-toolchain-1.0/bin/clang
* To: CLANG?=~/projects/baremetalsources/llvm-morello-releases/bin/clang

Several executables can be generated

* howdy
* howdy++
* `howdy-morello` - *C hello world compiled for Morello*
* howdy-cap-call
* `howdy-purecap` - *C hello world compiled for Morello pure capability*
* ccall

Run the make file and compile all of the files
```
make -f ~/projects/baremetalsources/llvm-morello-releases/examples/Makefile
```

The files are now ready to be turned into images for the Morello FVP.

## Format for compiling Morello code into binary files

You can also use the command line to compile your own source files to run on the Morello FVP. The format for compiling source files for Morello from the source file directory such as `examples` is

### C files
For Morello:
```
<clang path> -target aarch64-none-elf -march=morello -o image mycfile.c
```
For Morello pure capability:
```
<clang path> -target aarch64-none-elf -march=morello+c64 -mabi=purecap -o image mycfile.c
```

### C++ files

When compiling C++ files add the `-D_GNU_SOURCE` flag to the compilation command. Exceptions are not supported, so also add the `-fno-exceptions` flag.

For Morello:
```
<clang++ path> -target aarch64-none-elf -march=morello -fno-exceptions -c -o out.o myc++file.cpp -D_GNU_SOURCE
```
For Morello pure capability:
```
<clang++ path> -target aarch64-none-elf -march=morello+c64 -mabi=purecap -fno-exceptions -c -o out.o myc++file.cpp -D_GNU_SOURCE
```
### Example clang path

```
~/projects/baremetalsources/llvm-morello-releases/bin/clang
```

### Example clang++ path

```
~/projects/baremetalsources/llvm-morello-releases/bin/clang++
```

## Generating image files for the FVP

The LLVM project comes with a script `make-bm-image.sh` that produces an image to run on the Morello FVP from an ELF binary. From the `examples` directory

```
./make-bm-image.sh -i image -o morello_image
```

## Run the Hello World Application on the FVP

To run the model navigate to `~/projects/morello_workspace/model/FVP_Morello` directory

```
./models/Linux64_GCC-6.4/FVP_Morello --data Morello_Top.css.scp.armcortexm7ct=/home/osboxes/projects/baremetalsources/bare-metal-example-scpmcp-binaries/SCPMCPBuiltWithAPreset0x14000000/scp_romfw.bin@0x0 --data Morello_Top.css.mcp.armcortexm7ct=/home/osboxes/projects/baremetalsources/bare-metal-example-scpmcp-binaries/SCPMCPBuiltWithAPreset0x14000000/mcp_romfw.bin@0x0 -C Morello_Top.soc.scp_qspi_loader.fname=/home/osboxes/projects/baremetalsources/bare-metal-example-scpmcp-binaries/SCPMCPBuiltWithAPreset0x14000000/scp_fw.bin -C css.scp.armcortexm7ct.INITVTOR=0x0 -C css.mcp.armcortexm7ct.INITVTOR=0x0 --data=/home/osboxes/projects/baremetalsources/llvm-morello-releases/examples/morello_image@0x14000000 -C css.cluster0.cpu0.semihosting-heap_base=0 -C css.cluster0.cpu0.semihosting-heap_limit=0xff000000 -C css.cluster0.cpu0.semihosting-stack_limit=0xff000000 -C css.cluster0.cpu0.semihosting-stack_base=0xffff0000
```
