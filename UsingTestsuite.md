# Running the testsuite #

GF has testsuite. It is run with the following command:
```
$ runghc Setup.hs test
```
The testsuite architecture for GF is very simple but also very flexible.
GF by itself is an interpreter and could execute commands in batch mode.
This is everything that we need to organize a testsuite. The root of the
testsuite is the testsuite/ directory. It contains subdirectories which
themself contain GF batch files (with extension .gfs). The above command
searches the subdirectories of the testsuite/ directory for files with extension
.gfs and when it finds one it is executed with the GF interpreter.
The output of the script is stored in file with extension .out and is compared
with the content of the corresponding file with extension .gold, if there is one.
If the contents are identical the command reports that the test was passed successfully.
Otherwise the test had failed.

Every time when you make some changes to GF that have to be tested, instead of
writing the commands by hand in the GF shell, add them to one .gfs file in the testsuite
and run the test. In this way you can use the same test later and we will be sure
that we will not incidentaly break your code later.

If you don't want to run the whole testsuite you can write the path to the subdirectory
in which you are interested. For example:
```
$ runghc Setup.hs test testsuite/compiler
```
will run only the testsuite for the compiler.