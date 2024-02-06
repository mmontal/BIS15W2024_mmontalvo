---
title: "summarize practice, `count()`, `across()`"
date: "2024-02-05"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Produce clear, concise summaries using a variety of functions in `dplyr` and `janitor.`  
2. Use the `across()` operator to produce summaries across multiple variables.  

## Load the libraries

```r
library("tidyverse")
library("janitor")
library("skimr")
library("palmerpenguins")
```

## Review
The summarize() and group_by() functions are powerful tools that we can use to produce clean summaries of data. Especially when used together, we can quickly group variables of interest and save time. Let's do some practice with the [palmerpenguins(https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data to refresh our memory.

```r
glimpse(penguins)
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

As biologists, a good question that we may ask is how do the measured variables differ by island (on average)?

```r
levels(penguins$island)
```

```
## [1] "Biscoe"    "Dream"     "Torgersen"
```


```r
penguins %>% 
  group_by(island) %>% 
  summarize(mean_body_mass= mean(body_mass_g),
  n=n())
```

```
## # A tibble: 3 × 3
##   island    mean_body_mass     n
##   <fct>              <dbl> <int>
## 1 Biscoe               NA    168
## 2 Dream              3713.   124
## 3 Torgersen            NA     52
```

Why do we have NA here? Do all of these penguins lack data?

```r
penguins %>% 
  group_by(island) %>% 
  summarize(number_NAs=sum(is.na(body_mass_g)))
```

```
## # A tibble: 3 × 2
##   island    number_NAs
##   <fct>          <int>
## 1 Biscoe             1
## 2 Dream              0
## 3 Torgersen          1
```

Well, that won't work so let's remove the NAs and recalculate.

```r
penguins %>%
  filter(!is.na(body_mass_g)) %>% #pull out all observations, no NAs. 
  group_by(island) %>% 
  summarize(mean_body_mass= mean(body_mass_g),
  n=n())
```

```
## # A tibble: 3 × 3
##   island    mean_body_mass     n
##   <fct>              <dbl> <int>
## 1 Biscoe             4716.   167
## 2 Dream              3713.   124
## 3 Torgersen          3706.    51
```

What if we are interested in the number of observations (penguins) by species and island?

```r
penguins %>% 
  group_by(species, island) %>% 
  summarize(n=n(), .groups= 'keep')#the .groups argument here just prevents a warning message
```

```
## # A tibble: 5 × 3
## # Groups:   species, island [5]
##   species   island        n
##   <fct>     <fct>     <int>
## 1 Adelie    Biscoe       44
## 2 Adelie    Dream        56
## 3 Adelie    Torgersen    52
## 4 Chinstrap Dream        68
## 5 Gentoo    Biscoe      124
```

## Counts
Although these summary functions are super helpful, oftentimes we are mostly interested in counts. The [janitor package](https://garthtarr.github.io/meatR/janitor.html) does a lot with counts, but there are also functions that are part of dplyr that are useful.  

`count()` is an easy way of determining how many observations you have within a column. It acts like a combination of `group_by()` and `n()`.

```r
penguins %>% 
  count(island, sort = T) #sort=T sorts the column in descending order
```

```
## # A tibble: 3 × 2
##   island        n
##   <fct>     <int>
## 1 Biscoe      168
## 2 Dream       124
## 3 Torgersen    52
```

Compare this with `summarize()` and `group_by()`.

```r
penguins %>% 
  group_by(island) %>% 
  summarize(n=n())
```

```
## # A tibble: 3 × 2
##   island        n
##   <fct>     <int>
## 1 Biscoe      168
## 2 Dream       124
## 3 Torgersen    52
```

You can also use `count()` across multiple variables.

```r
penguins %>% 
  count(island, species, sort = T) # sort=T will arrange in descending order
```

```
## # A tibble: 5 × 3
##   island    species       n
##   <fct>     <fct>     <int>
## 1 Biscoe    Gentoo      124
## 2 Dream     Chinstrap    68
## 3 Dream     Adelie       56
## 4 Torgersen Adelie       52
## 5 Biscoe    Adelie       44
```

For counts, I also like `tabyl()`. Lots of options are supported in [tabyl](https://cran.r-project.org/web/packages/janitor/vignettes/tabyls.html)

```r
penguins %>% 
  tabyl(island, species)
```

```
##     island Adelie Chinstrap Gentoo
##     Biscoe     44         0    124
##      Dream     56        68      0
##  Torgersen     52         0      0
```

## Practice
1. How does the mean of `bill_length_mm` compare between penguin species?

```r
penguins %>% 
  tabyl(bill_length_mm, species)
```

```
##  bill_length_mm Adelie Chinstrap Gentoo
##            32.1      1         0      0
##            33.1      1         0      0
##            33.5      1         0      0
##            34.0      1         0      0
##            34.1      1         0      0
##            34.4      1         0      0
##            34.5      1         0      0
##            34.6      2         0      0
##            35.0      2         0      0
##            35.1      1         0      0
##            35.2      1         0      0
##            35.3      1         0      0
##            35.5      2         0      0
##            35.6      1         0      0
##            35.7      3         0      0
##            35.9      2         0      0
##            36.0      4         0      0
##            36.2      3         0      0
##            36.3      1         0      0
##            36.4      2         0      0
##            36.5      2         0      0
##            36.6      2         0      0
##            36.7      2         0      0
##            36.8      1         0      0
##            36.9      1         0      0
##            37.0      2         0      0
##            37.2      2         0      0
##            37.3      3         0      0
##            37.5      2         0      0
##            37.6      3         0      0
##            37.7      3         0      0
##            37.8      5         0      0
##            37.9      2         0      0
##            38.1      4         0      0
##            38.2      2         0      0
##            38.3      1         0      0
##            38.5      1         0      0
##            38.6      3         0      0
##            38.7      1         0      0
##            38.8      3         0      0
##            38.9      2         0      0
##            39.0      3         0      0
##            39.1      1         0      0
##            39.2      3         0      0
##            39.3      1         0      0
##            39.5      3         0      0
##            39.6      5         0      0
##            39.7      4         0      0
##            39.8      1         0      0
##            40.1      1         0      0
##            40.2      3         0      0
##            40.3      2         0      0
##            40.5      2         0      0
##            40.6      4         0      0
##            40.7      1         0      0
##            40.8      2         0      0
##            40.9      2         1      1
##            41.0      1         0      0
##            41.1      7         0      0
##            41.3      2         0      0
##            41.4      2         0      0
##            41.5      2         0      0
##            41.6      1         0      0
##            41.7      0         0      1
##            41.8      1         0      0
##            42.0      2         0      1
##            42.1      1         0      0
##            42.2      2         0      0
##            42.3      1         0      0
##            42.4      0         1      0
##            42.5      1         2      0
##            42.6      0         0      1
##            42.7      1         0      1
##            42.8      1         0      1
##            42.9      1         0      1
##            43.1      1         0      0
##            43.2      2         1      1
##            43.3      0         0      2
##            43.4      0         0      1
##            43.5      0         1      2
##            43.6      0         0      1
##            43.8      0         0      1
##            44.0      0         0      1
##            44.1      2         0      0
##            44.4      0         0      1
##            44.5      0         0      3
##            44.9      0         0      2
##            45.0      0         0      1
##            45.1      0         0      3
##            45.2      0         2      4
##            45.3      0         0      2
##            45.4      0         1      1
##            45.5      0         1      4
##            45.6      1         1      0
##            45.7      0         2      1
##            45.8      1         0      2
##            45.9      0         1      0
##            46.0      1         1      0
##            46.1      0         1      2
##            46.2      0         1      4
##            46.3      0         0      1
##            46.4      0         2      2
##            46.5      0         1      4
##            46.6      0         1      1
##            46.7      0         1      1
##            46.8      0         1      3
##            46.9      0         1      1
##            47.0      0         1      0
##            47.2      0         0      2
##            47.3      0         0      2
##            47.4      0         0      1
##            47.5      0         1      3
##            47.6      0         1      1
##            47.7      0         0      1
##            47.8      0         0      1
##            48.1      0         1      1
##            48.2      0         0      2
##            48.4      0         0      3
##            48.5      0         1      2
##            48.6      0         0      1
##            48.7      0         0      3
##            48.8      0         0      1
##            49.0      0         2      1
##            49.1      0         0      3
##            49.2      0         1      1
##            49.3      0         1      1
##            49.4      0         0      1
##            49.5      0         1      2
##            49.6      0         1      2
##            49.7      0         1      0
##            49.8      0         1      2
##            49.9      0         0      1
##            50.0      0         1      4
##            50.1      0         1      1
##            50.2      0         2      1
##            50.3      0         1      0
##            50.4      0         0      2
##            50.5      0         2      3
##            50.6      0         1      0
##            50.7      0         1      1
##            50.8      0         2      2
##            50.9      0         2      0
##            51.0      0         1      0
##            51.1      0         0      2
##            51.3      0         3      1
##            51.4      0         1      0
##            51.5      0         1      1
##            51.7      0         1      0
##            51.9      0         1      0
##            52.0      0         3      0
##            52.1      0         0      1
##            52.2      0         1      1
##            52.5      0         0      1
##            52.7      0         1      0
##            52.8      0         1      0
##            53.4      0         0      1
##            53.5      0         1      0
##            54.2      0         1      0
##            54.3      0         0      1
##            55.1      0         0      1
##            55.8      0         1      0
##            55.9      0         0      1
##            58.0      0         1      0
##            59.6      0         0      1
##              NA      1         0      1
```


```r
penguins %>% 
  group_by(species) %>% 
  count(bill_length_mm)
```

```
## # A tibble: 210 × 3
## # Groups:   species [3]
##    species bill_length_mm     n
##    <fct>            <dbl> <int>
##  1 Adelie            32.1     1
##  2 Adelie            33.1     1
##  3 Adelie            33.5     1
##  4 Adelie            34       1
##  5 Adelie            34.1     1
##  6 Adelie            34.4     1
##  7 Adelie            34.5     1
##  8 Adelie            34.6     2
##  9 Adelie            35       2
## 10 Adelie            35.1     1
## # ℹ 200 more rows
```


2. For some penguins, their sex is listed as NA. Where do these penguins occur?

```r
penguins %>% 
  count(sex) %>% 
  summarize(number_NAs=sum(is.na(sex)))
```

```
## # A tibble: 1 × 1
##   number_NAs
##        <int>
## 1          1
```

## `across()`
What about using `filter()` and `select()` across multiple variables? There is a function in dplyr called `across()` which is designed to work across multiple variables. Have a look at Rebecca Barter's awesome blog [here](http://www.rebeccabarter.com/blog/2020-07-09-across/).    

What if we wanted to apply `summarize()` in order to produce distinct counts over multiple variables; i.e. species, island, and sex? Although this isn't a lot of coding you can image that with a lot of variables it would be cumbersome.

```r
penguins %>%
  summarize(distinct_species = n_distinct(species),
            distinct_island = n_distinct(island),
            distinct_sex = n_distinct(sex))
```

```
## # A tibble: 1 × 3
##   distinct_species distinct_island distinct_sex
##              <int>           <int>        <int>
## 1                3               3            3
```

By using `across()` we can reduce the clutter and make things cleaner. 

```r
penguins %>%
  summarize(across(c(species, island, sex), n_distinct))
```

```
## # A tibble: 1 × 3
##   species island   sex
##     <int>  <int> <int>
## 1       3      3     3
```

This is very helpful for continuous variables.

```r
penguins %>%
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## Warning: There was 1 warning in `summarize()`.
## ℹ In argument: `across(contains("mm"), mean, na.rm = T)`.
## Caused by warning:
## ! The `...` argument of `across()` is deprecated as of dplyr 1.1.0.
## Supply arguments directly to `.fns` through an anonymous function instead.
## 
##   # Previously
##   across(a:b, mean, na.rm = TRUE)
## 
##   # Now
##   across(a:b, \(x) mean(x, na.rm = TRUE))
```

```
## # A tibble: 1 × 3
##   bill_length_mm bill_depth_mm flipper_length_mm
##            <dbl>         <dbl>             <dbl>
## 1           43.9          17.2              201.
```


```r
penguins %>%
  summarize(across(contains("mm"), \(x) mean(x, na.rm = TRUE))) #use this to correct the error.
```

```
## # A tibble: 1 × 3
##   bill_length_mm bill_depth_mm flipper_length_mm
##            <dbl>         <dbl>             <dbl>
## 1           43.9          17.2              201.
```

`group_by` also works.

```r
penguins %>%
  group_by(sex) %>% 
  summarize(across(contains("mm"), mean, na.rm=T))
```

```
## # A tibble: 3 × 4
##   sex    bill_length_mm bill_depth_mm flipper_length_mm
##   <fct>           <dbl>         <dbl>             <dbl>
## 1 female           42.1          16.4              197.
## 2 male             45.9          17.9              205.
## 3 <NA>             41.3          16.6              199
```

Here we summarize across all variables.

```r
penguins %>%
  summarise_all(mean, na.rm=T)
```

```
## Warning: There were 3 warnings in `summarise()`.
## The first warning was:
## ℹ In argument: `species = (function (x, ...) ...`.
## Caused by warning in `mean.default()`:
## ! argument is not numeric or logical: returning NA
## ℹ Run `dplyr::last_dplyr_warnings()` to see the 2 remaining warnings.
```

```
## # A tibble: 1 × 8
##   species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##     <dbl>  <dbl>          <dbl>         <dbl>             <dbl>       <dbl>
## 1      NA     NA           43.9          17.2              201.       4202.
## # ℹ 2 more variables: sex <dbl>, year <dbl>
```

Operators can also work, here I am summarizing across all variables except `species`, `island`, `sex`, and `year`.

```r
penguins %>%
  summarise(across(!c(species, island, sex, year), 
                   mean, na.rm=T))
```

```
## # A tibble: 1 × 4
##   bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
##            <dbl>         <dbl>             <dbl>       <dbl>
## 1           43.9          17.2              201.       4202.
```

All variables that include "bill"...all of the other dplyr operators also work.

```r
penguins %>%
  summarise(across(starts_with("bill"), mean, na.rm=T))
```

```
## # A tibble: 1 × 2
##   bill_length_mm bill_depth_mm
##            <dbl>         <dbl>
## 1           43.9          17.2
```

## Practice
1. Produce separate summaries of the mean and standard deviation for bill_length_mm, bill_depth_mm, and flipper_length_mm for each penguin species. Be sure to provide the number of samples.  


## Wrap-up  

Please review the learning goals and be sure to use the code here as a reference when completing the homework.  
-->[Home](https://jmledford3115.github.io/datascibiol/)
