Trivial Hello World OS

Sources: [sortie/dopsys/myos](https://cs.au.dk/~sortie/dopsys/myos/)
Instructions: [sortie/dopsys/osdev](https://cs.au.dk/~sortie/dopsys/osdev/) which starts with:

"This document will guide you through creating your own operating
system capable of writing "Hello, World!" to the screen, including
the details of how to set up a proper cross compilation environment
and a basic build system. The final result is designed to be minimal
and as easy to understand as possible: My reference implementation
is just 251 lines of source code."


The document ends with a pointer to [osdev.org](http://osdev.org)


Tweaks for the instructions:

1) I have gcc.5.2 and I got build errors trying to build the older
versions. Instead I got the latest versions:
```
wget ftp://ftp.gnu.org/gnu/binutils/binutils-2.25.1.tar.bz2
wget http://ftp.gnu.org/gnu/gcc/gcc-5.2.0/gcc-5.2.0.tar.bz2
wget https://gmplib.org/download/gmp/gmp-6.0.0a.tar.lz
wget http://www.mpfr.org/mpfr-current/mpfr-3.1.3.tar.xz
wget ftp://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz
```

2) The two lines for making and installing libgcc:
```
make all-libgcc # install gcc support library
make install-libgcc # install gcc support library
```
The are now:
```
make all-target-libgcc # install gcc support library
make install-target-libgcc # install gcc support library
```
