

# Using Cmake to generate firmware binaries for fvp or hardware (soc)
 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

This document describes the process to generate firmware ready for bare metal development.

Further information including building a full software stack can be found here: https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/user-guide.rst 

## Install dependencies

Ensure you have created a morello workspace directory
```
mkdir morello_workspace
```

Install the following dependencies
```
sudo apt-get update
sudo apt-get install autoconf autopoint bc bison build-essential curl \
    device-tree-compiler dosfstools doxygen flex gettext-base git \
    libssl-dev libtinfo5 linux-libc-dev-arm64-cross m4 mtools parted \
    pkg-config python python3-distutils rsync snapd unzip uuid-dev wget

sudo snap install cmake --classic
```

## Manual install Repo tool

Do a manual install of the repo tool because the apt-get method does not install the required version. More information can be found here: https://source.android.com/setup/develop#installing-repo. If you run into problems see: https://git.morello-project.org/morello/docs/-/blob/morello/release-1.3/troubleshooting-guide.rst. 

Before you start check you have a ~/bin/ directory in the system and add it to the PATH as follows:
```
PATH="${HOME}/bin:${PATH}"
```

Update the ca-certificates package:
```
sudo apt-get install ca-certificates
```
Install:
```
export REPO=$(mktemp /tmp/repo.XXXXXXXXX)
curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo
gpg --keyserver keys.openpgp.org --recv-key 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65
```
If this works you should get something like this:
```
Gpg: key 16530D5E920F5C65: public key “Repo Maintainer repo@android.kernel.org” imported
Gpg: Total number processed: 1
Gpg: imported: 1
```
Continue with:
```
curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} && install -m 755 ${REPO} 
~/bin/repo
```
you can check the version to make sure it is 2.15 or higher
```
repo version
```

## Fetch firmware sources for bsp only, release-1.3

From the `morello_workspace` directory download the files to build the firmware only for bare metal development.
```
repo init \
 -u https://git.morello-project.org/morello/manifest.git \
    -b  morello/release-1.3 \
    -g bsp 
repo sync
```

If this works correctly you should get:
```
Repo sync has finished successfully
```

## Build firmware binaries for hardware (soc)

First, check the dependencies for building source code. From `morello_workspace directory`
```
./build-scripts/check_dep.sh
```

If this passes, build for hardware with firmware only

```
./build-scripts/build-all.sh -p soc -f none
```
Once finished, the build files are located under `morello_workspace/output/soc` directory.
* fip.bin
* mcp_fw.bin
* scp_fw.bin

## Build firmware binaries for fvp
Because the -p flag has changed, clean should be performed before a build.
```
./build-scripts/build-all.sh -p fvp -f none clean
./build-scripts/build-all.sh -p fvp -f none
```
Once the build is finished, the build files are located under `morello_workspace/output/fvp` directory.
* fip.bin
* mcp_fw.bin
* scp_fw.bin
* mcp_romfw.bin
* scp_romfw.bin
* tf_bl1.bin