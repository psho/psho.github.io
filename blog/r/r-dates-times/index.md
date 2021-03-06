<!--
.. title: R: Dates/Times
.. slug: r-dates-times
.. date: 2018-08-04 00:33:56 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Dates
-----

-   Dates represented by **Date** class
-   Stored as days since 1970-01-01

<!-- -->

    x <- as.Date('1970-01-01')
    unclass(x)  # 0

    x <- as.Date('2017-01-01')
    unclass(x)  # 17167


    # Operations
    x <- as.Date("2016-03-01")
    y <- as.Date("2016-02-28")
    x-y  # Time difference of 2 days  (keeps track of leap years)

Times
-----

-   Times represented by:
    -   **POSIXct** - compact time, just the number of seconds
    -   **POSIXlt** - list time, list of different representations
-   Stored as seconds since 1970-01-01

<!-- -->

    x <- Sys.time()  # "2017-02-12 19:54:32 GMT"
    class(x)  # "POSIXct" "POSIXt"
    unclass(x)  # 1486929273 seconds

    p <- as.POSIXlt(x)
    names(unclass(p))  # "sec"    "min"    "hour"   "mday" etc.
    p$sec  # 32.98549
    p$min  # 54


    # Convert string to time
    date_strings <- c("10 February, 2017 9:40", "15 March, 2017 18:10")
    x <- strptime(date_strings, "%d %B, %Y %H:%M")  # "2017-02-10 09:40:00 GMT" "2017-03-15 18:10:00 GMT"
    class(x)  # "POSIXlt" "POSIXt"


    # Operations
    x <- strptime("1 Feb 2017 12:34:56", "%d %b %Y %H:%M:%S")
    y <- as.Date("2017-01-10")
    y <- as.POSIXlt(y)  # Convert from Date to POSIXlt to match x
    x-y  # Time difference of 22.52426
