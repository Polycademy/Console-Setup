Rust Setup
==========

These instructions are relevant for Rust 1.0

We need to install 64 bit Rust windows binary from the official Rust site.

Install Rust into `~/Rust`. Do not choose to allow Rust to associate the bin into your PATH. You will do this through ZSH.

Add these to your `.zshrc`.

```sh
# rust (windows installation)
if [ -d "${HOME}/Rust" ] ; then
    export PATH="$HOME/Rust/bin:$PATH"
fi
```

There may be a gcc issue: https://github.com/rust-lang/rust/wiki/Using-Rust-on-Windows Since Rust looks for gcc in PATH before checking. We may need to create a wrapper fuunction, that overrides gcc to be Rust specific gcc. (No problems at the moment.)

Run these to see if they working:

```sh
rustc --version
rustdoc --version
cargo --version
```

We might have a problem with cygwin based git providing a unix path to Cargo. Cargo won't understand how to use the path.

Change `~/.gitconfig` init.templatedir to point to windows style path. These are the path changes:

```
/cygdrive/c/cygwin64/usr/share/git-core/templates

to

C:/cygwin64/usr/share/git-core/templates
```

Cygwin git still seems to operate fine though. However this hack should not be required. Asking on https://github.com/rust-lang/cargo/issues/1295