<!--
.. title: R: Subsetting
.. slug: r-subsetting
.. date: 2018-08-04 00:35:36 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Basics
------

-   **\[ \]** returns same class as original, can select multiple
-   **\[\[ \]\]** extracts elements from list or data frame
-   **$** extracts elements from list or data frame by name
-   Analogy - If list x is a train carrying objects:
    -   x\[4:6\] is a train of cars 4-6
    -   x\[\[5\]\] is the object in car 5

<!-- -->

    x <- c('a', 'b', 'c', 'd', 'e')
    x[1]        # a
    x[2]        # b
    x[1:3]      # a b c
    x[x > 'b']  # c d e

    mask <- x > 'b'  # FALSE FALSE  TRUE  TRUE  TRUE
    x[mask]     # c d e

Lists
-----

    x <- list(a = 1:3, b = 'a', c = TRUE)

    x[1]       # List 1 2 3
    x['a']     # Same as above

    x[c(1,3)]  # List with first and third elements

    x[[1]]     # Vector 1 2 3
    x[['a']]   # Same as above
    x$a        # Same as above

    name <- 'b'
    x[[name]]  # Vector 'a', same as x[['b']]
    x$name     # NULL - $ only works with literal names


    # Nested lists
    y <- list(a = list(2,4,6), b = list(3,5,7))

    y[[1]][[3]]   # 6 - list 1, element 3
    y[[2]][[2]]   # 5 - list 2, element 2
    y[[c(2, 2)]]  # 5 - same as above

Matrix
------

    x <- matrix(1:6, 2, 3)

    x[1, 2]  # 3 - row 1, col 2
    x[2, 1]  # 2 - row 2, col 1
    x[2, 1, drop = FALSE]  # Don't drop dimension to vector, keep as 1x1 matrix

    x[1, ]   # 1 3 5 - row 1, all cols
    x[, 2]   # 3 4 - all rows, col 2
    x[, 2, drop = FALSE]   # Don't drop dimension to vector, keep as 2x1 matrix

Partial Matching
----------------

    x <- list(long_col_name = 1:5)

    x$long  # 1 2 3 4 5, successfully matched on first part of name
    x[['long', exact = FALSE]]  # Same as above

Remove NAs
----------

    x <- c(1, 2, NA, NA, 5)

    miss <- is.na(x)  # FALSE FALSE  TRUE  TRUE FALSE
    x[!miss]  # 1 2 5, choose all non-missings


    # Complete cases - No missings across all values
    y <- c('a', NA, NA, 'd', 'e')

    comp <- complete.cases(x, y)
    x[comp]  # 1, 5
    y[comp]  # a, e

    airquality[1:9, ]  # Rows 5, 6 have NAs
    comp <- complete.cases(airquality)
    airquality[comp, ][1:9, ]  # Rows 5, 6 dropped
