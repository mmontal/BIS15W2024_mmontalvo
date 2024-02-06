---
title: "Midterm 1 W24"
author: "Mariana Montalvo"
date: "2024-02-06"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document. You may use your notes, labs, and homework to help you complete this exam. Do not use any other resources- including AI assistance.  

Don't forget to answer any questions that are asked in the prompt!  

Be sure to push your completed midterm to your repository. This exam is worth 30 points.  

## Background
In the data folder, you will find data related to a study on wolf mortality collected by the National Park Service. You should start by reading the `README_NPSwolfdata.pdf` file. This will provide an abstract of the study and an explanation of variables.  

The data are from: Cassidy, Kira et al. (2022). Gray wolf packs and human-caused wolf mortality. [Dryad](https://doi.org/10.5061/dryad.mkkwh713f). 

## Load the libraries

```r
library("tidyverse")
library("janitor")
```

## Load the wolves data
In these data, the authors used `NULL` to represent missing values. I am correcting this for you below and using `janitor` to clean the column names.

```r
wolves <- read.csv("data/NPS_wolfmortalitydata.csv", na = c("NULL")) %>% clean_names()
```

## Questions
Problem 1. (1 point) Let's start with some data exploration. What are the variable (column) names?

```r
colnames(wolves)
```

```
##  [1] "park"         "biolyr"       "pack"         "packcode"     "packsize_aug"
##  [6] "mort_yn"      "mort_all"     "mort_lead"    "mort_nonlead" "reprody1"    
## [11] "persisty1"
```

Problem 2. (1 point) Use the function of your choice to summarize the data and get an idea of its structure. 

```r
glimpse(wolves)
```

```
## Rows: 864
## Columns: 11
## $ park         <chr> "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "DENA", "…
## $ biolyr       <int> 1996, 1991, 2017, 1996, 1992, 1994, 2007, 2007, 1995, 200…
## $ pack         <chr> "McKinley River1", "Birch Creek N", "Eagle Gorge", "East …
## $ packcode     <int> 89, 58, 71, 72, 74, 77, 101, 108, 109, 53, 63, 66, 70, 72…
## $ packsize_aug <dbl> 12, 5, 8, 13, 7, 6, 10, NA, 9, 8, 7, 11, 0, 19, 15, 12, 1…
## $ mort_yn      <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_all     <int> 4, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ mort_lead    <int> 2, 2, 0, 0, 0, 0, 1, 2, 1, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, …
## $ mort_nonlead <int> 2, 0, 2, 2, 2, 2, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, …
## $ reprody1     <int> 0, 0, NA, 1, NA, 0, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1…
## $ persisty1    <int> 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, …
```

Problem 3. (3 points) Which parks/ reserves are represented in the data? Don't just use the abstract, pull this information from the data.  

```r
wolves %>% 
  count(park)
```

```
##   park   n
## 1 DENA 340
## 2 GNTP  77
## 3  VNP  48
## 4  YNP 248
## 5 YUCH 151
```

Problem 4. (4 points) Which park has the largest number of wolf packs?

```r
wolves %>%
  group_by(park) %>% 
  summarize(distinct_packs=n_distinct(pack))
```

```
## # A tibble: 5 × 2
##   park  distinct_packs
##   <chr>          <int>
## 1 DENA              69
## 2 GNTP              12
## 3 VNP               22
## 4 YNP               46
## 5 YUCH              36
```

Problem 5. (4 points) Which park has the highest total number of human-caused mortalities `mort_all`?

```r
wolves %>% 
  group_by(park) %>% 
  summarize(max_mort_all=max(mort_all)) %>% 
  arrange(desc(max_mort_all))
```

```
## # A tibble: 5 × 2
##   park  max_mort_all
##   <chr>        <int>
## 1 YUCH            24
## 2 DENA             4
## 3 GNTP             4
## 4 YNP              4
## 5 VNP              2
```

The wolves in [Yellowstone National Park](https://www.nps.gov/yell/learn/nature/wolf-restoration.htm) are an incredible conservation success story. Let's focus our attention on this park.  

Problem 6. (2 points) Create a new object "ynp" that only includes the data from Yellowstone National Park.  

```r
ynp <- wolves %>% 
  filter(park=="YNP")
```

Problem 7. (3 points) Among the Yellowstone wolf packs, the [Druid Peak Pack](https://www.pbs.org/wnet/nature/in-the-valley-of-the-wolves-the-druid-wolf-pack-story/209/) is one of most famous. What was the average pack size of this pack for the years represented in the data?

```r
ynp %>%
  filter(pack=="druid") %>% 
  summarize(mean_pack_size=mean(packsize_aug), 
            n=n())
```

```
##   mean_pack_size  n
## 1       13.93333 15
```

Problem 8. (4 points) Pack dynamics can be hard to predict- even for strong packs like the Druid Peak pack. At which year did the Druid Peak pack have the largest pack size? What do you think happened in 2010?

```r
ynp %>% 
  filter(pack=="druid") %>% 
  select(biolyr, pack, packsize_aug, reprody1, persisty1) %>% 
  arrange(desc(packsize_aug))
```

```
##    biolyr  pack packsize_aug reprody1 persisty1
## 1    2001 druid           37        1         1
## 2    2000 druid           27        1         1
## 3    2008 druid           21        1         1
## 4    2003 druid           18        1         1
## 5    2007 druid           18        1         1
## 6    2002 druid           16        1         1
## 7    2006 druid           15        1         1
## 8    2004 druid           13        1         1
## 9    2009 druid           12        0         0
## 10   1999 druid            9        1         1
## 11   1998 druid            8        1         1
## 12   1997 druid            5        1         1
## 13   1996 druid            5        1         1
## 14   2005 druid            5        1         1
## 15   2010 druid            0        0        NA
```
### The largest pack size was in 2001, In 2010 the packsize was 0, if we look at the data we can see that for the year before 2009 and in 2010 there was no localization of the pack and there were no pups which means they did not reproduce. There is also a 0 in persisty1 which according to the data means that the pack dissolved and/or was left with a 'lone wolf'. 

Problem 9. (5 points) Among the YNP wolf packs, which one has had the highest overall persistence `persisty1` for the years represented in the data? Look this pack up online and tell me what is unique about its behavior- specifically, what prey animals does this pack specialize on?  

```r
ynp %>% 
  group_by(pack) %>% 
  filter(persisty1=="1") %>% 
  count(persisty1) %>% 
  arrange(desc(n))
```

```
## # A tibble: 38 × 3
## # Groups:   pack [38]
##    pack        persisty1     n
##    <chr>           <int> <int>
##  1 mollies             1    26
##  2 cougar              1    20
##  3 yelldelta           1    18
##  4 druid               1    13
##  5 leopold             1    12
##  6 agate               1    10
##  7 8mile               1     9
##  8 canyon              1     9
##  9 gibbon/mary         1     9
## 10 nezperce            1     9
## # ℹ 28 more rows
```

### The Molli pack pray on bison. They have a unique behavior of hunting bison and interacting with bears 

Problem 10. (3 points) Perform one analysis or exploration of your choice on the `wolves` data. Your answer needs to include at least two lines of code and not be a summary function.  

```r
dena <- wolves %>% 
  filter(park=="DENA")
```


```r
dena %>% 
  count(pack) %>% 
  arrange(desc(n))
```

```
##                pack  n
## 1         East Fork 31
## 2   McKinley Slough 17
## 3       Grant Creek 16
## 4           Bearpaw 14
## 5           Foraker 13
## 6          Stampede 13
## 7   Kantishna River 11
## 8            McLeod 11
## 9       Mt Margaret 10
## 10       Starr Lake 10
## 11     Headquarters  9
## 12       Hot Slough  9
## 13         100 Mile  8
## 14           Somber  8
## 15      Little Bear  7
## 16 Chitsia Mountain  6
## 17     Nenana River  6
## 18      Pinto Creek  6
## 19        Sanctuary  6
## 20          Chitsia  5
## 21  Iron Creek West  5
## 22      John Hansen  5
## 23  McKinley River1  5
## 24            Stony  5
## 25     Straightaway  5
## 26      Beaver Fork  4
## 27       Clearwater  4
## 28      Corner Lake  4
## 29     Death Valley  4
## 30        Highpower  4
## 31   McKinley River  4
## 32      Muddy River  4
## 33      Riley Creek  4
## 34      White Creek  4
## 35      Birch Creek  3
## 36    Castle Rocks3  3
## 37           Herron  3
## 38       North Fork  3
## 39          Savage1  3
## 40      Birch Hills  2
## 41        Boot Lake  2
## 42          Brooker  2
## 43    Castle Rocks2  2
## 44      Eagle Gorge  2
## 45        Ewe Creek  2
## 46      Hauke Creek  2
## 47       Iron Creek  2
## 48  Iron Creek East  2
## 49      Moose Creek  2
## 50           Myrtle  2
## 51      Otter Creek  2
## 52            Pinto  2
## 53           Savage  2
## 54   Slippery Creek  2
## 55     Turtle Hill   2
## 56     Turtle Hill1  2
## 57      Windy Creek  2
## 58         Bearpaw1  1
## 59    Birch Creek N  1
## 60    Caribou Creek  1
## 61    Chilchukabena  1
## 62      Jenny Creek  1
## 63      McLeod West  1
## 64          McLeod2  1
## 65       Otter Lake  1
## 66     Pirate Creek  1
## 67 Riley Creek West  1
## 68    Sandless Lake  1
## 69          Tonzona  1
```

