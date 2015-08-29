In GF we use [Cabal](http://www.haskell.org/cabal/) as infrastructure for building the source code. Cabal is a Haskell library which is distributed together with GHC and we use it in the Setup.hs script. Setup.hs is the main program that we use in the compilation.

## Configure ##

During the configuration phase Cabal will check that you have all necessary tools and libraries
needed for GF. The configuration is started by the command:
```
$ runghc Setup.hs configure
```
The command `runghc` comes with the GHC compiler and is batch interpreter which executes the specified script without the need to compile it advance. Setup.hs is our compilation driver which is based on Cabal. If you don't see any error message from the above command then you have everything that is needed for GF. You can also add the option `-v` to see more details about the configuration.

## Build ##

The build phase does two things. First it builds the GF compiler from the Haskell sources
and after that it builds the GF Resource Grammar Library using the newly built compiler.
The simplest command is:
```
$ runghc Setup.hs build
```
Again you can add the option `````-v````` if you want to see more details.

Sometimes you just want to work on the GF compiler and don't want to recompile the resource
library after each change. In this case use this extended command:
```
$ runghc Setup.hs build rgl-none
```
The resource library could also be compiled in two modes: with present tense only and
with all tenses. By default it is compiled with all tenses. If you want to use
the library with only present tense you can compile it in this special mode with
the command:
```
$ runghc Setup.hs build present
```
Before to use this command make sure that the script lib/src/mkPresent has executable
permissions on Linux.

In similar way, this command:
```
$ runghc Setup.hs build minimal
```
will build only minimal version of the library i.e. version that includes only the basic grammatical constructions.

You could also control which languages you want to be recompiled by adding the option
`````langs=list`````. For example the following command will compile only the English and the Swedish
language:
```
$ runghc Setup.hs build langs=Eng,Swe
```

There is also an option which compiles all resource libraries to a single PGF file:
```
$ runghc Setup.hs build rgl-pgf
```
Before to run this command you first have to have the libraries compiled by the simple build command i.e. run build without any options first and after that run the command above again.

## Install ##

After you have compiled GF you can install the binaries to make the system usable.
On Linux you will need root privileges to do this. Use the command:
```
$ su
```
and enter the root password. This step should be skipped on Windows.

The installation itself is started with the command:
```
$ runghc Setup.hs install
```
This command installs the GF compiler in the default place for executable
files in your system. For example on Linux this is usualy /usr/local/bin and on
Windows this is c:\Program Files\Haskell\bin. If you want to install in some
other place then use the `````--prefix````` option during the configuration phase.

The compiled GF Resource Grammar Library will be installed in /usr/local/share/gf-3.0/lib
on Linux and in c:\Program Files\Haskell\gf-3.0\lib on Windows. Again the location could
be changed using the `````--prefix````` option.

## Clean ##

Sometimes you want to clean up the compilation and start again from clean
sources. Use the clean command for this purpose:
```
$ runghc Setup.hs clean
```

## SDist ##

You can use the command:
```
$ runghc Setup.hs sdist
```
to prepare archive with all source codes needed to compile GF.