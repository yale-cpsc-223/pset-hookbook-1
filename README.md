# Pset 3.5: Pirate Profiles

### Due Fri, Mar 10 2023

This assignment builds on the other HookBook assignment, specified in [README (Part 1).md](README%20(part%201).md).
Follow those instructions before proceeding with this portion of the assignment.

**Move your entire solution to the first part of the HookBook assignment into a folder named `hookbook_1`, and create a second folder, `hookbook_2`, which will contain your solution to this part of the assignment. You should copy all files from `hookbook_1` into `hookbook_2` before beginning.**

In this portion of the HookBook assignment, you will be expanding upon your previous program by adding more flexibility and information to your collection of pirates.

These pieces of information are:
1. The pirate's name
1. The pirate's current captain
1. The name of the pirate's current vessel of employ
1. The number of treasures the pirate has collected
1. A list of skills the pirate posesses

You must write an application that reads data from **two** different text files&mdash;one containing pirate profile information and another containing pirate/captain pairs&mdash;add those pirates to a list, sort that list *according to an order defined at run-time*, and then print the pirates' information to `stdout`.

The application must accept the following command-line arguments (in this order):
1. The name of a (text) file containing pirate profiles
1. The name of a (text) file containing pirate/captain pairs
1. One of `-n`, `-v`, or `-t`, identifying the order by which to sort the pirates
    * If `-n` is specified, the output of the pirates must be sorted in increasing lexicographic order by the pirate's name
    * If `-v` is specified, the output of the pirates must be sorted in increasing lexicographic order by the pirate's current vessel of employ
    * If `-t` is specified, the output of the pirates must be sorted by *decreasing* order of the number of treasures the pirate has collected

## Input Specification
The input to your program is two text files, passed by their file names at the command line.

### Pirate Profiles
The first file will contain many pirate profiles, each beginning with a pirate's name on a line beginning with the label "`name`" followed by a colon (`':'`).
Each subsequent line of a profile contains a label followed by a colon (`':'`) and then the value for that label.
The possibilities for these lines are as follows:
* The pirate's job title, with label "`title`"
* The pirate's current vessel of employ, with label "`vessel`"
* The pirate's favorite port of call, with label "`port`"
* The number of treasures the pirate has collected, with label "`treasure`"
* Zero or more skills this pirate posesses, each on its own line with the label "`skill`"

The next profile starts at the next line labeled `name`.

### Pirate/Captain Pairs
The second text file will identify the captain under which each pirate is currently sailing.
Each pirate/captain pair will be on a single line, with the two names separated by a single forward slash (`'/'`)&mdash;the first name is the underling and the second name is their captain.

### Assumptions
There are several assumptions you may make about the structure of the input files:
* If it exists, the file containing pirate profiles is properly formatted, that is:
    * Every line has the form `<label>:<value>`
    * Every non-`name` labeled line is meant to be associated with the pirate whose name appeared most recently in the file.
        > **Corollary**: the first line of the file is a pirate's name.
    * Neither the label nor value on any line is longer than $64$ characters, nor will any label or value contain `':'`.
    * The value associated with the `treasure` for every pirate is a decimal integer between $0$ and $(2^{32} - 1)$.
* If it exists, the file containing pirate/captain pairs is properly formatted, that is:
    * Every line has the form `<pirate-name>/<captain-name>`
    * Every name in the file is the name of a pirate with a profile in the other file
    * No name of a pirate will contain `'/'`

### Anomalies
There are several anomalies that your program must handle:
* The first two command-line arguments do not necessarily refer to text files that exist and are readable by your program.
If either of the files is cannot be opened, your program must print a message to `stderr` and exit with status code `1`.
The error message must be `"Invalid filename: "` followed by the value of the command-line argument that caused the error and a newline.
    * If *both* files are unopenable, then the *first* filename should be displayed as the invalid one, and the second one should be ignored.
* The third command-line argument could be anything.
If it is not one of `-n`, `-v`, or `-t`, your program must print a message to `stderr` and exit with status code `1`.
The error message must be `"Invalid argument: "` followed by the value of the command-line argument that caused the error and a newline.
* The program may be invoked with fewer than 3 command line arguments, in which case your program must print the string `"Invalid arguments"` followed by a newline to `stderr` and exit with status code `1`.
* The program may be invoked with more than 3 command-line arguments, in which case your program must ignore any argument after the third.
* Except that the first line of a profile is the pirate's name, no order is defined among the fields in a pirate's profile.
* There may be some pirates that are missing a field in their profile (for example, a pirate that is unemployed will not have a vessel in their profile).
* Some lines may have a label that is not one of `name`, `title`, `vessel`, `port`, `treasure`, or `skill`.
Such lines should be ignored.
* There may be several pirate profiles that share a name, in which case only the profile for the *first* pirate with a given name must be stored in the list of pirates.
* There may be duplicate labels within a single profile (except for `name`).
In this case only the *last* value for a particular label must be persisted in the pirate's profile.
    > **Note**: Skills are an exception to this, in that *every* listed skill must be persisted to the pirate's profile
* There may be duplicate skills listed for a given pirate.
In this case, only *one instance* of each skill must be persisted in the pirate's profile.
* Not all pirates with a profile will be party to a pirate/captain pair
* A pirate may be on the left-hand side of several pirate/captain pairs.
In this case only the *last* appearance of a pirate on the left-hand side of a pair should be persisted in your application
* It may be the case that the captain of one pirate also is the underling to a different captain.
*Both* of those relationships must be persisted in your application (so-called "transitive captainships" of more than two degrees must also be persisted).
    * These kinds of relationships may even be cyclical!
    Your program must handle this possibility.
* If you encounter any other anomaly in the command-line arguments or input that is not explicitly forbidden by the [assumptions](#assumptions), your program must exit with status code `1` and must not cause a system error such as a segmentation fault.

## Output Specification

The output of your program must be the list of pirate profiles, sorted according to the third command-line argument, each followed by a blank line.
Each pirate profile must have the following format:
```
Pirate: <pirate-name>
    Title: <pirate-title>
    Captain: <captain-name>
        Captain's Title: <captain-title>
        Captain's Favorite Port of Call: <captain-port>
    Vessel: <pirate-vessel>
    Treasure: <pirate-treasure>
    Favorite Port of Call: <pirate-port>
    Skills: <skill-0>
            <skill-1>
            <skill-2>
            [...and so on]

```

In other words, the output for each pirate profile must begin with a line containing the text `"Pirate: "` followed by the pirate's title and name (do not print `<` or `>`).
Thereafter, each line must be indented by **4 spaces**, and contain&mdash;*in order*&mdash;the pirate's title (labeled `"Title: "`), the profile of the pirate's captain (labeled and indented as above), the pirate's vessel of employ (labeled `"Vessel: "`), the pirate's treasure count (labeled `"Treasure: "`), and the pirate's favorite port of call (labeled `"Favorite Port of Call: "`).
After those fields, there must be a list, labeled `"Skills: "`, followed by all of the pirate's skills, each on on its own line and left-aligned with one another (a total of 12 spaces from the beginning of the line).
The skills in the list must be sorted in increasing lexicographic order.

If there is any field that was not given a value in a pirate's profile, the text `"(None)"` must be displayed on the corresponding line.
If a pirate has no captain, then the lines containing the captain's title and favorite port of call must not be present at all.

> **Note**: It is a [requirement](#requirements) that each pirate's profile is printed in $O(s)$ time where $s$ is the *number of skills posessed by that pirate*.
> Among other things, this means that you *may not* search the list of pirates for the captain's name each time you print a pirate's profile.

## Efficiency and Source Code Requirements

Beyond correctness with regard to the [output specification](#output-specification) above, there are several other benchmarks your solution must pass to earn full credit on this assignment.
1. Creating a pirate from its profile in the input file must take no more than $O(s)$ time, where $s$ is the number of skills the pirate poseses.
    * There is some subtlety here: the input specification does not forbid arbitrarily many lines of input being associated with one pirate, due to its allowance of duplicates.
    The time bound for creating a pirate applies *only* for pirate profiles that have no duplicate entries (profiles with duplicate entries must meet no particular time bound for their instantiation).
1. Sorting the list of pirates must require no more than $O(n^2)$ comparisons, where $n$ is the number of pirates in the list, *no matter which field is used to sort it*.
1. Printing a pirate's profile must take no more than $O(s)$ time, where $s$ is the number of skills the pirate posesses.
1. You are required to make minimal modifications to the existing `pirate_list.h` header file.
In particular, you may only add at most one parameter to at most one function in the file, and you must modify that function's comment to account for this change&mdash;if you make any changes at all.
    * You may make any modifications you want to your `pirate_list.c` source file and you may remove print statements regarding the expansion and contraction of your list.
    Any modifications you make to source code *must* be reflected in relevant comments.
1. You must release all resources that you acquire over the course of the program, including:
    * You must close every file that you open, and
    * You must free all memory that you allocate

## Recommendations

Except for the efficiency and source code requirements above, the rest is up to you.
Here are some recommendations to help you tackle this assignment.
* Implement your input digestion algorithm as a [finite state machine](https://en.wikipedia.org/wiki/Finite-state_machine)
* Define and implement a type for a "list of strings" to hold each pirate's skills.
Pay careful attention to the runtime of the insertion operation!
    * This new type could be a slight modification of your `pirate_list` type, or it could be entirely different.
    * You might instead consider implementing a *generic* list type that can be used for both the pirates and their skills.
        > **Note**: If you choose to implement a generic list type, you may use it to replace `pirate_list`, and you may ignore the modification rules surrounding that file.
        > You might, however, find it challenging to correctly implement and integrate such a generic list into your solution for Part 1 within the time constraints of this assignment.
* This assignment is designed such that it takes around 50 additional lines of code to implement on top of a very good solution to Part 1 of the HookBook assignment.
Use that number as a target, but don't worry if you can't get your line count all the way down to 50.
    * Definitely do not "minify" your code to reach 50 added lines.
    Lines of code (LOC, or SLOC if you exclude comments) is only one metric for determining code quality and complexity, and it is quite a bad metric for both.

## Correctness

Your solution will be assessed for correctness according to the input/output specification above.
To aid you in your self-assessment of correctness, we will release a limited set of test cases against which you can confirm things like input and output format specifics, but the test cases will not be exhaustive and you should supplement them by devising more on your own.

Your code will also be tested against an autograder running on Gradescope's servers.
In the event that your code produces output from the autograder that does not match what you expect&mdash;or if the Gradescope autograder simply does not cooperate&mdash;your submission will be run on the Zoo, which in such a case will be treated as the "official" behavior of your code.
**It is therefore imperative that you test your program on the Zoo to confirm its behavior is correct.**

## Submission

This assignment is due **Friday March 10, 2023 at 11:59 PM NHT (New Haven Time)**.
You must submit your solution by uploading the following items or a `.zip` file containing the following items to Gradescope by that deadline for full credit.
* A folder named `hookbook_1`, which contains your solution to [Part 1](README%20(part%201).md) of the assignment, with the prescribed files
    * The `hookbook_1` folder must, in addition to other files, contain a `LOG.md` file, containing the information prescribed for the log file in earlier assignments, except that you do not need an exact account of all hours worked&mdash;especially if you have already completed Part 1 of this assignment
* A folder named `hookbook_2`, which contains your solution to Part 2 (this part) of the assignment.
Within that folder, there must be (at least) the following files:
    * `makefile` (or `Makefile`), containing a target named `HookBook` that generates an executable named `HookBook` that, when run, satisfies the requirements outlined in this document
    * `LOG.md`, containing the information prescribed for the log file in earlier assignments
    * All files required to compile and run your program

---
## Disclaimer

**Any apparent contradiction in this document is unintentional.**
We always do our best to keep these documents as unambiguous as possible, but as with all complex things small pieces sometimes fall through the cracks.
Before reaching out to an instructor, ULA, or on Ed about a perceived contradiction in this spec, first read this document very carefully and see if you just missed the clarification elsewhere, and then reflect on what you believe our *intent* was when writing this document. Only if it is still not clear, seek assistance on the matter after you have taken those two steps.
A good rule of thumb in judging our intent is that if there are several possible interpretations of some item in this spec, the one that leads to the *more elegant solution* is the one we meant&mdash;our goal is not to overwhelm you with tedium.
In any such case, even if our intent is clear, we urge you to note the ambiguity in your `LOG.md` file you submit with your program.
Doing so helps us in two ways: first, so we can better understand why you might have made a decision that differs from other students, and second, so that we can disambiguate a future version of this assignment.

### Pirate Skills
Here is a non-exhaustive list of the skills your pirates posess:
* marksmanship
* swashbuckling
* cannon loading
* pickpocketing
* lockpicking
* knot tying
* cooking
* navigating
* charting
* shanty singing
* whip cracking
* grog guzzling
* rum running

---
&copy;2023 Alan Weide and Ozan Erat
