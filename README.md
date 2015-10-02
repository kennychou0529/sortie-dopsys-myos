Trivial Hello World OS

This original code is from:
- Sources: [sortie/dopsys/myos](https://cs.au.dk/~sortie/dopsys/myos/)
- Instructions: [sortie/dopsys/osdev](https://cs.au.dk/~sortie/dopsys/osdev/) which starts with:

But it looks like better instructions is actually at [osdev.org](http://osdev.org). Within osdev.org see:
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC distrutation is 

But it looks like better instructions is actually at [osdev.org](http://osdev.org). Within osdev.org see:
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC distrutation is 

But it looks like better instructions is actually at [osdev.org](http://osdev.org). Within osdev.org see:
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC distrutation is 

But it looks like better instructions is actually at [osdev.org](http://osdev.org). Within osdev.org see:
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC distrutation is 

But it looks like better instructions is actually at [osdev.org](http://osdev.org). Within osdev.org see:
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC distrutation is 

But it looks like better instructions are actually at [osdev.org](http://osdev.org):
- [GCC Cross Compiler](http://wiki.osdev.org/GCC_Cross-Compiler) this actually refers to building gcc 5.2.0. One interesting item is that in the GCC 5.2.0 source code that you'll download is the bash script gcc-5.2.0/contrib/download_prerequisites. The interesting this is that is downloading "loader" versions not the ToT (Tip of Tree) that I used.
- [Bare Bones](http://wiki.osdev.org/Bare_Bones) from osdev.org for the "Hello, kernel World!" kernel which appears to be idential to sortie's, I wonder which came first?


So this "Operating System" is loaded via a grub loader. The OS is trivial and only writes a string to to the VGA display memory. It consits of an assembly langange boot.s file written in 386 assembler that simply sets up the stack and calls the C routine "kmain". Kmain initializes the VGA screen at 0xB8000 and writes the string into the VGA screen.

First, clone this code somewhere maybe:
```
mkdir -p ~/prgs/sortie-dopsys-myos
```
Then clone the repo
```
cd ~/prgs/sortie-dopsys-myos
git clone http://github.com/winksaville/sortie-dopsys-myos.git
```
or
```
cd ~/prgs/sortie-dopsys-myos
git clone git@github.com:winksaville/sortie-dopsys-myos.git
```
We can't use the host compiler because it assumes linx is the OS when infact we're starting with nothing. Instead a cross compiler must be built and since this only runs on a 386 PC we'll create a i586-elf compiler.  So choose where the cross toolchain bins, libraries ... will be installed, for instance $HOME/opt/cross. This will then be used as the prefix when configuring binutils and gcc and the resulting binaries need to be on the PATH:
```
echo 'export PATH="$HOME/opt/cross/bin:$PATH"' >> ~/.bashrc
```
Now get the latest versions of gcc tools and save to a convenient location:
```
mkdir -p ~/prgs/gcc-tool-chain/downloads
cd ~/prgs/gcc-tool-chain/downloads
wget ftp://ftp.gnu.org/gnu/binutils/binutils-2.25.1.tar.bz2
wget http://ftp.gnu.org/gnu/gcc/gcc-5.2.0/gcc-5.2.0.tar.bz2
wget https://gmplib.org/download/gmp/gmp-6.0.0a.tar.xz
wget http://www.mpfr.org/mpfr-current/mpfr-3.1.3.tar.xz
wget ftp://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz
cd ..
```
Untar the files
```
tar -xvf downloads/binutils-2.25.1.tar.bz2
tar -xvf downloads/gcc-5.2.0.tar.bz2
tar -xvf downloads/gmp-6.0.0a.tar.xz
tar -xvf downloads/mpfr-3.1.3.tar.xz
tar -xvf downloads/mpc-1.0.3.tar.gz
```
Make symbolic links in gcc to the multi-precision libraries that have been downloaded.
```
cd gcc-5.2.0
ln -s ../gmp-6.0.0 gmp
ln -s ../mpfr-3.1.3 mpfr
ln -s ../mpc-1.0.3 mpc
cd ..
```
Create build directories for binutils and gcc.
```
mkdir -p builds/binutils-2.25.1-i586
mkdir -p builds/gcc-5.2.0-i586
```
Then configure and build binutils with the prefix of $HOME/opt/cross (does not need to exist), target will be i586-elf and disable “native language support”.
```
cd builds/binutils-2.25.1-i586
../../binutils-2.25.1/configure --prefix=$HOME/opt/cross --target=i586-elf --disable-nls
make all -j4
make install
cd ../..
```
Then configure and build gcc c and c++ and we also specify --without-headers as this is a bare metal compiler.
```
cd builds/gcc-5.2.0-i586
../../gcc-5.2.0/configure --prefix=$HOME/opt/cross --target=i586-elf --disable-nls --enable-languages=c,c++ --without-headers
make all-gcc -j4
make install-gcc
make all-target-libgcc -j4
make install-target-libgcc
```
Now run make and then qemu to test.
```
cd ~/prgs/sortie-dopsys-myos
make
make run-qemu
```
