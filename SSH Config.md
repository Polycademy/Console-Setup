SSH Config
==========

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