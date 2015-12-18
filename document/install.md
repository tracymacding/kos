## 

Tools needed is in directory "tool" under kos

## Preparation

Before install necessary like gmp, gcc, etc ..., please prepare installation directory  
Suppose, we install all the necessary lib and tool to dir "/home/userX/tool"  
Create it if not exist

```
mkdir -p /home/userX/tool
```

Then, update PATH, LD_LIBRARY_PATH and C_INCLUDE_PATH

```
export PATH="$PATH:/home/userX/tool/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/home/userX/tool/lib"
export C_INCLUDE_PATH="$C_INCLUDE_PATH:/home/userX/tool/include"
```

if you update the .bashrc file, do not forget run
```
source ~/.bashrc
```

### install gmp from source code

```
tar xjf gmp-5.0.2.tar.bz2
cd gmp-5.0.2
./configure --prefix=/home/userX/tool
make & make install
cd ..
```

### install mpfr from source code

```
tar xjf mpfr-3.0.1.tar.bz2
cd mpfr-3.0.1
./configure --prefix=/home/userX/tool LDFLAGS="-L/home/userX/tool/lib"
make & make install
cd ..
```

### install mpc from source code

```
tar xzf mpc-0.9.tar.gz
cd mpc-0.9
./configure --prefix=/home/userX/tool \
	LDFLAGS="-L/home/userX/tool/lib"
make & make install
cd ..
```

### install binutil from source code

```
tar xjf binutils-2.21.1.tar.bz2
cd binutils-2.21.1
./configure --prefix=/home/userX/tool --target=i386-jos-elf --disable-werror \
	LDFLAGS="-L/home/userX/tool/lib"
make & make install

cd ..

i386-jos-elf-objdump -i
# Should produce output like:
# BFD header file version (GNU Binutils) 2.21.1
# elf32-i386
#  (header little endian, data little endian)
#   i386...
```

### install gcc from source code

```
tar xjf gcc-core-4.6.1.tar.bz2
cd gcc-4.6.1
mkdir build              # GCC will not compile correctly unless you build in a separate directory
cd build
../configure --prefix=/home/userX/tool \
    --target=i386-jos-elf --disable-werror \
    --disable-libssp --disable-libmudflap --with-newlib \
    --without-headers --enable-languages=c \
	LDFLAGS="-L/home/userX/tool/lib"

make all-gcc
make install-gcc 
make all-target-libgcc
make install-target-libgcc
cd ../..

i386-jos-elf-gcc -v
# Should produce output like:
# Using built-in specs.
# COLLECT_GCC=i386-jos-elf-gcc
# COLLECT_LTO_WRAPPER=/usr/local/libexec/gcc/i386-jos-elf/4.6.1/lto-wrapper
# Target: i386-jos-elf

```

### install gdb from source code

```
tar xjf gdb-7.3.1.tar.bz2
cd gdb-7.3.1
./configure --prefix=/usr/local --target=i386-jos-elf --program-prefix=i386-jos-elf- \
    --disable-werror LDFLAGS="-L/home/userX/tool/lib"
 
make all
make install
cd ..
```

### install qemu from source code

```
tar xjf qemu-2.5.0.tar.bz2
cd qemu-2.5.0
./configure --prefix=/home/userX/tool --target-list=i386-softmmu,i386-linux-user
make & make install
cd ..
```
