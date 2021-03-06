<!--
.. title: R: Data Types
.. slug: r-data-types
.. date: 2018-08-04 00:14:50 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Objects
-------

-   Five atomic classes of objects: **Numeric, Integer, Character,
    Logical, Complex**
-   Numbers are treated as double precision by default
    -   **Inf** is infinity, e.g. 1/0
    -   **NaN** is not a number (undefined), e.g. 0/0
-   Objects can have **attributes**
    -   names, dimensions, class, length
    -   user-defined metadata

Vectors
-------

-   All elements must be same type

<!-- -->

    # Create
    x <- c(1, 5.5, 2.7, 3)     # Numeric - Defaults to double
    x <- c(3L, 8L, 22L, 2L)    # Integer - Use L suffix
    x <- 1:4                   # Integer - Range defaults to integer
    x <- c(TRUE, FALSE, T, F)  # Logical
    x <- c('a', 'b', 'c')      # Character
    x <- c(1+0i, 2+4i)         # Complex
    x <- vector('numeric', length = 5)  # Numeric - Default values of 0

    # Inspect
    typeof(x)  # E.g. "character"
    is.integer(x)
    is.double(x)
    is.numeric(x)  # TRUE if either integer or numeric
    is.logical(x)
    is.character(x)

    # Check missing
    x <- c(1, 2, NaN, NA, 4)
    is.na(x)   # FALSE FALSE  TRUE  TRUE FALSE - NaN is also NA
    is.nan(x)  # FALSE FALSE  TRUE FALSE FALSE

    # Coercion
    x <- 0:3
    as.numeric(x)    # 0 1 2 3
    as.character(x)  # "0" "1" "2" "3"
    as.logical(x)    # FALSE TRUE TRUE TRUE - FALSE if value is 0, TRUE otherwise

    x <- c('abc', 123)  # c('abc', '123') - coerced so all elements are same type

    x <- c('a','b')
    as.numeric(x)    # NA NA - NAs introduced by coercion due to invalid conversion

    # Vectorized operations
    x <- 1:3; y <- 4:6
    x + y   # 5 7 9
    x >= 2  # F T T
    x == 3  # F F T
    x * y   # 4 10 18
    x / y   # 0.25 0.40 0.50

    # Summarise
    x <- c(1, 2, 3)
    length(x)  # 3
    sum(x)     # 6
    mean(x)    # 2

Matrices and Arrays
-------------------

-   Vectors with a dimension attribute will become multi-dimensional
    *array*
-   An array with 2 dimensions is a *matrix*
-   Can also apply dimensions to a list

<!-- -->

    # Create from vector
    x <- 1:6
    dim(x) <- c(2,3)  # Turn vector into 2x3 matrix - Column-wise by default "top-left then down"
    matrix(1:6, nrow = 2, ncol = 3)  # Same result
    matrix(1:6, c(2,3))  # Same result

    # Create using cbind/rbind
    x <- 1:3
    y <- 4:6
    cbind(x, y)  # 3x2 matrix - bind vectors as columns
    rbind(x, y)  # 2x3 matrix - bind vectors as rows

    # Inspect
    ncol(x)  # Number of columns
    nrow(x)  # Number of rows

    # Manipulate
    rownames(x) <- c('r1', 'r2')  # Set matrix row names
    colnames(x) <- c('c1', 'c2', 'c3')  # Set matrix col names
    t(x)  # Transpose matrix

    # Vectorized operations
    x <- matrix(1:4, 2, 2); y <- matrix(5:8, 2, 2)
    x * y    # element-wise multiplication
    x / y    # element-wise division
    x %*% y  # matrix multiplication

    # Array
    y <- array(1:12, c(2,2,3))  # 3-dimensional array
    dimnames(y) <- list(c('a1', 'a2'), c('b1', 'b2'), c('c1', 'c2', 'c3'))  # Set array dimension names

Factors
-------

-   Vectors that can only contain predefined values (levels)
-   Use for categorical data

<!-- -->

    # Create
    x <- factor(c('m', 'f', 'f', 'm', 'm'))

    # Inspect
    is.factor(x)
    levels(x)   # "f" "m" are the only valid values
    table(x)    # 2 f, 3 m

    # Define levels
    x[1] <- 'a'  # Invalid factor level, NA generated
    x <- factor(x, levels = c('m', 'f', 'a'))  # Define levels, m=1 f=2 a=3
    x[1] <- 'a'  # Will now work as 'a' is a valid factor level

Lists
-----

-   Elements can be any type, even nested lists

<!-- -->

    # Create
    x <- list(c(5.5, 2.7), 3L:8L, c(T, F, T), c('abc', 'def'), list(1, 'a', F))

    # Inspect
    is.list(x)

    # Coercion
    as.list(c(1, 2, 3))    # list(1, 2, 3)
    unlist(list(1, 2, 3))  # c(1, 2, 3), coerce to atomic vector

    # Manipulate
    names(x) <- c('num', 'int', 'logi', 'char', 'list')

Data Frames
-----------

-   List of equal length vectors
-   Shares same properties as matrix and list

<!-- -->

    # Create
    df <- data.frame(col1 = 1:3, col2 = c('a', 'b', 'c'))
    df <- data.frame(col1 = 1:3,col2 = c('a', 'b', 'c'),
                     stringsAsFactors = FALSE)  # Leave col2 as chr

    # Inspect
    is.data.frame(df)
    ncol(x)  # Number of columns
    nrow(x)  # Number of rows

    # Manipulate
    cbind(df, data.frame(col3 = c(T, F, T)))  # Add column
    rbind(df, data.frame(col1 = 4, col2 = 'd'))  # Add row
    names(df) <- c('col_a', 'col_b')   # Set names of columns

str
---

-   Display internal structure of an R object in a compact way

<!-- -->

    # Vectors
    x <- runif(100)
    summary(x)  # Quartiles
    str(x)  # num [1:100] 0.3257 0.3514 0.0628 0.2261 0.0838 ...

    # Matrix
    x <- matrix(1:100, 10, 10)
    x[, 1]  # [1]  1  2  3  4  5  6  7  8  9 10
    str(x)  # int [1:10, 1:10] 1 2 3 4 5 6 7 8 9 10 ...

    # Factors
    x <- gl(3, 5)
    summary(x)  # Frequency counts
    str(x)  # Factor w/ 3 levels "1","2","3": 1 1 1 1 1 2 2 2 2 2 ...

    # Lists
    x <- list(c(5.5, 2.7), list(1, 'a', F))
    str(x)
    # List of 2
    #  $ : num [1:2] 5.5 2.7
    #  $ :List of 3
    #   ..$ : num 1
    #   ..$ : chr "a"
    #   ..$ : logi FALSE

    # Data Frames
    str(airquality)
    # 'data.frame': 153 obs. of  6 variables:
    #  $ Ozone  : int  41 36 12 18 NA 28 23 19 8 NA ...
    #  $ Solar.R: int  190 118 149 313 NA NA 299 99 19 194 ...
