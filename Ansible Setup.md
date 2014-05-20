Ansible Setup
=============

Install from source. Git clone their Ansible directory. Don't source their `env-setup`. Install the Python dependencies. Run make install at the root.

Also add this to your `.zshrc`:

```
# ansible
hash ansible 2>/dev/null && {
    # Cygwin's openssh does support ControlMaster
    export ANSIBLE_SSH_ARGS="-o ControlMaster=no"
    export ANSIBLE_HOSTS="${HOME}/ansible_hosts"
}
```

Make sure you have a file called `ansible_hosts` at your home directory containing an IP such as `127.0.0.1`.