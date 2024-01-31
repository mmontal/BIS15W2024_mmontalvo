---
title: "dplyr Superhero"
date: "2024-01-30"
output:
  html_document: 
    theme: spacelab
    toc: true
    toc_float: true
    keep_md: true
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. This lab doubles as your homework. Please complete the lab and push your final code to GitHub.  

## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here. Before you do anything, first have a look at the names of the variables. You can use `rename()` or `clean_names()`.    

```r
superhero_info <- clean_names(superhero_info)
```


## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

1. Who are the publishers of the superheros? Show the proportion of superheros from each publisher. Which publisher has the highest number of superheros?  

```r
tabyl(superhero_info, publisher)
```

```
##          publisher   n     percent valid_percent
##        ABC Studios   4 0.005449591   0.005563282
##          DC Comics 215 0.292915531   0.299026426
##  Dark Horse Comics  18 0.024523161   0.025034771
##       George Lucas  14 0.019073569   0.019471488
##      Hanna-Barbera   1 0.001362398   0.001390821
##      HarperCollins   6 0.008174387   0.008344924
##     IDW Publishing   4 0.005449591   0.005563282
##        Icon Comics   4 0.005449591   0.005563282
##       Image Comics  14 0.019073569   0.019471488
##      J. K. Rowling   1 0.001362398   0.001390821
##   J. R. R. Tolkien   1 0.001362398   0.001390821
##      Marvel Comics 388 0.528610354   0.539638387
##          Microsoft   1 0.001362398   0.001390821
##       NBC - Heroes  19 0.025885559   0.026425591
##          Rebellion   1 0.001362398   0.001390821
##           Shueisha   4 0.005449591   0.005563282
##      Sony Pictures   2 0.002724796   0.002781641
##         South Park   1 0.001362398   0.001390821
##          Star Trek   6 0.008174387   0.008344924
##               SyFy   5 0.006811989   0.006954103
##       Team Epic TV   5 0.006811989   0.006954103
##        Titan Books   1 0.001362398   0.001390821
##  Universal Studios   1 0.001362398   0.001390821
##          Wildstorm   3 0.004087193   0.004172462
##               <NA>  15 0.020435967            NA
```

2. Notice that we have some neutral superheros! Who are they? List their names below.  

```r
superhero_info %>% 
  select(name, alignment) %>% 
  filter(alignment=="neutral")
```

```
## # A tibble: 24 × 2
##    name         alignment
##    <chr>        <chr>    
##  1 Bizarro      neutral  
##  2 Black Flash  neutral  
##  3 Captain Cold neutral  
##  4 Copycat      neutral  
##  5 Deadpool     neutral  
##  6 Deathstroke  neutral  
##  7 Etrigan      neutral  
##  8 Galactus     neutral  
##  9 Gladiator    neutral  
## 10 Indigo       neutral  
## # ℹ 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(name, alignment, race)
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ℹ 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info <- superhero_info %>% 
  mutate_all(tolower)
```

```r
not_human <- superhero_info %>% 
  select(name, race) %>% 
  filter(race!= "human")
not_human
```

```
## # A tibble: 222 × 2
##    name         race             
##    <chr>        <chr>            
##  1 abe sapien   icthyo sapien    
##  2 abin sur     ungaran          
##  3 abomination  human / radiation
##  4 abraxas      cosmic entity    
##  5 ajax         cyborg           
##  6 alien        xenomorph xx121  
##  7 amazo        android          
##  8 angel        vampire          
##  9 angel dust   mutant           
## 10 anti-monitor god / eternal    
## # ℹ 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info %>% 
  filter(alignment=="good")
good_guys
```

```
## # A tibble: 496 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 a-bo… male   yellow    human no hair    203    marvel c… <NA>       good     
##  2 abe … male   blue      icth… no hair    191    dark hor… blue       good     
##  3 abin… male   blue      unga… no hair    185    dc comics red        good     
##  4 adam… male   blue      <NA>  blond      <NA>   nbc - he… <NA>       good     
##  5 adam… male   blue      human blond      185    dc comics <NA>       good     
##  6 agen… female blue      <NA>  blond      173    marvel c… <NA>       good     
##  7 agen… male   brown     human brown      178    marvel c… <NA>       good     
##  8 agen… male   <NA>      <NA>  <NA>       191    marvel c… <NA>       good     
##  9 alan… male   blue      <NA>  blond      180    dc comics <NA>       good     
## 10 alex… male   <NA>      <NA>  <NA>       <NA>   nbc - he… <NA>       good     
## # ℹ 486 more rows
## # ℹ 1 more variable: weight <chr>
```


```r
bad_guys <- superhero_info %>% 
  filter(alignment=="bad")
bad_guys
```

```
## # A tibble: 207 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 abom… male   green     huma… no hair    203    marvel c… <NA>       bad      
##  2 abra… male   blue      cosm… black      <NA>   marvel c… <NA>       bad      
##  3 abso… male   blue      human no hair    193    marvel c… <NA>       bad      
##  4 air-… male   blue      <NA>  white      188    marvel c… <NA>       bad      
##  5 ajax  male   brown     cybo… black      193    marvel c… <NA>       bad      
##  6 alex… male   <NA>      human <NA>       <NA>   wildstorm <NA>       bad      
##  7 alien male   <NA>      xeno… no hair    244    dark hor… black      bad      
##  8 amazo male   red       andr… <NA>       257    dc comics <NA>       bad      
##  9 ammo  male   brown     human black      188    marvel c… <NA>       bad      
## 10 ange… female <NA>      <NA>  <NA>       <NA>   image co… <NA>       bad      
## # ℹ 197 more rows
## # ℹ 1 more variable: weight <chr>
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, race)
```

```
##               race   n     percent valid_percent
##              alien   3 0.006048387   0.010752688
##              alpha   5 0.010080645   0.017921147
##             amazon   2 0.004032258   0.007168459
##            android   4 0.008064516   0.014336918
##             animal   2 0.004032258   0.007168459
##          asgardian   3 0.006048387   0.010752688
##          atlantean   4 0.008064516   0.014336918
##         bolovaxian   1 0.002016129   0.003584229
##              clone   1 0.002016129   0.003584229
##             cyborg   3 0.006048387   0.010752688
##           demi-god   2 0.004032258   0.007168459
##              demon   3 0.006048387   0.010752688
##            eternal   1 0.002016129   0.003584229
##     flora colossus   1 0.002016129   0.003584229
##        frost giant   1 0.002016129   0.003584229
##      god / eternal   6 0.012096774   0.021505376
##             gungan   1 0.002016129   0.003584229
##              human 148 0.298387097   0.530465950
##    human / altered   2 0.004032258   0.007168459
##     human / cosmic   2 0.004032258   0.007168459
##  human / radiation   8 0.016129032   0.028673835
##         human-kree   2 0.004032258   0.007168459
##      human-spartoi   1 0.002016129   0.003584229
##       human-vulcan   1 0.002016129   0.003584229
##    human-vuldarian   1 0.002016129   0.003584229
##      icthyo sapien   1 0.002016129   0.003584229
##            inhuman   4 0.008064516   0.014336918
##    kakarantharaian   1 0.002016129   0.003584229
##         kryptonian   4 0.008064516   0.014336918
##            martian   1 0.002016129   0.003584229
##          metahuman   1 0.002016129   0.003584229
##             mutant  46 0.092741935   0.164874552
##     mutant / clone   1 0.002016129   0.003584229
##             planet   1 0.002016129   0.003584229
##             saiyan   1 0.002016129   0.003584229
##           symbiote   3 0.006048387   0.010752688
##           talokite   1 0.002016129   0.003584229
##         tamaranean   1 0.002016129   0.003584229
##            ungaran   1 0.002016129   0.003584229
##            vampire   2 0.004032258   0.007168459
##     yoda's species   1 0.002016129   0.003584229
##      zen-whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Vampires?

```r
good_guys %>% 
  filter(race=="vampire")
```

```
## # A tibble: 2 × 10
##   name  gender eye_color race   hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr>  <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 angel male   <NA>      vampi… <NA>       <NA>   dark hor… <NA>       good     
## 2 blade male   brown     vampi… black      188    marvel c… <NA>       good     
## # ℹ 1 more variable: weight <chr>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guys %>% 
  select(name, gender, height, race) %>% 
  filter(gender=="male", height>200, race=="human")
```

```
## # A tibble: 5 × 4
##   name        gender height race 
##   <chr>       <chr>  <chr>  <chr>
## 1 bane        male   203    human
## 2 doctor doom male   201    human
## 3 kingpin     male   201    human
## 4 lizard      male   203    human
## 5 scorpion    male   211    human
```

9. Are there more good guys or bad guys with green hair?  

```r
good_guys %>% 
  select(name, hair_color) %>% 
  filter(hair_color=="green")
```

```
## # A tibble: 7 × 2
##   name           hair_color
##   <chr>          <chr>     
## 1 beast boy      green     
## 2 captain planet green     
## 3 doc samson     green     
## 4 hulk           green     
## 5 lyja           green     
## 6 polaris        green     
## 7 she-hulk       green
```


```r
bad_guys %>% 
  select(name, hair_color) %>% 
  filter(hair_color=="green")
```

```
## # A tibble: 1 × 2
##   name  hair_color
##   <chr> <chr>     
## 1 joker green
```
### There are more good guys with green hair. 


```r
glimpse(superhero_info)
```

```
## Rows: 734
## Columns: 10
## $ name       <chr> "a-bomb", "abe sapien", "abin sur", "abomination", "abraxas…
## $ gender     <chr> "male", "male", "male", "male", "male", "male", "male", "ma…
## $ eye_color  <chr> "yellow", "blue", "blue", "green", "blue", "blue", "blue", …
## $ race       <chr> "human", "icthyo sapien", "ungaran", "human / radiation", "…
## $ hair_color <chr> "no hair", "no hair", "no hair", "no hair", "black", "no ha…
## $ height     <chr> "203", "191", "185", "203", NA, "193", NA, "185", "173", "1…
## $ publisher  <chr> "marvel comics", "dark horse comics", "dc comics", "marvel …
## $ skin_color <chr> NA, "blue", "red", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ alignment  <chr> "good", "good", "good", "bad", "bad", "bad", "good", "good"…
## $ weight     <chr> "441", "65", "90", "441", NA, "122", NA, "88", "61", "81", …
```

10. Let's explore who the really small superheros are. In the `superhero_info` data, which have a weight less than 50? Be sure to sort your results by weight lowest to highest. 

```r
small_superheros <- superhero_info %>% 
  mutate(weight_new= as.numeric(weight)) %>% 
  select(name, weight_new) %>% 
  filter(weight_new<50) %>% 
  arrange(weight_new)
small_superheros
```

```
## # A tibble: 19 × 2
##    name              weight_new
##    <chr>                  <dbl>
##  1 iron monger                2
##  2 groot                      4
##  3 jack-jack                 14
##  4 galactus                  16
##  5 yoda                      17
##  6 fin fang foom             18
##  7 howard the duck           18
##  8 krypto                    18
##  9 rocket raccoon            25
## 10 dash                      27
## 11 longshot                  36
## 12 robin v                   38
## 13 wiz kid                   39
## 14 violet parr               41
## 15 franklin richards         45
## 16 swarm                     47
## 17 hope summers              48
## 18 jolt                      49
## 19 snowbird                  49
```

11. Let's make a new variable that is the ratio of height to weight. Call this variable `height_weight_ratio`.

```r
superhero_info %>%
  mutate(weight_new= as.numeric(weight)) %>% 
  mutate(height_new= as.numeric(height)) %>% 
  mutate(height_weight_ratio= height_new/weight_new) %>% select(name, height_new, weight_new, height_weight_ratio)
```

```
## # A tibble: 734 × 4
##    name          height_new weight_new height_weight_ratio
##    <chr>              <dbl>      <dbl>               <dbl>
##  1 a-bomb               203        441               0.460
##  2 abe sapien           191         65               2.94 
##  3 abin sur             185         90               2.06 
##  4 abomination          203        441               0.460
##  5 abraxas               NA         NA              NA    
##  6 absorbing man        193        122               1.58 
##  7 adam monroe           NA         NA              NA    
##  8 adam strange         185         88               2.10 
##  9 agent 13             173         61               2.84 
## 10 agent bob            178         81               2.20 
## # ℹ 724 more rows
```

12. Who has the highest height to weight ratio?  

```r
superhero_info %>%
  mutate(weight_new= as.numeric(weight)) %>% 
  mutate(height_new= as.numeric(height)) %>% 
  mutate(height_weight_ratio= height_new/weight_new) %>% select(name, height_new, weight_new, height_weight_ratio) %>% 
  arrange(desc(height_weight_ratio))
```

```
## # A tibble: 734 × 4
##    name            height_new weight_new height_weight_ratio
##    <chr>                <dbl>      <dbl>               <dbl>
##  1 groot                  701          4              175.  
##  2 galactus               876         16               54.8 
##  3 fin fang foom          975         18               54.2 
##  4 longshot               188         36                5.22
##  5 jack-jack               71         14                5.07
##  6 rocket raccoon         122         25                4.88
##  7 dash                   122         27                4.52
##  8 howard the duck         79         18                4.39
##  9 swarm                  196         47                4.17
## 10 yoda                    66         17                3.88
## # ℹ 724 more rows
```
### Groot

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame. 

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                     <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abi…
## $ Agility                        <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ `Accelerated Healing`          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, …
## $ `Lantern Power Ring`           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE…
## $ `Dimensional Awareness`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Cold Resistance`              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Durability                     <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE,…
## $ Stealth                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Absorption`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Flight                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Danger Sense`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Underwater breathing`         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Marksmanship                   <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Weapons Master`               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Power Augmentation`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Attributes`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Longevity                      <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE,…
## $ Intelligence                   <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, …
## $ `Super Strength`               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TR…
## $ Cryokinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Telepathy                      <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Energy Armor`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Blasts`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Duplication                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Size Changing`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Density Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Stamina                        <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, F…
## $ `Astral Travel`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Audio Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Dexterity                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnitrix                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Super Speed`                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, …
## $ Possession                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Oriented Powers`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Weapon-based Powers`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Electrokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Darkforce Manipulation`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Death Touch`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Teleportation                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Enhanced Senses`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Telekinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Beams`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Magic                          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ Hyperkinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Jump                           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Clairvoyance                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Dimensional Travel`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Power Sense`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Shapeshifting                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Peak Human Condition`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Immortality                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE,…
## $ Camouflage                     <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE…
## $ `Element Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Phasing                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Astral Projection`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Electrical Transport`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Fire Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Projection                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Summoning                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Memory`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Reflexes                       <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ Invulnerability                <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE,…
## $ `Energy Constructs`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Force Fields`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Self-Sustenance`              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE…
## $ `Anti-Gravity`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Empathy                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Nullifier`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radiation Control`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Psionic Powers`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Elasticity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Substance Secretion`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Elemental Transmogrification` <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Technopath/Cyberpath`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Photographic Reflexes`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Seismic Power`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Animation                      <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE…
## $ Precognition                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Fire Resistance`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Absorption`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Hearing`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Nova Force`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Insanity                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Hypnokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Animal Control`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Natural Armor`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Intangibility                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Sight`               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Molecular Manipulation`       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Heat Generation`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Adaptation                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Gliding                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Suit`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Blast`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Probability Manipulation`     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Gravity Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Regeneration                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Light Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Echolocation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Levitation                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Toxin and Disease Control`    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Banish                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Manipulation`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ `Heat Resistance`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Natural Weapons`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Time Travel`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Smell`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Illusions                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Thirstokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Hair Manipulation`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Illumination                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnipotent                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Cloaking                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Changing Armor`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Power Cosmic`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE…
## $ Biokinesis                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Water Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radiation Immunity`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Telescopic`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Toxin and Disease Resistance` <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Spatial Awareness`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Energy Resistance`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Telepathy Resistance`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Molecular Combustion`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnilingualism                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Portal Creation`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Magnetism                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Mind Control Resistance`      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Plant Control`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Sonar                          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Sonic Scream`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Time Manipulation`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Enhanced Touch`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Magic Resistance`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Invisibility                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Sub-Mariner`                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE…
## $ `Radiation Absorption`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Intuitive aptitude`           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Microscopic`         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Melting                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Wind Control`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Super Breath`                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE…
## $ Wallcrawling                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Night`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Infrared`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Grim Reaping`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Matter Absorption`            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `The Force`                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Resurrection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Terrakinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Heat`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Vitakinesis                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Radar Sense`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Qwardian Power Ring`          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Weather Control`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - X-Ray`               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Thermal`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Web Creation`                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Reality Warping`              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Odin Force`                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Symbiote Costume`             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Speed Force`                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Phoenix Force`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Molecular Dissipation`        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ `Vision - Cryo`                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omnipresent                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
## $ Omniscient                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
```

13. How many superheros have a combination of agility, stealth, super_strength, stamina?

```r
superhero_powers <- clean_names(superhero_powers)
```


```r
superhero_powers %>% 
  select(hero_names, agility, stealth, super_strength, stamina) %>% filter(agility=="TRUE", stealth=="TRUE", super_strength=="TRUE", stamina=="TRUE")
```

```
## # A tibble: 40 × 5
##    hero_names  agility stealth super_strength stamina
##    <chr>       <lgl>   <lgl>   <lgl>          <lgl>  
##  1 Alex Mercer TRUE    TRUE    TRUE           TRUE   
##  2 Angel       TRUE    TRUE    TRUE           TRUE   
##  3 Ant-Man II  TRUE    TRUE    TRUE           TRUE   
##  4 Aquaman     TRUE    TRUE    TRUE           TRUE   
##  5 Batman      TRUE    TRUE    TRUE           TRUE   
##  6 Black Flash TRUE    TRUE    TRUE           TRUE   
##  7 Black Manta TRUE    TRUE    TRUE           TRUE   
##  8 Brundlefly  TRUE    TRUE    TRUE           TRUE   
##  9 Buffy       TRUE    TRUE    TRUE           TRUE   
## 10 Cable       TRUE    TRUE    TRUE           TRUE   
## # ℹ 30 more rows
```

## Your Favorite
14. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>% 
  filter(hero_names=="Spider-Man")
```

```
## # A tibble: 1 × 168
##   hero_names agility accelerated_healing lantern_power_ring
##   <chr>      <lgl>   <lgl>               <lgl>             
## 1 Spider-Man TRUE    TRUE                FALSE             
## # ℹ 164 more variables: dimensional_awareness <lgl>, cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>, …
```

15. Can you find your hero in the superhero_info data? Show their info!  

```r
superhero_info %>% 
  filter(name=="spider-man")
```

```
## # A tibble: 3 × 10
##   name   gender eye_color race  hair_color height publisher skin_color alignment
##   <chr>  <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 spide… male   hazel     human brown      178    marvel c… <NA>       good     
## 2 spide… <NA>   red       human brown      178    marvel c… <NA>       good     
## 3 spide… male   brown     human black      157    marvel c… <NA>       good     
## # ℹ 1 more variable: weight <chr>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
