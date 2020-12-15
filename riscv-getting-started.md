# CHERI-RISC-V Getting Started

This is a simplified getting started guide for CHERI-RISC-V, mainly based on https://ctsrd-cheri.github.io/cheri-exercises/cover/index.html.

This guide was written using Ubuntu 18.04.

## Build CheriBSD & QEMU

### Install prerequisites
```
sudo apt-get install libtool pkg-config clang bison cmake ninja-build samba flex texinfo libglib2.0-dev libpixman-1-dev libarchive-dev libarchive-tools libbz2-dev libattr1-dev libcap-ng-dev autoconf
```
 
### Get cheribuild 
 
```
cd ~
git clone https://github.com/CTSRD-CHERI/cheribuild.git
```

### Compile CheriBSD & QEMU (this will take some time)

```
cd cheribuild/
./cheribuild.py cheribsd-riscv64-purecap -d
./cheribuild.py run-riscv64-purecap -d
```

This will put the results into `~/cheri`.

## Build an exercise (in pure-capability and "hybrid" mode)

```
cd ~/cheri
git clone https://github.com/CTSRD-CHERI/cheri-exercises.git
cd cheri-exercises/src/exercises/buffer-overflow
../../../tools/ccc riscv64-purecap buffer-overflow.c -o buffer-overflow

[now remove the call to main_assert() in buffer-overflow.c]

../../../tools/ccc riscv64-hybrid buffer-overflow.c -o buffer-overflow-hybrid
```

## Launch QEMU and execute the binaries

```
cd ~/cheribuild
./cheribuild.py run-riscv64-purecap
```

*Hint:* To exit QEMU, press `Ctrl-a` then `x`

After boot has completed, login as `root` (no password). Now, we can mount `~/cheri` on the host into the guest (see also https://github.com/CTSRD-CHERI/cheri-exercises/pull/26): 

```
mount_smbfs -I 10.0.2.4 -N //10.0.2.4/source_root /mnt
```

Now we can copy the files over:

```
cp /mnt/cheri-exercises/src/exercises/buffer-overflow/buffer-overflow .
cp /mnt/cheri-exercises/src/exercises/buffer-overflow/buffer-overflow-hybrid .
```

Now you can exectute them. For `buffer-overflow`, you should get:

```
c = c
In-address space security exception (core dumped)
```

while `buffer-overflow-hybrid` should continue with an OOB memory access.
