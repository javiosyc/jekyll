---
layout: post
title: "basic data types in R"
description: ""
category: R
tags: [R]
---
{% include JB/setup %}

## R has five basic or "automic" classes of object

1. character
2. numeric (real number)
3. integer
4. complex
5. logical(True/False)

## The basic object is a vector
* A vector can only contain objects of the same class
* BUT : The one exception is a list, which is represented as a vector but can contain objets of differents classes (indeed, that's usually why we use them)

Empty vectors can be created with the `vector()` function

vector() 函数有两个基本参数 

*the class of the objects 
*the length of the vector


## Numbers
* Numbers in R a generally treated as numeric object (i.e. double precision real numbers)
* If you want an integer, you need specify the L suffix (ex: 1L)
* There is a special number **Inf** which represents infinity
* The value **NaN** represents an undefined value ("not a nubmer"); e.g. 0/0; NaN can also be thought of as a missing value


## Attributes
R objects can have attributes
* names,dimnames
* dimensions (e.g. matrices, arrays)
* class
* length
* other user-defined attributes/metedata

Attributes of an object can be accessed using the `attributes()` function.

## Entering Input
At the R prompt we type expressions. The **<-** symbol is the assignment operator.

The **#** character indicates a comment. Anything to the right of the # is ignored.

## Creating Vectors

The `c()` function can be used to create vectors of objects.

` x <- (0.5, 0.6)		## numeric`

` x <- (T, F)			## logical`

` x <- 9:29        		## integer`

` x <- c(1+0i, 2+4i)	## complex`

When different objects are mixed in a vector, coercion occurs so that every element in the vector is of the same class.

` y <- c(1.7,"a")		## character`

` y <- c(TRUE,2)		## numeric`

` y <- c("a",TRUE)		## character`

Using the `vector()` function
` x <- vector ("numeric" , length=10)`

##Explicit Coercion
Objects can explicitly coerced from one class to antoher using the `as.*` function, if available.

` x <- 0:6`

`as.numeric(x)`

`as.logical(x)`

`as.character(x)`

## Matrices
Matrices are vectors with a dimension attribute. The dimension attribute is itself an integer vector of lenght 2 (nrow,ncol)

` m <- matrix (nrow=2,ncol=3)`

Matrices are constructed column-wise, so entries can be though of starting in the "upper left" comer and running down the columns.

` m <- matrix (1:6, nrow=2, ncol=3)`

Matrices can also be created directly from vectors by adding a dimension attribute.

` m <- 1:10 `

` dim(m) <- c(2,5) `

##cbind-ing and rbind-ing

Matrices can be created by column-binding or row-binding with `cbind()` and `rbind()`.

` x <- 1:3 `

` y <- 10:12 `

## Lists
Lists are special type of vector that can contain elements of different classes. Lists are a very important data type in R .

` x <- list(1,"a",TRUE, 1+4i) `

## Factors 
Factors are used to represent categorical data. Factors can be unordered or ordered. One can think of a factor as an integer vector where each integer has a label.
* Factors are treated specially by modelling functions like `lm()` and `glm()`
* Using factors with labels is better than using integers because factors are self-describing; having a variable that has values "Male" and "Female" is better than a variable that has values 1 and 2.

` x <- factor (c("yes","yes","no","yes","no"))`

Levels : no yes

` table(x)`

` unclass(x)`

The order of the levels can be set using the `levels` argument to `factor()`.This can be important in linear modeling because the first level is used as the baseline level.

` x <- factor(c("yes","yes","no","yes","no"), levels= c("yes","no"))`

## Missing Values
Missing values are denoted by **NA** or **NaN** for undefined mathematial operations.

* `is.na()` is used to test objects if they are **NA**
* `is.nan()` is used to test for **NaN**
* NA values have a class also, so there are integer **NA**, character **NA**, etc.
* A **NaN** value is also **NA** but the converse is not true.

## Data Frames
Data frames are used to store tabular data

* They are represented as special type of list where every element of the list has to have same length
* Each element of the list can be thought of as a column and the length of each element of the list is the number of rows
* Unlike matrics, data frames can store different classes of objects in each column (just like lists); matrices must have every element be the same class
* Data frames also have special attribute called row.names
* Data frames are usually created by calling `read.table()` or `read.csv()`
* Can be converted to a matrix by calling `data.matrix()`

`x <- data.frame (foo =1:4,bar = c(T,T,F,T))`

` nrow(x)`

` ncol(x)`

##Names 
R objects can also have names, which is very useful for writing reabable code and self-describing objects.

` x <- 1:3 `

` names(x)`

NULL

` names(x) <- c("foo","bar","norf")`

` names(x)`

Lists can also have names.

` x <- list( a=1,b=2,c=3)`

matrices

` m <- matrix (1:4, nrow=2 ,ncol=2)`

` dimnames(m) <- list(c("a","b"),c("c","d"))`
