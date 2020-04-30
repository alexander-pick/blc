## WARNING: THIS CODE IS VERY RAW AND PROBABLY VERY BUGGY!

## Introduction

This is the blc (Binary Lifting Contraption) plugin for IDA Pro. It is the Bastard
love child of Ghidra's decompiler with Ida Pro.

The plugin integrates Ghidra's decompiler code into an Ida plugin an provides a 
basic decompiler capability for all platforms support by both Ida and Ghidra. It
provides a basic source code display that attempts to mimic that of the Hex-Rays
decompiler. It has only been written with Ida 7.x in mind.

## BUILDING:

On all platforms you should clone blc into your IDA SDK's plugins sub-directory
so that you end up with `<sdkdir>/plugins/blc`. This is because the build files
all use relative paths to find necessary IDA header files and link libraries.

### Build blc for Linux / OS X:

Use the include Makefile to build the plugin. You may need to adjust the paths
that get searched to find your IDA installation (`/Applications/IDA Pro N.NN` is
assumed on OSX and `/opt/ida-N.NN` is assumed on Linux, were N.NN is derived from
the name of your IDA SDK directory eg `idasdk73` associates with `7.3` and should
match your IDA version number). This is required to successfully link the plugin.

```
$ cd <sdkdir>/plugins/blc
$ make
```

Compiled binaries will end up in `<sdkdir>/plugins/blc/bin`

```
LINUX
         -------------------------------------------
         |        ida        |        ida64        |
         -------------------------------------------
IDA 7.x  |                   |                     |
 plugin  |     blc.so        |      blc64.so       |
         -------------------------------------------

OS/X
         -------------------------------------------
         |        ida        |        ida64        |
         -------------------------------------------
IDA 7.x  |                   |  |                  |
 plugin  |     blc.dylib     |      blc64.dylib    |
         -------------------------------------------
```

Copy the plugin(s) into your `<IDADIR>/plugins` directory and blc should be
listed as an available plugin for all architectures supported both Ida
and Ghidra.

### Build blc for Windows

Build with Visual Studio C++ 2019 or later using the included solution (`.sln`)
file (`blc.sln`). Two build targets are available depending on which version
of IDA you are using:

```
         -----------------------------------------
         |        ida        |        ida64      |
         -----------------------------------------
IDA 7.x  |    Release/x64    |   Release64/x64   |
 plugin  |       blc.dll     |       blc64.dll   |
         -----------------------------------------
```

Copy the plugin(s) into your `<IDADIR>/plugins` directory and blc should be
listed as an available plugin for all architectures supported by both Ida
and Ghidra.

## INSTALLATION

Assuming you have installed IDA to `<idadir>`, install the plugin by copying the
compiled binaries from `<sdkdir>/plugins/blc/bin` to `<idadir>/plugins` (Linux/Windows)
or `<idadir>/idabin/plugins` (OS X).

The plugin is dependent on Ghira processor specifications which you will need to
copy over from your own Ghidra installation. Installing Ghidra is a simple matter
of unzipping the latest Ghidra release, for example: 

https://ghidra-sre.org/ghidra_9.1.2_PUBLIC_20200212.zip

Within the extracted Ghidra folder, you will find a `Ghidra` subdirectory which,
in turn, contains a `Processors` subdirectory. The decompiler needs access to
files contained under `Ghidra/Processors`. By default the plugin looks for the 
environment variable `$GHIDRA_DIR` which it expects to point at your Ghidra
installation folder such that `$GHIDRA_DIR/Ghidra/Processors` exists. If
`$GHIDRA_DIR` is not set, then the plugin expects to find `<idadir>/plugins/Ghidra/Processors`
which you may create with a symlink or by copying the approprate directories
from your Ghidra installation.

### Pre-built binaries:

As an alternative to building the plugin yourself, pre-built binaries for 
IDA 7.x (Windows, Linux, OS X) are available in the `blc/bins` directory.

## USING THE PLUGIN

With the plugin installed, open a binary of interest in IDA. In order for the
plugin to be become available, the binary's architecture must be supported by
both Ida and Ghidra.

With the cursor placed inside the body of an IDA function, select
`Edit/Plugins/Ghidra Decompiler`. A successful decompilation (which may take a bit
of time, will open a new window containing the C source generated by Ghidra's
decompiler. Within the source window, you may double click on a function name to
decompile that function. Double clicking on a global data name will navigate you 
to that symbol in the IDA disassembly view. The `ESC` key will navigate back to a 
previous function, or close the source viewer if there is no previous function.

The `N` hot key may be used to rename any symbol in the source view. When a symbol
in the source view corresponds to a symbol in the Ida disassembly, the symbol will
also be renamed in the disassembly.

You can get xrefs of a functions by marking it and pressing `X`. To write a comment, 
mark the line and press `/`. If you have the hex-rays decompiler installed, some keys 
might conflict and need to be changed for hex-rays. A feature to freely select the 
hotkey values is planned. 

If you want to refresh the source view press `R`. 

## POTENTIAL FUTURE WORK

* Allow user to set data types for symbols in the source view
* Provide IDA derived type information to the decompiler so that it can 
  do a better job with things like structures and pointer dereferencing
* ~~Better (at least some) support for string literals~~
* Investigate what settings/info are necessary to get this standalone decompiler
  to yield results identical to Ghidra's. Is this symbol information? Type information?
  arch/platform/compiler settings?