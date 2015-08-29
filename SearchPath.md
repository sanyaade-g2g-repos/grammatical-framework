# How to Setup the Search Path for Grammars in GF #

---


The GF interpreter is using a search path to find the file that defines a given module. For example if you load file LangEng.gf, the interpreter sees that the file contains the following definition:

```
concrete LangEng of Lang = 
  GrammarEng,
  LexiconEng
  ** {
 
....
 
} ;
```

Now the interpreter needs to know where to find modules Lang, GrammarEng and LexiconEng. Since every file could contain only one module and it has to have the same name as the name of the module, the only question is how know in which directory to look for the file. For that purpose the user specifies a list of directories in which the interpreter should look for file with appropriate name. This list of directories is the search path and the following sections describe how to set it up.

First we have to define some other concepts:

# 1. PREDEFINED DIRECTORIES #

Here we define three absolute directory locations and how they are determined by the interpreter.

## 1.1. Working Directory ##

This is the current directory for the operating system.

## 1.2. Target Directory ##

This is the directory where the top-level module that you want to load is located i.e. if you do:
```
> i lib/src/english/LangEng.gf
```
then lib/src/english is the target directory. When the target directory is a relative path then the interpreter will try to resolve the absolute location as sub directory of the current directory. If such directory doesn't exist the it will try to find it as subdirectory of the library directory.

## 1.3. Тhe Library Directory ##

This is the directory where the GF Resource Library is located. It could be set in three different ways:
by passing option -gf-lib-path=PATH to GF
by setting environment variable GF\_LIB\_PATH
by using the hard-coded location. This location is set by Cabal during the configure phase. It has these default values: '/usr/share/gf-3.0/lib' on Linux and 'c:\Program Files\Haskell\gf-3.0\lib' on Windows
The interpreter attempts each of the ways above in sequence untill it succeeds. The last option is just hard-coded constant so it is always successful.


# 2. SETTING THE SEARCH PATH #

The first element of the search path is always the target directory, so a module could refer to other modules in the same directory without specifying any paths. Additional elements could be added by the option -path=DIR1:DIR2:DIR3... . This option could be given both to the import command and to gf as command line argument i.e.
```
$ gf -path=english:abstract:common FoodsEng.gf
```
loads the FoodsEng grammar and sets the search path to 'english', 'abstract' and 'common'. The same could be achieved with the import command:
```
$ gf
....
> i -path=english:abstract:common FoodsEng.gf
```
When all the elements in the path are absolute directory paths then everything is clear. The important question is how to threat relative paths like 'english' and 'abstract'. This are not absolute locations in the file system but are relative to some other location. In this case the interpreter tries to find the absolute location by first looking at locations relative to the current working directory and if no match is found then it tries locations relative to the library directory. For example if there is a subdirectory called english in the working directory then the interpreter will resolve the path 'english' to '$(workdir)/english'. If the directory doesn't exist then the interpreter will check whether there is a directory '$(libdir)/english', if this also fails then element 'english' from the search path will be just ignored.

This is the whole story if we always give the -path option when loading a file, but in many cases this is not convinient. We want just to load a module in the interpreter and it should be able to automatically resolve the dependencies and find the right directories for them. For this purpose the developer is able to specify some search path as pragma in the beginning of every module. For example this is how it is usually done in the resource grammars:
```
--# -path=.:../abstract:../common:../prelude
 
concrete LangEng of Lang = ...
```
When the interpreter loads a file and there is no -path option given at the command line then the interpreter looks at the beginning of the file for the pragma --# -path=... . If there is something specified then the interpreter will use it. The only difference is that if there are local paths then they will be resoved agains the target directory first instead of against the working directory. In both cases as second attempt the library directory will be tried. The rationale for this is that when the developer writes the module he/she should think only about the module and his dependencies and not about in which directory the user should be working while loading the module.

If no search path is given at all it is empty by default. If a search path is given both in the module and on the command line then only the search path on the command line is considered. This could be overriden by using the option -iDIR instead of -path=PATH. In this case the new location DIR is added to the search path instead of overriding the current one. For example if the module is LangEng and the command line is:
```
> i -iextra/common LangEng.gf
```
then the search path will be extra/common:.:../abstract:../common:../prelude

# 3. PATH WITH WILDCARDS #

The elements of the search path could end with wild card i.e. 'lib/**' in this case this is a short cut for all subdirectories of the directory lib. The directory lib itself will not be included in the path.**

# 4. EXTRA SEARCH LOCATIONS #

If some module could not be found in any of the locations above then the interpreter will try two other locations. First it will look in the library directory itself and second it will try the extra search path which is specified by environment variable GF\_GRAMMAR\_PATH. The path in the variable is a list of directories separated by colons ':' on Linux and semicolons ';' on Windows. If the variable is not set it has the default value '$(libdir)/prelude'.