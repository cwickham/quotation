Quotes, Quotation and Quasiquotation
================
Charlotte Wickham
May 8th 2018

# The game plan

Three examples:

1.  Using a dplyr function interactively
2.  Using a dplyr function with a saved variable
3.  Using a dplyr function inside a function

<!-- end list -->

``` r
library(tidyverse)
library(rlang)
```

Motivated by an [Exercise in Advanced
R](https://adv-r.hadley.nz/quasiquotation.html#exercises-60)

> Implement `arrange_desc()`, a variant of `dplyr::arrange()` that sorts
> in descending order by default.

# 1\. Using a dplyr function interactively

## dplyr

dplyr provides **data manipulation** verbs:

  - `filter()`
  - `select()`
  - `arrange()`
  - `mutate()`
  - `summarise()`
  - `group_by()`

## Star Wars

``` r
starwars
```

    ## # A tibble: 87 x 13
    ##    name     height  mass hair_color skin_color eye_color birth_year gender
    ##    <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ##  1 Luke Sk…    172  77.0 blond      fair       blue            19.0 male  
    ##  2 C-3PO       167  75.0 <NA>       gold       yellow         112   <NA>  
    ##  3 R2-D2        96  32.0 <NA>       white, bl… red             33.0 <NA>  
    ##  4 Darth V…    202 136   none       white      yellow          41.9 male  
    ##  5 Leia Or…    150  49.0 brown      light      brown           19.0 female
    ##  6 Owen La…    178 120   brown, gr… light      blue            52.0 male  
    ##  7 Beru Wh…    165  75.0 brown      light      blue            47.0 female
    ##  8 R5-D4        97  32.0 <NA>       white, red red             NA   <NA>  
    ##  9 Biggs D…    183  84.0 black      light      brown           24.0 male  
    ## 10 Obi-Wan…    182  77.0 auburn, w… fair       blue-gray       57.0 male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

## `arrange()`: Reorder rows in data

First argument, `.data`, is a data frame or tibble,  
other arguments, `...`, are columns to order by.

Reorder by `mass`:

``` r
arrange(starwars, mass)
```

    ## # A tibble: 87 x 13
    ##    name     height  mass hair_color skin_color eye_color birth_year gender
    ##    <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ##  1 Ratts T…     79  15.0 none       grey, blue unknown        NA    male  
    ##  2 Yoda         66  17.0 white      green      brown         896    male  
    ##  3 Wicket …     88  20.0 brown      brown      brown           8.00 male  
    ##  4 R2-D2        96  32.0 <NA>       white, bl… red            33.0  <NA>  
    ##  5 R5-D4        97  32.0 <NA>       white, red red            NA    <NA>  
    ##  6 Sebulba     112  40.0 none       grey, red  orange         NA    male  
    ##  7 Dud Bolt     94  45.0 none       blue, grey yellow         NA    male  
    ##  8 Padmé A…    165  45.0 brown      light      brown          46.0  female
    ##  9 Wat Tam…    193  48.0 none       green, gr… unknown        NA    male  
    ## 10 Sly Moo…    178  48.0 none       pale       white          NA    female
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

Reorder by `mass`, breaking ties with `height`:

``` r
arrange(starwars, mass, height)
```

    ## # A tibble: 87 x 13
    ##    name     height  mass hair_color skin_color eye_color birth_year gender
    ##    <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ##  1 Ratts T…     79  15.0 none       grey, blue unknown        NA    male  
    ##  2 Yoda         66  17.0 white      green      brown         896    male  
    ##  3 Wicket …     88  20.0 brown      brown      brown           8.00 male  
    ##  4 R2-D2        96  32.0 <NA>       white, bl… red            33.0  <NA>  
    ##  5 R5-D4        97  32.0 <NA>       white, red red            NA    <NA>  
    ##  6 Sebulba     112  40.0 none       grey, red  orange         NA    male  
    ##  7 Dud Bolt     94  45.0 none       blue, grey yellow         NA    male  
    ##  8 Padmé A…    165  45.0 brown      light      brown          46.0  female
    ##  9 Sly Moo…    178  48.0 none       pale       white          NA    female
    ## 10 Wat Tam…    193  48.0 none       green, gr… unknown        NA    male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

Reorder by decreasing `mass`:

``` r
arrange(starwars, desc(mass))
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Darth …    202   136 none        white      yellow          41.9 male  
    ##  5 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

## The alternative with `order()` and `[`

``` r
starwars[order(starwars$mass, starwars$height), ]
```

    ## # A tibble: 87 x 13
    ##    name     height  mass hair_color skin_color eye_color birth_year gender
    ##    <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ##  1 Ratts T…     79  15.0 none       grey, blue unknown        NA    male  
    ##  2 Yoda         66  17.0 white      green      brown         896    male  
    ##  3 Wicket …     88  20.0 brown      brown      brown           8.00 male  
    ##  4 R2-D2        96  32.0 <NA>       white, bl… red            33.0  <NA>  
    ##  5 R5-D4        97  32.0 <NA>       white, red red            NA    <NA>  
    ##  6 Sebulba     112  40.0 none       grey, red  orange         NA    male  
    ##  7 Dud Bolt     94  45.0 none       blue, grey yellow         NA    male  
    ##  8 Padmé A…    165  45.0 brown      light      brown          46.0  female
    ##  9 Sly Moo…    178  48.0 none       pale       white          NA    female
    ## 10 Wat Tam…    193  48.0 none       green, gr… unknown        NA    male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

## Why dplyr?

  - First argument is always a data frame / tibble, **amenable to
    piping**.
  - Other arguments are evaluated in the context of the data, **saves
    typing**.
  - Function names describe the action, **code is easier to read**.

<!-- end list -->

``` r
starwars %>%
  filter(height < 160) %>%
  select(name, height, mass) %>%
  arrange(mass)
```

    ## # A tibble: 13 x 3
    ##    name                  height  mass
    ##    <chr>                  <int> <dbl>
    ##  1 Ratts Tyerell             79  15.0
    ##  2 Yoda                      66  17.0
    ##  3 Wicket Systri Warrick     88  20.0
    ##  4 R2-D2                     96  32.0
    ##  5 R5-D4                     97  32.0
    ##  6 Sebulba                  112  40.0
    ##  7 Dud Bolt                  94  45.0
    ##  8 Leia Organa              150  49.0
    ##  9 Mon Mothma               150  NA  
    ## 10 Watto                    137  NA  
    ## 11 Gasgano                  122  NA  
    ## 12 Cordé                    157  NA  
    ## 13 R4-P17                    96  NA

## Quoted versus evaluated arguments

`order()` uses **standard evaluation**, its arguments are **evaluated**.

That is, we can run the argument alone, it works, and it is all
`order()` needs to do its
    job:

``` r
starwars$mass
```

    ##  [1]   77.0   75.0   32.0  136.0   49.0  120.0   75.0   32.0   84.0   77.0
    ## [11]   84.0     NA  112.0   80.0   74.0 1358.0   77.0  110.0   17.0   75.0
    ## [21]   78.2  140.0  113.0   79.0   79.0   83.0     NA     NA   20.0   68.0
    ## [31]   89.0   90.0     NA   66.0   82.0     NA     NA     NA   40.0     NA
    ## [41]     NA   80.0     NA   55.0   45.0     NA   65.0   84.0   82.0   87.0
    ## [51]     NA   50.0     NA     NA   80.0     NA   85.0     NA     NA   80.0
    ## [61]   56.2   50.0     NA   80.0     NA   79.0   55.0  102.0   88.0     NA
    ## [71]     NA   15.0     NA   48.0     NA   57.0  159.0  136.0   79.0   48.0
    ## [81]   80.0     NA     NA     NA     NA     NA   45.0

`arrange()` uses **non-standard evaluation**, some of its arguments are
**quoted**. We can’t just run the argument value alone, it doesn’t work:

``` r
mass
```

    ## Error in eval(expr, envir, enclos): object 'mass' not found

## Why does dplyr use quotation?

dplyr uses quotation:

  - to save you typing\!
  - so dbplyr can translate to SQL

When you work interactively, this is great\!

But, sometimes it causes problems…

# 2\. Using a dplyr function with a saved variable

## Using `arrange()` with a saved variable

``` r
arrange(starwars, desc(mass))
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Darth …    202   136 none        white      yellow          41.9 male  
    ##  5 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

What if if the column to order by is stored in a variable?

This doesn’t
    work:

``` r
col <- mass
```

    ## Error in eval(expr, envir, enclos): object 'mass' not found

``` r
arrange(starwars, col)
```

    ## Error in arrange_impl(.data, dots): cannot arrange column of class 'function' at position 1

Neither does this:

``` r
col <- "mass"
arrange(starwars, col)
```

    ## Error in arrange_impl(.data, dots): incorrect size (1) at position 1, expecting : 87

## A similar problem with `$`

`$` can be used to pull out a column from a data
    frame:

``` r
starwars$mass
```

    ##  [1]   77.0   75.0   32.0  136.0   49.0  120.0   75.0   32.0   84.0   77.0
    ## [11]   84.0     NA  112.0   80.0   74.0 1358.0   77.0  110.0   17.0   75.0
    ## [21]   78.2  140.0  113.0   79.0   79.0   83.0     NA     NA   20.0   68.0
    ## [31]   89.0   90.0     NA   66.0   82.0     NA     NA     NA   40.0     NA
    ## [41]     NA   80.0     NA   55.0   45.0     NA   65.0   84.0   82.0   87.0
    ## [51]     NA   50.0     NA     NA   80.0     NA   85.0     NA     NA   80.0
    ## [61]   56.2   50.0     NA   80.0     NA   79.0   55.0  102.0   88.0     NA
    ## [71]     NA   15.0     NA   48.0     NA   57.0  159.0  136.0   79.0   48.0
    ## [81]   80.0     NA     NA     NA     NA     NA   45.0

Even though you can’t just ask for `mass`:

``` r
mass
```

    ## Error in eval(expr, envir, enclos): object 'mass' not found

So, `$` quotes its argument too.

But, what if the column I want is stored in a string?

``` r
col <- "mass"
```

This doesn’t work:

``` r
starwars$col
```

    ## Warning: Unknown or uninitialised column: 'col'.

    ## NULL

What to do? Use a function that doesn’t use quote its argument:

``` r
starwars[, col]
```

    ## # A tibble: 87 x 1
    ##     mass
    ##    <dbl>
    ##  1  77.0
    ##  2  75.0
    ##  3  32.0
    ##  4 136  
    ##  5  49.0
    ##  6 120  
    ##  7  75.0
    ##  8  32.0
    ##  9  84.0
    ## 10  77.0
    ## # ... with 77 more rows

This is an example where you don’t want the function to quote what you
give it. The solution in this case is to use a different function, one
that doesn’t quote its arguments.

The tidyverse implements a different solution: tidy evaluation.

## Tidy evaluation

An opinion on how **non-standard evaluation** should be implemented, and
a set of tools for implementing it: the rlang package.

The way non-standard evaluation *is* (will be) implemented in the
tidyverse.

Two audiences:

1.  For users of the tidyverse: tidy evaluation provides a way to
    selectively evaluate things if needed

2.  For developers of tidy tools: tidy evaluation provides a framework
    for implementing your own non-standard evaluation tools, so your
    users get the benefits of \#1.

We’ll focus on the first case. We need two pieces: a way to quote
things, and a way to selectively unquote things.

## \#1: Quoting with rlang

Quoting means: to capture the intent of the code, not the result of
evaluating it.

How do I capture the **intent** of this code: `x + y + z`

If I just run it in R,

``` r
x + y + z
```

    ## Error in eval(expr, envir, enclos): object 'x' not found

R tries to evaluate it, and I get an error, because some of the objects
involved don’t exist.

I could put quotes around it, to capture it without evaluating it:

``` r
"x + y + z"
```

    ## [1] "x + y + z"

But this doesn’t convey that this is in fact R code.

In rlang, one way is to use `expr()`:

``` r
expr(x + y + z)
```

    ## x + y + z

It returns a quoted expression.

(Base R has tools for quoting too, but rlang’s implementation is more
consistent)

## \#2: Unquoting with `!!` (bang-bang)

**Inside** an rlang quoting function (e.g. `expr()`, and dplyr’s quoted
arguments), you can unquote something with the `!!` operator.

This quotes the expression `mass`:

``` r
my_var <- expr(mass)
```

This quotes the expression `my_var`:

``` r
expr(my_var)
```

    ## my_var

This unquotes `my_var` before quoting the result:

``` r
expr(!!my_var)
```

    ## mass

## Practice

``` r
x <- expr(z)
y <- expr(x + y)
```

Can you guess what expression each of these will return?

``` r
expr(x + y)
expr(!!x + y)
expr(x + !!y)
expr(!!x + !!y)
```

## Back to our problem

This didn’t
    work:

``` r
col <- mass
```

    ## Error in eval(expr, envir, enclos): object 'mass' not found

``` r
arrange(starwars, col)
```

    ## Error in arrange_impl(.data, dots): incorrect size (1) at position 1, expecting : 87

## A Solution

First we want to quote `height`, not evaluate it:

``` r
var <- expr(mass)
```

Then we can unquote `var` when we pass it to `arrange()`:

``` r
arrange(starwars, !!var)
```

    ## # A tibble: 87 x 13
    ##    name     height  mass hair_color skin_color eye_color birth_year gender
    ##    <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ##  1 Ratts T…     79  15.0 none       grey, blue unknown        NA    male  
    ##  2 Yoda         66  17.0 white      green      brown         896    male  
    ##  3 Wicket …     88  20.0 brown      brown      brown           8.00 male  
    ##  4 R2-D2        96  32.0 <NA>       white, bl… red            33.0  <NA>  
    ##  5 R5-D4        97  32.0 <NA>       white, red red            NA    <NA>  
    ##  6 Sebulba     112  40.0 none       grey, red  orange         NA    male  
    ##  7 Dud Bolt     94  45.0 none       blue, grey yellow         NA    male  
    ##  8 Padmé A…    165  45.0 brown      light      brown          46.0  female
    ##  9 Wat Tam…    193  48.0 none       green, gr… unknown        NA    male  
    ## 10 Sly Moo…    178  48.0 none       pale       white          NA    female
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

You can selectively unquote things, so to get reverse ordering:

``` r
arrange(starwars, desc(!!var))
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Darth …    202   136 none        white      yellow          41.9 male  
    ##  5 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

# 3\. Using a dplyr function inside a function

## Now, we might want to turn this into a function

``` r
var <- expr(height)
arrange(starwars, desc(!!var))
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color skin_color  eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>      <chr>       <chr>          <dbl> <chr> 
    ##  1 Yarael…    264  NA   none       white       yellow          NA   male  
    ##  2 Tarfful    234 136   brown      brown       blue            NA   male  
    ##  3 Lama Su    229  88.0 none       grey        black           NA   male  
    ##  4 Chewba…    228 112   brown      unknown     blue           200   male  
    ##  5 Roos T…    224  82.0 none       grey        orange          NA   male  
    ##  6 Grievo…    216 159   none       brown, whi… green, y…       NA   male  
    ##  7 Taun We    213  NA   none       grey        black           NA   female
    ##  8 Rugor …    206  NA   none       green       orange          NA   male  
    ##  9 Tion M…    206  80.0 none       grey        black           NA   male  
    ## 10 Darth …    202 136   none       white       yellow          41.9 male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

One attempt:

``` r
arrange_desc <- function(.data, var){
  arrange(.data, desc(!!var))
}

arrange_desc(starwars, expr(mass))
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Darth …    202   136 none        white      yellow          41.9 male  
    ##  5 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

But it would be nicer if we could just say `arrange_desc(starwars,
mass)`

## Another attempt

Why doesn’t this work?

``` r
arrange_desc <- function(.data, var){
  var <- expr(var)
  arrange(.data, desc(!!var))
}
arrange_desc(starwars, mass)
```

    ## Error in arrange_impl(.data, dots): cannot arrange column of class 'name' at position 1

``` r
debugonce(arrange_desc)
arrange_desc(starwars, mass)
```

`expr(var)` returns the expression `var`, it gets quoted. In rlang this
is solved by using `enexpr()`
instead.

## Use `enexpr()` instead of `expr()` to capture arguments inside of functions

And finally it works

``` r
arrange_desc <- function(.data, var){
  var <- enexpr(var)
  arrange(.data, desc(!!var))
}
arrange_desc(starwars, mass)
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Darth …    202   136 none        white      yellow          41.9 male  
    ##  5 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

But it only accepts one argument…

## A multiple argument version

New things:

  - `exprs()` to capture multiple expressions in a list
  - `purrr::map()` to operate of the list of expressions
  - `!!!` to “splice” unquote: each element is inserted as an argument

<!-- end list -->

``` r
arrange_desc2 <- function(.data, ...){
  vars <- enexprs(...)
  vars_desc <- map(vars, function(var) expr(desc(!!var)))
  arrange(.data, !!!vars_desc)
}
arrange_desc2(starwars, mass, height)
```

    ## # A tibble: 87 x 13
    ##    name    height  mass hair_color  skin_color eye_color birth_year gender
    ##    <chr>    <int> <dbl> <chr>       <chr>      <chr>          <dbl> <chr> 
    ##  1 Jabba …    175  1358 <NA>        green-tan… orange         600   herma…
    ##  2 Grievo…    216   159 none        brown, wh… green, y…       NA   male  
    ##  3 IG-88      200   140 none        metal      red             15.0 none  
    ##  4 Tarfful    234   136 brown       brown      blue            NA   male  
    ##  5 Darth …    202   136 none        white      yellow          41.9 male  
    ##  6 Owen L…    178   120 brown, grey light      blue            52.0 male  
    ##  7 Bossk      190   113 none        green      red             53.0 male  
    ##  8 Chewba…    228   112 brown       unknown    blue           200   male  
    ##  9 Jek To…    180   110 brown       fair       blue            NA   male  
    ## 10 Dexter…    198   102 none        brown      yellow          NA   male  
    ## # ... with 77 more rows, and 5 more variables: homeworld <chr>,
    ## #   species <chr>, films <list>, vehicles <list>, starships <list>

## Quosures

In reality you’ll see `quo()`, `enquo()`, `quos()` and `enquos()`,
instead of `expr()`, `enexpr()`, `exprs()` and `enexprs()`.

These capture both the code and the **environment**.

# Resources

If dplyr is new to you, start by learning to use it interactively with
the [Data Transformation chapter in R for Data
Science](http://r4ds.had.co.nz/transform.html).

If you have never written your own function before, start with
[Functions in R for Data Science](http://r4ds.had.co.nz/functions.html)
or [DataCamp’s Writing Functions in
R](https://www.datacamp.com/courses/writing-functions-in-r).

If you’ve written functions but want to formalise your knowledge,
[Functions in Advanced R](http://adv-r.had.co.nz/Functions.html).

If you want to see how to program with dplyr functions, the [Programming
with dplyr
vignette](https://dplyr.tidyverse.org/articles/programming.html).

If the vignette seems a little unapproachable, find another resource in
[Mara Averick’s roundup of tidy eval
resources](https://maraaverick.rbind.io/2017/08/tidyeval-resource-roundup/).

If you’d prefer to watch than read, try [RStudio’s tidy evaluation
webinar](https://www.rstudio.com/resources/webinars/tidy-eval/), or
[Hadley’s tidy evaluation in 5
minutes](https://www.youtube.com/watch?v=nERXS3ssntw).

If you are interested in the underpinnings, and possibly using tidy
evaluation ideas in your own packages, look at [Metaprogramming in the
**new** (in progress) edition of Advanced
R](https://adv-r.hadley.nz/meta.html).
