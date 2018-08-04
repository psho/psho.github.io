<!--
.. title: R: Functions
.. slug: r-functions
.. date: 2018-08-04 00:34:23 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Define
------

-   R objects of class "function"
-   R functions are **first class objects**
-   Can be passed as arguments to other functions
-   Can be nested, define function inside another
-   Return value is last expression to be evaluated
    -   Use return() to return early

<!-- -->

    # Named arguments with some default values
    f <- function(a, b = 1, c = TRUE, d = NULL) {
      print(a)
      print(b)

      if (d == 3) {
        return(d)  # Can return early
      }

      c  # Last expression, so this is the return value
    }

    # Print arguments
    args(f)

    # List of formal arguments
    formals(f)

Call
----

-   Arguments can be matched by position or name
-   Arguments are *lazy* evaluated, only evaluated when needed

<!-- -->

    # These function calls are the same
    f(1, 2, FALSE, 'a')
    f(b = 2, d = 'a', c = FALSE, a = 1)

'...' Arguments
---------------

-   Use when extending another function, no need to copy entire argument
    list of original function
-   Also useful when number of arguments to be passed is unknown

<!-- -->

    new_plot <- function(x, y, type = "l", ...) {
      plot(x, y, type = type, ...)
    }

    args(paste)  # function (..., sep = " ", collapse = NULL)
