<!--
.. title: R: Files
.. slug: r-files
.. date: 2018-08-04 00:34:07 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Tabular Data
------------

    # Read
    infile <- 'input.csv'
    df <- read.table(file = infile,
                     header = TRUE,  # Has header line?
                     sep = ',',  # Separator character
                     nrows = 100,  # Number of rows to read
                     comment.char = '#',  # Skip rows starting with this character
                     skip = 2, # Skip this many rows from start
                     stringsAsFactors = TRUE)  # Encode chars as factors
    df <- read.csv(infile)  # Same as read.table() with comma as default separator


    # Read large files
    sample <- read.table('large_file.csv', nrows = 100)
    classes <- sapply(sample, class)
    df <- read.table('large_file.csv',
                     colClasses = classes,  # Pre-defining column classes is much faster
                     nrows = 50000)  # Pre-defining rows (Unix wc) can help with memory usage


    # Write
    outfile <- 'output.csv'
    write.table(df,
                file = outfile,
                append = FALSE)  # FALSE will overwrite
    write.csv(df, outfile)

R Objects
---------

-   Text format and includes meta-data
-   Easy to edit but less space efficient
-   Works nicely with version control programs like git
-   **dget** pairs with **dput**
-   **source** pairs with **dump**

<!-- -->

    # Read
    dget('data.R')

    source('data.R')

    # Write
    df <- data.frame(a = 1, b = 'b')
    dput(df)  # Prints out structure
    dput(df, 'data.R')

    x <- 5
    y <- 'abc'
    dump(c('x','y','df'), file = 'data.R')

Connections
-----------

#### Interfaces

-   file()
-   gzfile() = connection to gzip compressed file
-   url() = connection to webpage

#### Modes

-   'r' = read only
-   'w' = writing/creating
-   'a' = appending
-   'rb', 'wb', 'ab' = read/write/append in binary mode (Windows)

<!-- -->

    # Read
    con <- file('input.csv', 'r')
    con <- gzfile('input.gz', 'r')
    con <- url('http://cran.r-project.org/', 'r')
    x <- readLines(con, 10)  # Read in first 10 lines
    close(con)

Workspace
---------

    # Read
    load('mydata.RData')

    # Write
    save.image(file = 'mydata.Rdata')  # Defaults to '.Rdata' if not specified
