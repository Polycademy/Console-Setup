Graphics
========

graphicsmagick
--------------

```
apt-cyg-main install GraphicsMagick
```

This is much more powerful and commands can be accessed from `gm`. It can convert between different formats and even compress images!

trimage
-------

This does lossless compression on png and jpeg images. It will autoselect the compression to use. It will also remove metadata.

1. Setup X server, see `X Setup`.
2. Download these in order:

Other:

```sh
apt-cyg-main install python
apt-cyg-main install python-pyqt4
apt-cyg-main install optipng
apt-cyg-main install pngcrush
apt-cyg-main install libjpeg-devel
```

advancecomp:

```sh
pushd ~/Source
curl -L https://github.com/amadvance/advancecomp/releases/download/v1.20/advancecomp-1.20.tar.gz | tar xz
pushd advancecomp-1.20
./configure
make
make install
make clean
popd
popd
```

jpegoptim:

```sh
pushd ~/Source
git clone https://github.com/tjko/jpegoptim.git
pushd jpegoptim
./configure
make
make strip
make install
make clean
popd
popd
```

3. Compiling trimage

```sh
pushd ~/Source
git clone https://github.com/Kilian/Trimage.git
pushd Trimage
python setup.py install
popd
```

Run it with:

```
startxwin $(which trimage)
```

Command line usage (yes, you still need X to run it, and yes it is ugly!):

```
startxwin $(which trimage) -f /path/to/image.jpg
startxwin $(which trimage) -d /path/to/directory/of/images
```

Or you could just use this: http://hugogiraudel.com/2013/07/29/optimizing-with-bash/

Image Feature Extraction
------------------------

* http://ilastik.org/index.html