PlantUML
=======

Prerequisites:

1. You need Java on the commandline. See `Java Setup.md`.
2. You need Graphviz installed (see version requirements at PlantUML website).

```
apt-cyg-main install graphviz
```

Installation:

```
curl -L -o "~/bin/plantuml.jar" http://sourceforge.net/projects/plantuml/files/plantuml.jar/download
chmod 666 ~/bin/plantuml.jar
```

Add this to `.zshrc`:

```
# plantuml & graphviz dot
if [ -f "${HOME}/bin/plantuml.jar" ] ; then
    export GRAPHVIZ_DOT="$(cygpath -m $(which dot)).exe"
    plantuml_path="$(cygpath -m "${HOME}/bin/plantuml.jar")"
    plantuml () {
        java -jar $plantuml_path $@
    }
fi
```

Testing:

```
plantuml -version
plantuml -help
plantuml -checkversion
```

If you're generating large diagrams and you get memory errors. You need to tune Java's heapsize. The `plantuml` bash function would have to incorporate something like this:

```
java -Xmx1024m -jar ...
```

PlantUML is a programming language, and you find advanced features like conditionals, macros, includes here: http://plantuml.com/preprocessing.html

Running:

Create a `version.pu` file like:

```pu
@startuml
version
@enduml
```

Generate an image:

```
plantuml version.pu
```

You should get a `version.png` produced.

To get syntax highlighting in Sublime Text Editor:

Hit Browse Packages.

Create folder "PlantUML".

Create file "PlantUML.tmLanguage".

Put this into the file:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>fileTypes</key>
    <array>
        <string>pu</string>
        <string>plantuml</string>
    </array>
    <key>name</key>
    <string>PlantUML</string>
    <key>patterns</key>
    <array>
        <dict>
            <key>begin</key>
            <string>^(#|')</string>
            <key>end</key>
            <string>\n</string>
            <key>name</key>
            <string>comment.line.source.wsd</string>
        </dict>
        <dict>
            <key>match</key>
            <string>\b(abstract|actor|class|component|enum|interface|object|participant|state|usecase)\b</string>
            <key>name</key>
            <string>support.function.source.wsd</string>
        </dict>
        <dict>
            <key>begin</key>
            <string>\btitle\b</string>
            <key>beginCaptures</key>
            <dict>
                <key>0</key>
                <dict>
                    <key>name</key>
                    <string>keyword.control.source.wsd</string>
                </dict>
            </dict>
            <key>end</key>
            <string>\n</string>
            <key>name</key>
            <string>string.quoted.double.source.wsd</string>
        </dict>
        <dict>
            <key>match</key>
            <string>\b(@enduml|@startuml|activate|again|also|alt|as|autonumber|bottom|box|break|center|create|critical|deactivate|destroy|down|else|end|endif|endwhile|footbox|footer|fork|group|header|hide|if|is|left|link|loop|namespace|newpage|note|of|on|opt|over|package|page|par|partition|ref|repeat|return|right|rotate|show|skin|skinparam|start|stop|title|top|top to bottom direction|up|while)\b</string>
            <key>name</key>
            <string>keyword.control.source.wsd</string>
        </dict>
        <dict>
            <key>match</key>
            <string>\b(AliceBlue|AntiqueWhite|Aqua|Aquamarine|Azure|Beige|Bisque|Black|BlanchedAlmond|Blue|BlueViolet|Brown|BurlyWood|CadetBlue|Chartreuse|Chocolate|Coral|CornflowerBlue|Cornsilk|Crimson|Cyan|DarkBlue|DarkCyan|DarkGoldenRod|DarkGray|DarkGreen|DarkGrey|DarkKhaki|DarkMagenta|DarkOliveGreen|DarkOrchid|DarkRed|DarkSalmon|DarkSeaGreen|DarkSlateBlue|DarkSlateGray|DarkSlateGrey|DarkTurquoise|DarkViolet|Darkorange|DeepPink|DeepSkyBlue|DimGray|DimGrey|DodgerBlue|FireBrick|FloralWhite|ForestGreen|Fuchsia|Gainsboro|GhostWhite|Gold|GoldenRod|Gray|Green|GreenYellow|Grey|HoneyDew|HotPink|IndianRed|Indigo|Ivory|Khaki|Lavender|LavenderBlush|LawnGreen|LemonChiffon|LightBlue|LightCoral|LightCyan|LightGoldenRodYellow|LightGray|LightGreen|LightGrey|LightPink|LightSalmon|LightSeaGreen|LightSkyBlue|LightSlateGray|LightSlateGrey|LightSteelBlue|LightYellow|Lime|LimeGreen|Linen|Magenta|Maroon|MediumAquaMarine|MediumBlue|MediumOrchid|MediumPurple|MediumSeaGreen|MediumSlateBlue|MediumSpringGreen|MediumTurquoise|MediumVioletRed|MidnightBlue|MintCream|MistyRose|Moccasin|NavajoWhite|Navy|OldLace|Olive|OliveDrab|Orange|OrangeRed|Orchid|PaleGoldenRod|PaleGreen|PaleTurquoise|PaleVioletRed|PapayaWhip|PeachPuff|Peru|Pink|Plum|PowderBlue|Purple|Red|RosyBrown|RoyalBlue|SaddleBrown|Salmon|SandyBrown|SeaGreen|SeaShell|Sienna|Silver|SkyBlue|SlateBlue|SlateGray|SlateGrey|Snow|SpringGreen|SteelBlue|Tan|Teal|Thistle|Tomato|Turquoise|Violet|Wheat|White|WhiteSmoke|Yellow|YellowGreen)\b</string>
            <key>name</key>
            <string>variable.source.wsd</string>
        </dict>
        <dict>
            <key>begin</key>
            <string>([A-Za-z_0-9]+) +((-?-&gt;)|(&lt;-?-)) +([A-Za-z_0-9]+)(:)</string>
            <key>beginCaptures</key>
            <dict>
                <key>1</key>
                <dict>
                    <key>name</key>
                    <string>constant.string.source.wsd</string>
                </dict>
                <key>2</key>
                <dict>
                    <key>name</key>
                    <string>keyword.control.source.wsd</string>
                </dict>
                <key>5</key>
                <dict>
                    <key>name</key>
                    <string>constant.string.source.wsd</string>
                </dict>
            </dict>
            <key>end</key>
            <string>\n</string>
            <key>name</key>
            <string>string.quoted.double.source.wsd</string>
        </dict>
        <dict>
            <key>begin</key>
            <string>"</string>
            <key>end</key>
            <string>"</string>
            <key>name</key>
            <string>string.quoted.double.source.wsd</string>
        </dict>
        <dict>
            <key>begin</key>
            <string>'</string>
            <key>end</key>
            <string>'</string>
            <key>name</key>
            <string>string.quoted.single.source.wsd</string>
        </dict>
        <dict>
            <key>match</key>
            <string>\b[A-Z]+[A-Za-z_0-9]*\b</string>
            <key>name</key>
            <string>constant.string.source.wsd</string>
        </dict>
        <dict>
            <key>match</key>
            <string>\b[a-z_]+[A-Za-z_0-9]*\b</string>
            <key>name</key>
            <string>variable.parameter.source.wsd</string>
        </dict>
    </array>
    <key>scopeName</key>
    <string>source.wsd</string>
    <key>uuid</key>
    <string>ca03e751-04ef-4330-9a6b-2199aae1c418</string>
</dict>
</plist>
```