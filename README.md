# Cobol Test

Implement a sorting algorithm of your choice (bubblesort, quicksort, heapsort, mergesort, selectionsort, insertionsort... or any other) in a reusable program.

# Requirements

## Requirement 1

Write a main program that calls a `sort` program. `sort` receives as input a static table filled with data and sorts it. The main program then prints out the ordered table (to the console, to a text file or where you want).

Something like the following:

```cobol
identification division.
    program-id.  usage-example.
environment division.
working-storage section.

78 my-table-len value 5.

01  my-table value zeros.
    03 my-table-arr pic 9(09) occurs my-table-len.

77 i pic 9(03) value 0.

linkage section.

procedure division.
    move 10 to my-table(1).
    move  6 to my-table(2).
    move  1 to my-table(3).
    move  2 to my-table(4).
    move  7 to my-table(5).

    call "sort" using my-table.

    perform varying i from 1 until i >= my-table-len
        display my-table(i) upon console
        display " " upon console
    end-perform.

    | should print 
    | 1 2 6 7 10

    goback.
```

## Requirement 2

It should be possible to sort also alphanumeric content. Example:

```cobol

... 

01  my-table value spaces.
    03 my-table-arr pic x(02) occurs my-table-len.

...

    move "10" to my-table(1).
    move "6"  to my-table(2).
    move "1"  to my-table(3).
    move "2"  to my-table(4).
    move "7"  to my-table(5).

...

    | should print 
    | 1  10 2  6  7
    | because "10" is compared as alphanumeric to "1" and other numbers.
```

## Requirement 3

It should be possible to sort also by partial group content:

```cobol

... 

01  my-table.
    03 my-table-arr occurs my-table-len.
        05 my-table-arr-a pic x(02).
        05 my-table-arr-b pic 9(09). 
        |  ^--------------- I want the table to be sorted by the content of my-table-arr-b
        05 my-table-arr-c pic x(05).    

...
```

## Requirement 4

It should be possible to define a-posteriori arbitrary sorting logic without changing anything in the `"sort"` program.

The concept is similar of having a [`Comparable`](https://www.geeksforgeeks.org/comparable-interface-in-java-with-examples/) Java interface.
