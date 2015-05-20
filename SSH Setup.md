SSH Setup
=========

Setting up SSH
--------------

Create a `.ssh` folder at your home directory.

Follow the instructions here (https://help.github.com/articles/generating-ssh-keys) to create an `identity` and `identity.pub`.

Send the public key to wherever you want to access. It's public!

You can add your identity key by just running `ssh-add`. By default it will look for your identity key.

Once you've added your keys to your `ssh-agent`, you can show your currently added keys:

```
ssh-add -l
```

To remove the cached keys:

```
ssh-add -D
```

This means now for any git repositories, you must use the SSH clone URL, not the HTTPS url. Only the SSH clone URL works with SSH keys.
So if you're cloning projects, always the SSH clone URL. If you have projects that is already using the HTTPS url, you can convert them:

```
git remote set-url REMOTENAME SSHURL
```

For example:

```
git remote set-url origin git@github.com:Polycademy/Console-Setup.git
```

Remember ssh-agent is only there for caching connection credentials. It's only useful for the current session. When you restart, credentials are clean-slated. You can use the `.ssh/config` instead to make ssh to use particular credentials when connecting to certain places. It has a defaulting mechanism, where it will the default identity file if it doesn't have a particular host record.


Setting Up SSH Server
---------------------

Use this: http://www.larsavery.com/blog/how-to-install-sshd-secure-shell-server-on-windows-using-cygwin/

Once this is go into `Start > Settings > Control Panel > Administrative Tools > Services` and look for `CYGWIN sshd`, and change the startup type to manual or automatic depending on whether you want sshd to be running on Windows bootup.

When starting the sshd, open up Cygwin as Administrator and run:

```
net start sshd
```

To close it, run:

```
net stop sshd
```

Also Cygwin's sshd by default uses password. So just run this for a test:

```
ssh USER@127.0.0.1
```

Putty
-----

Sometimes applications require the use of Putty. Putty uses its own key format, so we need to convert our openssh keys to putty. First get putty package.

```
apt-cyg-port install putty
```

Run this:

```
puttygen ~/.ssh/key -o ~/.ssh/key.ppk
```

Make sure to add `.ppk` extension to differentiate it from OpenSSH keys.

Public keys do not need to be converted, unless you're intending to run a putty SSH server.

Now you have putty equivalent to your openssh keys. You can use these `.ppk` keys to initiate putty connections using plink. All of the putty functionality has been installed.

SSH Config
----------

```
# Current identity file
IdentityFile ~/.ssh/identity

# Allow local commands to run from ~C !command
PermitLocalCommand yes

# Allow sharing of already existing connections
# Unfortunately does not currently work on Cygwin
# ControlMaster auto
# ControlPath ~/.ssh/control:%h:%p:%r

# Host declarations...
```