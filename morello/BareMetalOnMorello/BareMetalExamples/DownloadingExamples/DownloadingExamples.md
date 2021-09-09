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

## Firmware binaries license

**SCP-firmware binaries** are released under a BSD-3-Clause license 

https://git.morello-project.org/morello/scp-firmware/-/blob/morello/master/license.md

---
License

Copyright (c) 2011-2021, Arm Limited and Contributors. All rights reserved.
Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:


Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.


Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.


Neither the name of Arm nor the names of its contributors may be used to
endorse or promote products derived from this software without specific prior
written permission.


THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

---


**Trusted Firmware-A binaries** are released under a BSD-3-Clause license

https://git.morello-project.org/morello/trusted-firmware-a/-/blob/morello/master/docs/license.rst

---

License

Copyright (c) [XXXX-]YYYY, <OWNER>. All rights reserved.

Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

-  Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.

-  Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.

-  Neither the name of Arm nor the names of its contributors may be used to
endorse or promote products derived from this software without specific
prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


---

