<!--
.. title: R: Scoping Rules
.. slug: r-scoping-rules
.. date: 2018-08-04 00:34:56 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Namespaces
----------

R searches for a symbol in the following order:

1.  Search the global environment
    -   User's workspace, always searched first
2.  Search the namespaces of each package on the search list
    -   Ordering matters
    -   Most recently loaded package goes in position 2 and everything
        else shifts down
    -   Base package is always last

Separate namespaces for functions and non-functions, e.g. can have both
'x' function and 'x' object.

    # Search list
    search()

    ##  [1] ".GlobalEnv"        "package:rmarkdown" "tools:rstudio"    
    ##  [4] "package:stats"     "package:graphics"  "package:grDevices"
    ##  [7] "package:utils"     "package:datasets"  "package:methods"  
    ## [10] "Autoloads"         "package:base"

Lexical Scoping
---------------

-   R uses *Lexical Scoping* or *Static Scoping*
    -   As opposed to *Dynamic Scoping*
-   Determines how values are assigned to *free variables*
-   Values of free variables are searched for in the environment in
    which the function as defined
    -   If value not found, search is continued in the *parent
        environment*
    -   Continues upwards until *top-level environment*, usually global
        environment or package namespace
    -   Then continues downwards until *empty environment*
    -   If still not found, throw error

<!-- -->

    # Function below has formal arguments x/y, and free variable z
    f <- function(x, y) {
      x + y - z
    }


    # Example: Function generator - Value of free variable n taken from function definition environment
    make_power_fn <- function(n) {
      pow <- function(x) {
        x^n
      }
      pow
    }

    cube_fn <- make_power_fn(3)
    cube_fn(3)                      # 27
    square_fn <- make_power_fn(2)
    square_fn(3)                    # 9

    # Explore function environment
    ls(environment(cube_fn))        # "n"   "pow"
    get('n', environment(cube_fn))  # 3
    ls(environment(square_fn))      # "n"   "pow"
    get('n', environment(square_fn))  # 2


    # Example: Lexical (value from definition env) vs Dynamic Scoping (value from calling env)
    y <- 10

    f <- function(x) {
      y <- 2
      y^2 + g(x)
    }

    g <- function(x) {
      x*y
    }

    f(3)  # 34 because y=10 in environment where g() is defined, so (2^2 + 3*10)
