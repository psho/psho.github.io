<!--
.. title: R: Apply
.. slug: r-apply
.. date: 2018-08-04 00:16:04 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

lapply
------

-   Apply function over each element in list
-   Always returns a list

<!-- -->

    # Summarise each list element
    x <- list(a = 1:5, b = 6:10)
    lapply(x, mean)

    # Generate list of vectors
    x <- 1:3
    lapply(x, seq)

    # Pass in function arguments
    x <- 1:3
    lapply(x, runif, min=0, max=10)

    # Anonymous function to get first column of each data frame
    x <- list(a = data.frame(col1 = 1:3, col2 = c('a', 'b', 'c')),
              b = data.frame(col1 = 4:6, col2 = c('d', 'e', 'f')))
    lapply(x, function(elem) elem[,1])  # 1,2,3 and 4,5,6

sapply
------

-   Apply function over each element in list and simplify result
-   Simplifies to a vector or matrix if possible, otherwise returns list
    as usual

<!-- -->

    # Summarise each list element, will return a vector
    x <- list(a = 1:5, b = 6:10)
    sapply(x, mean)

apply
-----

-   Apply function over margins of an array
-   Commonly used over rows/cols of a matrix

<!-- -->

    # Sum rows and cols
    x <- matrix(1:30, 5, 6)
    apply(x, 1, sum)  # Rows - 81  87  93  99 105
    apply(x, 2, sum)  # Cols - 15  40  65  90 115 140

    # Shortcut functions - Faster than apply()
    rowSums(x)
    colSums(x)
    rowMeans(x)
    colMeans(x)

    # Quantiles
    apply(x, 1, quantile, probs = c(0.25, 0.75))

    # Multidimensional array
    x <- array(1:30, c(2,3,5))
    apply(x, c(1,2), sum)  # Over rows and cols
    rowSums(x, dims=2)  # Same as above

mapply
------

-   Apply function using multiple lists/vectors as arguments
-   Vectorize arguments to a function that doesn't usually accept
    vectors as arguments

<!-- -->

    list(rep(1,4),
         rep(2,3),
         rep(3,2),
         rep(4,1))  # Tedious to type out
    mapply(rep, 1:4, 4:1)  # Same result as above


    rnorm(5, 1, 2)  # Generates 5 random normal values with mean=1 sd=2
    rnorm(1:5, 1, 2)  # Still only generates 5 values despite passing in vector for n
    mapply(rnorm, 1:5, 1, 2)  # Now correctly generates multiple vectors of random values
    list(rnorm(1, 1, 2),
         rnorm(2, 1, 2),
         rnorm(3, 1, 2),
         rnorm(4, 1, 2),
         rnorm(5, 1, 2))  # Equivalent to this

tapply
------

-   Apply function over subsets of a vector
-   Will simplify by default

<!-- -->

    # Summarise over groups
    x <- c(1, 2, 3, 4, 5, 6, 7, 8, 9)
    g <- c(1, 1, 1, 2, 2, 2, 3, 3, 3)  # gl(3,3)
    tapply(x, g, sum)  # Vector 6 15 24
    tapply(x, g, sum, simplify = FALSE)  # List 6 15 24
    tapply(x, g, mean)   # 2 5 8
    tapply(x, g, range)  # 1 3,  4 6,  7 9

split
-----

-   Split object into list of sub-objects
-   Useful in conjuction with lapply()

<!-- -->

    # Split vector into groups
    x <- c(1, 2, 3, 4, 5, 6, 7, 8, 9)
    g <- c(1, 1, 1, 2, 2, 2, 3, 3, 3)
    split(x, g)  # 1 2 3,  4 5 6,  7 8 9

    # Use with lapply()
    lapply(split(x, g), sum)  # 6, 15, 24


    # Split on multiple levels and drop any empty levels
    g1 <- c(1, 1, 1, 2, 2, 2, 3, 3, 3)
    g2 <- c(1, 1, 2, 2, 3, 3, 4, 4, 5)
    interaction(g1, g2)  # 1.1 1.1 1.2 2.2 2.3 2.3 3.4 3.4 3.5
    split(x, list(g1, g2), drop = TRUE)


    # Split data frame and summarise
    s <- split(airquality, airquality$Month)
    lapply(s, function(x) colMeans(x[, c('Ozone', 'Solar.R', 'Wind')], na.rm =  TRUE))
