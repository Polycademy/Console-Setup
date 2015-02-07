Media
=====

Youtube Downloader
------------------

```
pip install youtube-dl
```

ffmpeg
------

This has not been fully confirmed yet. I have the source ffmpeg. But I have not made it. The dependencies are not there, and I'm not sure about the compiler flags.

```
apt-cyg-main install binutils
apt-cyg-main install pkg-config
apt-cyg-main install gcc-core
apt-cyg-main install make
apt-cyg-main install git
apt-cyg-main install texinfo
apt-cyg-main install bc
apt-cyg-main install diffutils
apt-cyg-main install yasm
apt-cyg-main install libvorbis-devel
apt-cyg-main install libtheora-devel
apt-cyg-main install liborc0.4-devel
apt-cyg-main install speex-devel
apt-cyg-main install libgsm-devel
apt-cyg-main install libvpx-devel

apt-cyg-port install libmp3lame-devel
apt-cyg-port install libschroedinger1.0-devel
apt-cyg-port install libxvidcore-devel
apt-cyg-port install libx264-devel
apt-cyg-port install libopus-devel

cd Source
git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg

dos2unix configure 

make distclean

./configure \
    --enable-shared \
    --disable-static \
    --enable-gpl \
    --enable-nonfree \
    --enable-libvorbis \
    --enable-libtheora \
    --enable-libxvid \
    --enable-libspeex \
    --enable-libgsm \
    --enable-libmp3lame \
    --enable-libschroedinger \
    --enable-libx264 \
    --enable-libopus \
    --enable-libvpx

make

make install
```

These AAC has not been figured out yet. We still need to download the libraries.

```
#--enable-libfaac
#--enable-libfdk-aac
#--enable-libfaad
```

There is also some problems with regards to the configuration options.

Use this:

```
pkg-config --libs X
```

Where X can be particular library you're looking for in PKG_CONFIG_PATH.

See this for further information: http://juliensimon.blogspot.com.au/2008/12/howto-compiling-ffmpeg-x264-mp3-xvid.html

You need to uninstall all the codecs, reinstall it. See if pkg-config changes. Then try changing compiler flags...?

See this: http://wiki.collectiveaccess.org/index.php?title=Compiling_ffmpeg