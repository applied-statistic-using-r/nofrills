# Resubmission (23-07-2017)

This is a resubmission to fix the following issue: the package description
requires elaboration.

The description has been changed from

> Provides a compact way to declare functions using the syntax of
> 'function()', with support for Tidyverse-style quasiquotation.

to

> Provides a compact variation of the usual syntax of function
> declaration, in order to support Tidyverse-style quasiquotation of a
> function's arguments and body.

## Test environments

* OS X 10.11.6 (local): R 3.4.1
* Ubuntu 14.04.5 LTS (on Travis CI): R 3.3.3, 3.4.0, devel (2017-07-21 r72947)
* Windows (on win-builder): R 3.3.3, 3.4.1, devel (2017-07-20 r72935)

## R CMD check results

There were no ERRORs or WARNINGSs.

There was one NOTE:

>  * checking CRAN incoming feasibility ... NOTE
>  Maintainer: 'Eugene Ha <eha@posteo.de>'
>  
>  New submission
>  
>  License components with restrictions and base license permitting such:
>    MIT + file LICENSE
>  File 'LICENSE':
>    YEAR: 2017
>    COPYRIGHT HOLDER: Eugene Ha
>  
>  Possibly mis-spelled words in DESCRIPTION:
>    Tidyverse (7:34)
>    quasiquotation (7:50)

The words "Tidyverse" and "quasiquotation" are correctly spelt.

## Downstream dependencies

None (first submission).
