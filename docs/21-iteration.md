# Chapter 21 - Iteration {-}




```r
library(tidyverse)
library(stringr)
library(purrr)
```

## 21.2 For loops {-}

### Problem 1 {-}

Write a for loop to compute the mean of every column in mtcars.


```r
x <- vector("double", ncol(mtcars))
for (i in seq_along(mtcars)) {
  x[[i]] <- mean(mtcars[[i]])
}
```

Write a for loop to determine the type of each column in nycflights13::flights.


```r
x <- vector("character", ncol(nycflights13::flights))
for (i in seq_along(nycflights13::flights)) {
  x[[i]] <- str_c(class(nycflights13::flights[[i]]), collapse = ", ")
}
```

Write a for loop to compute the number of unique values in each column of iris.


```r
x <- vector("integer", ncol(iris))
for (i in seq_along(iris)) {
  x[[i]] <- length(unique(iris[[i]]))
}
```

Write a for loop to generate 10 random normals for each of mean = -10, 0, 10, 100. 

```r
means <- c(rep(-10, 10), rep(0, 10), rep(10, 10), rep(100, 10))
x <- vector("double", 40)
for (i in seq_along(means)) {
  x[[i]] <- rnorm(1, means[[i]])
}
```

### Problem 2 {-}

Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors:


```r
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}

str_c(letters[1:26], collapse = "")
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```


```r
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))

sd(x)
```

```
## [1] 29.01149
```


```r
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}

cumsum(x)
```

```
##   [1]  0.6491359  1.1189467  1.9836567  2.6281408  2.8601745  3.7842518
##   [7]  3.8785881  4.1077402  4.3098596  5.1572951  5.8187330  6.3710691
##  [13]  7.3503414  7.6334977  7.7088672  8.4843921  9.3941163 10.3443280
##  [19] 10.8723795 11.3322107 11.4186022 11.8683037 12.7353834 13.4867984
##  [25] 14.3308043 14.5164748 14.6462585 14.9083421 15.8664168 16.8531047
##  [31] 17.3578829 17.7606797 17.8265363 18.5644014 19.1751086 19.8533481
##  [37] 19.9765075 20.2753464 20.8749396 21.6541861 22.3415827 22.9922229
##  [43] 23.7571154 24.2595636 24.6167492 25.2993760 25.8520014 25.9827640
##  [49] 26.5286731 26.8632338 27.7237207 28.6405882 29.2140815 30.0120071
##  [55] 30.1440027 30.5948993 30.9036359 31.5594255 32.3788071 32.5965569
##  [61] 32.9371783 33.2865887 34.1277014 34.2057202 34.6154503 35.3882793
##  [67] 36.0449687 36.7061307 36.7564339 37.7079828 38.6564586 39.6240817
##  [73] 39.7509596 40.5543907 40.7539901 41.3358658 41.7431942 42.6997809
##  [79] 43.3635022 44.3321069 44.3832789 45.3455916 46.1264129 46.5855531
##  [85] 47.5265705 47.6496660 48.3306021 49.1509863 49.6676351 50.4511858
##  [91] 50.9520125 51.6684470 52.4115732 53.0381338 53.7733257 54.3519036
##  [97] 54.9787012 55.3942718 56.3008661 57.0864922
```

### Problem 3 {-}

Write a for loop that prints() the lyrics to the children’s song “Alice the camel”.


```r
humps <- c("five", "four", "three", "two", "one", "no")

y <- vector("character", 6)
for (i in seq_along(humps)) {
  x <- str_replace_all(str_c(c(rep("Alice the camel has x humps; ", 3), "So go, Alice, go. "), collapse = ""), "x", humps)
}
print(str_c(c(x, " Now Alice has no humps!"), collapse = ""))
```

```
## [1] "Alice the camel has five humps; Alice the camel has five humps; Alice the camel has five humps; So go, Alice, go. Alice the camel has four humps; Alice the camel has four humps; Alice the camel has four humps; So go, Alice, go. Alice the camel has three humps; Alice the camel has three humps; Alice the camel has three humps; So go, Alice, go. Alice the camel has two humps; Alice the camel has two humps; Alice the camel has two humps; So go, Alice, go. Alice the camel has one humps; Alice the camel has one humps; Alice the camel has one humps; So go, Alice, go. Alice the camel has no humps; Alice the camel has no humps; Alice the camel has no humps; So go, Alice, go.  Now Alice has no humps!"
```

Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.


```r
roll_bed <- function(n, structure) {
  
n <- n:2 
y <- vector("numeric", length(n))
for (i in seq_along(n)) {
  y <- str_c("There were ",n," in the ",structure," and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out.")
}
y <- c(y, str_c("There was 1 in the ", structure, " and the little one said, 'Alone at last!'"))
 print(y)
}
roll_bed(10, "bed")
```

```
##  [1] "There were 10 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out."
##  [2] "There were 9 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [3] "There were 8 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [4] "There were 7 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [5] "There were 6 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [6] "There were 5 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [7] "There were 4 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [8] "There were 3 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
##  [9] "There were 2 in the bed and the little one said, 'Roll over! Roll over!' So they all rolled over and  none fell out." 
## [10] "There was 1 in the bed and the little one said, 'Alone at last!'"
```

Convert the song “99 bottles of beer on the wall” to a function. Generalise to any number of any vessel containing any liquid on any surface.


```r
beer_wall <- function(n, liquid) {

n <- n:1
y <- vector("numeric", length(n))
for (i in seq_along(n)) {
  y <- str_replace_all(str_c(n, " bottles of beer on the wall, ", n, " bottles of beer. Take one down and pass it around, ", (n-1)," bottles of beer on the wall."), "beer", liquid)
}
n <- n[1]
y <- c(y, str_replace_all(str_c("No more bottles of beer on the wall, no more bottles of beer. Go to the store and buy some more, ", n, " bottles of beer on the wall."), "beer", liquid))
 print(y)
}
beer_wall(10, "beer")
```

```
##  [1] "10 bottles of beer on the wall, 10 bottles of beer. Take one down and pass it around, 9 bottles of beer on the wall."            
##  [2] "9 bottles of beer on the wall, 9 bottles of beer. Take one down and pass it around, 8 bottles of beer on the wall."              
##  [3] "8 bottles of beer on the wall, 8 bottles of beer. Take one down and pass it around, 7 bottles of beer on the wall."              
##  [4] "7 bottles of beer on the wall, 7 bottles of beer. Take one down and pass it around, 6 bottles of beer on the wall."              
##  [5] "6 bottles of beer on the wall, 6 bottles of beer. Take one down and pass it around, 5 bottles of beer on the wall."              
##  [6] "5 bottles of beer on the wall, 5 bottles of beer. Take one down and pass it around, 4 bottles of beer on the wall."              
##  [7] "4 bottles of beer on the wall, 4 bottles of beer. Take one down and pass it around, 3 bottles of beer on the wall."              
##  [8] "3 bottles of beer on the wall, 3 bottles of beer. Take one down and pass it around, 2 bottles of beer on the wall."              
##  [9] "2 bottles of beer on the wall, 2 bottles of beer. Take one down and pass it around, 1 bottles of beer on the wall."              
## [10] "1 bottles of beer on the wall, 1 bottles of beer. Take one down and pass it around, 0 bottles of beer on the wall."              
## [11] "No more bottles of beer on the wall, no more bottles of beer. Go to the store and buy some more, 10 bottles of beer on the wall."
```

### Problem 4 {-}

It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:
``` {}
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```
How does this affect performance? Design and execute an experiment.


```r
data <- nycflights13::flights %>% 
  select(-year, -carrier, -tailnum, -origin, -dest, -time_hour)

library(microbenchmark)

# With preallocation
x <- vector("numeric", 13)
microbenchmark(for (i in seq_along(data))
  x[[i]] <- mean(data[[i]])
)
```

```
## Unit: milliseconds
##                                                  expr      min       lq
##  for (i in seq_along(data)) x[[i]] <- mean(data[[i]]) 120.7422 121.3531
##      mean   median       uq      max neval
##  122.4673 122.2494 123.1647 127.5896   100
```

```r
#  Without preallocation
y <- vector("numeric", 0)
microbenchmark(for (i in seq_along(data))
  y[[i]] <- mean(data[[i]])
)
```

```
## Unit: milliseconds
##                                                  expr      min       lq
##  for (i in seq_along(data)) y[[i]] <- mean(data[[i]]) 120.6699 121.3925
##      mean   median       uq      max neval
##  123.4028 122.1242 123.3345 181.5547   100
```

## 21.3 For loop variations {-}

### Problem 1 {-}

Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, files <- dir("data/", pattern = "\\.csv$", full.names = TRUE), and now want to read each one with read_csv(). Write the for loop that will load them into a single data frame.


```r
files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)
output <- vector("list", length(files))
for (i in seq_along(files)) {
  output[[i]] <- read_csv(files[[i]])
}
output <- purrr::flatten_df(output)
```

### Problem 2 {-}

What happens if you use for (nm in names(x)) and x has no names? 


```r
no_iris <- unname(iris)
no_iris <- no_iris[, 1:4]

# Leaves the list empty and does not throw an error.

y <- vector("list")
for (nm in names(no_iris)) {
  y[[nm]] <- mean(no_iris[[nm]])
}
y
```

```
## list()
```

What if only some of the elements are named? 


```r
# Returns a list of length 2 with the value of the named element. 

names(no_iris[[1]]) <- "Sepal.length"
y <- vector("list")
for (nm in names(no_iris)) {
  y[[nm]] <- mean(no_iris[[nm]])
}
y
```

```
## list()
```

What if the names are not unique?


```r
# Only returns the mean of the first element and does not throw an error.

names(no_iris) <- c("Sepal.length", "Sepal.length", "Sepal.length", "Sepal.length")
y <- vector("list")
for (nm in names(no_iris)) {
  y[[nm]] <- mean(no_iris[[nm]])
}
y
```

```
## $Sepal.length
## [1] 5.843333
```

### Problem 3 {-}

Write a function that prints the mean of each numeric column in a data frame, along with its name. For example, show_mean(iris) would print:

show_mean(iris)
#> Sepal.Length: 5.84
#> Sepal.Width:  3.06
#> Petal.Length: 3.76
#> Petal.Width:  1.20
(Extra challenge: what function did I use to make sure that the numbers lined up nicely, even though the variable names had different lengths?)


```r
print_means <- function(data) {
data <- select_if(data, is.numeric)

Means <- vector("double", length(data))
for (i in seq_along(data)) {
  Means[[i]] <- mean(data[[i]], na.rm = TRUE)
}

Names <- names(data)

print(as_data_frame(cbind(Names, Means)))
}
```

### Problem 4 {-}

What does this code do? How does it work?


```r
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}

# The list trans contains two functions: disp and am. Disp takes its input and  multiplies by .0163871 and am takes its input and creates a factor variable with the value "auto" or "manual." The for loop loops over the two functions in the list, uses the variables disp" and am in the dataset mtcars as inputs, and outputs a new mtcars dataset.
```

## 21.4 For loops vs. functionals {-}

### Problem 1 {-}

Read the documentation for apply(). In the 2d case, what two for loops does it generalise?


```r
# Unsure. 
```

### Problem 2 {-}

Adapt col_summary() so that it only applies to numeric columns You might want to start with an is_numeric() function that returns a logical vector that has a TRUE corresponding to each numeric column.


```r
# Original col_summary: 
col_summary <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}

# Adapted col_summary:
col_summary <- function(df, fun) {
  df <- select_if(df, is.numeric)
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}
```

## 21.5 The map functions {-}

### Problem 1

Write code that uses one of the map functions to compute the mean of every column in mtcars.


```r
mtcars %>% 
  map_dbl(mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500   3.780862 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500         NA   3.687500   2.812500
```

Write code that uses one of the map functions to determine the type of each column in nycflights13::flights


```r
nycflights13::flights %>% 
  map_chr(~str_c(class(.), collapse = ", "))
```

```
##              year             month               day          dep_time 
##         "integer"         "integer"         "integer"         "integer" 
##    sched_dep_time         dep_delay          arr_time    sched_arr_time 
##         "integer"         "numeric"         "integer"         "integer" 
##         arr_delay           carrier            flight           tailnum 
##         "numeric"       "character"         "integer"       "character" 
##            origin              dest          air_time          distance 
##       "character"       "character"         "numeric"         "numeric" 
##              hour            minute         time_hour 
##         "numeric"         "numeric" "POSIXct, POSIXt"
```

Write code that uses one of the map functions to compute the number of unique values in each column of iris


```r
iris %>% 
  map_int(~n_distinct(.))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

Write code that uses one of the map functions to generate 10 random normals for each of μ = -10, 0, 10, and 100. 


```r
x <- c(-10, 0, 10, 100)
x %>% 
  map(~rnorm(10, mean = .))
```

```
## [[1]]
##  [1]  -9.275358  -9.894666 -10.619987 -10.164058 -10.544812 -11.713057
##  [7]  -9.478906 -10.161219 -11.153040 -10.128712
## 
## [[2]]
##  [1] -0.17041746  0.86517151  1.32111538 -1.12463427 -2.11399801
##  [6] -0.78223786  1.24563929  0.06665328 -1.55818345  1.06671964
## 
## [[3]]
##  [1] 11.327863  8.396614 10.359517 11.316058  8.059115 10.157814  9.829151
##  [8]  9.539890  9.542242 11.838309
## 
## [[4]]
##  [1]  99.08590 102.12234  99.49221 100.07767 100.54440  99.11360  99.31003
##  [8]  99.28098  98.00330  99.34028
```

### Problem 2

How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?


```r
factor_fun <- function(data) {
  data %>%
    map_lgl(~is.factor(.))
}
```

### Problem 3

What happens when you use the map functions on vectors that aren’t lists? What does map(1:5, runif) do? Why?


```r
#The function iterates across each value, 1:5, in the vector and returns a list with five vectors containing between one and five random deviates:

map(1:5, runif)
```

```
## [[1]]
## [1] 0.8161435
## 
## [[2]]
## [1] 0.3758068 0.2859730
## 
## [[3]]
## [1] 0.69149642 0.34511628 0.07844706
## 
## [[4]]
## [1] 0.1451172 0.2624909 0.2905888 0.9175585
## 
## [[5]]
## [1] 0.3598436 0.1957438 0.1351827 0.8133851 0.9242730
```

```r
#If the vector 1:5 is a list, the function does not iterate across five values but rather takes one value, the length five vector, as its sole input. Since the input is not a single value, the map function uses the length of the vector as its input:
  
map(list(1:5), runif)
```

```
## [[1]]
## [1] 0.21493971 0.18366156 0.70680406 0.85090020 0.01070594
```

```r
# And as a result, the above function is equivalent to: 

map(list(5), runif)
```

```
## [[1]]
## [1] 0.27043688 0.93452846 0.41696199 0.04394776 0.72281121
```

```r
map(5, runif)
```

```
## [[1]]
## [1] 0.907441581 0.462734716 0.914114956 0.836442103 0.008625985
```

```r
map(list(5:9), runif)
```

```
## [[1]]
## [1] 0.62948239 0.23950448 0.67474887 0.09911762 0.35890088
```

### Problem 4

What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?


```r
map(-2:2, rnorm, n = 5)
```

```
## [[1]]
## [1] -2.64227765 -0.92196397 -2.42932793 -0.03068192 -3.02070309
## 
## [[2]]
## [1] -0.3226896 -1.9813397 -1.9522519 -1.1372016 -0.9186899
## 
## [[3]]
## [1] -1.3353828  1.1975488 -0.3371379 -1.3993751 -1.4023539
## 
## [[4]]
## [1] 0.8180347 1.5569307 1.3211730 0.4476104 2.2527676
## 
## [[5]]
## [1] 2.320296 1.273936 1.303491 0.973927 1.935582
```

```r
# The function above takes a length five vector from -2:2 and returns a list of five vectors of five observations along a normal distribution given mean values of -2, -1, 0, 1, and 2, respectively. It takes each value from -2:2 and returns a vector of five observations, looping over the entire vector and storing the vectors in a single list. 

#map_dbl(-2:2, rnorm, n = 5)

# The above function throws the error "Result 1 is not a length 1 atomic vector," because it is unable to store the output of the function as a single value in a vector. Map_dbl expects each iterative output to be a single value, but each iteration of this function is a vector of five observations. In order to return the function's output as a single vector, the map() function would need to be wrapped in unlist(): 

unlist(map(-2:2, rnorm, n = 5))
```

```
##  [1] -2.42109619 -1.97195458 -3.20126253 -2.58487353 -3.09380864
##  [6] -1.14798984 -0.80008515  0.45832876 -0.40936816 -2.06156555
## [11] -1.52731361  0.60919215 -1.70056847  0.01172178  0.22145957
## [16]  0.07884616  1.99542377  2.41429012  1.28563225  1.04332495
## [21]  2.82510369  2.36819671  2.23763944  1.44620218  3.04253771
```

### Problem 5

Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function.


```r
# map(x, ~lm(mpg ~ wt, data = .))
```

## 21.9 Other patterns of for loops {-}

### Problem 1

Implement your own version of every() using a for loop. Compare it with purrr::every(). What does purrr’s version do that your version doesn’t?


```r
x <- list(1:5, letters, list(10))


bens_every <- function(x, f) {
out <- vector("logical", 1)
for (. in seq_along(x))
  out <- all((f(.)))
out
}

bens_every(iris, is.character)
```

```
## [1] FALSE
```

```r
bens_every(iris, is.vector)
```

```
## [1] TRUE
```

```r
# Unlike dplyr's every(), my function does not support shortcuts for anonymous functions

iris %>% 
  select_if(is.numeric) %>% 
  every(~mean(.) > 3)
```

```
## [1] FALSE
```

### Problem 2

Create an enhanced col_sum() that applies a summary function to every numeric column in a data frame.


```r
col_summary <- function(df, fun) {
  df <- keep(df, is.numeric)
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[[i]] <- fun(df[[i]])
  }
  out
}

col_summary(iris, mean)
```

```
## [1] 5.843333 3.057333 3.758000 1.199333
```

### Problem 3

A possible base R equivalent of col_sum() is:

``` {}
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]

  sapply(df_num, f)
}
```

But it has a number of bugs as illustrated with the following inputs:

``` {}
df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)
# OK
boom col_sum3(df, mean)
# Has problems: don't always return numeric vector
col_sum3(df[1:2], mean)
col_sum3(df[1], mean)
#col_sum3(df[0], mean)
```

What causes the bugs?


```r
#The bug stems from sapply, which returns a list rather than a vector when given a dataframe with no variables. Here is a more consistent approach in base R: 

col_sum3 <- function(df, f) {
  is_num <- vapply(df, is.numeric, logical(1))
  df_num <- df[, is_num]

  sapply(df_num, f)
}

df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)

col_sum3(df[0], mean)
```

```
## named list()
```

```r
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



