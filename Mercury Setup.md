Mercury Setup
=============

Doesn't yet work in 64bit cygwin! This is what you would do if it did work in 64 bit cygwin!

Attempt to compile Mercury on this computer?

* https://github.com/Mercury-Language/mercury/blob/master/README.Cygwin

So there may be problems for 64 bit Cygwin. You'll need to test it out. You have to use ROTD. http://dl.mercurylang.org/index.html

Extract the archive to a directory.

Mercury has a lot of dependencies. I'm running this after I've installed all the other things. So the only thing extra is flex at the moment. The `configure` command should tell you the rest.

```
apt-cyg install flex
apt-cyg install bison

configure --prefix "/cygdrive/c/Users/CMCDragonkai/mercury"
make PARALLEL=-j2
make PARALLEL=-j2 install
make clean
```