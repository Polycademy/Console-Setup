PHP Setup
---------

You can install PHP via the apt-cyg-port.

```
apt-cyg-port install php
apt-cyg-port install php-json
apt-cyg-port install php-curl
apt-cyg-port install php-ctype
apt-cyg-port install php-session
apt-cyg-port install php-pdo_mysql
apt-cyg-port install php-sockets
apt-cyg-port install php-posix
apt-cyg-port install php-mbstring
apt-cyg-port install php-zlib
apt-cyg-port install php-zip
apt-cyg-port install php-bcmath
apt-cyg-port install php-readline
apt-cyg-port install php-tokenizer
//...etc
```

Then get Composer, and put it in your `~/bin`. Then:

```
chmod +x composer.phar
mv composer.phar composer
```

Then add this to your `.zshrc`:

```
    # Bring in Composer binaries
    if [ -d "${HOME}/.composer/vendor/bin" ] ; then
        export PATH="${HOME}/.composer/vendor/bin:${PATH}"
    fi
```

Then install psy/psysh via `composer global require psysh/psy`.

Issues
------

Attempt PHPBrew, but will require dependencies on 64 bit Cygwin. Instead attempt on 32 bit Cygwin.