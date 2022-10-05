# R-Overview

## Getting Started

### Download R
First you will need a copy of R from [the R Project website](https://cloud.r-project.org/).

[R Studio](https://www.rstudio.com/products/rstudio/download/) is a free Integrated Development Environment (IDE) for R, which makes it easier to manage your work.

### The REPL

The R console (and R Studio) use a Read-Evaluate-Print Loop (REPL) to process the code you enter in. The console will read each line of code you provide until it reaches a complete statement. The console will present a `>` symbol at the beginning of the first line it reads, and a `+` at the beginning of any lines that follow in a multi-line statement.

For example, consider the following sentence:

```poetry
And, as in uffish thought he stood,
      The Jabberwock, with eyes of flame,
Came whiffling through the tulgey wood,
      And burbled as it came!
```

There are four lines in this stanza, but all belong to one sentence and the thought (that the Jabberwock emerged from the wood while the hero stood waiting) is incomplete until the reader reaches the final exclamation point.

Similarly, in R, we can type: `2 + ` and hit `ENTER`; the console will add a new line starting with a `+` to complete the thought:

```R
>  2 +
+  2
[4]
```

`#` represents a comment. Anything between the comment symbol and the end of the line is not evaluated.

The final [4] is the result of evaluating `2 + 2` and is printed to the console. It is often helpful to break up long commands into multiple lines to help keep the input organized. Use braces (`{` and `}`) to group terms together and to ensure the console does not reach a termination too soon.

## Getting help

Use `?` to open an internal help file on any function or command in the console. E.g., `?data.frame`.

### Basic Data and Functions

## Data Types

R provides the following basic data types:

Name           | Description                                                 | Note
:------------  | :-------------------------------------------------------    | :---------
numeric        | Any rational number (floating point representation)         | Integer values can be specified by adding an `L` to the end, e.g., `1L`; Hexadecimal numbers can be specified using `0x` e.g., `0xff`. By default, all numbers are treated as having a potential decimal portion.
character      | A string of text values                                     | Create text using single- or double-quote marks, e.g., `"Hello, World!"`. Backslashes (\) have a special purpose for marking ('escaping') special characters like the line break ('\n'). Use double backslashes to represent one in text {'\\') or use the raw string syntax `r"()"` where the text you want goes inside the parenteses, e.g., `r"(\\fs02\users\me\My Documents)"`
logical        | Boolean TRUE or FALSE                                       | TRUE can convert to a number 1 and FALSE to a number 0. Use `==` to test equality and `>`, `<`, `>=`, or `<=` for inequalities. `!` as a prefix negates the logical value, so `!TRUE == FALSE`. Use `&` to test logical and (A and B are true) and `|` for logical or (A, B, or both is true).

## Mathematical Operators

Numeric values can use the following mathematical operators. 

Operator | Function
:------: | :-------
\+        | Addition
\-        | Subtraction
\*        | Multiplication
/        | Division
%/%      | Integer Division (divide and keep only whole portion of result)
%%       | Modulus (remainder from integer division)
^        | Exponent

All these operators can be used infix (between the two numbers) or as a prefix (function format). Use backticks (\`\) around these operators if using as prefixes, e.g., 
```R
`+`(2, 2) == 2 + 2
```

## Missing values

R has a special `NA` value to represent missingness. `NA` values propagate, meaning many operations will return `NA` if one of the inputs is `NA`, e.g., 
```R
1 + NA # yields NA
0 * NA # also yields NA
FALSE | NA # still yields NA
NA & FALSE # yields FALSE, because in this one situation we know both sides cannot be TRUE!
```

The reason for this is that `NA` represents complete uncertainty about the value that is missing. For all the R interpreter knows, `NA` could be any number, but it could also be a string of text, a function, or something not even considered!

## Assignment

R has two primary assignment operators, which tie a name to a value. `<-` is the most common and creates a variable with the requested name, e.g. `myDog <- "Fido"` or 
```R
circle.radius <- 10
circle.area   <- pi * circle.radius ^ 2
```
A single `=` can also be used in most places where `<-` works, but also can be used to bind values to argument names inside a function. See the next section for details.

## Functions

Most operations in R take the form of functions. Functions take some number of inputs and return a result value. Most R functions are 'pure' meaning they compute a result but do not alter the inputs, and will return the same value for the same inputs. Exceptions are random number generators (e.g., `rnorm`) unless a seed value is set, and the plotting functions which alter the drawing canvas to display a plot.

R functions take the form of the function name, open parenthesis, a sequence of arguments (separated by commas), and a closing parenthesis. Functions can be nested, so an argument can be computed from within the outer function. Some functions also take another function as an argument and apply it across a context (e.g., `Map`, `Reduce`, `apply`).

Function arguments have names (find them in the help file for the function), but names are optional. Often, the first argument will go unnamed (usually, this represents a data source and is called something generic like `data` or `x` anyway). Subsequent arguments should be named. Unspecified arguments will fall back on the default value supplied in the function definition, but the function will fail if there is not default available.

```R
rnorm(n = 10, mean = 100, sd = 15) # generate 10 random numbers pulled from a normal distribution with an average of 100 and standard deviation of 15.
rnorm(50, sd = 2) # generate 50 random numbers pulled from a normal distribution with an average of 0 (default) and standard deviation of 2.
rnorm(mean = 100, sd = 15) # returns error message: argument "n" is missing, with no default
rnorm(mean = 100, sd = 15, 10) # works like first example. Named arguments are placed first, then unnamed arguments are applied to the remaining values in order.
```

### Collections

Most of the time, you will work with collections of data with one or more dimensions. In fact, all 'single' values in R are actually vectors with a length of 1.

```R
length(10) # yields 1
```

## Vectors

Use `c()` to create a one-dimensional vector of values. The values in the vector can be named.

```R
vector_with_no_name <- c(1, 4, 7, 2)
named_vector <- c(first = 2, second = 5, next = 8, last = 12)
```

The `:` operator will also fill out integer ranges between the two arguments.
```R
1:10 # yields [1]  1  2  3  4  5  6  7  8  9 10

# use seq() for more control
seq(from = 0, to = 98, by = 7) # count by 7s from 0 to 98
seq(from = 0, to = 100, length.out = 11) # create a sequence of 11 evenly-spaced values from 0 to 100 inclusive.
```

Most mathematical operations are vectorized, meaning no extra work on your end is required to generalize from `2 + 2` to  `seq(from = 0, to = 98, by = 7) + 2`. There is a recycling rule in place, meaning that values from the shorter vector cycle through until the end of the longer vector is reached.

```R
{1:10} + {1:2}
# is equivalent to
c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) + c(1, 2, 1, 2, 1, 2, 1, 2, 1, 2)

{1:10} + {1:3} # will produce a result, but warn that the longer vector is not evenly divisble, so some value is left over at the end.
```

## Data Frames

Data frames are the most common collection you'll see. These are tabular collections (ordered into rows and columns). Every row must have the same number of columns and every column the same number of rows. Columns represent attributes and rows observations. For example, the `mtcars` dataset presents specifications for 32 models of cars. Each column has a name, and rows may also have names (though this is much less common).

You can access a single column from a data frame by name using the `$` operator: `mtcars$mpg`. Rows, columns, and cells from a data frame can also be accessed using indices in the form `dataframe[row, column]`, e.g., `mtcars[1, 1]` or `mtcars["Mazda RX4", "mpg"]`. See Lesson 2 for detail on the `subset` function for more flexible/intuitive extraction.

You can construct a data frame from vectors using the `data.frame` function.

```R
randomData <- data.frame(
  x = rnorm(n = 10, mean = 100, sd = 15), # normal random variable
  y = runif(n = 10, min = 0, max = 100), # uniform random variable
  z = rbinom(n = 10, size = 1, prob = .5) # Bernoulli random variable (10 fair coin flips)
)
```

Row and column names for data frames can be retrieved and overwritten using `colnames` and `rownames`.

```R
colnames(randomData) # yields [1] "x" "y" "z"
colnames(randomData) <- c("Larry", "Moe", "Curly") # changes the column names in randomData

rownames(mtcars) # retrieves the names for each row of mtcars (32 in all)
```

`with` sets a context for calculations within a data frame, saving the trouble of retyping the data frame name and the $ operator.

```R
mtcars$gear * mtcars$carb # is equivalent to:
with(mtcars, gear * carb)
```
