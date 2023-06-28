# Structures + Binary Files

Learning Goals
==============

* Understand structures
* Read data from a binary file using `fread`
* Write data to a binary file using `fwrite`
* Call `qsort` function to sort an array of structure 
* Write your own Makefile for testing
* Check memory error using `valgrind`

Makefile
========

You need to write your own Makefile from now on.  Makefile can save a
lot of time because you do not have to type many characters when you
test your program.

Functions Needed
================

* `int main(int argc, char * * argv)`
* `int countVector(char* filename)`
* `bool readVector(char* filename, Vector * vecArr, int size)`
* `bool writeVector(char* filename, Vector * vecArr, int size)`
* `int compareVector(const void *p1, const void *p2)`
	
Structure
=========
The structure looks like
``` 
    	#typedef struct
	{
		int x;
		int y;
		int z;
	} Vector;
```

It contains 3 integer variables, `x`, `y`, and `z`. 

Summary
========

The goal of this homework is: To read from a binary file, into an
array of `Vector` structure using the `fread` function. Then to sort,
using `qsort`, this array of structures based on the attribute of `x`
first.  If two vectors have the same `x` value, compare the `y`
values.  If two vectors have the same `y` value, compare the `z`
values.  Finally, write the sorted array of structures to the output
file, using `fwrite`.


