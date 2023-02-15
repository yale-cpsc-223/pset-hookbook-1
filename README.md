# Pset3: Ahoy Matey!

### Due Wed, Mar 1 2023

Ahoy, matey!
This exercise will get you started programming Hookbook.
In this assignment, you will be practicing **file I/O operations**, **structs**, **dynamic memory allocation**, **array lists**, **sorting**, and **software engineering** in C.

## The Assignment
HookBook is a hot new social media platform built for pirates, by pirates (bulldogs are kind of piratey, right?).
To create a HookBook, you'll load a list of pirates from a file and add them to a list that you've created.
For this assignment, the only information about each pirate that you will be given is their name. 

However, your swashbuckling clientele is in the process of creating profiles for themselves that they will give you at a later date.
This means that your Hookbook program must easily transform to accept a list of pirates that includes all kinds of additional information about them:
* Their current vessel of employ
* Their current captain (who is another pirate!)
* The number of treasures they've found
* Their favorite port of call
* Maybe others

After the list of pirates is loaded by your program, your job will be to display the name of each pirate, sorted in alphabetical order, with each one followed by a newline.

> **Note**: Once again, the printing of names only is something of a placeholder until the pirates give you their full profiles.
> You'll be given instructions on how to display the pirate's profiles later, but your program must be ready to easily adapt to that future requirement.

### To summarize
Your program must:
1. Load a list of pirate names from a file whose name is given to you as a command-line argument
1. Add those pirates into a list, and sort that list in alphabetical order
1. Display the names of the pirates, sorted alphabetically
1. All of the above steps must be taken with the fact in mind that future requirements will increase the information you will hold about each pirate.

## Files you are given
* `pirate_list.h` (Interface), with the following declarations.
    **You may not change anything in this file**.
    ```C
    // Type of a list of pirates
    typedef struct implementation pirate_list;
    /*
     * Allocate memory for a new pirate_list and return a pointer to it
     */
    pirate_list *list_create();

    /*
     * Return the index of the pirate with the same name as p, or a value greater than or equal to the length of the list if the pirate is not in the list.
     */
    size_t list_index_of(pirate_list* pirates, void* p);

    /*
     * Only if there is no pirate in the list with the same name as p, insert the pirate p into the list at index idx, shifting the latter part of the list one “slot” to the right.
     * If there is a pirate in the list with the same name as p, do nothing, and return a pointer to the pirate that was not inserted.
     * If the pirate was inserted into the list, return NULL
     */
    void* list_insert(pirate_list* pirates, void* p, size_t idx);

    /*
     * Remove the pirate from the list with the same name as p, and return it.
     * If there is no pirate in the list with the same name as p, return NULL
     */
    void* list_remove(pirate_list* pirates, void* p);

    /*
     * Return a pointer to the pirate at index idx in the list, or NULL if idx is not a valid index (i.e., it is >= the length of the list).
     */
    void* list_access(pirate_list* pirates, size_t idx);

    /*
     * Sort the list of pirates in alphabetical order by the pirate name
     */
    void list_sort(pirate_list* pirates);

    /*
     * Return the number of pirates in the list.
     */
    size_t list_length(pirate_list* pirates);

    /*
     * Free all memory associated with the pirate_list, but leave the memory associated with the pirates in the list untouched (it is someone else's job to free the pirates; maybe that dog with the keys in the Pirates of the Caribbean).
     */
    void list_destroy(pirate_list* pirates);
    ```

* `makefile` (Incomplete)
    * The provided makefile defines two variables, `CC` and `CFLAGS`.
        You must not change those values.
    * The first target, `HookBook`, is incomplete and you must complete it. 
        However you must not change its name or its position in the makefile.
    * The rest of the makefile is up to you.

## Files you must create
* `hookbook.c` (Driver)
    * This will be the file where your `main` function is going to be.
        Your `main` function must:
        1. Take as the only command-line argument the path to a file containing the pirates' names who will be placed into our HookBook.
        1. Open that file and read from it the list of pirate names, appearing one on each line, storing them in a `pirate_list*`
        1. Sort the list in alphabetical order by pirate name
        1. Print the sorted list to `stdout`, with one pirate name per line, following the last pirate's name with a newline character
        1. Release all resources (files, memory, _etc._)
* `pirate_list.c` (Implementation), containing a body for every function declared in `pirate_list.h`, and a definition for `struct implementation`.
    * `pirate_list` must be implemented as an **array list**, _a.k.a._ **resizing array**.
    * The initial capacity of the list must be defined as a constant named `INITIAL_CAPACITY`, having value `25`.
    * When the array capacity changes, it must change by a factor of `2`, which must be the value of a constant named `RESIZE_FACTOR`
    * In addition to functions declared in `pirate_list.h`, `pirate_list.c` must declare and implement two "helper" functions that are not declared in the header file:
        ```C
        /*
         * Check if the actual number of pirates in the array is "too large"; if it is, increase the capacity of the array by a factor of RESIZE_FACTOR.
         * If the array capacity was changed, print to stderr the phrase "Expand to", followed by the new capacity of the list and a newline. Here is a possible print statement:
         *
         *     fprintf(stderr, "Expand to %zu\n", new_capacity);
         *
         * If the capacity was not changed, do nothing.
         */
        void list_expand_if_necessary(pirate_list* pirates);

        /*
         * Check if the actual number of pirates in the array is "too small"; if it is, decrease the capacity of the array by a factor of RESIZE_FACTOR.
         * If the array capacity was changed, print to stderr the phrase "Contract to" followed by the new capacity of the list. Here is a possible print statement:
         *
         *     fprintf(stderr, Contract to %zu\n, new_capacity);
         *
         * If the capacity was not changed, do nothing.
         *
         * The capacity of the array must never fall below INITIAL_CAPACITY!
         */
        void list_contract_if_necessary(pirate_list* pirates);
        ```

## Other requirements
* If you factor out some or all of the functionality of `main` into helper functions (such as a function to print the list), you must create a header file and a source file **separate from `hookbook.c`** with their declarations and bodies, respectively.
    The additional files may be named anything you like, but it must be a "good" name to earn full style points.
    We suggest "`libhookbook`".
* If you define a new type to store information about each pirate, you must create a header file and a source file **separate from all others** with relevant declarations and definitions, respectively.
    The additional files may be named anything, but we suggest `pirate.h` and `pirate.c`, and we suggest the type is named `pirate`.
    Whatever you choose, as above, the names must be "good" to earn full style points.
* Each header and source file must use `#include` directives to include things that are directly needed by that file&mdash;no more, no less.
    Style points will be deducted for clearly extraneous `#include` directives.
* You may not use `qsort` to sort the list, nor any other pre-implemented sorting function.
    You **must** implement a sorting algorithm on your own, and it must run in $O(n^2)$ time or better and use $O(n)$ additional memory (or less).

## Suggestions and Notes
* In class, we implemented a naive array list to which you could append and remove `int`s from just one end.
    Use that to jump start your implementation of `pirate_list`.
* Factor out the functionality of `main` into several well-named, easily-understood functions
* Do not use a raw `char*` for pirate information&mdash;remember, your program must easily adapt to store additional information about each pirate later on!
* To help manage `#include` directives, draw a "map" of your project's files, with arrows marking direct dependencies, and use that to write your include directives
* `void*` is C-speak for "a pointer to something".
    That may not seem terribly usefule, but sometimes you happen to know that a `void*` is actually "a pointer to a pirate" (such as when you remove an item from your `pirate_list`).
    You can make this knowledge explicit in your program by using a **type cast**, for example `pirate* p = (pirate*) list_remove(...);`.
    It's important to note here that type casting doesn't actually _do_ anything; it is just a way for you to tell the compiler "hey, I know this `void*` is actually a `pirate*`, so let's treat it as such."
* `NULL` is a special pointer value that means "points to nothing".
    Attempting to dereference `NULL` will result in a segmentation fault, which your program must avoid.

## Correctness
We will release a downloadable testing script on Feb 17 with which you can test that your code conforms to the prescribed output format (which is quite a bit more straightforward than in previous assignments) for a limited set of test cases.
The README file associated with that download will contain instructions for adding your own test cases to the `tests` directory.
We recommend that you add some to increase the breadth of tests for your project.

Your code will also be tested against an autograder running on Gradescope's servers.
In the event that your code produces output from the autograder that does not match what you expect, the program will be run on the Zoo, which will be treated as the "official" output from your code.
It is therefore imperative that you test your program on the Zoo to confirm its behavior.

## Assumptions
You may assume the following about the command-line arguments to the program:
* Nothing! 
    We will run your program with no command-line arguments, many command-line arguments, and a single command-line argument that is the name of a file that does not exist.
    In all cases where the command-line argument is not the name of an existing text file, your program must exit with status code `1`.

You may assume the following about the file referred to by the command-line argument, if it exists:
* It is a text file.
    That is, we will not try to open a binary file, _e.g._, an image of a pirate ship
* The last line of the file is an empty line

## Submission
Submit your project to Gradescope by uploading all files needed to run your program.
An easy way to do so is by creating a zip file containing those files and uploading that to Gradescope.
Be careful to zip _only the files_, and not the _directory_ containing those files.

You may also upload the files individually to Gradescope through its upload interface.
