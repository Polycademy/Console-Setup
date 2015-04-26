X Setup
=======

Install X server first:

```
apt-cyg-main install xorg-server
apt-cyg-main install xinit
```

To run X:

```
startxwin
```

To run some GUI program (where `name` is the executable).

```
startxwin $(which name) param1 param2
```