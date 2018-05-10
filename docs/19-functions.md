# Chapter 19 - Functions {-}

Load the libraries needed for these exercises.




```r
library(tidyverse)
```

## 19.2 - When should you write a function? {-}

### Problem 1 {-}

Why is `TRUE` not a parameter to `rescale01()`? What would happen if `x` contained a single missing value, and `na.rm` was FALSE.

`TRUE` is not a paramater to `rescale01()` because it does not vary from function call to function call. 

If `x` contained a single missing value, and `na.rm` was FALSE then `rescale01()` would return a vector of NAs with the length of X. 

### Problem 2 {-}

In the second variant of `rescale01()`, infinite values are left unchanged. Rewrite `rescale01()` so that -Inf is mapped to zero and Inf is mapped to 1. 


```r
rescale01 <- function(x) {
  ifelse(x == Inf, max(x), x)
  rng <- range(x, na.rm = TRUE, finite = TRUE)
  x <- (x - rng[1]) / (rng[2] - rng[1])
  x <- ifelse(x == Inf, 1, x)
  ifelse(x == -Inf, 0, x)
}

rescale01(c(Inf, 2, 3, 1))
```

```
## [1] 1.0 0.5 1.0 0.0
```

### Problem 3 {-}

Practice turning the following code snippets into functions. Think about what each function does. What would you call it? How many arguments does it need? Can you rewrite it to be more expressive or less duplicative?


```r
missing_share <- function(x) {
  mean(is.na(x))
}

percent <- function(x) {
  x / sum(x, na.rm = TRUE)
}

coef_of_variation <- function(x) {
 sd(x, na.rm = TRUE) / mean(x, na.rm = TRUE)
}
```

### Problem 4 {-}

Follow *http://nicercode.github.io/intro/writing-functions.html* to write your own functions to compute the variance and skew of a numeric vector. 

$var(x) = \frac{1}{n-1}\sum_{i=1}^{n}{(x_{i}-\bar{x})^2}$


```r
variance <- function(x) {
  # remove missing values
  x <- x[!is.na(x)]
  # calculate the mean
  xbar <- mean(x)
  # calculate n
  n <- length(x)
  # calculate square errors
  square_errors <- (x - xbar) ^ 2
  # calculate variance
  sum(square_errors) / (n - 1)
}
```

Sample skewness is the ratio of the third moment about the mean to the second moment about the mean raised to the power of $\frac{3} {2}$.

$skew(x) = \frac{n\sqrt{n-1}}{n-2} \frac{\sum_{i=1}^{n}(x_{i}-\bar{x})^3} {(\sum_{i=1}^{n}(x_{i}-\bar{x})^2)^{3/2}}$


```r
skew <- function(x) {
  # remove missing values
  x <- x[!is.na(x)]  
  # calculate n
  n <- length(x)
  # calculate variance 
  v <- var(x)
  # calculate xbar
  xbar <- mean(x)
  # calculate the third moment about the mean
  third.moment <- (1/(n - 2)) * sum((x - xbar)^3)
  # calculate the ratio of the third moment about the mean to the second moment about the mean raised to the power of 3/2
  third.moment / (var(x) ^ (3 / 2))
}
```

### Problem 5 {-}

Write `both_na()`, a function that takes two vectors of the same length and returns the number of positions than have an `NA` in both vectors.


```r
both_na <- function(x, y) {
  # test condition that both observation are missing values
  missing_pairs <- is.na(x) & is.na(y)
  # sum up missing pairs
  sum(missing_pairs)
}
```

### Problem 6 {-}

What do the following functions do? Why are they useful even though they are short?

`is_directory <- function(x) file.info(x)$isdir`

This function returns a boolean for if the specified file is a directory or not. `file.info()` returns a lot of information and this function is useful because it extracts a simple subset of that infromation. 

`is_readable <- function(x) file.access(x, 4) == 0`

This function returns a boolean for if the specified file is readable or not. It's useful because mode doesn't need to be specified in the function call while it is needed for `file.access()`. Additionally, this function extracts and summarizes an interesting subset of the information from `file.access()`.

### Problem 7 {-}

Read the complete lyrics (http://bit.ly/littlebunnyfoofoo) to "Little Bunny Foo Foo." There's a lot of duplication in this song. Extend the piping example to recreate the complete song, and use functions to reduce the duplication. 

```

warning <- function(chance) {
  if (chance == 3) {
    goon(foo_foo, from = good_fairy, to = foo_foo)
  }
}

mouse_bop <- function() {
  foo_foo %>%
    hop(through = forest) %>%
    scoop(up = field_mouse) %>%
    bop(on = head)
}

down_came(good_fairy)
said(audience = foo_foo,
     statement = "Little bunny Foo Foo
                  I don't want to see you
                  Scooping up the field mice
                  And bopping them on the head.
                  I'll give you three chances,
                  And if you don't behave, I will turn you into a goon!")

mouse_bop()
warning(1)
mouse_bop()
warning(2)
mouse_bop()
warning(3)
end()
```

## 19.3 - Functions are for humans and computers {-}

### Problem 1 {-}

Read the source code for each of the following three functions, puzzle out what they do, and brainstorm better names:


```r
f1 <- function(string, prefix) {
  substr(string, 1, nchar(prefix)) == prefix
}
```

`is_prefix()`


```r
f2 <- function(x) {
  if (length(x) <= 1) return(NULL)
  x[-length(x)]
}
```

`drop_last()`


```r
f3 <- function(x, y) {
  rep(y, length.out = length(x)) 
}
```

`match_length()`

### Problem 2 {-}

Take a function that you've written and spend five minutes brainstorming a better name for it and its arguments. 

### Problem 3 {-}

Compare and contrast `rnorm()` and `MASS::mvrnorm()`. How can you make them more consistent?

mvrnorm(n = 1, mu, Sigma, tol = 1e-6, empirical = FALSE, EISPACK = FALSE)
rnorm(n, mean = 0, sd = 1)

* Match the argument names so both functions use greek names or English names. 
* Set common default arguments for both functions. `mvrnorm()` has a default value for `n` but not for `mu` or `Sigma`, and `rnorm()` has default values for `mean` and `sd` but not `n`. 

### Problem 4 {-}

Make a case for why `norm_r()`, `norm_d()`, etc., would be better than `rnorm()`, `dnorm()`. Make a case for the opposite. 

*For:* Families of functions should start with the same prefix. In this case, norm is the family of function. 

*Against:* Sometimes it is more useful to search based on the use of the distribution (i.e. "r", "d") than the type of distribution. The current function names allow for search of a constant use across different distributions. 

## 19.4 - Conditional execution {-}

### Problem 1 {-}

What’s the difference between `if` and `ifelse()`? Carefully read the help and construct three examples that illustrate the key differences.

`if()` evaluates to a single value for the expression evaluated. `ifelse()` returns a value with the same shape as the argument test. `ifelse()` can return a vector with length greater than one that is a combination of `TRUE`s and `FALSE`s. 


```r
x <- c(1, 2, 3)

if (x > 0) {"yes"}
```

```
## [1] "yes"
```

```r
if (x > 1) {"yes"}
ifelse(x > 1, "yes", "no")
```

```
## [1] "no"  "yes" "yes"
```

### Problem 2 {-}

Write a greeting function that says “good morning”, “good afternoon”, or “good evening”, depending on the time of day. (Hint: use a time argument that defaults to lubridate::now(). That will make it easier to test your function.)


```r
library(lubridate)

greeting <- function() {
  if (now() < today() + hms("12:00:00 EST")) {
    "good morning"
    } else if (now() < today() + hms("18:00:00 EST")) {
    "good afternoon"
    } else {
    "good evening"
  }
}
```

### Problem 3 {-}

Implement a fizzbuzz function. It takes a single number as input. If the number is divisible by three, it returns “fizz”. If it’s divisible by five it returns “buzz”. If it’s divisible by three and five, it returns “fizzbuzz”. Otherwise, it returns the number. Make sure you first write working code before you create the function.


```r
fizzbuzz <- function(x) {
  if (near(x %% 3, 0) && near(x %% 5, 0)) {
    "fizzbuzz"
  } else if (near(x %% 3, 0)) {
    "fizz" 
  } else if (near(x %% 5, 0)) {
    "buzz"
  } else {
    x
  }
}  

fizzbuzz(3)
```

```
## [1] "fizz"
```

```r
fizzbuzz(5)
```

```
## [1] "buzz"
```

```r
fizzbuzz(15)
```

```
## [1] "fizzbuzz"
```

```r
fizzbuzz(16)
```

```
## [1] 16
```

### Problem 4 {-}

How could you use cut() to simplify this set of nested if-else statements?

```
if (temp <= 0) {
  "freezing"
} else if (temp <= 10) {
  "cold"
} else if (temp <= 20) {
  "cool"
} else if (temp <= 30) {
  "warm"
} else {
  "hot"
}
```


```r
temps <- c(-10, 15, 40)

cut(temps, c(-Inf, 0, 10, 20, 30, Inf), labels = c("freezing", "cold", "cool", "warm", "hot"))
```

```
## [1] freezing cool     hot     
## Levels: freezing cold cool warm hot
```

How would you change the call to cut() if I’d used < instead of <=? What is the other chief advantage of cut() for this problem? (Hint: what happens if you have many values in temp?)

Add `right = FALSE` as an argument to the function so the intervals are closed on the left instead of closed on the right. `cut()` is advantageous because it is vectorized. 

### Problem 5 {-}

What happens if you use `switch()` with numeric values?

It throws back an error. `switch()` evaluates EXPR and accordingly chooses one of the further arguments (in ...). Arguments can't be numbers. 

### Problem 6 {-}

What does this `switch()` call do? What happens if `x` is “e”?

`a` evaluates to the subsequent argument and returns "ab". `c` evaluates to the subsequent argument and returns "cd". `e` returns nothing because it isn't present in `switch()`. 

## 19.5 - Function arguments {-}

### Problem 1 {-}

What does commas(letters, collapse = "-") do? Why?

It returns an error because the argument `collapse` has multiple values. 

### Problem 2 {-}

It’d be nice if you could supply multiple characters to the pad argument, e.g. rule("Title", pad = "-+"). Why doesn’t this currently work? How could you fix it?

This works now. I guess `library(stringr)` resolved this. 


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}
rule("Important output", pad = "+-")
```

```
## Important output +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-
```

### Problem 3 {-}

What does the `trim` argument to `mean()` do? When might you use it?

`trim` is the proportion of observations (0 to 0.5) to be dropped from each end of x before the mean is computed. It is useful with the extreme values on each of the distribution are outliers or are noisy. 

### Problem 4 {-}

The default value for the `method` argument to `cor()` is `c("pearson", "kendall", "spearman")`. What does that mean? What value is used by default?

It means the possible methods are `"pearson", "kendall", "spearman"`. It defaults to `"pearson"` for the Pearson correlation coefficient. 

