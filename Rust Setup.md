Rust Setup
==========

These instructions are relevant for Rust 0.12.

We need to install Rust windows binary from the official Rust site.

Cargo is not installed with Rust on Windows, so you then go to Cargo official site and download the Windows binaries.

All binaries should be 64 bit.

Install Rust into `~/Rust`. Do not choose to allow Rust to associate the bin into your PATH. You will do this through ZSH.

Unzip cargo into `~/.cargo`.

Add these to your `.zshrc`.

```sh
# rust (windows installation)
if [ -d "${HOME}/Rust" ] ; then
    export PATH="$HOME/Rust/bin:$PATH"
fi

# cargo (windows installation)
if [ -d "${HOME}/.cargo" ] ; then
    export PATH="$HOME/.cargo/bin:$PATH"
fi
```

There may be a gcc issue: https://github.com/rust-lang/rust/wiki/Using-Rust-on-Windows Since Rust looks for gcc in PATH before checking. We may need to create a wrapper fuunction, that overrides gcc to be Rust specific gcc. (No problems at the moment.)

Run these to see if they working:

```sh
rustc --version
rustdoc --version
cargo --version
```