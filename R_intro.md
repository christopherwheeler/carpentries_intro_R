Intro to R
================
Christopher Wheeler

## Before we Start

``` r
#install.packages(c("tidyverse", "hexbin", "patchwork", "RSQLite"))
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.1.3

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --v ggplot2 3.3.5      v purrr   0.3.4 
    ## v tibble  3.1.8      v dplyr   1.0.10
    ## v tidyr   1.2.0      v stringr 1.4.0 
    ## v readr   2.1.3      v forcats 0.5.1

    ## Warning: package 'tibble' was built under R version 4.1.3

    ## Warning: package 'tidyr' was built under R version 4.1.3

    ## Warning: package 'readr' was built under R version 4.1.3

    ## Warning: package 'dplyr' was built under R version 4.1.3

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(hexbin)
```

    ## Warning: package 'hexbin' was built under R version 4.1.2

``` r
library(patchwork)
```

    ## Warning: package 'patchwork' was built under R version 4.1.2

``` r
library(RSQLite)
```

    ## Warning: package 'RSQLite' was built under R version 4.1.2

## Konwing your way around RStudio

RStudio is divided into 4 “panes”:

-   The Source for your scripts and documents (top-left, in the default
    layout)
-   Your Environment/History (top-right) which shows all the objects in
    your working space (Environment) and your command history (History)
-   Your Files/Plots/Packages/Help/Viewer (bottom-right)
-   The R Console (bottom-left)

## Getting set up

It is good practice to keep a set of related data, analyses, and text
self-contained in a single folder, called the working directory. All of
the scripts within this folder can then use relative paths to files that
indicate where inside the project a file is located (as opposed to
absolute paths, which point to where a file is on a specific computer).
Working this way allows you to move your project around on your computer
and share it with others without worrying about whether or not the
underlying scripts will still work.

-   Start RStudio.
-   Under the File menu, click on New Project. Choose New Directory,
    then New Project.
-   Enter a name for this new folder (or “directory”), and choose a
    convenient location for it. This will be your working directory for
    the rest of the day (e.g., \~/data-carpentry).
-   Click on Create Project.

## Make neccessary directories

``` r
#dir.create("data_raw")
#dir.create("data")
#dir.create("fig")
#dir.create("scripts")
```

## Creating objects in R

``` r
3 + 5
```

    ## [1] 8

``` r
12 / 7
```

    ## [1] 1.714286

``` r
weight_kg <- 55
```

\<- is the assignment operator. It assigns values on the right to
objects on the left. So, after executing x \<- 3, the value of x is 3.
For historical reasons, you can also use = for assignments, but not in
every context. Because of the slight differences in syntax, it is good
practice to always use \<- for assignments.

In RStudio, typing Alt + - (push Alt at the same time as the - key) will
write \<- in a single keystroke in a PC, while typing Option + - (push
Option at the same time as the - key) does the same in a Mac.

-   You want your object names to be explicit and not too long.
-   They cannot start with a number (2x is not valid, but x2 is).
-   R is case sensitive, so for example, weight_kg is different from
    Weight_kg.
-   There are some names that cannot be used because they are the names
    of fundamental functions in R (e.g., if, else, for, see here for a
    complete list). In general, even if it’s allowed, it’s best to not
    use other function names (e.g., c, T, mean, data, df, weights). If
    in doubt, check the help to see if the name is already in use.
-   It’s best to avoid dots (.) within names. Many function names in R
    itself have them and dots also have a special meaning (methods) in R
    and other programming languages. To avoid confusion, don’t include
    dots in names.
-   It is recommended to use nouns for object names and verbs for
    function names.
-   Be consistent in the styling of your code, such as where you put
    spaces, how you name objects, etc. Styles can include “lower_snake”,
    “UPPER_SNAKE”, “lowerCamelCase”, “UpperCamelCase”, etc. Using a
    consistent coding style makes your code clearer to read for your
    future self and your collaborators. In R, three popular style guides
    come from Google, Jean Fan and the tidyverse. The tidyverse style is
    very comprehensive and may seem overwhelming at first. You can
    install the lintr package to automatically check for issues in the
    styling of your code

What are known as objects in R are known as variables in many other
programming languages. Depending on the context, object and variable can
have drastically different meanings. However, in this lesson, the two
words are used synonymously.

``` r
weight_kg <- 55    # doesn't print anything
(weight_kg <- 55)  # but putting parenthesis around the call prints the value of `weight_kg`
```

    ## [1] 55

``` r
weight_kg          # and so does typing the name of the object
```

    ## [1] 55

``` r
2.2 * weight_kg
```

    ## [1] 121

``` r
weight_kg <- 57.5
2.2 * weight_kg
```

    ## [1] 126.5

``` r
weight_lb <- 2.2 * weight_kg

weight_kg <- 100
```

What do you think is the current content of the object weight_lb? 126.5
or 220?

## Saving your Code and Comments

Saving your code Up to now, your code has been in the console. This is
useful for quick queries but not so helpful if you want to revisit your
work for any reason. A script can be opened by pressing Ctrl + Shift +
N. It is wise to save your script file immediately. To do this press
Ctrl + S. This will open a dialogue box where you can decide where to
save your script file, and what to name it. The .R file extension is
added automatically and ensures your file will open with RStudio.

Don’t forget to save your work periodically by pressing Ctrl + S.

Comments The comment character in R is \#. Anything to the right of a \#
in a script will be ignored by R. It is useful to leave notes and
explanations in your scripts. For convenience, RStudio provides a
keyboard shortcut to comment or uncomment a paragraph: after selecting
the lines you want to comment, press at the same time on your keyboard
Ctrl + Shift + C. If you only want to comment out one line, you can put
the cursor at any location of that line (i.e. no need to select the
whole line), then press Ctrl + Shift + C

## Challenge 1

What are the values after each statement in the following?

``` r
mass <- 47.5            # mass?
age  <- 122             # age?
mass <- mass * 2.0      # mass?
age  <- age - 20        # age?
mass_index <- mass/age  # mass_index?
```

## Functions and their Arguments

Functions are “canned scripts” that automate more complicated sets of
commands including operations assignments, etc. Many functions are
predefined, or can be made available by importing R packages (more on
that later). A function usually takes one or more inputs called
arguments. Functions often (but not always) return a value. A typical
example would be the function sqrt(). The input (the argument) must be a
number, and the return value (in fact, the output) is the square root of
that number. Executing a function (‘running it’) is called calling the
function. An example of a function call is:

``` r
weight_kg <- sqrt(10)
```

Here, the value of 10 is given to the sqrt() function, the sqrt()
function calculates the square root, and returns the value which is then
assigned to the object weight_kg. This function takes one argument,
other functions might take several.

The return ‘value’ of a function need not be numerical (like that of
sqrt()), and it also does not need to be a single item: it can be a set
of things, or even a dataset. We’ll see that when we read data files
into R.

Arguments can be anything, not only numbers or filenames, but also other
objects. Exactly what each argument means differs per function, and must
be looked up in the documentation (see below). Some functions take
arguments which may either be specified by the user, or, if left out,
take on a default value: these are called options. Options are typically
used to alter the way the function operates, such as whether it ignores
‘bad values’, or what symbol to use in a plot. However, if you want
something specific, you can specify a value of your choice which will be
used instead of the default.

Let’s try a function that can take multiple arguments: round()

``` r
round(3.14159)
```

    ## [1] 3

Here, we’ve called round() with just one argument, 3.14159, and it has
returned the value 3. That’s because the default is to round to the
nearest whole number. If we want more digits we can see how to do that
by getting information about the round function. We can use args(round)
to find what arguments it takes, or look at the help for this function
using ?round.

``` r
args(round)
```

    ## function (x, digits = 0) 
    ## NULL

``` r
?round
```

    ## starting httpd help server ... done

We see that if we want a different number of digits, we can type digits
= 2 or however many we want.

``` r
round(3.14159, digits = 2)
```

    ## [1] 3.14

If you provide the arguments in the exact same order as they are defined
you don’t have to name them:

``` r
round(3.14159, 2)
```

    ## [1] 3.14

And if you do name the arguments, you can switch their order:

``` r
round(digits = 2, x = 3.14159)
```

    ## [1] 3.14

It’s good practice to put the non-optional arguments (like the number
you’re rounding) first in your function call, and to then specify the
names of all optional arguments. If you don’t, someone reading your code
might have to look up the definition of a function with unfamiliar
arguments to understand what you’re doing

## Vectors and data types

A vector is the most common and basic data type in R, and is pretty much
the workhorse of R. A vector is composed by a series of values, which
can be either numbers or characters. We can assign a series of values to
a vector using the c() function. For example we can create a vector of
animal weights and assign it to a new object weight_g:

``` r
weight_g <- c(50, 60, 65, 82)
weight_g
```

    ## [1] 50 60 65 82

A vector can also contain characters:

``` r
animals <- c("mouse", "rat", "dog")
animals
```

    ## [1] "mouse" "rat"   "dog"

The quotes around “mouse”, “rat”, etc. are essential here. Without the
quotes R will assume objects have been created called mouse, rat and
dog. As these objects don’t exist in R’s memory, there will be an error
message.

There are many functions that allow you to inspect the content of a
vector. length() tells you how many elements are in a particular vector:

``` r
length(weight_g)
```

    ## [1] 4

``` r
length(animals)
```

    ## [1] 3

An important feature of a vector, is that all of the elements are the
same type of data. The function class() indicates what kind of object
you are working with:

``` r
class(weight_g)
```

    ## [1] "numeric"

``` r
class(animals)
```

    ## [1] "character"

The function str() provides an overview of the structure of an object
and its elements. It is a useful function when working with large and
complex objects:

``` r
str(weight_g)
```

    ##  num [1:4] 50 60 65 82

``` r
str(animals)
```

    ##  chr [1:3] "mouse" "rat" "dog"

You can use the c() function to add other elements to your vector:

``` r
weight_g <- c(weight_g, 90) # add to the end of the vector
weight_g <- c(30, weight_g) # add to the beginning of the vector
weight_g
```

    ## [1] 30 50 60 65 82 90

An atomic vector is the simplest R data type and is a linear vector of a
single type. Above, we saw 2 of the 6 main atomic vector types that R
uses: “character” and “numeric” (or “double”). These are the basic
building blocks that all R objects are built from. The other 4 atomic
vector types are:

“logical” for TRUE and FALSE (the boolean data type) “integer” for
integer numbers (e.g., 2L, the L indicates to R that it’s an integer)
“complex” to represent complex numbers with real and imaginary parts
(e.g., 1 + 4i) and that’s all we’re going to say about them “raw” for
bitstreams that we won’t discuss further

You can check the type of your vector using the typeof() function and
inputting your vector as the argument.

Vectors are one of the many data structures that R uses. Other important
ones are lists (list), matrices (matrix), data frames (data.frame),
factors (factor) and arrays (array)

# Challenge 2

We’ve seen that atomic vectors can be of type character, numeric (or
double), integer, and logical. But what happens if we try to mix these
types in a single vector?

R implicitly converts them to all be the same type

HINT USE class()

``` r
num_char <- c(1, 2, 3, "a")
num_logical <- c(1, 2, 3, TRUE)
char_logical <- c("a", "b", "c", TRUE)
tricky <- c(1, 2, 3, "4")

class(num_char)
```

    ## [1] "character"

``` r
class(num_logical)
```

    ## [1] "numeric"

``` r
class(char_logical)
```

    ## [1] "character"

``` r
class(tricky)
```

    ## [1] "character"

Vectors can be of only one data type. R tries to convert (coerce) the
content of this vector to find a “common denominator” that doesn’t lose
any information.

How many values in combined_logical are “TRUE” (as a character) in the
following example (reusing the 2 …\_logicals from above):

``` r
combined_logical <- c(num_logical, char_logical)

class(combined_logical)
```

    ## [1] "character"

You’ve probably noticed that objects of different types get converted
into a single, shared type within a vector. In R, we call converting
objects from one class into another class coercion. These conversions
happen according to a hierarchy, whereby some types get preferentially
coerced into other types. Can you draw a diagram that represents the
hierarchy of how these data types are coerced?

ANSWER

logical → numeric → character ← logical

# Subsetting Vectors

If we want to extract one or several values from a vector, we must
provide one or several indices in square brackets. For instance:

``` r
animals <- c("mouse", "rat", "dog", "cat")
animals[2]
```

    ## [1] "rat"

``` r
animals[c(3, 2)]
```

    ## [1] "dog" "rat"

We can also repeat the indices to create an object with more elements
than the original one:

``` r
more_animals <- animals[c(1, 2, 3, 2, 1, 4)]
more_animals
```

    ## [1] "mouse" "rat"   "dog"   "rat"   "mouse" "cat"

R indices start at 1. Programming languages like Fortran, MATLAB, Julia,
and R start counting at 1, because that’s what human beings typically
do. Languages in the C family (including C++, Java, Perl, and Python)
count from 0 because that’s simpler for computers to do.

## Conditional subsetting

weight_g \<- c(21, 34, 39, 54, 55) weight_g\[c(TRUE, FALSE, FALSE, TRUE,
TRUE)\]

Typically, these logical vectors are not typed by hand, but are the
output of other functions or logical tests. For instance, if you wanted
to select only the values above 50:

``` r
weight_g > 50    # will return logicals with TRUE for the indices that meet the condition
```

    ## [1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE

``` r
## so we can use this to select only the values above 50
weight_g[weight_g > 50]
```

    ## [1] 60 65 82 90

You can combine multiple tests using & (both conditions are true, AND)
or \| (at least one of the conditions is true, OR):

``` r
weight_g[weight_g > 30 & weight_g < 50]
```

    ## numeric(0)

``` r
weight_g[weight_g <= 30 | weight_g == 55]
```

    ## [1] 30

``` r
weight_g[weight_g >= 30 & weight_g == 21]
```

    ## numeric(0)

Here, \> for “greater than”, \< stands for “less than”, \<= for “less
than or equal to”, and == for “equal to”. The double equal sign == is a
test for numerical equality between the left and right hand sides, and
should not be confused with the single = sign, which performs variable
assignment (similar to \<-).

A common task is to search for certain strings in a vector. One could
use the “or” operator \| to test for equality to multiple values, but
this can quickly become tedious. The function %in% allows you to test if
any of the elements of a search vector are found:

``` r
animals <- c("mouse", "rat", "dog", "cat", "cat")

# return both rat and cat
animals[animals == "cat" | animals == "rat"]
```

    ## [1] "rat" "cat" "cat"

``` r
# return a logical vector that is TRUE for the elements within animals
# that are found in the character vector and FALSE for those that are not
animals %in% c("rat", "cat", "dog", "duck", "goat", "bird", "fish")
```

    ## [1] FALSE  TRUE  TRUE  TRUE  TRUE

``` r
# use the logical vector created by %in% to return elements from animals
# that are found in the character vector
animals[animals %in% c("rat", "cat", "dog", "duck", "goat", "bird", "fish")]
```

    ## [1] "rat" "dog" "cat" "cat"

# Challenge

Can you figure out why “four” \> “five” returns TRUE?

``` r
"four" > "five"
```

    ## [1] TRUE

## Missing Data

As R was designed to analyze datasets, it includes the concept of
missing data (which is uncommon in other programming languages). Missing
data are represented in vectors as NA.

When doing operations on numbers, most functions will return NA if the
data you are working with include missing values. This feature makes it
harder to overlook the cases where you are dealing with missing data.
You can add the argument na.rm = TRUE to calculate the result as if the
missing values were removed (rm stands for ReMoved) first.

``` r
heights <- c(2, 4, 4, NA, 6)
mean(heights)
```

    ## [1] NA

``` r
max(heights)
```

    ## [1] NA

``` r
mean(heights, na.rm = TRUE)
```

    ## [1] 4

``` r
max(heights, na.rm = TRUE)
```

    ## [1] 6

If your data include missing values, you may want to become familiar
with the functions is.na(), na.omit(), and complete.cases(). See below
for examples.

``` r
## Extract those elements which are not missing values.
heights[!is.na(heights)]
```

    ## [1] 2 4 4 6

``` r
## Returns the object with incomplete cases removed.
#The returned object is an atomic vector of type `"numeric"` (or #`"double"`).
na.omit(heights)
```

    ## [1] 2 4 4 6
    ## attr(,"na.action")
    ## [1] 4
    ## attr(,"class")
    ## [1] "omit"

``` r
## Extract those elements which are complete cases.
#The returned object is an atomic vector of type `"numeric"` (or #`"double"`).
heights[complete.cases(heights)]
```

    ## [1] 2 4 4 6

# Challenge

Using this vector of heights in inches, create a new vector,
heights_no_na, with the NAs removed.

``` r
heights <- c(63, 69, 60, 65, NA, 68, 61, 70, 61, 59, 64, 69, 63, 63, NA, 72, 65, 64, 70, 63, 65)
```

Use the function median() to calculate the median of the heights vector.

Use R to figure out how many people in the set are taller than 67
inches.

``` r
heights <- c(63, 69, 60, 65, NA, 68, 61, 70, 61, 59, 64, 69, 63, 63, NA, 72, 65, 64, 70, 63, 65)

# 1.
heights_no_na <- heights[!is.na(heights)]
# or
heights_no_na <- na.omit(heights)
# or
heights_no_na <- heights[complete.cases(heights)]

# 2.
median(heights, na.rm = TRUE)
```

    ## [1] 64

``` r
# 3.
heights_above_67 <- heights_no_na[heights_no_na > 67]
length(heights_above_67)
```

    ## [1] 6
