Coq Setup
=========

```
apt-cyg-main install ocaml-base
apt-cyg-main install libcairo-devel
apt-cyg-main install libgdk_pixbuf2.0-devel
apt-cyg-main install libgtk2.0-devel
apt-cyg-port install coq

coqtop
```

To use `coqide`, you need X11.

```
startxwin $(which coqide)
```