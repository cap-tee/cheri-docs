          # EL2N_c_loop - Loading programs at EL2N

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

 ## Overview

 This example shows how to load a small c program at EL2N and follows on from [EL2N_s_loop](./../EL2N_s_loop/EL2N_s_loop.md) which should be followed first.

 ## Creating a program for EL2N without any EL3 initialisation code

Follow the steps to create and run a small program at EL2N without any Morello initialisation code. Use the linker and image generation script from [EL2N_s_loop](./../EL2N_s_loop/EL2N_s_loop.md).

**Step 1/2 - create a new directory and obtain files**

Either create a new directory or use the example code in `commandLine/bootflow/EL2N_c_loop` directory. It uses the same `testloop.s` and `testloopmain.c` files as [EL3_c_loop](./../EL3_c_loop/EL3_c_loop.md)

**Step 3 - create object file**

For the remaining steps a script called make_el2.sh is provided, but you must ensure the paths are correct which will depend upon where you have installed the various different repositories. Ensure you are in the `commandLine/bootflow/EL2_c_loop` directory and run the script 

```
./make_el2.sh
```

The following describes the steps to follow, or the steps that the script carries out.

Create an object file of the test program using clang.

```
~/projects/baremetalsources/llvm-morello-releases/bin/clang --target=aarch64-none-elf -march=morello -O0 -g -MMD -MP -c -o "output/testloopmain.o" "testloopmain.c"
~/projects/baremetalsources/llvm-morello-releases/bin/clang --target=aarch64-none-elf -c -o "output/testloop.o" "testloop.s"
```

**Step 4 - Create elf file**
Create an elf file from the object file with minimal initialisation. You must ensure the image entry point is pointing to the `main` function using the `-Xlinker --entry=main` option. This will bypass the _start Morello initialisation code thats sets up specific EL3 registers (see the [Initialisation sequence for Morello](./../../../BareMetalOnMorello/DefaultSetup/InitSequence/InitSequence.md) for more information).

```
~/projects/baremetalsources/llvm-morello-releases/bin/clang --target=aarch64-none-elf -march=morello -Xlinker --entry=main -o "output/testloop.elf"  ./output/testloopmain.o output/testloop.o
```
**Step 5 - Create image file**

Create the image file using the image script `make-bm-image_el2.sh`.

```
~/projects/baremetalsources/llvm-morello-releases/make-bm-image_el2.sh -i output/testloop.elf -o output/testloop_image_el2
```

**Step 6 - Run the FVP model**
Run the FVP with the loaded image. To see the program running in [**Development Studio**](./../../../BareMetalOnMorello/InstallingArmDevStudio/InstallingArmDevStudio.md) you need to include the `--cadi-server` option. Once the model is running, connect to the model from Development Studio using a debug connection with **Connect only** selected and then run (see the section **Creating a Connect Only Debug Connection** under [EL3_s_loop](./../EL3_s_loop/EL3_s_loop.md)). You can run the model without Development Studio but you will not see any output and will not be able to check the memory.

Note the following part of the script is where the image is loaded.

`--data=/home/osboxes/projects/morello-baremetal-examples/commandLine/bootflow/EL2N_c_loop/output/testloop_image_el2@0x14200000`

```
~/projects/morello_workspace/model/FVP_Morello/models/Linux64_GCC-6.4/FVP_Morello --data Morello_Top.css.scp.armcortexm7ct=/home/osboxes/projects/morello_workspace/bsp/build-poky/tmp-poky/deploy/images/morello-fvp/scp_romfw.bin@0x0 --data Morello_Top.css.mcp.armcortexm7ct=/home/osboxes/projects/morello_workspace/bsp/build-poky/tmp-poky/deploy/images/morello-fvp/mcp_romfw.bin@0x0 -C Morello_Top.soc.scp_qspi_loader.fname=/home/osboxes/projects/morello_workspace/bsp/build-poky/tmp-poky/deploy/images/morello-fvp/scp_fw.bin -C Morello_Top.soc.mcp_qspi_loader.fname=/home/osboxes/projects/morello_workspace/bsp/build-poky/tmp-poky/deploy/images/morello-fvp/mcp_fw.bin -C css.scp.armcortexm7ct.INITVTOR=0x0 -C css.mcp.armcortexm7ct.INITVTOR=0x0 --data=/home/osboxes/projects/morello-baremetal-examples/commandLine/bootflow/EL2N_c_loop/output/testloop_image_el2@0x14200000 -C css.pl011_uart_ap.out_file=/home/osboxes/projects/morello_workspace/run-scripts/uart0-bootflow.log -C css.scp.pl011_uart_scp.out_file=/home/osboxes/projects/morello_workspace/run-scripts/scp-bootflow.log -C css.mcp.pl011_uart0_mcp.out_file=/home/osboxes/projects/morello_workspace/run-scripts/mcp-bootflow.log -C css.pl011_uart_ap.unbuffered_output=1 -C displayController=0 -C board.virtio_rng.enabled=1 -C board.virtio_rng.seed=0 -C num_clusters=2 -C num_cores=2 -C board.hostbridge.userNetworking=true -C board.smsc_91c111.enabled=true -C board.hostbridge.userNetPorts="5555=5555" --cadi-server
```

## Checking the Morello Memory

From **Development Studio** open the **Disassembly Window** at 0x14200000. If everything is working you should be able to see the code sections in the BOOT memory region.

**.text section**

**__init start address**

```
EL2N:0x0000000014200000 : ADRP     x0,#0x2000				
```

**memcpy start address**

```
EL2N:0x0000000014200014 : CBZ      x2,#0x18				
```

**memset start address**

```
EL2N:0x0000000014200030 : CBZ      x2,#0x14				

```

**load_elf start address**

```
EL2N:0x0000000014200048 : STP      x29,x30,[sp,#-0x40]!
```

**.rodata section**

**init/fini start address**

```
EL3:0x00000000142002B4 : STP      c29,c30,[sp,#-0x20]!
```

**program**

```
EL3:0x000000001420032C : SUB      sp,sp,#0x20
EL3:0x0000000014200330 : STP      x29,x30,[sp,#0x10]
EL3:0x0000000014200334 : ADD      x29,sp,#0x10
EL3:0x0000000014200338 : MOV      w8,wzr
EL3:0x000000001420033C : STUR     w8,[x29,#-4]
EL3:0x0000000014200340 : STR      w8,[sp,#8]
EL3:0x0000000014200344 : BL       {pc}+20 ; 0x14000358
EL3:0x0000000014200348 : LDR      w0,[sp,#8]
EL3:0x000000001420034C : LDP      x29,x30,[sp,#0x10]
EL3:0x0000000014200350 : ADD      sp,sp,#0x20
EL3:0x0000000014200354 : RET


EL3:0x0000000014200358 : MOV      x0,#0x43
EL3:0x000000001420035C : NOP
EL3:0x0000000014200360 : NOP
EL3:0x0000000014200364 : B        {pc}-12 ; 0x14000358
EL3:0x0000000014200368 : RET
```

**exit code start address**

```
EL3:0x000000001420036C : MOV      x1,x0
```
From **Development Studio** open the **Disassembly Window** at 0xE000000. If everything is working you should be able to see a complete copy of the code that is in the BOOT memory region. 

**.text section**

**__init start address**

```
EL2N:0x00000000E0000000 : ADRP     x0,#0x2000				
```

**memcpy start address**

```
EL2N:0x00000000E0000014 : CBZ      x2,#0x18			
```

**memset start address**

```
EL2N:0x00000000E0000030 : CBZ      x2,#0x14			
```

**load_elf start address**

```
EL2N:0x00000000E0000048 : STP      x29,x30,[sp,#-0x40]!
```

**.rodata section**

**init/fini start address**

```
EL2N:0x00000000E00002B4 : STP      c29,c30,[sp,#-0x20]!
```

**Program**

```
EL2N:0x00000000E000032C : SUB      sp,sp,#0x20
EL2N:0x00000000E0000330 : STP      x29,x30,[sp,#0x10]
EL2N:0x00000000E0000334 : ADD      x29,sp,#0x10
EL2N:0x00000000E0000338 : MOV      w8,wzr
EL2N:0x00000000E000033C : STUR     w8,[x29,#-4]
EL2N:0x00000000E0000340 : STR      w8,[sp,#8]
EL2N:0x00000000E0000344 : BL       {pc}+20 ; 0xE0000358
EL2N:0x00000000E0000348 : LDR      w0,[sp,#8]
EL2N:0x00000000E000034C : LDP      x29,x30,[sp,#0x10]
EL2N:0x00000000E0000350 : ADD      sp,sp,#0x20
EL2N:0x00000000E0000354 : RET

EL2N:0x00000000E0000358 : MOV      x0,#0x43
EL2N:0x00000000E000035C : NOP
EL2N:0x00000000E0000360 : NOP
EL2N:0x00000000E0000364 : B        {pc}-12 ; 0xE0000358
EL2N:0x00000000E0000368 : RET
```

**exit code start address**

```
EL2N:0x00000000E000036C : MOV      x1,x0
```
From **Development Studio** open the **Disassembly Window** at 0x80000000. If everything is working you should be able to see that the program has been copied to DRAM0, along with the init/fini and exit code. You should be able to step through the instructions and see the program looping around.

**init/fini start address**

```
EL2N:0x00000000800101D4 : STP      c29,c30,[sp,#-0x20]!
```

**program**

```
EL2N:0x000000008001024C : SUB      sp,sp,#0x20
EL2N:0x0000000080010250 : STP      x29,x30,[sp,#0x10]
EL2N:0x0000000080010254 : ADD      x29,sp,#0x10
EL2N:0x0000000080010258 : MOV      w8,wzr
EL2N:0x000000008001025C : STUR     w8,[x29,#-4]
EL2N:0x0000000080010260 : STR      w8,[sp,#8]
EL2N:0x0000000080010264 : BL       {pc}+20 ; 0x80010278
EL2N:0x0000000080010268 : LDR      w0,[sp,#8]
EL2N:0x000000008001026C : LDP      x29,x30,[sp,#0x10]
EL2N:0x0000000080010270 : ADD      sp,sp,#0x20
EL2N:0x0000000080010274 : RET

EL2N:0x0000000080010278 : MOV      x0,#0x43
EL2N:0x000000008001027C : NOP
EL2N:0x0000000080010280 : NOP
EL2N:0x0000000080010284 : B        {pc}-12 ; 0x80010278
EL2N:0x0000000080010288 : RET
```

**exit code start address**

```
EL2N:0x000000008001028C : MOV      x1,x0
```