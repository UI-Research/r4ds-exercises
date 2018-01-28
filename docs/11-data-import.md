# Chapter 11 - Data Import {-}

Load the libraries needed for these exercises.




```r
library(tidyverse)
```

## 11.2 - Getting Started {-}

### Problem 1 {-}

What function would you use to read a file where fields were separated with
“|”?

Use `read_delim()`, using `|` as the delimiter:


```r
data <- 'a|b|c\n1|2|3'
read_delim(data, delim = '|')
```

```
## # A tibble: 1 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
```

### Problem 2 {-}

Apart from `file`, `skip`, and `comment`, what other arguments do `read_csv()` 
and `read_tsv()` have in common?

`read_csv()` and `read_tsv()` are essentially just `read_delim` with the 
delimiter preset to either a comma or a tab. All of their other arguments are 
the same.

### Problem 3 {-}

What are the most important arguments to `read_fwf()`?

The most important argument to `read_fwf()` is `col_positions`, as this 
determines how data is read:


```r
fwf_sample <- readr_example("fwf-sample.txt")
cat(read_lines(fwf_sample))
```

```
## John Smith          WA        418-Y11-4111 Mary Hartford       CA        319-Z19-4341 Evan Nolan          IL        219-532-c301
```

```r
read_fwf(fwf_sample, fwf_widths(c(20, 10, 12), c("name", "state", "ssn")))
```

```
## Parsed with column specification:
## cols(
##   name = col_character(),
##   state = col_character(),
##   ssn = col_character()
## )
```

```
## # A tibble: 3 x 3
##            name state          ssn
##           <chr> <chr>        <chr>
## 1    John Smith    WA 418-Y11-4111
## 2 Mary Hartford    CA 319-Z19-4341
## 3    Evan Nolan    IL 219-532-c301
```

```r
read_fwf(fwf_sample, fwf_widths(c(5, 10, 12), c("name", "state", "ssn")))
```

```
## Parsed with column specification:
## cols(
##   name = col_character(),
##   state = col_character(),
##   ssn = col_character()
## )
```

```
## # A tibble: 3 x 3
##    name    state   ssn
##   <chr>    <chr> <chr>
## 1  John    Smith    WA
## 2  Mary Hartford    CA
## 3  Evan    Nolan    IL
```

### Problem 4 {-}

Sometimes strings in a CSV file contain commas. To prevent them from causing 
problems they need to be surrounded by a quoting character, like `"` or `'`. 
By convention, `read_csv()` assumes that the quoting character will be `"`, 
and if you want to change it you’ll need to use `read_delim()` instead. 
What arguments do you need to specify to read the following text 
into a data frame?

`"x,y\n1,'a,b'"`

Since `read_delim()` must be used instead of `read_csv()`, the delimiter must 
be set. The `quote` argument can then be set to a single quote instead of a 
double quote:


```r
data <- "x,y\n1,'a,b'"
read_delim(data, delim = ',', quote = '\'')
```

```
## # A tibble: 1 x 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

### Problem 5 {-}

dentify what is wrong with each of the following inline CSV files. What happens 
when you run the code?

There are more data than columns, which results in a parsing failure. The extra 
data are dropped from the data frame:


```r
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col  expected    actual         file expected   <int> <chr>     <chr>     <chr>        <chr> actual 1     1  <NA> 2 columns 3 columns literal data file 2     2  <NA> 2 columns 3 columns literal data
```

```
## # A tibble: 2 x 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

There is too little data in row 2 and too much data in row 3 - row 2 is filled 
in with a missing value while row 3 drops data:


```r
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col  expected    actual         file expected   <int> <chr>     <chr>     <chr>        <chr> actual 1     1  <NA> 3 columns 2 columns literal data file 2     2  <NA> 3 columns 4 columns literal data
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

There are two variables but only one data point - `b` is filled in with a 
missing value:


```r
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col                     expected    actual         file expected   <int> <chr>                        <chr>     <chr>        <chr> actual 1     1     a closing quote at end of file           literal data file 2     1  <NA>                    2 columns 1 columns literal data
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

Appears that the header was entered twice, so the data are parsed as character 
instead of a string. Or if the goal here was to enter a missing value `NA`, 
note that the `n` was processed as a new line `\n`. 


```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 x 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
read_csv("a,b\n1,2\nna,b")
```

```
## # A tibble: 2 x 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2    na     b
```

`read_csv()` has a delimiter set to `,`, use `read_csv2()` instead:


```r
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 x 1
##   `a;b`
##   <chr>
## 1   1;3
```

```r
read_csv2("a;b\n1;3")
```

```
## Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <int>
## 1     1     3
```

