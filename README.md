
<!-- README.md is generated from README.Rmd. Please edit that file -->
nofrills
========

*Everyday low-cost anonymous functions*

[![Travis-CI Build Status](https://travis-ci.org/egnha/nofrills.svg?branch=master)](https://travis-ci.org/egnha/nofrills) [![codecov](https://codecov.io/gh/egnha/nofrills/branch/master/graph/badge.svg)](https://codecov.io/gh/egnha/nofrills)

Overview
--------

*nofrills* is a tiny R package that provides a function `fn()`, which enables you to create (anonymous) functions, of *arbitrary* call signature. It’s a lower cost, drop-in replacement for the usual `function(<arguments>) <body>` invocation, in that:

-   it is **shorter**:

    ``` r
    fn(x, y = 1 ~ x + y)

    ..(x, y = 1 ~ x + y)
    ```

    are both equivalent to

    ``` r
    function(x, y = 1) x + y
    ```

-   it is **safer**: by enabling [quasiquotation](http://rlang.tidyverse.org/reference/quasiquotation.html), `fn()` allows you to “burn in” values, which guards your function from being affected by unexpected scope change

Installation
------------

``` r
# install.packages("devtools")
devtools::install_github("egnha/nofrills")
```

Usage
-----

For details and more examples, see the package documentation (`?fn`).

### Same syntax as `function()`, just shorter

``` r
fn(x ~ x + 1)
#> function (x) 
#> x + 1
fn(x, y ~ x + y)
#> function (x, y) 
#> x + y
fn(x, y = 2 ~ x + y)
#> function (x, y = 2) 
#> x + y
fn(x, y = 1, ... ~ log(x + y, ...))
#> function (x, y = 1, ...) 
#> log(x + y, ...)
# to specify `...` in the middle, write `... = `.
fn(x, ... = , y ~ log(x + y, ...))
#> function (x, ..., y) 
#> log(x + y, ...)
fn(~ NA)
#> function () 
#> NA
```

### Supports quasiquotation

#### Unquoting values

``` r
z <- 0
fn(x, y = !! z ~ x + y)
#> function (x, y = 0) 
#> x + y
fn(x ~ x > !! z)
#> function (x) 
#> x > 0
```

#### Unquoting argument names

``` r
arg <- "y"
fn(x, !! arg := 0 ~ x + !! as.name(arg))
#> function (x, y = 0) 
#> x + y
```

#### Splicing in argument lists

``` r
args <- alist(x, y = 0)
fn(!!! args, ~ x + y)
#> function (x, y = 0) 
#> x + y
```

(Note that the function body here is a one-sided formula.)

#### Protect functions against scope changes

``` r
x <- "x"
f <- function() x
f_solid <- fn(~ !! x)
```

Both return the same value of x

``` r
f()
#> [1] "x"
f_solid()
#> [1] "x"
```

But if the binding `x` is (unwittingly) changed, `f()` changes, while `f_solid()` remains unaffected.

``` r
x <- sin
f()
#> function (x)  .Primitive("sin")
f_solid()
#> [1] "x"
```

### Smiley functions

A riddle: Both of these smileys produce functions.

``` r
..(~8^D)
..(8~D)
```

But which one is actually callable?

Alternatives
------------

The following packages provide alternative anonymous-function constructors. Unlike `fn()`, they are specialized rather than general, so they can afford to be more concise.

-   [lambda](https://github.com/jimhester/lambda) provides `f()`. It uses a `bquote()`-like notation for function declaration, which, by forgoing explicit call signature specification, is very compact. Quasiquotation is not supported, because that wasn't available when lambda was developed.

-   [rlang](https://github.com/tidyverse/rlang) provides [`as_function()`](http://rlang.tidyverse.org/reference/as_function.html), which allows you to create anonymous functions of up to two arguments.

License
-------

MIT Copyright © 2017 [Eugene Ha](https://github.com/egnha)
