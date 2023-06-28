This assignment is mostly reading (and little coding). 

# Understand C's Design Principles

C is an old language. When it was designed, computers were very
expensive. Thus, some design decisions were made to make C programs
faster, possibly making C programs more difficult to write.  C
programs will not do anything for you unless you explicitly tell the C
programs to do something.

Learning Tools
==============

This assignment will help you learn some most widely used tools:
`git`, `gcc`, `gcov`, and `make`.

You will learn to
* Detect likely mistakes using gcc warnings
* Write a program that has multiple files
* Understand how programming assignments are graded
* Use conditional compilation to turn on and off sections of code
* Use gcov to find untested code

`gcc` warnings
==============

When you write a program, you create text files. These text files are
not computer programs.  To convert the text files from human-readable
to machine-readable format, you need to use a *compiler*. This book
uses `gcc` for the compiler.

`gcc` takes one or several `.c` files as input.

You can  specify the output file's name by adding

`-o name`

If you do not specify the name, the default name is called `a.out`

Please be careful **not** to use the file you write as the output's name, 
for example

`> gcc myprogram.c -o myprogram.c`

This will **erase** `myprogram.c` (i.e., your program).  You definitely do not want to do this.

Do not use `test` as the name of the output because `test` is a
command in Linux.  When you run `test`, is it your program or the
Linux command? The answer depends on your settings.  If you do not
understand what *path* means, that's all right for now.  Just do not
use `test` as the output of `gcc`.

If you read the manual (also called the "man page") of `gcc`, you can
find many options.  In this assignment, you should use `gcc` in the
following way.

`> gcc -std=c99 -g -Wall -Wshadow -pedantic -Wvla -Werror`

* `>` is the command prompt
* `-std=c99` means using the C standard announced in 1999
* `-g` means enabling debugging
* `-Wall` means enable warning messages
* `-Wshadow` means detecting shadow variables
* `-pedantic` means restricting standard C
* `-Wvla` means detecting variable length array
* `-Werror` means treating warnings as errors 

You should create an *alias* in Linux (or use `make` as explained
below) so that whenever you use `gcc`, all these options are
automatically added.  If you do not know how to create an alias in
Linux, look it up on the Internet.  If you do not create an alias, it
is very likely that you forget some parts of the long command.  It is
very important to use `gcc` this way to detect mistakes. In almost all
cases, the warnings by `gcc` indicate serious problems.  This is your
first "line of defense" writing good programs.  There were many cases
when people forgot to use warnings, failed to detect these
common problems early, and spent many hours later debugging.

Here are some common warning messages:

* create a variable but never use it (most likely it is mistyped)
* create a function but never use it (most likely it is mistyped)
* read a variable before a value is assigned. C does not initialize
   variables. If a variable is not assigned, its value is garbage, not
   necessarily zero.  Please be aware that `gcc` does not *always* detect
   uninitialized variables. You cannot rely on `gcc` completely.
* write code that can never be reached (for example, code after `return`)
* assign a value to a wrong type
* create two variables that have overlapping scopes (called *shadow variables*)

Conditional Compilation
=======================

In some cases, you want to turn on or off sections of code. For
example, you may want to print messages showing the progress and
status of your programs, such as

`printf("The value of x is %d\n", x);`

After you finish the program, you want to remove these messages.
There is an easy solution: enclose the message by
conditions using `ifdef` and `endif`:

```
#ifdef DEBUG
printf("The value of x is %d\n", x);
#endif
```

If you add `-DDEBUG` after `gcc`, this line is included in the program.
If you do not add `-DDEBUG` after `gcc`, this line is excluded in the
program. What does `-DDEBUG` mean? `-D` means defining the `DEBUG`
symbol.  If `DEBUG` is defined, the `#ifdef` condition is true and that
line is included.

You can define multiple symbols. For example, if you want to test two
different solutions of the same function, you can do the following:

```
#ifdef SOLUTION1
void func(....) // solution 1
{
	....
}
#endif

#ifdef SOLUTION2
void func(....) // solution 2
{
	....
}
#endif
```

If you have `-DSOLUTION1` after `gcc`, the first solution is included.
If you have `-DSOLUTION2` after `gcc`, the second solution is included.

If you have neither, neither solution is included.  You will get an error message
because `gcc` does not know how to handle the call of `func`.

If you have both `-DSOLUTION1` and `-DSOLUTION2`, you will get another
error message because `gcc` has two options and does not know which
one to use.

You can also use `ifndef` (notice `n` between `if` and `def`).


```
#ifndef DEBUG
do something
#endif
```

This program will "do something" if `DEBUG` is *not* defined in `gcc`.

Multiple Files
==============

Every non-trivial system is composed of multiple components.

C uses two types of files: header (`.h`) and source (`.c`). Header
files contain declarations of functions and types. Source files
implement functions. Header files are "included".

If a header file is available from the C language, use

`#include <header.h>`


If `header.h` is created by you, use

`#include "header.h"`

Please notice the difference using `""` and `<>`.  Source files are
"compiled" and "linked".  To link source files, simply put the list of
source (.c) files after `gcc`.

You should notput a .h file after `gcc`.  You should not include a .c file.

Do not write anything like

#include <myprogram.c>

nor

#include "myprogram.c"

As your programs become even larger, compiling every source file after
only a few changes becomes problematic.  Imagine that you need to
spend an hour compiling a large program (such as an operating
system) if you have changed only one line.  To solve this problem, C
adopts a two-stage process:

1. Source files (.c extension) can be converted (called *compilation*) into an intermediate format called
*object files* (.o extension).

2.  Object files are *linked* to create the program (also called *executable*).

By convention, an executable file in Linux has no extension (no `.c`,
no `.h`, no `.exe`).  If only one line in one source file is changed,
only this source file needs to be compiled.  Then, the object files
are linked.  Compilation has done a lot of work already and linking
object files can be much faster (maybe only a few seconds).

`make`
======

By now, you may feel that there is a lot of work to do before you
write any code.  If you feel that you need to type a lot of things
after `gcc`, you are correct.  Fortunately, many tools have been
created to greatly simplify things.

All you need to do is five keystrokes: `make [Enter]`.

`make` is a Linux command.  You need to write a file telling `make` what
to do. This file, by convention, is called `Makefile`.

`Makefile` can also specify the sets of tests to run.

`Makefile` may contain multiple sections.  Each section has three parts:

```
target: dependence
[TAB] action
```

For example

```
myprogram: myprogram.c myprogram.h
	 gcc myprogram.c -o myprogram
```

This means the program `myprogram` depends on `myprogram.c` and
`myprogram.h`. If either `myprogram.c` or `myprogram.h` is changed, `gcc`
will be called to compile `myprogram.c` and generate `myprogram`. Note
that it is important that a TAB character is before the action (not
spaces).

You can define symbols in `Makefile`. For example,

`GCC = gcc -std=c99 -g -Wall -Wshadow --pedantic -Wvla -Werror`

defines the flags mentioned earlier.

Please carefully study the `Makefile` included in this
assignment. The first few lines of the
`Makefile` define symbols that can be used elsewhere to avoid
typing the same things over and over again. We use `GCC` in the rest of
`Makefile` to make sure that whenever we invoke `gcc`, we use the
command line flags specified above.

You can use symbols for symbols, for examples:

```
DEBUGFLAGS = -DTEST_FUNC1 -DTEST_FUNC2 
CFLAGS = -std=c99 -g -Wall -Wshadow --pedantic -Wvla -Werror
GCC = gcc $(CFLAGS) $(DEBUGFLAGS)
```
This is equivalent to

`GCC = gcc -std=c99 -g -Wall -Wshadow --pedantic -Wvla -Werror -DTEST_FUNC1 -DTEST_FUNC2`


Test Coverage
=============

You would not write code for no purpose. You would think every line of
code does something meaningful. However, sometimes, a program's
conditions make certain portions of code unreachable. This is called
"dead code". Here are two examples of dead code.

```
if ((x > 1) && (x < 0))
{  
   // cannot reach here
}

int func(...)
{
    return ...;
    // whatever after return cannot be reached
}
```

Sometimes, the program's execution flow is not expected. Consider the
following example.

```
x = 0;
...
if (x > 0)
{
    // do something 
}
else
{
    // do something else
}
```

You may expect that `x` is greater than zero and the program should
"do something".  However, `x` is actually not greater than zero and
the program always does "something else".  You can use many methods to
detect such an unexpected program execution flow. One of the methods
is called *test coverage*.  If `x` is not greater than zero, then the
code `do something` within the braces is not tested.

To use `gcov`, add

`-fprofile-arcs -ftest-coverage`

after `gcc`.

Then, execute the program as normal.  To see the coverage of a particular
source file type

`> gcov filename.c `

If any line is marked `#####`, this line is not tested.

Additional Tools to Learn
=========================

Please learn the following tools:

* A good editor. You are encouraged to learn how to use *hot keys*
  (for example, for copy-paste) without using a mouse. Using hot keys
  can *dramatically* reduce the time you spend on editing.

* shell commands, such as `grep`, `awk`, `find`, `mkdir`, `cd`, `mv`,
  `ssh`, `scp`, `diff`, `sort`, `rm`, `cp`, `cat -n`, `man`, `head`,
  `tail`, `more`, `less`.

* Touch typing. If you use only one or two fingers in typing and need
  to look at the keyboard, you will spend a lot of time on
  typing. There are many on-line tutorials about touch typing. Spend a
  few hours learning touch typing and save many thousand hours in the
  coming years.

