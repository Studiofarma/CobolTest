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

### Sidenotes

 - The length value `78 my-table-len value 5.` is just an example. The sorting program should be able to work with tables of different 
 lengths.
 - The size `9(09)` of the variable `my-table-arr` is just an example. The sorting program should be able to work with table elements of arbitrary size.
 - The `sort` program should sort the original table (`my-table` in the example). It should not rely on a supporting table defined to match exacly a linkage table defined in the `sort` program.


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

### Sidenotes

  - The size `x(02)` of the variable `my-table-arr` is just an example. The sorting program should be able to work with table elements of arbitrary size.

## Requirement 3

It should be possible to sort also by partial group content:

Examples:

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

or

```cobol

... 

01  my-table.
    03 my-table-arr occurs my-table-len.
        05 my-table-arr-a pic x(36).        
        |  ^--------------- I want the table to be sorted by the content of my-table-arr-a
        05 my-table-arr-1 pic x(256).
        05 my-table-arr-2 pic 9(02). 
        05 my-table-arr-3 pic 9(06)v9(02).

...
```

or

```cobol

... 

01  my-table.
    03 my-table-arr occurs my-table-len.
        05 my-table-arr-a pic x(36).        
        05 my-table-arr-b pic x(256).
            07 my-table-arr-1 pic 9(02). 
            |  ^--------------- I want the table to be sorted by the content of my-table-arr-1
            07 my-table-arr-2 pic 9(06)v9(02).
        05 my-table-arr-c pic 9(06)v9(02).

...
```

### Sidenotes

  - The sub-elements of the grouping variable `my-table-arr` could be of any number, at any level, of any kind (`pic 9, pic x` etc...) of any size. The definitions in the example, are just examples.

## Requirement 4

It should be possible to define a-posteriori arbitrary sorting logic without changing anything in the `"sort"` program. 

This requirement comes from the fact that I don't know which is the definition of "greater/lower" of everything when I write the the `"sort"` program. If the array contains numbers or strings, then "greater/lower" already has a definition.

But think about the case that you have an array containing names of fruits, and you want to sort it smallest to largest by the dimension of each fruit. Example:

```cobol
... 

01  my-table value spaces.
    03 my-table-arr pic x(25) occurs my-table-len.

...

    move "banana"    to my-table(1).
    move "anguria"   to my-table(2).
    move "fragola"   to my-table(3).
    move "albicocca" to my-table(4).

...

    | should print 
    | fragola albicocca banana anguria
```

Once the `sort` program is finished, it should be able to accept this new definition of "greater/lower", or any other new definition that comes to your mind in the future, without changing anything in the `sort` program. That means, you can't add a new `when` branch in an `evaluate` statement of the different sorting logics in the `sort` program!

The concept is similar of having a [`Comparable`](https://www.geeksforgeeks.org/comparable-interface-in-java-with-examples/) Java interface.

[`Strategy` pattern](https://it.wikipedia.org/wiki/Strategy_pattern) from Object Oriented Programming, also could give useful hints about possible implementations of this requirement 😉.
