---
layout: post
title: "subsetting, vectorizzed opeartions, rreading and writing data"
description: ""
category: R
tags: [R]
---
{% include JB/setup %}

## Subsetting

There are a number of operators that can be used to extract subsets of R objects.

* **[** always returns **an object of the same class as the original**; can be used to select **more than one element** (there is one exception)
	* numeric index
		* `x[1]`
		* `x[1:4]`
	* logical index
		* `x[x> "a"]`
		* `u <- x > "a"`   `x[u]` 

	* exception

		`x <- matrix (1:6,2,3)`

		`x[1,2]`

		[1] 3  #a vector with that number in it

* **[[** is used to extract elements of a **list or a data frame**; it can only be used to **extract a single element** and the class of the returned object will not necessarily be a list or data frame

* **$** is used to extract elements of a **list or data frame by name**; semantics are similar to hat of [[ 

## Subsetting a Matrix
Matrices can be subsetted in the usual way with (i,j) type indices
 `x[1,2]`

You don't have to always specify both indices when subsetting a matrix.
 `x[1,]`
 `x[,2]`


By default, when a single element of a matrix is retrived, it is returned as a vector of lenght 1 rather a 1 * 1 matrix.

This behavior can be turned off by setting **drop = FALSE**

` x <- matrix(1:6,2,3)`

` x[1,2]

[1] 3

` x[1,2,drop =FALSE]`

	[,1]

[1,] 3

Similarly, subsetting a single column or a single row will give you a vector, not a matrix (by default)

` x <- matrix(1:6,2,3)`

` x[1,]

[1] 1 3 5

` x[1,,drop =FALSE]`

	[,1] [,2] [,3]

[1,]  1    3    5

## Subsetting Lists
	* the double bracket operator
	* the dollar sign operator
	* the single bracket operator

`x <- list (foo =1:4,bar = 0.6)`

` x[1]`   # list

$foo

[1] 1 2 3 4

`x[[1]]`   # vector

[1] 1 2 3 4

`x$bar`    # vector

[1] 0.6

`x[["bar"]]` # vector

[1] 0.6

`x["bar"]`  # list

$bar

[1] 0.6

extract multiple elements

`x <- list (foo=1:4,bar =0.6,baz="hello")`

`x[c(1,3)]`

$foo

[1] 1 2 3 4


$baz

[1] "hello"

The **[[** operator can be used with computed indices; **$** can only be used with literal names.

`x <- list (foo = 1:4, bar = 0.6,baz="hello")`

`name <- "foo"`

`x[[name]]`		##computed index for 'foo'

[1] 1 2 3 4

`x$name`		## element 'name' doesn't exist!

NULL

`x$foo`

[1] 1 2 3 4		## element 'foo' does exist

## Subsetting Nested Elements of a List
The **[[** can take an integer sequence.

`x <- list(a=list(10,12,4),b=c(3.13,2.81))`

`x[[c(1,3))]`

[1] 14

`x[[1]][[3]]`

[1] 14

`x[[c(2,1)]]`

[1] 3.14

##Partial Matching
Partial matching of names is allowed with **[[ and $**

` x <- list (aardvark = 1: 5)`

`x$a`

[1] 1 2 3 4 5

`x[["a"]]`

NULL

`x[["a", exact =FALSE]]`

[1] 1 2 3 4 5

## Removing NA Values
A common task is to remove missing values (NAS).

` x <- c (1,2,NA,4,NA,5)`

` bad <- is.na(x)`

`x[!bad]`

[1] 1 2 4 5

What if there are multiple things and you want to take the subset with no missing values?

`x <- c(1,2,NA,4,NA,5)`

`y <- c("a","b",NA,"d",NA,"f")`

`good <- complete.cases(x,y)`

`good`

[1] TRUE TRUE FALSE TRUE FALSE TRUE

`x[good]`

[1] 1 2 4 5

`y[good]`

[1] "a" "b" "d" "f"

matrix

` airquality[1:6,]`

`good <- complete.cases(airquality)`

`airquality[good,][1:6,]`

## Vectorized Operations
Many operations in R are vectorized making code more efficient, concise, and easier to read.

` x <- 1:4; y <- 6:9`

` x + y`

[1] 7 9 11 13

`x > 2`

[1] FALSE FALSE TRUE TRUE

`y==8`

[1] FALSE FALSE TRUE FALSE

`x * y`

[1] 6 14 24 36

`x / y`

[1] 0.16667 0.28571 0.37500 0.44444

## Vectorized Matrix Operations

x <- matrix(1:4, 2, 2); y <- matrix(rep(10, 4), 2, 2)

`x * y ## element-wise multiplication`

`x %*% y ## true matrix multiplication`

## Reading Data

There are a few principal function reading data into R

* `read.table`, `read.csv`, for reading tabular data

* `readLines`, for reading lines of a text file

* `source`, for reading in R code files (inverse of dump)

* `dget`, for reading in R code files (inverse of dput)

* `load`, for reading in saved workspace

* 'unserialize', for reading single R objects in binary form

## Writing Data

There are analogous functions for writing data to files

* `write.table`

* `writeLines`

* `dump`

* `dput`

* `save`

* `serialize`

