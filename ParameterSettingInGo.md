# Parameter Setting In Go

This is a discussion of how program parameters (or flags) can be set in
Go. There are a variety of packages available to the programmer for doing
this. Obviously we have the flag package that comes with the Go standard
library but [awesome-go](https://github.com/avelino/awesome-go) lists many
others. I'll be talking about what you want from a configuration package,
describing the requirements from the perspective of the consumers of the
package.

Having set the scene I'll write a simple program using several different
packages and compare them.

## Requirements of a configuration package

Let's think about what we want from a configuration package.

There are two types of consumer of a configuration package:

* The user
* The programmer

The user is the person who will be running the program. They will provide the
parameters and they will want to get help on how to use the program and on
the available parameters.

The programmer will be using the package to create the set of allowed
parameters and will use the values that get set from the parameters. They
want to have useful error messages if the package has been misused and they
want to be able to exercise fine control over the parameters that can be
supplied. Additionally they will want the API to be simple to use and hard to
misuse.

### User requirements

Let's look at the user's requirements in more detail. Here's a list of
requirements that a user might have:

* Helpful errors

the user wants to have any errors explained and useful suggestions made about
what they should do to fix them.

* access to help

one of the first things a user will need is help on how to use the program:
"show me what you can do". This feature should be provided in the same way to
all programs so the user can be confident of how to get help.

* Summary help

once you know a program you might only want a brief reminder of what the
parameters are.

* Selective help

the user might only want help on a specific parameter or group of parameters.

* Examples

some examples of how the program can be used can be a very useful starting
point when exploring how to use a new tool.

* References

references to other related commands can help the user to find other programs
in a suite of applications.

* Confidence that they can use the program safely

any errors should be caught early and reported before the program goes on to
do anything dangerous with them. This confidence will encourage the user to
experiment and thereby to understand and know the program more quickly.

* Default values

when there are parameters that the user always wants to be set to a
particular value it can be useful to have them set either in configuration
files or else as environment variables. This saves having to type the same
values time after time and reduces the chance of making a mistake.

* Seeing where parameters are set

if you are allowed to set parameters in multiple places then it can be
difficult to know where a parameter has been set and easy to make a false
assumption about the current value of a parameter. The user thinks "I haven't
set this parameter so it will have its default value" but it has been set
elsewhere and the user has forgotten, or never knew. It is very useful if you
can see where they have been set without having to check each of the sources
by hand. This problem typically only arises where there are alternative
sources of parameters such as environment variables or configuration
files. It is especially useful when one of the configuration files is shared
amongst many users or if the environment variables are set in some shared
configuration.

* Shell parameter completion mechanisms

it's very convenient to be able to use the shell completion features and this
can also provide an immediate help feature showing what is available as you
type the command.
