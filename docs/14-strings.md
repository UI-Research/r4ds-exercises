# Chapter 14 - Strings {-}




```r
library(tidyverse)
```

## 14.2 String basics {-}

### Problem 1 {-}

In code that doesn't use `stringr`, you'll often see `paste()` and `paste0()`. What's the difference between the two functions? What `stringr` function are they equivalent to? How do the functions differ in their handling of NA?

`paste()` automatically includes a space between each character string it combines. `paste0()` does not include a space. They are ~equivalent to `str_c()` from `library(stringr)`. `paste()` and `paste0()` include NA as text. `str_c()` returns an NA for the entire string if the string contains an NA. 

### Problem 2 {-}

In your own words, describe the difference between the `sep` and `collapse` arguments to `str_c()`.

`sep` is a character string to insert between input vectors. Its input vector and output vector always have the same length. 


```r
length(str_c("Letter", letters, sep = ": "))
```

```
## [1] 26
```

`collapse` is a character string to insert between input vectors and to turn the vector into a single string. `collapse` always returns a vector with length one. 


```r
length(str_c("Letter", letters, collapse = ": "))
```

```
## [1] 1
```

### Problem 3 {-}

Use `str_length()` and `str_sub()` to extract the middle character from a string. What will you do if the string has an even number of characters. 


```r
string_middle <- function(string) {
  string_length <- str_length(string)
  
  if (string_length %% 2 == 1) {
    str_sub(string, floor((string_length + 1) / 2), ceiling((string_length) / 2))
  } 
  else if (string_length %% 2 == 0) {
    NULL
  } 
  else {"Error!"}
}

string_middle("abc")
```

```
## [1] "b"
```

```r
string_middle("abcd")
```

```
## NULL
```

It returned a NULL if `string_length()` is even. 

### Problem 4 {-}

What does `str_wrap()` do? When might you want to use it?

It implements the Knuth-Plass paragraph wrapping algorithm. It "breaks text paragraphs into lines, of total width - if it is possible - of at most given `width`.


```r
graph <- "It implements the Knuth-Plass paragraph wrapping algorithm. It breaks text paragraphs into lines, of total width - if it is possible - of at most given width."

str_wrap(graph, width = 20)
```

```
## [1] "It implements\nthe Knuth-Plass\nparagraph wrapping\nalgorithm. It breaks\ntext paragraphs\ninto lines, of total\nwidth - if it is\npossible - of at\nmost given width."
```

This could be useful for formatting in html and rmarkdown. Especially for graphics and sidebars. Custom width is useful - especially in reproducible documents. 

### Problem 5 {-}

What does `str_trim()` do? What's the opposite of `str_trim()`?

It trims whitespace from the left, right, or both sides of a character string. It is the string version of `trimws()`.

`str_pad()` is the opposite of `str_trim()`. It adds whitespace to the left, right, or both sides of a character string. 

### Problem 6 {-}

Write a function that turns (e.g.) a vector `c("a", "b", "c")` into a string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2. 


```r
list_maker <- function(string) {
  
  if (length(string) > 1) {
  stringa <- string[1:length(string)-1]
  stringb <- string[length(string)]
  
  stringa <- str_c(stringa, collapse = ", ")
  
  str_c(stringa, ", and ", stringb, collapse = "")
  } else {
    string
    }
  }

string <- c("a", "b", "c", "d", "e")

list_maker(string)
```

```
## [1] "a, b, c, d, and e"
```

14.3 - Matching patterns with regular expressions {-}

### Problem 1 {-}

Explain why each of these strings don't match a \: "\", "\\", "\\\".

* "\" escapes the quotation mark and isn't a valid character string in R.
* "\\" returns a character string with two backslashes which doesn't match one  backslash. 
* "\\\" escapes the quotation mark and isn't a valid character string.

### Problem 2 {-}

How would you match the sequence "'\?



### Problem 3 {-}

What patterns will the regular expression \..\..\.. match? How would you represent it as a string?

It will match a string of three periods separated by characters. `\\..\\..\\..`.


```r
str_view(".a.b.c", "\\..\\..\\..")
```

<!--html_preserve--><div id="htmlwidget-ec3457c491945cdbd0df" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ec3457c491945cdbd0df">{"x":{"html":"<ul>\n  <li><span class='match'>.a.b.c<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.3.2 - Anchors {-}

### Problem 1 {-}

How would you match the literal string "$^$"?


```r
x <- "$^$"
str_view(x, "\\$\\^\\$")
```

<!--html_preserve--><div id="htmlwidget-97c1d7b759b01c904406" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-97c1d7b759b01c904406">{"x":{"html":"<ul>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Problem 2 {-}

Given the corpus of common words in stringr::words, create regular expressions that will find all words that:

1. Start with "y"


```r
str_view(words, "^y", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-d899324b487e703c6ea5" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-d899324b487e703c6ea5">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. End with "x"


```r
table(str_detect(words, "x$"))
```

```
## 
## FALSE  TRUE 
##   976     4
```

3. Are exactly three letters long. (Don't cheat by using `str_length()`!)


```r
table(str_detect(words, "^...$"))
```

```
## 
## FALSE  TRUE 
##   870   110
```

4. Have seven letters or more.


```r
table(str_detect(words, "^......."))
```

```
## 
## FALSE  TRUE 
##   761   219
```

## 14.3.2 - Character classes and alternatives {-}

### Problem 1 {-}

Create regular expressions to find all words that:

1. Start with a vowel


```r
str_view(words[1:10], "^[aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-ddcd33490297c735d808" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ddcd33490297c735d808">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span><\/li>\n  <li><span class='match'>a<\/span>ble<\/li>\n  <li><span class='match'>a<\/span>bout<\/li>\n  <li><span class='match'>a<\/span>bsolute<\/li>\n  <li><span class='match'>a<\/span>ccept<\/li>\n  <li><span class='match'>a<\/span>ccount<\/li>\n  <li><span class='match'>a<\/span>chieve<\/li>\n  <li><span class='match'>a<\/span>cross<\/li>\n  <li><span class='match'>a<\/span>ct<\/li>\n  <li><span class='match'>a<\/span>ctive<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Only contain consonants. (Hint: think about match "not"-vowels.)


```r
str_view(words, "^[^aeiou]+$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-98e6c73f374a40ca4444" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-98e6c73f374a40ca4444">{"x":{"html":"<ul>\n  <li><span class='match'>by<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

I'm not sure if this can be done with `+` which is introduced on page 204 after the exercises. 

3. End with ed, but not with eed.


```r
str_view(words, "[^e]ed$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-360b0ec8a01249b7cfca" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-360b0ec8a01249b7cfca">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

4. End with ing or ize. 


```r
str_view(words, "ing$|ize$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-4d5a413b8b0b35c30eda" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-4d5a413b8b0b35c30eda">{"x":{"html":"<ul>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>organ<span class='match'>ize<\/span><\/li>\n  <li>recogn<span class='match'>ize<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>s<span class='match'>ize<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Problem 2 {-}

Empirically verify the rule "i before e except after c."

Let's try this with proof by contradiction. We need to look for two conditions:

* ie after c
* ei 


```r
str_view(words, "ei|[c]ie", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-24a1af46ce9cf12ca7c6" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-24a1af46ce9cf12ca7c6">{"x":{"html":"<ul>\n  <li><span class='match'>ei<\/span>ght<\/li>\n  <li><span class='match'>ei<\/span>ther<\/li>\n  <li>rec<span class='match'>ei<\/span>ve<\/li>\n  <li>s<span class='match'>cie<\/span>nce<\/li>\n  <li>so<span class='match'>cie<\/span>ty<\/li>\n  <li>w<span class='match'>ei<\/span>gh<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Six words violate the rules. "i before e except after c" is and always will be rubbish. 

### Problem 3 {-}

Is "q" always followed by a "u"?

Proof by contradiction: look for a "q" not followed by a "u".


```r
str_view(words, "q^[u]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-995c934b98f3b85564a3" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-995c934b98f3b85564a3">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Yes, "q" is always followed by a "u" in this data set. 

### Problem 4 {-}

Write a regular expression that matches a word if it's probably written in British English, not American English. 


```r
str_view(words, "our|ise|ogue", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-98c6447b1db0f0c552af" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-98c6447b1db0f0c552af">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>col<span class='match'>our<\/span><\/li>\n  <li>c<span class='match'>our<\/span>se<\/li>\n  <li>c<span class='match'>our<\/span>t<\/li>\n  <li>enc<span class='match'>our<\/span>age<\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>fav<span class='match'>our<\/span><\/li>\n  <li>f<span class='match'>our<\/span><\/li>\n  <li>h<span class='match'>our<\/span><\/li>\n  <li>lab<span class='match'>our<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>res<span class='match'>our<\/span>ce<\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Problem 5 {-}

Create a regular expression that will match telephone numbers as commonly written in your country. 


```r
phone <- c("212-555-7891", "(212)-555-7891")

str_view(phone, "\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d|\\(\\d\\d\\d\\)-\\d\\d\\d-\\d\\d\\d\\d", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c6efd67943d99b58fe3d" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c6efd67943d99b58fe3d">{"x":{"html":"<ul>\n  <li><span class='match'>212-555-7891<\/span><\/li>\n  <li><span class='match'>(212)-555-7891<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.3.4 - Repetition {-}

### Problem 1 {-}

Describe the equivalents of ?, +, and * in {n, m} form.

* `? == {1}`
* `+ == {1,}`
* `* == {0,}`

### Problem 2 {-}

Describe in words what these regular expressions match (read carefully to if I'm using a regular expression or a string that defines a regular expressions):

1. `^.*$` matches an entire string. `^` matches the start of a string. `.` is any character which is repeated 0 or more times with `*`. `$` matches the end of a string. 
2. `"\\{.+\\}"`
3.`\d{4}-\d{2}-\d{2}` matches exactly 4 digits followed by a dash followed by exactly two digits followed by a dash followed by exactly two digits. This is the same as the ISO8601 date international standard. 
4. `\\\\{4}` matches exactly four backslashes. 

### Problem 3 {-}

Create regular expressions to find all words that:

1. Start with three consonants.


```r
string <- c("scratch", "apple")
str_view(string, "^[^aeiou]{3}")
```

<!--html_preserve--><div id="htmlwidget-ea3817c3313a200f3e10" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ea3817c3313a200f3e10">{"x":{"html":"<ul>\n  <li><span class='match'>scr<\/span>atch<\/li>\n  <li>apple<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Have three or more vowels in a row.


```r
string <- c("scratch", "aaapple")
str_view(string, "^[aeiou]{3,}")
```

<!--html_preserve--><div id="htmlwidget-ea852ad3a42c2c36eee6" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ea852ad3a42c2c36eee6">{"x":{"html":"<ul>\n  <li>scratch<\/li>\n  <li><span class='match'>aaa<\/span>pple<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

3. Have two or more vowel consonant pairs in a row.


```r
string <- c("banana", "coconut")
str_view(string, "([aeiou][^aeiou]){2,}")
```

<!--html_preserve--><div id="htmlwidget-54c2d4c42fdf8e5c8796" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-54c2d4c42fdf8e5c8796">{"x":{"html":"<ul>\n  <li>b<span class='match'>anan<\/span>a<\/li>\n  <li>c<span class='match'>oconut<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Problem 4 {-}

Solve the beginner regexp crosswords at http://regexcrossword.com/challenges/beginner

## 14.3.5 - Grouping and backreferences {-}

### Problem 1 {-}

Describe in words what these expressions will match:

1. `(.)\1\1` will match any string of three repeated letters or symbols. 
2. `"(.)(.)\\2\\1"` will match a four letter palindrome (spelled the same forwards and backwards).
3. `(..)\1` will match a four letter string where the second half is a reptition of the first half. 
4. `"(.).\\1.\\1"` will match and repetition of the same character three times where each character is spearated by a character (ex. "ababa" and "&&&&&").
5. `"(.)(.)(.)*\\3\\2\\1"` will match a string of characters where the first three characters are repeted in reverse and the middle character can be repeated multiple times (ex. "abccba" and "abcccccba").

### Problem 2 {-}

##### 2. Construct regular expressions to match words that:
 1. Start and end with the same character. `"^(.).*\\1$"`
 2. Contain a repeated pair of letters (e.g. "church" contains "ch" repeated twice) `".*(..).*\\1.*"`
 3. Contain one letter repeated in at least three places (e.g., "eleven" contains three "e"s). `".*(.).*\\1.*\\1.*"`


## 14.4 Tools {-}

### Problem 1 {-}

For each of the following challenges, try solving it by using both a singular regular expression, and a combination of multiple `str_detect()` calls:
  1. Find all words that start of end with x. `str_detect(words, "^x.*x$")` & `str_detect(str_detect(words, "^x"), "x$")`
  2. Find all words that start with a vowel and end with a consonant. `str_detect(words, "^[aeiou].*[^aeiou]$")` & `str_detect(str_detect(words, "^[aeiou]"), "[^aeiou]$")`
  3. Are there any words that contain at least one of each different vowel? TODO(aaron): hmm?
  4. What word has the highest number of vowels? What word has the highest proportion of vowels? (Hint: what is the denominator)
  

```r
as_tibble(words) %>%
  mutate(vowels = str_count(value, "[aeiou]")) %>%
  filter(vowels == max(vowels))
```

```
## # A tibble: 8 x 2
##   value       vowels
##   <chr>        <int>
## 1 appropriate      5
## 2 associate        5
## 3 available        5
## 4 colleague        5
## 5 encourage        5
## 6 experience       5
## 7 individual       5
## 8 television       5
```


```r
as_tibble(words) %>%
  mutate(letters = str_count(value), 
         vowels = str_count(value, "[aeiou]"),
         proportion = vowels / letters) %>%
  filter(proportion == max(proportion))
```

```
## # A tibble: 1 x 4
##   value letters vowels proportion
##   <chr>   <int>  <int>      <dbl>
## 1 a           1      1       1.00
```

## 14.4.3 - Extract Matches {-}

### Problem 1 {-}

In the previous example, you might have noticed that the regular expression matched "flickered", which is not a color. Modify the regex to fix the problem. 


```r
  colors <- "\\b(red|orange|yellow|green|blue|purple)\\b"
  more <- sentences[str_count(sentences, colors) > 1]
  str_view_all(more, colors)
```

<!--html_preserve--><div id="htmlwidget-bdf570f5af2d2f691a69" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-bdf570f5af2d2f691a69">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or <span class='match'>red<\/span> ink.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span> <span class='match'>red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Problem 2 {-}

From the Harvard sentences data, extract:
  1. The first word from each sentence.
  `str_extract(sentences, "[^\\s]*")`
  2. All words ending in ing. 
  `str_extract_all(sentences, "\\b[^\\s]*ing\\b")`
  3. All plurals.
  TODO(aaron): hmm?
  

## 14.4.4 Grouped matches {-}

### Problem 1 {-}

Find all words that come after a "number" like "one", "two", "three", etc. Pull out the number and the word.


```r
numbers <- "([Oo]ne|[Tt]wo|[Tt]hree|[Ff]our|[Ff]ive|[Ss]ix|[Ss]even|[Ee]ight|[Nn]ine|[Tt]en) ([^ ]+)"
tibble(sentence = sentences) %>%
  extract(
    sentence, c("number", "word"), numbers,
    remove = FALSE
  ) %>% 
  filter(!is.na(number))
```

```
## # A tibble: 46 x 3
##    sentence                                    number word   
##    <chr>                                       <chr>  <chr>  
##  1 Rice is often served in round bowls.        ten    served 
##  2 Four hours of steady work faced us.         Four   hours  
##  3 Two blue fish swam in the tank.             Two    blue   
##  4 Lift the square stone over the fence.       one    over   
##  5 The rope will bind the seven books at once. seven  books  
##  6 The two met while playing on the sand.      two    met    
##  7 There are more than two factors here.       two    factors
##  8 He lay prone and hardly moved a limb.       one    and    
##  9 Ten pins were set in order.                 Ten    pins   
## 10 Type out three lists of orders.             three  lists  
## # ... with 36 more rows
```

### Problem 2 {-}

Find all contractions. Separate out the pieces before and after the apostrophe. 

`"[^ ]*'[^ ]*"` could be used, but it returns possessive nouns. The following string of regular expressions gets around this problem. 


```r
contractions <- "[^ ]*'m|[^ ]*n't|[^ ]*'ve|[^ ]*'d|[^ ]*'re|[^ ]*'ll|[Ll]et's|[Ss]he's|[Hh]e's"
tibble(sentence = sentences) %>%
  mutate(contraction = str_extract(sentences, contractions)) %>%
  filter(!is.na(contraction)) %>%
  extract(contraction, c("before", "apostrophe", "after"), "(.*)(')(.*)")
```

```
## # A tibble: 4 x 4
##   sentence                                   before apostrophe after
##   <chr>                                      <chr>  <chr>      <chr>
## 1 Open the crate but don't break the glass.  don    '          t    
## 2 Let's all join as we sing the last chorus. Let    '          s    
## 3 We don't get much money but we have fun.   don    '          t    
## 4 We don't like to admit our small faults.   don    '          t
```

## 14.4.5 - Replacing matches {-}

### Problem 1 {-}

Replace all forward slashes in a string with backslashes. `str_replace_all("a/b/c", "/", "\\\\")`

### Problem 2 {-}

Implement a simple version of `str_to_lower()` using `str_replace_all()`. `str_replace_all("AbC", "[A-Z]", tolower)`

### Problem 3 {-}

Switch the first and last letters in words. Which of those strings are still words?


```r
new.words <- str_replace(words, "(^.)(.*)(.$)", "\\3\\2\\1")
words[new.words %in% words]
```

```
##  [1] "a"          "america"    "area"       "dad"        "dead"      
##  [6] "deal"       "dear"       "depend"     "dog"        "educate"   
## [11] "else"       "encourage"  "engine"     "europe"     "evidence"  
## [16] "example"    "excuse"     "exercise"   "expense"    "experience"
## [21] "eye"        "god"        "health"     "high"       "knock"     
## [26] "lead"       "level"      "local"      "nation"     "no"        
## [31] "non"        "on"         "rather"     "read"       "refer"     
## [36] "remember"   "serious"    "stairs"     "test"       "tonight"   
## [41] "transport"  "treat"      "trust"      "window"     "yesterday"
```

## 14.4.6 - Splitting {-}

### Problem 1 {-}

Split up a string like "apples, pears, and bananas" into individual components.

`str_split("apples, pears, and bananas", boundary("word"))`

### Problem 2 {-}

Why is it better to split up by boundary("word") than " "?

`" "` captures non-words like the space after the period while `boundary("word")` only captures words. 

### Problem 3 {-}

What does splitting with an empty string ("") do? Experiment and read the documentation.

"An empty pattern, "", is equivalent to boundary("character")."

## 14.5 - Other types of pattern {-}

### Problem 1 {-}

How would you find all strings containing "\" with regex() versus fixed. `regex("\\\\")` & `fixed("\")`

### Problem 2 {-}

What are the five most common words in setences? 

The five most common words are "the", "a", "of", "to", and "and".


```r
str_split(sentences, boundary("word")) %>%
  flatten_chr() %>%
  str_to_lower() %>%
  as_tibble() %>%
  group_by(value) %>%
  count() %>%
  arrange(-n) %>%
  ungroup() %>%
  top_n(5)
```

```
## Selecting by n
```

```
## # A tibble: 5 x 2
##   value     n
##   <chr> <int>
## 1 the     751
## 2 a       202
## 3 of      132
## 4 to      123
## 5 and     118
```

## 14.6 - Other uses of regular expressions {-}

### Problem 1 {-}

Find the **stringi** function that:
  1. Count the number of words `stri_count_words`
  2. Find duplicated strings. `stri_duplicated()`
  3. Generate random text. `stri_rand_strings()`

### Problem 2 {-}
  
How do you control the language that str_sort() uses for sorting?
With the `locale =` argument in the `opts_collator` argument. 
