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
be set. The `quote` argument can be set to a single quote instead of a 
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

Identify what is wrong with each of the following inline CSV files. What happens 
when you run the code?

* There are more data than columns, which results in a parsing failure. 
The extra data are dropped from the data frame:


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

* There is too little data in row 2 and too much data in row 3 - row 2 is filled 
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

* There are two variables but only one data point - `b` is filled in with a 
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

* Appears that the header was entered twice, so the data are parsed as character 
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

* `read_csv()` has a delimiter set to `,`, use `read_csv2()` instead:


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

## 11.3 - Parsing a Vector {-}

### Problem 1 {-}

What are the most important arguments to `locale()`?

The `date_names` argument provides useful defaults for a `locale()` object:


```r
dutch <- locale('nl')
japanese <- locale('ja')

str(dutch)
```

```
## List of 7
##  $ date_names   :List of 5
##   ..$ mon   : chr [1:12] "januari" "februari" "maart" "april" ...
##   ..$ mon_ab: chr [1:12] "jan." "feb." "mrt." "apr." ...
##   ..$ day   : chr [1:7] "zondag" "maandag" "dinsdag" "woensdag" ...
##   ..$ day_ab: chr [1:7] "zo" "ma" "di" "wo" ...
##   ..$ am_pm : chr [1:2] "a.m." "p.m."
##   ..- attr(*, "class")= chr "date_names"
##  $ date_format  : chr "%AD"
##  $ time_format  : chr "%AT"
##  $ decimal_mark : chr "."
##  $ grouping_mark: chr ","
##  $ tz           : chr "UTC"
##  $ encoding     : chr "UTF-8"
##  - attr(*, "class")= chr "locale"
```

```r
str(japanese)
```

```
## List of 7
##  $ date_names   :List of 5
##   ..$ mon   : chr [1:12] "1月" "2月" "3月" "4月" ...
##   ..$ mon_ab: chr [1:12] "1月" "2月" "3月" "4月" ...
##   ..$ day   : chr [1:7] "日曜日" "月曜日" "火曜日" "水曜日" ...
##   ..$ day_ab: chr [1:7] "日" "月" "火" "水" ...
##   ..$ am_pm : chr [1:2] "午前" "午後"
##   ..- attr(*, "class")= chr "date_names"
##  $ date_format  : chr "%AD"
##  $ time_format  : chr "%AT"
##  $ decimal_mark : chr "."
##  $ grouping_mark: chr ","
##  $ tz           : chr "UTC"
##  $ encoding     : chr "UTF-8"
##  - attr(*, "class")= chr "locale"
```

Be sure to read the full documentation for `locale()`. Common data import issues 
can probably be solved with `decimal_mark`, `grouping_mark`, and/or `encoding`.

### Problem 2 {-}

What happens if you try and set `decimal_mark` and `grouping_mark` to the same 
character? What happens to the default value of `grouping_mark` when you set 
`decimal_mark` to `“,”`? What happens to the default value of `decimal_mark` when 
you set the grouping_mark to `“.”`?

`locale()` requires that `decimal_mark` and `grouping_mark` be different:


```r
x <- locale(decimal_mark = '.', grouping_mark = '.')
```

```
## Error: `decimal_mark` and `grouping_mark` must be different
```

Setting `decimal_mark` to `,` will automatically update `grouping_mark` to `.`. 
Similarly setting `grouping_mark` to `.` will automatically update `decimal_mark` 
to `,`:


```r
x <- locale(decimal_mark = ',')
x$grouping_mark
```

```
## [1] "."
```

```r
y <- locale(grouping_mark = '.')
y$decimal_mark
```

```
## [1] ","
```

### Problem 3 {-}

I didn’t discuss the `date_format` and `time_format` options to `locale()`. 
What do they do? Construct an example that shows when they might be useful.

A specific `date_format` and `time_format` structure can be specified in a 
`locale()`. This can be useful for formatting data with non-standard formatting:


```r
parse_date('June/7/90', locale = locale(date_format = '%B/%d/%y'))
```

```
## [1] "1990-06-07"
```

```r
parse_time('1:15PM', locale = locale(time_format = '%I:%M%p'))
```

```
## 13:15:00
```

### Problem 4 {-}

If you live outside the US, create a new locale object that encapsulates the 
settings for the types of file you read most commonly.

Create a locale with the time zone updated:


```r
str(locale(tz = 'US/Eastern'))
```

```
## List of 7
##  $ date_names   :List of 5
##   ..$ mon   : chr [1:12] "January" "February" "March" "April" ...
##   ..$ mon_ab: chr [1:12] "Jan" "Feb" "Mar" "Apr" ...
##   ..$ day   : chr [1:7] "Sunday" "Monday" "Tuesday" "Wednesday" ...
##   ..$ day_ab: chr [1:7] "Sun" "Mon" "Tue" "Wed" ...
##   ..$ am_pm : chr [1:2] "AM" "PM"
##   ..- attr(*, "class")= chr "date_names"
##  $ date_format  : chr "%AD"
##  $ time_format  : chr "%AT"
##  $ decimal_mark : chr "."
##  $ grouping_mark: chr ","
##  $ tz           : chr "US/Eastern"
##  $ encoding     : chr "UTF-8"
##  - attr(*, "class")= chr "locale"
```

### Problem 5 {-}

What’s the difference between `read_csv()` and `read_csv2()`?

`read_csv()` has a delimiter set to `,` while `read_csv2` is set to `;`, as 
some countries use `,` as the `decimal_mark`:


```r
read_csv('a,b\n1,2')
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <int>
## 1     1     2
```

```r
read_csv2('a;b\n1;2')
```

```
## Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <int>
## 1     1     2
```

### Problem 6 {-}

What are the most common encodings used in Europe? What are the most common 
encodings used in Asia? Do some googling to find out.

See [this list](https://en.wikipedia.org/wiki/Character_encoding#Common_character_encodings) 
of common encodings (via Wikipedia). 

### Problem 7 {-}

Generate the correct format string to parse each of the following dates and times:


```r
d1 <- "January 1, 2010"
d2 <- "2015-Mar-07"
d3 <- "06-Jun-2017"
d4 <- c("August 19 (2015)", "July 1 (2015)")
d5 <- "12/30/14" # Dec 30, 2014
t1 <- "1705"
t2 <- "11:15:10.12 PM"
```

Build up a datetime format using the pieces described in the chapter:


```r
parse_date(d1, format = '%B %d, %Y')
```

```
## [1] "2010-01-01"
```

```r
parse_date(d2, format = '%Y-%b-%d')
```

```
## [1] "2015-03-07"
```

```r
parse_date(d3, format = '%d-%b-%Y')
```

```
## [1] "2017-06-06"
```

```r
parse_date(d4, format = '%B %d (%Y)')
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
parse_date(d5, format = '%m/%d/%y')
```

```
## [1] "2014-12-30"
```

```r
parse_time(t1, format = '%H%M')
```

```
## 17:05:00
```

```r
parse_time(t2, format = '%I:%M:%OS %p')
```

```
## 23:15:10.12
```

