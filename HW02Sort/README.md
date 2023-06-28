# Selection Sort

Learning Goals 
==============

This assignment asks you to write a program that uses *selection sort*
to sort an array of integers in the ascending order.

You will learn to
* Implement selection sort
* Learn how to write code to check the correctness
* Use Makefile to test a program

Selection Sort
==============

Sorting is widely used in computing. When you use a map to find a
route, the program finds several options and orders them by the
estimated travel time.  When you search a product online, a search
engine looks for good matches and orders them by relevance.  When you
decide which homework to do, you order them by difficulty and the due
days.  If you own several stores, you may want to reward store
managers by the amounts of sales.  

Many sorting algorithms have been invented for different situations.
In fact, you can an entire book on the subject of sorting.  One of the
most famous books in computing is "The Art of Computer Programming" by
Donald E. Knuth and Volume 3 is devoted to the topics of Sorting and
Searching.  Some sorting algorithms have fewer data movements; some
others have fewer comparisons.  Some are better if the data can fit in
memory; some are designed for data scattered in different geographical
regions.

Selection sort (for the ascending order), operates on the following
principle:

1. find the smallest number
2. move it to the beginning of the list
3. exclude the number, and sort the rest of the data

Structure of the Files
======================

* The expected files are in `expected` folder
*  run `make` to make your binary for running
* `make test(x)` will run `x` test case
* output from your code will be save in `output(x).txt`




