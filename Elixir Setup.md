Elixir Setup
============

Elixir requires Erlang.

Building Erlang
---------------

On Windows, building on Cygwin seems possible but tedious.

Get precompiled binaries: https://www.erlang-solutions.com/downloads/download-erlang-otp

On Unixy systems you can use Kerl to build Erlang versions: https://github.com/yrashk/kerl

Install it into "~/erlang". We cannot use "~/.erlang" because thats seems to cause an error in the Window's version of Erlang.

Add this to your `.zshrc`:

```
# erlang (windows installation)
if [ -d "${HOME}/erlang" ] ; then
    export PATH="$HOME/erlang/bin:$PATH"
fi
```

Try: `$ erl`.

Building Elixir
---------------

Building Elixir on Cygwin also seems possible but tedious.

Get the precompiled binaries: https://github.com/elixir-lang/elixir/releases/

On Unixy systems try using kiev https://github.com/taylor/kiex or exenv https://github.com/mururu/exenv.

Extract the archive into "~/.elixir".

```
# elixir (precompiled)
if [ -d "${HOME}/.elixir" ] ; then
    export PATH="$HOME/.elixir/bin:$PATH"
    # we have to use the windows versions
    alias elixir='elixir.bat'
    alias elixirc='elixirc.bat'
    alias iex='iex.bat'
    alias mix='mix.bat'
fi
```

Please note that these aliases will not be expanded in `exs` script hashbangs. You can either force the shell to expand the aliases, or you can force symlink the Elixir binaries to the `.bat` equivalents. The Elixir binaries don't work inside Cygwin.

For now to use iex: `iex --werl`.