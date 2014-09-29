Elm Setup
=========

Elm requires Haskell.

Installing Haskell
------------------

Download and install the Haskell Platform http://hackage.haskell.org/platform/

On installation, check the Portable style, and only associate with `.hl` or `.hls` files.

Install the Haskell Platform to: `~/Haskell Platform`. Ignore the version date.

Add this to your `.zshrc`:

```zsh
# haskell platform
if [ -d "${HOME}/Haskell Platform" ] ; then
    export PATH="$HOME/Haskell Platform/bin:$PATH"
    export PATH="$HOME/Haskell Platform/lib/extralibs/bin:$PATH"
    export PATH="$HOME/Haskell Platform/winghci:$PATH"
    export PATH="$APPDATA/cabal/bin:$PATH"
    # cabal requires mingw gcc in order to compile certain packages
    run_cabal () {
        oldpath=$PATH
        export PATH="$HOME/Haskell Platform/mingw/bin:$PATH"
        cabal $@
        export PATH=$oldpath
    }
    alias cabal=run_cabal
fi
```

Note that cabal packages are stored inside `$APPDATA/cabal`.

Installing Elm
--------------

Run these commands:

```
cabal update
cabal install elm elm-server elm-repl elm-get
```

You should have those executables in the PATH now.

Try these commands and navigate your browser to 127.0.0.1:8000 to see it work:

```
mkdir helloElm
cd helloElm
printf "import Mouse\n\nmain = lift asText Mouse.position" > Main.elm
elm-server
```

Allowing ghci to work properly
------------------------------

Edit the `~/Haskell Platform/bin/ghcii.sh`, change the contents to this:

```
#!/bin/sh
exec "~/Haskell Platform/bin/ghc" --interactive ${1+"$@"}
```

Now instead of running `ghci`, use `ghcii.sh`. This is the command you'll use for entering into interactive Haskell.