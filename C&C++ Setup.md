C & C++ Setup
=============

GCC, Make.. etc tools should already be installed by your other stuff.

Otherwise also install these things:

```
apt-cyg-main install subversion
apt-cyg-main install gcc-fortran
apt-cyg-main install clang
apt-cyg-main install llvm
apt-cyg-main install binutils
apt-cyg-main install flex
apt-cyg-main install bison
apt-cyg-main install m4
apt-cyg-main install ncurses
apt-cyg-main install make
apt-cyg-main install patch
apt-cyg-main install gcc-core
apt-cyg-main install gcc-g++
apt-cyg-main install perl
apt-cyg-main install autoconf
apt-cyg-main install automake
apt-cyg-main install libtool
apt-cyg-main install libxml2
apt-cyg-main install python
apt-cyg-main install gdb
apt-cyg-main install cgdb
```

Check the last goo llvm and clang version: http://root.cern.ch/svn/root/trunk/interpreter/cling/LastKnownGoodLLVMSVNRevision.txt

The last good version of cling is the revision number located in: http://root.cern.ch/svn/root/trunk/interpreter/cling/

```sh
mkdir Cling && cd Cling
svn co -r 179269  http://llvm.org/svn/llvm-project/llvm/trunk llvm
cd llvm/tools
svn co -r 179269  http://llvm.org/svn/llvm-project/cfe/trunk clang
svn co -r 49365 http://root.cern.ch/svn/root/trunk/interpreter/cling/
cd ..
cat tools/cling/patches/*.diff | patch -p0
cd ..
mkdir build && cd build
../llvm/configure --enable-cxx11 --prefix=/usr/cling --disable-compiler-version-checks --enable-targets=host
make -j 4
make install
export PATH=/usr/cling/bin:$PATH
cling
```

This script can be used in the future instead: https://raw.githubusercontent.com/karies/cling-all-in-one/master/clone.sh

It doesn't work!

Clang on 64 bit cygwin seems to have problems. Until it's ready, we can't do anything here.