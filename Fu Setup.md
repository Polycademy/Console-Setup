Fu Setup
========

Fu is a python utility that queries CommandlineFu.com. It's useful for telling you what commands you can use.

```
cd ~/Source
git clone git://github.com/samirahmed/fu.git
cd ~/bin
ln -s `readlink -f ~/Source/fu/fu` fu
fu --help
```

To get `--open` to work. You need to set the BROWSER environment variable and export it.

```
export BROWSER=`cygpath /path/to/browser`
```

OR you can just export to an executable such as `export BROWSER="firefox"`. Then add `firefox` as an executable to your PATH.

```
cd ~/bin
ln -s "C:/path/to/firefox.exe" firefox
```