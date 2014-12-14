CasperJS & PhantomJS & SlimerJS Setup
=====================================

Download the full zip files from both PhantomJS and CasperJS official website.

Extract the phantomjs binary, and move the executable to your `~/bin/phantomjs.exe`. CasperJS doesn't understand Cygwin symlinks.

Extract CasperJS into `~/Source/casperjs`.

Symlink an cygwin absolute path from `~/Source/casperjs/bin/casperjs.exe` to `~/bin/casperjs.exe`.

For example:

```
ln -s /cygdrive/c/Users/CMCDragonkai/Source/casperjs/bin/casperjs.exe ~/bin/casperjs.exe
```

Run `casperjs`.

Remember you can also just use git clone too!

Todo: SlimerJS