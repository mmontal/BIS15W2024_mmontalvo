---
title: "Warmup_lab6"
output: 
  html_document: 
    keep_md: yes
date: "2024-01-30"
---



## Load tidyverse

```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```
## 1. load bison data

```r
getwd()
```

```
## [1] "/Users/memontal/Desktop/BIS15W2024_mmontalvo/lab6"
```

```r
bison <- read_csv("data/bison.csv")
```

```
## Rows: 8325 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): data_code, animal_code, animal_sex
## dbl (5): rec_year, rec_month, rec_day, animal_weight, animal_yob
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
## 2. dimensions and structure of data.

```r
dim(bison)
```

```
## [1] 8325    8
```


```r
str(bison)
```

```
## spc_tbl_ [8,325 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ data_code    : chr [1:8325] "CBH01" "CBH01" "CBH01" "CBH01" ...
##  $ rec_year     : num [1:8325] 1994 1994 1994 1994 1994 ...
##  $ rec_month    : num [1:8325] 11 11 11 11 11 11 11 11 11 11 ...
##  $ rec_day      : num [1:8325] 8 8 8 8 8 8 8 8 8 8 ...
##  $ animal_code  : chr [1:8325] "813" "834" "B-301" "B-402" ...
##  $ animal_sex   : chr [1:8325] "F" "F" "F" "F" ...
##  $ animal_weight: num [1:8325] 890 1074 1060 989 1062 ...
##  $ animal_yob   : num [1:8325] 1981 1983 1983 1984 1984 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   data_code = col_character(),
##   ..   rec_year = col_double(),
##   ..   rec_month = col_double(),
##   ..   rec_day = col_double(),
##   ..   animal_code = col_character(),
##   ..   animal_sex = col_character(),
##   ..   animal_weight = col_double(),
##   ..   animal_yob = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


```r
bison
```

```
## # A tibble: 8,325 × 8
##    data_code rec_year rec_month rec_day animal_code animal_sex animal_weight
##    <chr>        <dbl>     <dbl>   <dbl> <chr>       <chr>              <dbl>
##  1 CBH01         1994        11       8 813         F                    890
##  2 CBH01         1994        11       8 834         F                   1074
##  3 CBH01         1994        11       8 B-301       F                   1060
##  4 CBH01         1994        11       8 B-402       F                    989
##  5 CBH01         1994        11       8 B-403       F                   1062
##  6 CBH01         1994        11       8 B-502       F                    978
##  7 CBH01         1994        11       8 B-503       F                   1068
##  8 CBH01         1994        11       8 B-504       F                   1024
##  9 CBH01         1994        11       8 B-601       F                    978
## 10 CBH01         1994        11       8 B-602       F                   1188
## # ℹ 8,315 more rows
## # ℹ 1 more variable: animal_yob <dbl>
```
## 3. New data frame with code, sex, weight, and age

```r
new_bison <- select(bison, "data_code", "animal_sex", "animal_weight", "animal_yob")
new_bison
```

```
## # A tibble: 8,325 × 4
##    data_code animal_sex animal_weight animal_yob
##    <chr>     <chr>              <dbl>      <dbl>
##  1 CBH01     F                    890       1981
##  2 CBH01     F                   1074       1983
##  3 CBH01     F                   1060       1983
##  4 CBH01     F                    989       1984
##  5 CBH01     F                   1062       1984
##  6 CBH01     F                    978       1985
##  7 CBH01     F                   1068       1985
##  8 CBH01     F                   1024       1985
##  9 CBH01     F                    978       1986
## 10 CBH01     F                   1188       1986
## # ℹ 8,315 more rows
```
## 4. Restrict to year 1980 to 1990. 

```r
new_bison %>% 
  filter(between(animal_yob, 1980, 1990))
```

```
## # A tibble: 435 × 4
##    data_code animal_sex animal_weight animal_yob
##    <chr>     <chr>              <dbl>      <dbl>
##  1 CBH01     F                    890       1981
##  2 CBH01     F                   1074       1983
##  3 CBH01     F                   1060       1983
##  4 CBH01     F                    989       1984
##  5 CBH01     F                   1062       1984
##  6 CBH01     F                    978       1985
##  7 CBH01     F                   1068       1985
##  8 CBH01     F                   1024       1985
##  9 CBH01     F                    978       1986
## 10 CBH01     F                   1188       1986
## # ℹ 425 more rows
```
## 5. filter by sex

```r
female_bison <- new_bison %>% 
  filter(between(animal_yob, 1980, 1990)) %>% 
  filter(animal_sex=="F")
female_bison
```

```
## # A tibble: 414 × 4
##    data_code animal_sex animal_weight animal_yob
##    <chr>     <chr>              <dbl>      <dbl>
##  1 CBH01     F                    890       1981
##  2 CBH01     F                   1074       1983
##  3 CBH01     F                   1060       1983
##  4 CBH01     F                    989       1984
##  5 CBH01     F                   1062       1984
##  6 CBH01     F                    978       1985
##  7 CBH01     F                   1068       1985
##  8 CBH01     F                   1024       1985
##  9 CBH01     F                    978       1986
## 10 CBH01     F                   1188       1986
## # ℹ 404 more rows
```



```r
male_bison <- new_bison %>% 
  filter(between(animal_yob, 1980, 1990)) %>% 
  filter(animal_sex=="M")
male_bison
```

```
## # A tibble: 21 × 4
##    data_code animal_sex animal_weight animal_yob
##    <chr>     <chr>              <dbl>      <dbl>
##  1 CBH01     M                   1728       1987
##  2 CBH01     M                   1726       1988
##  3 CBH01     M                   1712       1988
##  4 CBH01     M                   1306       1989
##  5 CBH01     M                   1682       1989
##  6 CBH01     M                   1594       1989
##  7 CBH01     M                   1552       1990
##  8 CBH01     M                   1572       1990
##  9 CBH01     M                   1538       1990
## 10 CBH01     M                   1422       1990
## # ℹ 11 more rows
```

## how many males and females?

```r
dim(female_bison)
```

```
## [1] 414   4
```

```r
dim(male_bison)
```

```
## [1] 21  4
```

```r
table(male_bison$animal_sex)
```

```
## 
##  M 
## 21
```

## 6. Are males or females larger on average from 1980-1990?

```r
mean(female_bison$animal_weight)
```

```
## [1] 1017.314
```


```r
mean(male_bison$animal_weight)
```

```
## [1] 1543.333
```

## Males are larger on average from year 1980-1990. 
