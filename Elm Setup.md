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

There's an extra directory also at `$APPDATA/ghc`. This stores the `ghci` history and you can configure GCHI here using a file called `ghci.conf`.

Here's an example `ghci.conf` that I use:

```
:set prompt "%l-λ:> "
:set prompt2 "%l-λ:> | "
:set editor "C:/Program Files/Sublime Text 3/subl.exe"
:set +t
:set +s
:set +m
```

It no longer shows the currently imported modules on the prompt. To view currently imported modules use `:show imports` and `:show language` or just `:set` to view currently used lanaguage extensions.

If you want to fully uninstall Haskell Platform in order to get a fresh update:

```
rm -rf $HOME/Haskell\ Platform/
rm -rf $APPDATA/cabal
rm -rf $APPDATA/ghc
```

Here are the descriptions for the directories in `$APPDATA/cabal`:

```
/packages -- This is the downloaded package cache.
/logs -- This contains the installation logs for the downloaded package.
/doc -- This contains the documentation fo the downloaded packages.
/bin -- This contains generated binaries from executables.
/{architecture} -- This is the actual installed & compiled libraries for your specific GHC & host architecture. Packages that are inside here, can be imported and used by your installed GHC compiler. Different GHC compilers could have different compiled packages.
```

Because you also have pre-installed packages from Haskell Platform, newer packages installed inside `$APPDATA/cabal` will override the Haskell Platform packages. You don't modify the Haskell Platform packages. These packages are not Node like. Every package is shared. There is potential for dependency hell.

Deleting packages from `cabal` is difficult. There is no native command for this. Start by unregistering the package (only if it is installed as a compiled library into the specific GHC architecture): `ghc-pkg unregister {pkg-id}`. Then find each instance of it in the folders mentioned above, and delete them. The package itself may have generated files or folders, and you will need to find them to delete them too.

Installing Elm
--------------

Run these commands:

```
cabal update
cabal install -j elm-compiler-0.15 elm-package-0.5 elm-make-0.1.2
cabal install -j elm-repl-0.4.1 elm-reactor-0.3.1
```

You should have those executables in the PATH now.

Try these commands and navigate your browser to 127.0.0.1:8000 to see it work:

```
mkdir helloElm
cd helloElm
printf "import Graphics.Element exposing (..)\nimport Mouse\n\nmain : Signal Element\nmain = Signal.map show Mouse.position" > Main.elm
elm-reactor
```

A new directory is placed in `$APPDATA/elm`. This stores the files related to the REPL and Elm packages.

Allowing ghci to work properly
------------------------------

Edit the `~/Haskell Platform/bin/ghcii.sh`, change the contents to this:

```
#!/bin/sh
exec "~/Haskell Platform/bin/ghc" --interactive ${1+"$@"}
```

Now instead of running `ghci`, use `ghcii.sh`. This is the command you'll use for entering into interactive Haskell.

GHCI Performance for Large Projects
-----------------------------------

Too many modules will slow down compilation. Use these tricks:

* https://downloads.haskell.org/~ghc/7.8.1/docs/html/users_guide/ghci-compiled.html
* https://downloads.haskell.org/~ghc/7.10.1/docs/html/users_guide/ghci-obj.html
* https://wiki.haskell.org/Making_GHCi_scale_better_and_faster