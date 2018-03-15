---
output: html_document
editor_options: 
  chunk_output_type: console
---
# Chapter 21 - Iteration {-}




```r
library(tidyverse)
```

## 21.2 - For loops {-}

### Problem 1 {-}

Write for loops to:

Compute the mean of every column in mtcars.


```r
means <- vector(mode = "double", length = ncol(mtcars))

for (i in seq_along(mtcars)) {
  means[[i]] <- mean(mtcars[[i]])
}
```

Determine the type of each column in nycflights13::flights.


```r
library(nycflights13)

types <- vector(mode = "character", length = ncol(flights))

for (i in seq_along(flights)) {
  types[[i]] <- typeof(flights[[i]])
}
```

Compute the number of unique values in each column of iris.


```r
uniques <- vector(mode = "integer", length = ncol(iris))

for (i in seq_along(iris)) {
  uniques[[i]] <- length(unique(iris[[i]]))
}
```

Generate 10 random normals for each of  
μ=−10, 0, 10, and 100.


```r
mu <- c(-10, 0, 10, 100)

means <- vector(mode = "list", length = length(mu))

for (i in seq_along(mu)) {
  means[[i]] <- rnorm(10, mu[[i]])
}
```
 
Think about the output, sequence, and body before you start writing the loop.

### Problem 2 {-}

Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors:

```
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
```


```r
str_c(letters, collapse = "")
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```

```
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))
```


```r
x <- sample(100)
sd(x)
```

```
## [1] 29.01149
```

```
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
```


```r
x <- runif(100)
cumsum(x)
```

```
##   [1]  0.06030477  0.75742118  1.69223178  2.35422111  2.72246468
##   [6]  3.67966522  3.72324884  4.50973683  5.44731544  5.80765030
##  [11]  6.40561882  6.92470796  7.04642363  7.46450121  7.70113562
##  [16]  7.93046446  8.04482687  8.73551545  9.22180294  9.42266355
##  [21]  9.52799487  9.83999521 10.60093500 10.86012633 11.69918734
##  [26] 12.34664251 13.28359726 13.95228765 14.47616801 15.34560148
##  [31] 16.30425587 17.05519106 17.07623238 17.89635132 18.09061577
##  [36] 18.82034431 19.41470017 20.16952820 20.99551643 21.57229480
##  [41] 21.70375800 22.23202871 22.48396282 22.51326563 23.01452899
##  [46] 23.05260217 23.50482650 24.31992424 24.44944922 24.71880348
##  [51] 24.78475777 25.62374697 25.93598207 26.49051693 26.69721214
##  [56] 27.25935695 27.73715236 28.64560495 29.37741005 29.86748751
##  [61] 29.92920748 30.60484837 31.49035629 31.92410010 32.08372988
##  [66] 32.16561631 33.00098727 33.58140343 33.80156996 34.67180574
##  [71] 35.29647946 36.10209921 36.67074135 36.86176417 37.68857715
##  [76] 38.37004790 38.97167990 39.62529428 40.61496692 41.01846524
##  [81] 41.08595753 41.16703179 41.69145914 42.50166870 42.75396548
##  [86] 43.41057635 43.50030912 43.78397371 43.83507276 43.85966632
##  [91] 44.76932271 44.78832266 45.41748462 45.52085776 45.73368836
##  [96] 46.36236083 46.84908308 47.79787528 48.00451683 48.77955002
```

### Problem 3 {-}

Write a for loop that `prints()` the lyrics to the children’s song “Alice the camel”.


```r
library(stringr)

humps <- c("five", "four", "three", "two", "one", "no")
for (i in seq_along(humps)) {
  for (j in 1:3) {
    print(str_glue("Alice the camel has {humps[i]} humps."))
  }
  if (i < 6) {
    # str_glue is unecessary. it drops [1]
    print(str_glue("So go, Alice, go."))
  } else {
    print(str_glue("Now Alice is a horse"))
  }
}
```

```
## Alice the camel has five humps.
## Alice the camel has five humps.
## Alice the camel has five humps.
## So go, Alice, go.
## Alice the camel has four humps.
## Alice the camel has four humps.
## Alice the camel has four humps.
## So go, Alice, go.
## Alice the camel has three humps.
## Alice the camel has three humps.
## Alice the camel has three humps.
## So go, Alice, go.
## Alice the camel has two humps.
## Alice the camel has two humps.
## Alice the camel has two humps.
## So go, Alice, go.
## Alice the camel has one humps.
## Alice the camel has one humps.
## Alice the camel has one humps.
## So go, Alice, go.
## Alice the camel has no humps.
## Alice the camel has no humps.
## Alice the camel has no humps.
## Now Alice is a horse
```

Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.


```r
n_in_the_bed <- function(occupants = 10L) {
  
  for (i in seq(from = occupants, to = 1)) {
    if (i > 1) {
      print(str_glue(
        "There were {i} in a bed
        And the little one said
        'Roll over, roll over'
        So they all rolled over
        And one fell out
        
        "
      ))
    } else if (i == 1) {
      print(str_glue("There was one in a bed
      And the little one said
      'Good night!'"))
    }
  }
}

n_in_the_bed()
```

```
## There were 10 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 9 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 8 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 7 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 6 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 5 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 4 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 3 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There were 2 in a bed
## And the little one said
## 'Roll over, roll over'
## So they all rolled over
## And one fell out
## 
## There was one in a bed
## And the little one said
## 'Good night!'
```

Convert the song “99 bottles of beer on the wall” to a function. Generalise to any number of any vessel containing any liquid on any surface.


```r
bottles_of_beer <- function(bottles = 99L) {
  for (i in seq(from = bottles, to = 0)) {
    
    bottles_left <- ifelse(i == 0, "no more", i) 
    
    print(str_glue(
      "{i} bottle of beer on the wall, {i} bottle of beer.
      Take one down and pass it around, {bottles_left} bottles of beer on the wall."
      )
    )
  }
}

bottles_of_beer()
```

```
## 99 bottle of beer on the wall, 99 bottle of beer.
## Take one down and pass it around, 99 bottles of beer on the wall.
## 98 bottle of beer on the wall, 98 bottle of beer.
## Take one down and pass it around, 98 bottles of beer on the wall.
## 97 bottle of beer on the wall, 97 bottle of beer.
## Take one down and pass it around, 97 bottles of beer on the wall.
## 96 bottle of beer on the wall, 96 bottle of beer.
## Take one down and pass it around, 96 bottles of beer on the wall.
## 95 bottle of beer on the wall, 95 bottle of beer.
## Take one down and pass it around, 95 bottles of beer on the wall.
## 94 bottle of beer on the wall, 94 bottle of beer.
## Take one down and pass it around, 94 bottles of beer on the wall.
## 93 bottle of beer on the wall, 93 bottle of beer.
## Take one down and pass it around, 93 bottles of beer on the wall.
## 92 bottle of beer on the wall, 92 bottle of beer.
## Take one down and pass it around, 92 bottles of beer on the wall.
## 91 bottle of beer on the wall, 91 bottle of beer.
## Take one down and pass it around, 91 bottles of beer on the wall.
## 90 bottle of beer on the wall, 90 bottle of beer.
## Take one down and pass it around, 90 bottles of beer on the wall.
## 89 bottle of beer on the wall, 89 bottle of beer.
## Take one down and pass it around, 89 bottles of beer on the wall.
## 88 bottle of beer on the wall, 88 bottle of beer.
## Take one down and pass it around, 88 bottles of beer on the wall.
## 87 bottle of beer on the wall, 87 bottle of beer.
## Take one down and pass it around, 87 bottles of beer on the wall.
## 86 bottle of beer on the wall, 86 bottle of beer.
## Take one down and pass it around, 86 bottles of beer on the wall.
## 85 bottle of beer on the wall, 85 bottle of beer.
## Take one down and pass it around, 85 bottles of beer on the wall.
## 84 bottle of beer on the wall, 84 bottle of beer.
## Take one down and pass it around, 84 bottles of beer on the wall.
## 83 bottle of beer on the wall, 83 bottle of beer.
## Take one down and pass it around, 83 bottles of beer on the wall.
## 82 bottle of beer on the wall, 82 bottle of beer.
## Take one down and pass it around, 82 bottles of beer on the wall.
## 81 bottle of beer on the wall, 81 bottle of beer.
## Take one down and pass it around, 81 bottles of beer on the wall.
## 80 bottle of beer on the wall, 80 bottle of beer.
## Take one down and pass it around, 80 bottles of beer on the wall.
## 79 bottle of beer on the wall, 79 bottle of beer.
## Take one down and pass it around, 79 bottles of beer on the wall.
## 78 bottle of beer on the wall, 78 bottle of beer.
## Take one down and pass it around, 78 bottles of beer on the wall.
## 77 bottle of beer on the wall, 77 bottle of beer.
## Take one down and pass it around, 77 bottles of beer on the wall.
## 76 bottle of beer on the wall, 76 bottle of beer.
## Take one down and pass it around, 76 bottles of beer on the wall.
## 75 bottle of beer on the wall, 75 bottle of beer.
## Take one down and pass it around, 75 bottles of beer on the wall.
## 74 bottle of beer on the wall, 74 bottle of beer.
## Take one down and pass it around, 74 bottles of beer on the wall.
## 73 bottle of beer on the wall, 73 bottle of beer.
## Take one down and pass it around, 73 bottles of beer on the wall.
## 72 bottle of beer on the wall, 72 bottle of beer.
## Take one down and pass it around, 72 bottles of beer on the wall.
## 71 bottle of beer on the wall, 71 bottle of beer.
## Take one down and pass it around, 71 bottles of beer on the wall.
## 70 bottle of beer on the wall, 70 bottle of beer.
## Take one down and pass it around, 70 bottles of beer on the wall.
## 69 bottle of beer on the wall, 69 bottle of beer.
## Take one down and pass it around, 69 bottles of beer on the wall.
## 68 bottle of beer on the wall, 68 bottle of beer.
## Take one down and pass it around, 68 bottles of beer on the wall.
## 67 bottle of beer on the wall, 67 bottle of beer.
## Take one down and pass it around, 67 bottles of beer on the wall.
## 66 bottle of beer on the wall, 66 bottle of beer.
## Take one down and pass it around, 66 bottles of beer on the wall.
## 65 bottle of beer on the wall, 65 bottle of beer.
## Take one down and pass it around, 65 bottles of beer on the wall.
## 64 bottle of beer on the wall, 64 bottle of beer.
## Take one down and pass it around, 64 bottles of beer on the wall.
## 63 bottle of beer on the wall, 63 bottle of beer.
## Take one down and pass it around, 63 bottles of beer on the wall.
## 62 bottle of beer on the wall, 62 bottle of beer.
## Take one down and pass it around, 62 bottles of beer on the wall.
## 61 bottle of beer on the wall, 61 bottle of beer.
## Take one down and pass it around, 61 bottles of beer on the wall.
## 60 bottle of beer on the wall, 60 bottle of beer.
## Take one down and pass it around, 60 bottles of beer on the wall.
## 59 bottle of beer on the wall, 59 bottle of beer.
## Take one down and pass it around, 59 bottles of beer on the wall.
## 58 bottle of beer on the wall, 58 bottle of beer.
## Take one down and pass it around, 58 bottles of beer on the wall.
## 57 bottle of beer on the wall, 57 bottle of beer.
## Take one down and pass it around, 57 bottles of beer on the wall.
## 56 bottle of beer on the wall, 56 bottle of beer.
## Take one down and pass it around, 56 bottles of beer on the wall.
## 55 bottle of beer on the wall, 55 bottle of beer.
## Take one down and pass it around, 55 bottles of beer on the wall.
## 54 bottle of beer on the wall, 54 bottle of beer.
## Take one down and pass it around, 54 bottles of beer on the wall.
## 53 bottle of beer on the wall, 53 bottle of beer.
## Take one down and pass it around, 53 bottles of beer on the wall.
## 52 bottle of beer on the wall, 52 bottle of beer.
## Take one down and pass it around, 52 bottles of beer on the wall.
## 51 bottle of beer on the wall, 51 bottle of beer.
## Take one down and pass it around, 51 bottles of beer on the wall.
## 50 bottle of beer on the wall, 50 bottle of beer.
## Take one down and pass it around, 50 bottles of beer on the wall.
## 49 bottle of beer on the wall, 49 bottle of beer.
## Take one down and pass it around, 49 bottles of beer on the wall.
## 48 bottle of beer on the wall, 48 bottle of beer.
## Take one down and pass it around, 48 bottles of beer on the wall.
## 47 bottle of beer on the wall, 47 bottle of beer.
## Take one down and pass it around, 47 bottles of beer on the wall.
## 46 bottle of beer on the wall, 46 bottle of beer.
## Take one down and pass it around, 46 bottles of beer on the wall.
## 45 bottle of beer on the wall, 45 bottle of beer.
## Take one down and pass it around, 45 bottles of beer on the wall.
## 44 bottle of beer on the wall, 44 bottle of beer.
## Take one down and pass it around, 44 bottles of beer on the wall.
## 43 bottle of beer on the wall, 43 bottle of beer.
## Take one down and pass it around, 43 bottles of beer on the wall.
## 42 bottle of beer on the wall, 42 bottle of beer.
## Take one down and pass it around, 42 bottles of beer on the wall.
## 41 bottle of beer on the wall, 41 bottle of beer.
## Take one down and pass it around, 41 bottles of beer on the wall.
## 40 bottle of beer on the wall, 40 bottle of beer.
## Take one down and pass it around, 40 bottles of beer on the wall.
## 39 bottle of beer on the wall, 39 bottle of beer.
## Take one down and pass it around, 39 bottles of beer on the wall.
## 38 bottle of beer on the wall, 38 bottle of beer.
## Take one down and pass it around, 38 bottles of beer on the wall.
## 37 bottle of beer on the wall, 37 bottle of beer.
## Take one down and pass it around, 37 bottles of beer on the wall.
## 36 bottle of beer on the wall, 36 bottle of beer.
## Take one down and pass it around, 36 bottles of beer on the wall.
## 35 bottle of beer on the wall, 35 bottle of beer.
## Take one down and pass it around, 35 bottles of beer on the wall.
## 34 bottle of beer on the wall, 34 bottle of beer.
## Take one down and pass it around, 34 bottles of beer on the wall.
## 33 bottle of beer on the wall, 33 bottle of beer.
## Take one down and pass it around, 33 bottles of beer on the wall.
## 32 bottle of beer on the wall, 32 bottle of beer.
## Take one down and pass it around, 32 bottles of beer on the wall.
## 31 bottle of beer on the wall, 31 bottle of beer.
## Take one down and pass it around, 31 bottles of beer on the wall.
## 30 bottle of beer on the wall, 30 bottle of beer.
## Take one down and pass it around, 30 bottles of beer on the wall.
## 29 bottle of beer on the wall, 29 bottle of beer.
## Take one down and pass it around, 29 bottles of beer on the wall.
## 28 bottle of beer on the wall, 28 bottle of beer.
## Take one down and pass it around, 28 bottles of beer on the wall.
## 27 bottle of beer on the wall, 27 bottle of beer.
## Take one down and pass it around, 27 bottles of beer on the wall.
## 26 bottle of beer on the wall, 26 bottle of beer.
## Take one down and pass it around, 26 bottles of beer on the wall.
## 25 bottle of beer on the wall, 25 bottle of beer.
## Take one down and pass it around, 25 bottles of beer on the wall.
## 24 bottle of beer on the wall, 24 bottle of beer.
## Take one down and pass it around, 24 bottles of beer on the wall.
## 23 bottle of beer on the wall, 23 bottle of beer.
## Take one down and pass it around, 23 bottles of beer on the wall.
## 22 bottle of beer on the wall, 22 bottle of beer.
## Take one down and pass it around, 22 bottles of beer on the wall.
## 21 bottle of beer on the wall, 21 bottle of beer.
## Take one down and pass it around, 21 bottles of beer on the wall.
## 20 bottle of beer on the wall, 20 bottle of beer.
## Take one down and pass it around, 20 bottles of beer on the wall.
## 19 bottle of beer on the wall, 19 bottle of beer.
## Take one down and pass it around, 19 bottles of beer on the wall.
## 18 bottle of beer on the wall, 18 bottle of beer.
## Take one down and pass it around, 18 bottles of beer on the wall.
## 17 bottle of beer on the wall, 17 bottle of beer.
## Take one down and pass it around, 17 bottles of beer on the wall.
## 16 bottle of beer on the wall, 16 bottle of beer.
## Take one down and pass it around, 16 bottles of beer on the wall.
## 15 bottle of beer on the wall, 15 bottle of beer.
## Take one down and pass it around, 15 bottles of beer on the wall.
## 14 bottle of beer on the wall, 14 bottle of beer.
## Take one down and pass it around, 14 bottles of beer on the wall.
## 13 bottle of beer on the wall, 13 bottle of beer.
## Take one down and pass it around, 13 bottles of beer on the wall.
## 12 bottle of beer on the wall, 12 bottle of beer.
## Take one down and pass it around, 12 bottles of beer on the wall.
## 11 bottle of beer on the wall, 11 bottle of beer.
## Take one down and pass it around, 11 bottles of beer on the wall.
## 10 bottle of beer on the wall, 10 bottle of beer.
## Take one down and pass it around, 10 bottles of beer on the wall.
## 9 bottle of beer on the wall, 9 bottle of beer.
## Take one down and pass it around, 9 bottles of beer on the wall.
## 8 bottle of beer on the wall, 8 bottle of beer.
## Take one down and pass it around, 8 bottles of beer on the wall.
## 7 bottle of beer on the wall, 7 bottle of beer.
## Take one down and pass it around, 7 bottles of beer on the wall.
## 6 bottle of beer on the wall, 6 bottle of beer.
## Take one down and pass it around, 6 bottles of beer on the wall.
## 5 bottle of beer on the wall, 5 bottle of beer.
## Take one down and pass it around, 5 bottles of beer on the wall.
## 4 bottle of beer on the wall, 4 bottle of beer.
## Take one down and pass it around, 4 bottles of beer on the wall.
## 3 bottle of beer on the wall, 3 bottle of beer.
## Take one down and pass it around, 3 bottles of beer on the wall.
## 2 bottle of beer on the wall, 2 bottle of beer.
## Take one down and pass it around, 2 bottles of beer on the wall.
## 1 bottle of beer on the wall, 1 bottle of beer.
## Take one down and pass it around, 1 bottles of beer on the wall.
## 0 bottle of beer on the wall, 0 bottle of beer.
## Take one down and pass it around, no more bottles of beer on the wall.
```


### Problem 4 {-}

It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:

```
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```

How does this affect performance? Design and execute an experiment.


```r
library(microbenchmark)

grow_vector <- function() {
  
  # create a character vector
  x <- rep("abc", 1000)
  
  # create an empty vector of length 0 for the output
  output <- vector("integer", 0)
  
  # find the length of each observation in x and grow and concatenate it to 
  # output
  for (i in seq_along(x)) {
    output <- c(output, lengths(x[[i]]))
  }
  output
}

microbenchmark(grow_vector())
```

```
## Unit: milliseconds
##           expr      min       lq    mean   median       uq      max neval
##  grow_vector() 1.867523 2.053598 3.22277 2.756735 3.565339 22.35156   100
```

This first chunk grows a vector from length zero to length 1,000. 


```r
preallocate_vector <- function() {
  
  # create a character vector  
  x <- rep("abc", 1000)
  
  # create an empty vector of length length(x) for the output
  output <- vector("integer", length(x))
  
  # find the length of each observation in x and assignment it the corresponding
  # location in the output vector
  for (i in seq_along(x)) {
    output[i] <- lengths(x[[i]])
  }
  output
}

microbenchmark(preallocate_vector())
```

```
## Unit: microseconds
##                  expr     min      lq     mean   median       uq      max
##  preallocate_vector() 610.026 648.247 771.9045 666.4425 714.0675 5742.906
##  neval
##    100
```

This second chunk preallocates the space in the vector and then populates it. It is an order of magnitude faster. 

## 21.3 - For loop variations {-}

### Problem 1 {-}

Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, `files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)`, and now want to read each one with `read_csv()`. Write the for loop that will load them into a single data frame.


```r
library(tidyverse)

files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)

data <- vector("list", length(files))

for (i in seq_along(files)) {
  data[[i]] <- read_csv(files[[i]])
}

bind_rows(data)
```

### Problem 2 {-}

What happens if you use `for (nm in names(x))` and `x` has no names? 

Nothing, it doesn't iterate. 


```r
x <- c(1, 2, 3)

for (nm in names(x)) {
  print(names(x[nm]))
}
```

What if only some of the elements are named?

It iterates along the entire vector. 


```r
x <- c("a" = 1, 2, "c" = 3)

for (nm in names(x)) {
  print(names(x[nm]))
}
```

```
## [1] "a"
## [1] NA
## [1] "c"
```

What if the names are not unique?
 
It does not matter. It still iterates along the entire vector. 
 

```r
x <- c("a" = 1, "a" = 2, "c" = 3)

for (nm in names(x)) {
  print(names(x[nm]))
}
```

```
## [1] "a"
## [1] "a"
## [1] "c"
```

### Problem 3 {-}

Write a function that prints the mean of each numeric column in a data frame, along with its name. For example, `show_mean(iris)` would print:

```
show_mean(iris)
#> Sepal.Length: 5.84
#> Sepal.Width:  3.06
#> Petal.Length: 3.76
#> Petal.Width:  1.20
```

(Extra challenge: what function did I use to make sure that the numbers lined up nicely, even though the variable names had different lengths?)



```r
print_means <- function(data = iris, digits = 2) {

  max_str_length <- max(str_length(names(data)))
  
    for (i in seq_along(data)) {
    
    if (is.numeric(data[[i]])) {
      cat("#> ", 
          str_pad(str_c(names(data)[[i]], ": "), width = max_str_length + 2, side = "right"), 
          format(mean(data[[i]]), digits = digits, nsmall = 2),
          "\n",
          sep = "")
    }
  }
}

print_means()
```

```
## #> Sepal.Length: 5.84
## #> Sepal.Width:  3.06
## #> Petal.Length: 3.76
## #> Petal.Width:  1.20
```

### Problem 4 {-}

What does this code do? How does it work?

```
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}
```

It creates a list of functions. Then it iterates across vectors in the `mtcars` data set and the correspondingly named function in the `trans` list. In the end, the variable `disp` is multiplied by `0.0163871` and the variable nm is labelled with "auto" and "manual".

## 21.4 For loops vs. functionals {-}

### Problem 1 {-}

Read the documentation for `apply()`. In the 2d case, what two for loops does it generalise?

It generalizes a for loop along a vector inside of a for loop along a series of columns in a data frame or matrix. 

### Problem 2 {-}

Adapt `col_summary()` so that it only applies to numeric columns You might want to start with an `is_numeric()` function that returns a logical vector that has a TRUE corresponding to each numeric column.


```r
col_summary <- function(df, fun) {
  
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    if (is_numeric(df[[i]])) {
      out[i] <- fun(df[[i]])
    }
  }
  out
}

col_summary(mtcars, median)
```

```
##  [1]  19.200   6.000 196.300 123.000   3.695   3.325  17.710   0.000
##  [9]   0.000   4.000   2.000
```

## 21.5 The map functions {-}

### Problem 1 {-}

Write code that uses one of the map functions to:

Compute the mean of every column in mtcars.


```r
mtcars %>%
  map_dbl(mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

Determine the type of each column in nycflights13::flights.


```r
library(nycflights13)

flights %>%
  map_chr(typeof)
```

```
##           year          month            day       dep_time sched_dep_time 
##      "integer"      "integer"      "integer"      "integer"      "integer" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##       "double"      "integer"      "integer"       "double"    "character" 
##         flight        tailnum         origin           dest       air_time 
##      "integer"    "character"    "character"    "character"       "double" 
##       distance           hour         minute      time_hour 
##       "double"       "double"       "double"       "double"
```

Compute the number of unique values in each column of iris.


```r
iris %>%
  map_int(~length(unique(.)))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```


```r
c(-10, 0, 10, 100) %>%
  map(rnorm, n = 10)
```

```
## [[1]]
##  [1]  -9.065648 -10.704343  -9.707397 -10.416879 -10.362859 -10.473410
##  [7] -10.049273  -8.601976 -10.151722  -9.518969
## 
## [[2]]
##  [1]  0.210845194  0.620974646  0.220459853  0.278875913  0.003617283
##  [6]  0.940772933 -0.350757276  0.377905648 -0.525865509  1.574778261
## 
## [[3]]
##  [1] 11.008693  8.757225 10.164525 11.187652 10.100596 10.677929  9.566846
##  [8]  9.175875 10.218792  9.392930
## 
## [[4]]
##  [1]  99.39022  99.41616 100.83423 100.52258 101.05746  99.21127  98.54773
##  [8]  99.72452  99.36030 100.25473
```

### Problem 2 {-}

How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?


```r
iris %>%
  map_lgl(is.factor)
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##        FALSE        FALSE        FALSE        FALSE         TRUE
```

### Problem 3 {-}

What happens when you use the map functions on vectors that aren’t lists? What does `map(1:5, runif)` do? Why?

It iterates the function across each value in the vector. Here, `1:5` are treated as the argument `n`, so each iteration draws `n` values from the standard normal distribution. 

### Problem 4 {-}

What does `map(-2:2, rnorm, n = 5)` do? Why? What does `map_dbl(-2:2, rnorm, n = 5)` do? Why?

`map(-2:2, rnorm, n = 5)` returns a list with five vectors of length five with values draw from the standard normal distribution with means ranging from -2 to 2. 

`map_dbl(-2:2, rnorm, n = 5)` returns the error `Error: Result 1 is not a length 1 atomic vector` because vectors can't be nested inside of vectors. 

The first works because `map()` returns a list and the second doesn't work because `map_dbl()` returns a vector. 

### Problem 5 {-}

Rewrite `map(x, function(df) lm(mpg ~ wt, data = df))` to eliminate the anonymous function.


```r
x <- mtcars %>%
  split(.$cyl)

map(x, ~lm(mpg ~ wt, data = .))
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

## 21.9 Other patterns of for loops {-}

### Problem 1 {-}

Implement your own version of `every()` using a for loop. Compare it with `purrr::every()`. What does purrr’s version do that your version doesn’t?


```r
x <- list(1:5, rep("a", 5), 1:5)

x %>% 
  every(is_vector)
```

```
## [1] TRUE
```

```r
my_every <- function(df, fun) {
  out <- vector("logical", length(x))
  for (i in seq_along(df)) {
    out[[i]] <- fun(df[[i]])
  }
  !FALSE %in% out
}

x %>% 
  my_every(is.numeric)
```

```
## [1] FALSE
```


```r
every
```

```
## function (.x, .p, ...) 
## {
##     .p <- as_mapper(.p, ...)
##     for (i in seq_along(.x)) {
##         val <- .p(.x[[i]], ...)
##         if (is_false(val)) 
##             return(FALSE)
##         if (anyNA(val)) 
##             return(NA)
##     }
##     TRUE
## }
## <environment: namespace:purrr>
```

`purrr:every()` is clever. It doesn't create a vector and store output. Instead, it tests every object and then returns a FALSE when the first vector fails the test. This is more efficient than my code. 

### Problem 2 {-}

Create an enhanced `col_sum()` that applies a summary function to every numeric column in a data frame.


```r
col_sum <- function(df, fun) {
  is_num <- map_lgl(df, is.numeric)
  
  df_num <- df[, is_num]
  
  map_dbl(df_num, fun)
}

col_sum(mpg, mean)
```

```
##       displ        year         cyl         cty         hwy 
##    3.471795 2003.500000    5.888889   16.858974   23.440171
```

### Problem 3 {-}

A possible base R equivalent of `col_sum()` is:


```r
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]

  sapply(df_num, f)
}
```

But it has a number of bugs as illustrated with the following inputs:

```
df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)
# OK
col_sum3(df, mean)
# Has problems: don't always return numeric vector
```


```r
df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)

col_sum3(df[1:2], mean)
```

```
## x y 
## 2 2
```


```r
col_sum3(df[1], mean)
```

```
## x 
## 2
```

```
col_sum3(df[0], mean)
```

The first two "problem cases" run fine. The third problem case has an issue because `Error: Unsupported index type: list`. 
