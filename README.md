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
logical        | Boolean TRUE or FALSE                                       | TRUE can convert to a number 1 and FALSE to a number 0. Use `==` to test equality and `>`, `<`, `>=`, or `<=` for inequalities. `!` as a prefix negates the logical value, so `!TRUE == FALSE`

Numeric values can use the following mathematical operators. 

Operator | Function
:------: | :-------
+        | Addition
-        | Subtraction
*        | Multiplication
/        | Division
%/%      | Integer Division (divide and keep only whole portion of result)
%%       | Modulus (remainder from integer division)
^        | Exponent

All these operators can be used infix (between the two numbers) or as a prefix (function format). Use backticks (`) around these operators if using as prefixes, e.g., ``+`(2, 2) == 2 + 2`

