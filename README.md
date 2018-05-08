# Quotes, Quotation and Quasiquotation

A talk given at the [Eugene R Users](https://www.meetup.com/meetup-group-cwPiAlnB/) Meetup, May 8 2018.

## Original Outline

I'll start with a common question for new R users: When do I put quotes (`"`) around something in R? I'll give you a few strategies for identifying whether you need *quotes*, backticks, or nothing at all.  

Then we'll transition to talking about *quotation*, which by the way, doesn't mean surrounding something in quotes.  What does it mean?  You'll learn what quotation is, and why developers use it to make your life easier.

Finally, I'll introduce the idea of unquoting and help you understand what it means when a function says it supports *quasiquotation*. This will allow you to to use functions like `dplyr::filter()` inside your own functions.

## Package Requirements

I'll aim to have the talk be interactive without you having a laptop with you, but if you would like to follow along in R, make sure you have the tidyverse packages, rlang and lobstr installed:

```
install.packages(c("tidyverse", "rlang"))
# install.packages("devtools")
devtools::install_github("r-lib/lobstr")
```

## What actually happened

We worked through three examples:

1. Using a dplyr function interactively
2. Using a dplyr function with a saved variable
3. Using a dplyr function inside a function

See the [annotated output](three-examples-annotated.md) or [source](three-examples-annotated.Rmd), or motivating [slides](three-examples-slides.pdf).

This meant I omitted the "Quotes" part of the talk, but you can look at my [notes](quotes.md).

## Resources

If dplyr is new to you, start by learning to use it interactively with the [Data Transformation chapter in R for Data Science](http://r4ds.had.co.nz/transform.html).

If you have never written your own function before, start with [Functions in R for Data Science](http://r4ds.had.co.nz/functions.html) or [DataCamp's Writing Functions in R](https://www.datacamp.com/courses/writing-functions-in-r).

If you've written functions but want to formalise your knowledge, [Functions in Advanced R](http://adv-r.had.co.nz/Functions.html).

If you want to see how to program with dplyr functions, the [Programming with dplyr vignette](https://dplyr.tidyverse.org/articles/programming.html).

If the vignette seems a little unapproachable, find another resource in [Mara Averick's roundup of tidy eval resources](https://maraaverick.rbind.io/2017/08/tidyeval-resource-roundup/).

If you'd prefer to watch than read, the [RStudio's tidy evaluation webinar](https://www.rstudio.com/resources/webinars/tidy-eval/), or [Hadley's tidy evaluation in 5 minutes](https://www.youtube.com/watch?v=nERXS3ssntw).

If you are interested in the underpinnings, and possibly using tidy evaluation ideas in your own packages, look at [Metaprogramming in the **new** (in progress) edition of Advanced R](https://adv-r.hadley.nz/meta.html).

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Quotes, Quotation and Quasiquotation</span> by <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName">Charlotte Wickham</span> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.