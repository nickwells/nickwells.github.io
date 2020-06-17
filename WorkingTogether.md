# Tools working together

The tools in the utilities repository are intended to work together to help
you build software and to work with the by-products of that software.

They also work with the packages given in the other repositories under
github.com/nickwells.

## standards - why they're worth having and some suggestions
The benefit of having a standard way of building your applications and
arranging your code is that it makes it easier for you to write tools that
operate on the code. You can have the default behaviour of your tools work
with the standard behaviour and layour of your code and thereby reduce the
number of parameters you need to give.

For instance several of the tools and libraries keep backup copies of files
and they all will give these backup copies an extension of `.orig`. This
means that the `findCmpRm` command can have as its default behaviour to look
for files with this extension. You can still give an option to it and have it
search for a different extension but the default behaviour will work with the
other tools to give the desired results.


### Suggested standards
- keep all the `go generate` comments in a single file called `generate.go`. This makes it easy identify all the directories that have auto-generated code.
- keep any package-wide documentary comments in a file called `doc.go`. This saves having to search all the other files for package documentation and makes it easy to check that you have such documentation.
- for packages containing commands (with package name `main`) have `func main()` in a file called `main.go`. This saves having to search the directory for it.

## tools
### mkdoc
The `mkdoc` tool assumes that a program is using the param package
(`github.com/nickwells/param.mod/v5/param`). It will build an instance of the
binary for the directory it is running in (which must contain a main package)
and then run the program passing parameters `-help-format markdown` and
`-help-show intro`, `-help-show refs` and `-help-show examples`. This
generates a markdown file with links to the examples and external references
(if any). This file can then be linked from the main README.md file in your
github repository.

The advantage of this approach is that you only have one source of
documentation for the program. You can avoid having duplicated information in
both a usage message generated by the program and in a markdown file.

### mkparamfilefunc
This will generate the boilerplate code needed to set the config file for a
program. By default it will use the directories given by the XDG standard
(see `github.com/nickwells/xdg.mod/xdg`). It can generate code to register
config files either for a whole program or for a group of parameters and
either personal or system-wide config files (or both). This assumes that the
code is using the param package.

### findCmpRm
This searches a directory for files with the extension `.orig` and for any it
finds it will promt the user to compare it with the corresponding file
without the extension. The user is then prompted to delete the `.orig` file
or revert the corresponding file to the original contents. This works with
both the `gosh` program which will generate `.orig` files when editing files
in place and also with the testhelper package
(`github.com/nickwells/testhelper.mod/testhelper`) which will generate files
with a `.orig` extension when replacing golden files (which contain expected
test code output) with a new version.

### findGoDirs
This searches directories for Go code and either print the directory name,
run `go install` or run `go generate` or any mix of the three. It is a useful
standard to have all generate instructions in a single file called
`generate.go`.