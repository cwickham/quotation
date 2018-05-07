Quotes
================
Charlotte Wickham
May 8th 2018

``` r
library(tidyverse)
library(lobstr)
library(rlang)
```

# Part I: Quotes

When do you need to surround something in quote marks?

## Quiz

What’s the difference between

``` r
3
```

and

``` r
"3"
```

?

*(For experienced users, how is this one different?)*

``` r
`3`
```

## Quotes tell R something is a character string

`"` and `'` are used in R to form character strings.

`3` is the number three,  
`"3"` is the character string containing the digit 3.

``` r
str(3)
```

    ##  num 3

``` r
str("3")
```

    ##  chr "3"

## Backticks are for *non-syntatic* names

Backticks `` ` `` are used for a object **name** (also known as symbol).
You don’t need them unless the name is unusual and wouldn’t usually be
considered a valid R object name, e.g. it begins with a number, contains
spaces, or other reserved characters.

Here, we are asking for an object called `3`,

``` r
`3`
```

    ## Error in eval(expr, envir, enclos): object '3' not found

but one doesn’t exist, so R returns an error. If we really wanted to
confuse ourselves, we could create an object with the name 3:

``` r
`3` <- 4
`3`
```

    ## [1] 4

But, I wouldn’t advise it. More often, weird names occur as the result
of inputting data where column names might involve spaces or special
characters.

``` r
?Quotes          # Read about ",' and `
?make.names      # Read rules for names to be valid
```

## Quiz

``` r
c <- 2
```

In the code below:

  - Which `c` is a *character string*?
  - Which `c` refers to a *the name of an object*?
  - Which `c` refers to a *function name*?
  - Which `c` refers to an *argument name*?

<!-- end list -->

``` r
c(c = "c", c)
```

## Quiz: solutions

The character string containing the letter `c`

    c(c = "c", c)
           ^
           |

An object with the name `c`

    c(c = "c", c)
               ^
               |

Function name

    c(c = "c", c)
    ^
    |

Argument name

    c(c = "c", c)
      ^
      |

## You don’t need quotes around function names or argument names

Function names and argument names **very rarely** need quotes. If you do
put quotes around them, you probably won’t get an error, but it’s
considered bad style.

These all work, and give the same result:

``` r
"mean"( x  =  c (1:5))
"mean"("x" =  c (1:5))
"mean"("x" = "c"(1:5))
 mean ( x  = "c"(1:5))
 mean ("x" = "c"(1:5))
```

But, you should write:

``` r
mean(x = c(1:5))
```

## Except…if you need a backtick `` ` ``

The exception is function names or argument names that aren’t considered
valid R names, and you’ll need backticks (`` ` ``).

E.g. the function with the name `+`

``` r
`+`(1, 2)
```

    ## [1] 3

OK that leaves **argument values**… to quote or not to quote?

## Strategy \#0: Trial and `Error` + Practice

Where most of us start\!

The more you use a function the better you get at guessing/remembering
what it is expecting.

But, it helps to have strategies to remove the guessing.

## Strategy \#1: What are you referring to?

| You are referring to:                               |           |
| --------------------------------------------------- | --------- |
| an **object** that exists in your workspace by name | No quotes |
| a particular character **string**                   | Quotes    |

``` r
n_times <- 10
```

I want to repeat the string “A”, `n_times` times:

``` r
rep(x = "A", times = n_times)
```

    ##  [1] "A" "A" "A" "A" "A" "A" "A" "A" "A" "A"

`A` is the **character string** I want to repeat, so it needs quotes,
`n_times` is the **name of an object**, so it doesn’t need quotes.

If I stored the string I wanted to repeat in `str_to_repeat`:

``` r
string_to_repeat <- "A"
```

Now, my `x` argument is the **name of an object** so it doesn’t need
quotes:

``` r
rep(x = string_to_repeat, times = n_times)
```

    ##  [1] "A" "A" "A" "A" "A" "A" "A" "A" "A" "A"

## Strategy \#1: Your Turn

``` r
books <- c("The Art of R Programming", "R for Data Science",
  "Advanced R")
R <- R.version.string
```

I want to see where the character string `R` occurs in the titles in my
`books` variable.

**Which code is correct?**

``` r
str_view(string = "books", pattern = "R")
str_view(string = "books", pattern =  R )
str_view(string =  books,  pattern = "R")
str_view(string =  books,  pattern =  R )
```

## Strategy \#1: Your Turn Solution

`books` is the name of an object, it doesn’t need quotes, `R` is a
specific character string, it needs quotes, so the correct option
    is:

``` r
str_view(string = books, pattern = "R")
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

<!--html_preserve-->

<div id="htmlwidget-37af9ee8853690aae1ee" class="str_view html-widget" style="width:960px;height:auto;">

</div>

<script type="application/json" data-for="htmlwidget-37af9ee8853690aae1ee">{"x":{"html":"<ul>\n  <li>The Art of <span class='match'>R<\/span> Programming<\/li>\n  <li><span class='match'>R<\/span> for Data Science<\/li>\n  <li>Advanced <span class='match'>R<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

(Caveat: this strategy may not work if the function **quotes** its
arguments, more in Quotation)

## Strategy \#2: Read the documentation

If the argument value is described as:

  - a character string, or string, use quotes,
  - an object, name, symbol, or expression don’t use quotes

## Strategy \#2: Example

The `pull()` function in dplyr extracts a column from a data frame.

If I want the `cyl` column from `mtcars`, do I want `pull(mtcars, cyl)`
or `pull(mtcars, "cyl")`?

``` r
?dplyr::pull
```

    Pull out a single variable
    
    Arguments:
    
       .data: A table of data
    
         var: A variable specified as:
    
                • a literal variable name
    
                • a positive integer, giving the position counting from the
                  left
    
                • a negative integer, giving the position counting from the
                  right.
    
              The default returns the last column (on the assumption that's
              the column you've created most recently).
    
              This argument is taken by expression and supports
              quasiquotation (you can unquote column names and column
              positions).

No quotes needed since this is a name. Also, look at examples section
and
    see:

``` r
mtcars %>% pull(cyl)
```

    ##  [1] 6 6 4 6 8 6 8 4 4 6 6 8 8 8 8 8 8 4 4 4 4 8 8 8 8 4 4 4 8 6 8 4

(Aside, `pull(mtcars, "cyl")` works as well)

When referring to an exsiting column inside a data frame, most (all?),
tidyverse functions will not require quotes.

## Strategy \#2: Your Turn

The `gather()` function in tidyr takes multiple columns and collapses
then into `key` and `value` columns.

Do the values of the `key` and `value` arguments need quotes?

``` r
?tidyr::gather
```

    Gather columns into key-value pairs.
    
    Arguments:
    
        data: A data frame.
    
    key, value: Names of new key and value columns, as strings or symbols.
    
              This argument is passed by expression and supports
              quasiquotation (you can unquote strings and symbols). The
              name is captured from the expression with 'rlang::quo_name()'
              (note that this kind of interface where symbols do not
              represent actual objects is now discouraged in the tidyverse;
              we support it here for backward compatibility).
    
         ...: A selection of columns. If empty, all variables are selected.
              You can supply bare variable names, select all variables
              between x and z with 'x:z', exclude y with '-y'. For more
              options, see the 'dplyr::select()' documentation. See also
              the section on selection rules below.
    
       na.rm: If 'TRUE', will remove rows from output where the value
              column in 'NA'.
    
     convert: If 'TRUE' will automatically run 'type.convert()' on the key
              column. This is useful if the column names are actually
              numeric, integer, or logical.
    
    factor_key: If 'FALSE', the default, the key values will be stored as a
              character vector. If 'TRUE', will be stored as a factor,
              which preserves the original ordering of the columns.

## Strategy \#2: Your Turn Solution

Do the values of the `key` and `value` arguments need quotes?

“as strings” -\> YES\!

``` r
gather(table4a, -country, key = "year", value = "count")
```

“or symbols” -\> NO\!

``` r
gather(table4a, -country, key =  year , value =  count )
```

So, in this case it doesn’t matter.

## Strategy \#3: `<-` Strategy \#0

Reading documentation isn’t always enlightening:

``` r
?purrr::pluck
```

For the `...` argument:

> Can be an integer position, a string name, or an accessor function.

A “string name”, is that the name of a string, or name as a string?

Revert to strategy \#0…
