Compiler
========

When you're in Cygwin, you can actually compile your programs into Cygwin or MinGW.

If you're compiling to Cygwin, the program can only run in a Cygwin environment.

If you're compiling to MinGW, the program can run on Windows without any extra libraries.

This means for portability, you should be compiling to MinGW.

The cygwin compiler tools are actually capable of choosing to compile to Cygwin or MinGW.

That being said, I would like to build tools that are cross compiler into both Cygwin and MinGW.

This is because I work in a Cygwin environment, and having apps compiled to Cygwin ensures better integration especially with pathing issues. But MinGW is useful for Windows people who are not using Cygwin. That being said, Cygwin can still work with MinGW apps, because Cygwin can launch Windows applications.

Note that many of your language virtual machines are built on MinGW. But some are built on Cygwin. For example, our current PHP installation is built on Cygwin, thus it has a tighter integration with the Cygwin environment.

Note also that the compiler tools we're talking about here is GCC. LLVM is another compiler tool, but it also probably can compiler to Cygwin or MinGW as well.

Anyway this means there's a lack of tools built for Cygwin, unless it's built by Cygwin people. Many cross-compilers don't support Cygwin at all!

Cross Compilation
-----------------

Cross compilation can be done in 3 ways:

1. Run the compiler on the target. Requires VMs or a compile farm.

2. Compile a cross compiler toolchain. This allows host to compile program to target. This is freaking complicated!!! (preshing.com/20141119/how-to-build-a-gcc-cross-compiler/)

3. Compiler natively supports cross compilation so you just point the target and away you go! Go supports this. C and C++ not so much. Rust not yet.