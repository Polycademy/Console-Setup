Java Setup
----------

Java is split up in JRE and JDK. The JRE is all you need to run java applications, whereas JDK is required to create java applications.

JRE is probably already installed into your system (your browser usually asks to do so). You can leave that there.

We need to install JDK. This JDK will use its own JRE. So you will have 2 JREs in your system, however the official JRE will be your already installed JRE. The JDK's JRE will only be used by the JDK or during software development.

Go here: https://github.com/alexkasko/openjdk-unofficial-builds

Download the zip file. And unzip it into `~/.jdk`.

Add these 2 to your `.zshrc`.

```sh
# java (windows installation)
if [ -d "${HOME}/.jdk" ] ; then
    export JAVA_HOME="$HOME/.jdk"
    export PATH="$HOME/.jdk/bin:$PATH"
fi
```
