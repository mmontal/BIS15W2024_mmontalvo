---
title: "Homework 8"
author: "Please Add Your Name Here"
date: "2024-02-14"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
library("here")
```

```
## here() starts at /Users/marianamontalvo/Desktop/BIS15W2024_mmontalvo
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("homework", "data", "sydneybeaches.csv")) %>% clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
glimpse(sydneybeaches)
```

```
## Rows: 3,690
## Columns: 8
## $ beach_id              <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ region                <chr> "Sydney City Ocean Beaches", "Sydney City Ocean …
## $ council               <chr> "Randwick Council", "Randwick Council", "Randwic…
## $ site                  <chr> "Clovelly Beach", "Clovelly Beach", "Clovelly Be…
## $ longitude             <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.2675…
## $ latitude              <dbl> -33.91449, -33.91449, -33.91449, -33.91449, -33.…
## $ date                  <chr> "02/01/2013", "06/01/2013", "12/01/2013", "18/01…
## $ enterococci_cfu_100ml <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8, 3,…
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

```r
sydneybeaches
```

```
## # A tibble: 3,690 × 8
##    beach_id region  council site  longitude latitude date  enterococci_cfu_100ml
##       <dbl> <chr>   <chr>   <chr>     <dbl>    <dbl> <chr>                 <dbl>
##  1       25 Sydney… Randwi… Clov…      151.    -33.9 02/0…                    19
##  2       25 Sydney… Randwi… Clov…      151.    -33.9 06/0…                     3
##  3       25 Sydney… Randwi… Clov…      151.    -33.9 12/0…                     2
##  4       25 Sydney… Randwi… Clov…      151.    -33.9 18/0…                    13
##  5       25 Sydney… Randwi… Clov…      151.    -33.9 30/0…                     8
##  6       25 Sydney… Randwi… Clov…      151.    -33.9 05/0…                     7
##  7       25 Sydney… Randwi… Clov…      151.    -33.9 11/0…                    11
##  8       25 Sydney… Randwi… Clov…      151.    -33.9 23/0…                    97
##  9       25 Sydney… Randwi… Clov…      151.    -33.9 07/0…                     3
## 10       25 Sydney… Randwi… Clov…      151.    -33.9 25/0…                     0
## # ℹ 3,680 more rows
```
The data is not tidy, it is long. 

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>% 
  select(site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ℹ 3,680 more rows
```

4. Pivot the data such that the dates are column names and each beach only appears once (wide format). Name the object `sydneybeaches_wide`

```r
sydneybeaches2 <- sydneybeaches %>% 
  pivot_wider(names_from = "date", 
              values_from = "site")
sydneybeaches2
```

```
## # A tibble: 754 × 350
##    beach_id region council longitude latitude enterococci_cfu_100ml `02/01/2013`
##       <dbl> <chr>  <chr>       <dbl>    <dbl>                 <dbl> <chr>       
##  1       25 Sydne… Randwi…      151.    -33.9                    19 Clovelly Be…
##  2       25 Sydne… Randwi…      151.    -33.9                     3 <NA>        
##  3       25 Sydne… Randwi…      151.    -33.9                     2 <NA>        
##  4       25 Sydne… Randwi…      151.    -33.9                    13 <NA>        
##  5       25 Sydne… Randwi…      151.    -33.9                     8 <NA>        
##  6       25 Sydne… Randwi…      151.    -33.9                     7 <NA>        
##  7       25 Sydne… Randwi…      151.    -33.9                    11 <NA>        
##  8       25 Sydne… Randwi…      151.    -33.9                    97 <NA>        
##  9       25 Sydne… Randwi…      151.    -33.9                     0 <NA>        
## 10       25 Sydne… Randwi…      151.    -33.9                     6 <NA>        
## # ℹ 744 more rows
## # ℹ 343 more variables: `06/01/2013` <chr>, `12/01/2013` <chr>,
## #   `18/01/2013` <chr>, `30/01/2013` <chr>, `05/02/2013` <chr>,
## #   `11/02/2013` <chr>, `23/02/2013` <chr>, `07/03/2013` <chr>,
## #   `25/03/2013` <chr>, `02/04/2013` <chr>, `12/04/2013` <chr>,
## #   `18/04/2013` <chr>, `24/04/2013` <chr>, `01/05/2013` <chr>,
## #   `20/05/2013` <chr>, `31/05/2013` <chr>, `06/06/2013` <chr>, …
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches2 %>% 
  pivot_longer(-c(beach_id, region, council, longitude, latitude, enterococci_cfu_100ml), 
               names_to= "date")
```

```
## # A tibble: 259,376 × 8
##    beach_id region  council longitude latitude enterococci_cfu_100ml date  value
##       <dbl> <chr>   <chr>       <dbl>    <dbl>                 <dbl> <chr> <chr>
##  1       25 Sydney… Randwi…      151.    -33.9                    19 02/0… Clov…
##  2       25 Sydney… Randwi…      151.    -33.9                    19 06/0… <NA> 
##  3       25 Sydney… Randwi…      151.    -33.9                    19 12/0… <NA> 
##  4       25 Sydney… Randwi…      151.    -33.9                    19 18/0… <NA> 
##  5       25 Sydney… Randwi…      151.    -33.9                    19 30/0… <NA> 
##  6       25 Sydney… Randwi…      151.    -33.9                    19 05/0… <NA> 
##  7       25 Sydney… Randwi…      151.    -33.9                    19 11/0… <NA> 
##  8       25 Sydney… Randwi…      151.    -33.9                    19 23/0… <NA> 
##  9       25 Sydney… Randwi…      151.    -33.9                    19 07/0… <NA> 
## 10       25 Sydney… Randwi…      151.    -33.9                    19 25/0… <NA> 
## # ℹ 259,366 more rows
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches3 <- sydneybeaches_long %>% 
  separate(date, into=c("month","day", "year"), sep="/")
sydneybeaches3
```

```
## # A tibble: 3,690 × 5
##    site           month day   year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ℹ 3,680 more rows
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
sydneybeaches3 %>% 
  group_by(year) %>% 
  summarize(mean_enterococci=mean(enterococci_cfu_100ml, na.rm=T))
```

```
## # A tibble: 6 × 2
##   year  mean_enterococci
##   <chr>            <dbl>
## 1 2013              50.6
## 2 2014              26.3
## 3 2015              31.2
## 4 2016              42.2
## 5 2017              20.7
## 6 2018              33.1
```

8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
sydneybeaches3 %>% 
  group_by(year) %>% 
  summarize(mean_enterococci=mean(enterococci_cfu_100ml, na.rm=T)) %>% 
  pivot_wider(names_from = "year",
              values_from = "mean_enterococci")
```

```
## # A tibble: 1 × 6
##   `2013` `2014` `2015` `2016` `2017` `2018`
##    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
## 1   50.6   26.3   31.2   42.2   20.7   33.1
```

9. What was the most polluted beach in 2013?

```r
sydneybeaches3 %>% 
  group_by(site) %>% 
  filter(year=="2013") %>% 
  summarize(mean_pollution=mean(enterococci_cfu_100ml)) %>% 
  arrange(desc(mean_pollution))
```

```
## # A tibble: 11 × 2
##    site                    mean_pollution
##    <chr>                            <dbl>
##  1 Little Bay Beach                122.  
##  2 Malabar Beach                   101.  
##  3 South Maroubra Rockpool          96.4 
##  4 Maroubra Beach                   47.1 
##  5 Coogee Beach                     39.7 
##  6 South Maroubra Beach             39.3 
##  7 Bondi Beach                      32.2 
##  8 Tamarama Beach                   29.7 
##  9 Bronte Beach                     26.8 
## 10 Gordons Bay (East)               24.8 
## 11 Clovelly Beach                    9.28
```

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
