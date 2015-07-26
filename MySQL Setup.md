MySQL Setup
===========

```
apt-cyg-main install mysqld
apt-cyg-main install mysql
```

Then run:

```
# To begin MySQL setup run the following:
mysql_install_db

# Run mysql - you'll get a firewall alert from windows if you have it active.
mysqld_safe &

# Immediately following that, it would be wise to run the following:
mysql_secure_installation
```

You should prevent remote access, create your own user matching your cygwin username like `'CMCDragonkai'@'localhost'`.

Then login via: `mysql`.

Shut it down with: `mysqladmin shutdown`.