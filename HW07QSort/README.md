# Quick Sort

Learning Goals 
==============

This assignment asks you to write a program that uses *qsort* (a
function provided by C) to sort an array of integers in the ascending
order.

You will learn to
* Count the number of integers in a file
* Allocate memory (an array) to store the integers
* Read integers from the file and store them in the array
* Call `qsort` function to sort the integers
* Write the sorted array to another file
* Release memory
* Check memory error using `valgrind`

Quick Sort
==============

The [`qsort` function](https://linux.die.net/man/3/qsort) is a generic
function in C for sorting arrays. Please go to the link and read the
explanation.  For this assignment, you need to understand how to use
`qsort`. You do not need to understand the algorithm. You will learn
the algorithm later (after understanding recursion).

What You Need to Do
===================

Need to complete the following functions:
* `int main(int argc, char * * argv)`
* `int countInt(char* filename)`
* `bool readInt(char* filename, int * intArr, int size)`
* `bool writeInt(char* filename, int * intArr, int size)`
* `int compareInt(const void *p1, const void *p2)`

