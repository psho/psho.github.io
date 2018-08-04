<!--
.. title: R: Control Flow
.. slug: r-control-flow
.. date: 2018-08-04 00:18:38 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

If
--

-   Be careful of short form (&, |) vs long form (&&, ||) logical
    operators
    -   Short form does element-wise comparison
    -   Long form evaluates left to right using only first element of
        each vector
    -   Long form preferred in if clauses, just ensure it's a single
        element comparison

<!-- -->

    if (x > 5) {
      y <- 6
    } else if (x == 5) {
      y <- 5
    } else {
      y <- 0
    }

    # Short form logical operators
    ((-2:2) >= 0) & ((-2:2) <= 0)   # FALSE FALSE  TRUE FALSE FALSE

    # Long form logical operators
    ((-2:2) >= 0) && ((-2:2) <= 0)  # FALSE  (-2 not >= 0)

For
---

    for (i in 1:10) {
      print(i)
    }


    # Process a vector - Following loops all have same result
    x <- c('a', 'b', 'c', 'd', 'e')

    for (i in 1:5) {
      print(x[i])
    }

    for (i in seq_len(length(x))) {
      print(x[i])
    }

    for (i in seq_along(x)) {
      print(x[i])
    }

    for (letter in x) {
      print(letter)
    }


    # Nested loops
    x <- matrix(1:6, 2, 3)

    for (i in seq_len(nrow(x))) {
      for (j in seq_len(ncol(x))) {
        print(x[i, j])
      }
    }


    # Interrupt loop iteration
    for (i in 1:100) {
      if (i <= 20) {
        next  # Skip to next iteration
      }

      print(i)

      if (i == 30) {
        break  # Break out of loop
      }
    }

While
-----

-   Loops while condition is true

<!-- -->

    i <- 0
    while (i < 10) {
      print(i)
      i <- i + 1
    }
