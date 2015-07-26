PHP Setup
---------

Now this would be ideal if it was possible to build PHP inside Cygwin. Unfortunately it's not!

Some of thes dependencies have moved to `apt-cyg-main`.

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
apt-cyg-port install php-PEAR
apt-cyg-port install php-mcrypt
apt-cyg-port install php-gmp
apt-cyg-port install php-shmop
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

When phpbrew works!
-------------------

What we're going to do use Cygwin Ports to install a base of PHP. Then install phpbrew. And from here, install Composer, and then all subsequent cool stuff. 

Using our apt-cyg-port. We can install PHP!

```
apt-cyg-port install php
apt-cyg-port install php-PEAR
//...etc
```

Now we need to bring in all the Cygwin dependencies for us to use phpbrew!

```
apt-cyg-port install re2c
apt-cyg-main install autoconf automake curl libxslt libxslt-devel libtool libxml2 libxml2-devel bison libbz2_1 libreadline7 libreadline-devel libfreetype6 libfreetype-devel libpng15 libjpeg8 libgd2 libicu51 libicu-devel libmhash2 libmhash-devel libmcrypt4 libmcrypt-devel gettext libgettextpo0 libXpm4 libbz2-devel libiconv libcrypt-devel
```

Get phpbrew:

```
curl -OL https://raw.github.com/c9s/phpbrew/master/phpbrew
chmod +x phpbrew
mv phpbrew ~/bin/phpbrew
phpbrew init
```

Add this to your `.zshrc`

```
# phpbrew
if [ -d "${HOME}/.phpbrew" ] ; then
     . $HOME/.phpbrew/bashrc
fi
```

Potentially delete any existing `~/.pearrc`. This would prevent phpbrew from complaining.

Build our PHP and start using it.

```
phpbrew known
phpbrew install X.X.X +default +mb +openssl -- --enable-maintainer-zts
phpbrew clean
phpbrew list
phpbrew switch X.X.X
```

Install some extensions:

```
phpbrew ext install xdebug
phpbrew ext install pthreads
phpbrew ext install yaml
```

Install Composer

```
phpbrew install-composer
```

Then add this to your `.zshrc`:

```
export COMPOSER_HOME="$HOME/.composer"
if [ -d "${HOME}/.composer/vendor/bin" ] ; then
    export PATH="${HOME}/.composer/vendor/bin:${PATH}"
fi
```

Then install psy/psysh via `composer global require psysh/psy`.

Note that phpbrew relies on existing system php. But once you built your own custom versions, you can install extensions.. etc.

HHVM
----

hhvm is not installable on Cygwin yet. Mainly the dependencies required to build are not packaged yet.

Psysh
-----

Create a file `~/.psysh/rc.php` containing:

```php
<?php

if (file_exists(getcwd() . '/vendor/autoload.php')) {
    $composerAutoloader = getcwd() . '/vendor/autoload.php';
    return [
        'defaultIncludes' => array(
            $composerAutoloader,
        ),
    ];
}

// sets the default timezone to the TZ environment variable, that is usually available on Linux and Cygwin
date_default_timezone_set(getenv('TZ'));

?>
```

PECL extensions
---------------

Most extensions that are useful are already available over Cygwin.

For the ones that are not, you can install like:

```
pecl channel-update pecl.php.net
pecl install <ext>
cd /etc/php5/conf.d
echo "extension = <ext>.dll" >> <ext>.ini
```

I installed the `sync` extension.

Even on Cygwin, we still use `dll` and no `so`.

If you replace it with the cygwin packages, you should uninstall them first `pecl uninstall <ext>; rm /etc/php5/conf.d/<ext>.ini`, and then install the package.

Check if it all works by using:

```
php -m
```

The phpbrew extensions are not relevant to this. They are installed for phpbrew built runtimes.