---
title: "Lab 3 Homework"
author: "Mariana Montalvo"
date: "2024-01-21"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

### Mammals Sleep  
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R. The name of the data is `msleep`.  

```r
msleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # ℹ 73 more rows
## # ℹ 2 more variables: brainwt <dbl>, bodywt <dbl>
```

2. Store these data into a new data frame `sleep`.  

```r
sleep <- data.frame(msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)
```

```
## [1] 83 11
```
##### The dimensions are shown to be 83 observations and 11 variables, we can find these values by using the dim () function. 

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
is.na(sleep)
```

```
##        name genus  vore order conservation sleep_total sleep_rem sleep_cycle
##  [1,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
##  [2,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
##  [3,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
##  [4,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
##  [5,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
##  [6,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
##  [7,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
##  [8,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE        TRUE
##  [9,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [10,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [11,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [12,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [13,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [14,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [15,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [16,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
## [17,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [18,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [19,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [20,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [21,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [22,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [23,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [24,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [25,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [26,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [27,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [28,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [29,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [30,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [31,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [32,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [33,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [34,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [35,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [36,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [37,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [38,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [39,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [40,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [41,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [42,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [43,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [44,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [45,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [46,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [47,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [48,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [49,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [50,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [51,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [52,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [53,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [54,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [55,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE        TRUE
## [56,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [57,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE        TRUE
## [58,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE        TRUE
## [59,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [60,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [61,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
## [62,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [63,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE        TRUE
## [64,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [65,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [66,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
## [67,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [68,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [69,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE        TRUE
## [70,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE        TRUE
## [71,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [72,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE        TRUE
## [73,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE       FALSE
## [74,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [75,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [76,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [77,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE       FALSE
## [78,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
## [79,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
## [80,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [81,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE        TRUE
## [82,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE        TRUE
## [83,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE       FALSE
##       awake brainwt bodywt
##  [1,] FALSE    TRUE  FALSE
##  [2,] FALSE   FALSE  FALSE
##  [3,] FALSE    TRUE  FALSE
##  [4,] FALSE   FALSE  FALSE
##  [5,] FALSE   FALSE  FALSE
##  [6,] FALSE    TRUE  FALSE
##  [7,] FALSE    TRUE  FALSE
##  [8,] FALSE    TRUE  FALSE
##  [9,] FALSE   FALSE  FALSE
## [10,] FALSE   FALSE  FALSE
## [11,] FALSE   FALSE  FALSE
## [12,] FALSE   FALSE  FALSE
## [13,] FALSE    TRUE  FALSE
## [14,] FALSE   FALSE  FALSE
## [15,] FALSE   FALSE  FALSE
## [16,] FALSE   FALSE  FALSE
## [17,] FALSE   FALSE  FALSE
## [18,] FALSE   FALSE  FALSE
## [19,] FALSE   FALSE  FALSE
## [20,] FALSE   FALSE  FALSE
## [21,] FALSE   FALSE  FALSE
## [22,] FALSE   FALSE  FALSE
## [23,] FALSE   FALSE  FALSE
## [24,] FALSE   FALSE  FALSE
## [25,] FALSE   FALSE  FALSE
## [26,] FALSE   FALSE  FALSE
## [27,] FALSE    TRUE  FALSE
## [28,] FALSE   FALSE  FALSE
## [29,] FALSE   FALSE  FALSE
## [30,] FALSE    TRUE  FALSE
## [31,] FALSE    TRUE  FALSE
## [32,] FALSE   FALSE  FALSE
## [33,] FALSE   FALSE  FALSE
## [34,] FALSE   FALSE  FALSE
## [35,] FALSE    TRUE  FALSE
## [36,] FALSE   FALSE  FALSE
## [37,] FALSE    TRUE  FALSE
## [38,] FALSE   FALSE  FALSE
## [39,] FALSE    TRUE  FALSE
## [40,] FALSE   FALSE  FALSE
## [41,] FALSE    TRUE  FALSE
## [42,] FALSE   FALSE  FALSE
## [43,] FALSE   FALSE  FALSE
## [44,] FALSE    TRUE  FALSE
## [45,] FALSE   FALSE  FALSE
## [46,] FALSE    TRUE  FALSE
## [47,] FALSE    TRUE  FALSE
## [48,] FALSE   FALSE  FALSE
## [49,] FALSE   FALSE  FALSE
## [50,] FALSE   FALSE  FALSE
## [51,] FALSE    TRUE  FALSE
## [52,] FALSE   FALSE  FALSE
## [53,] FALSE    TRUE  FALSE
## [54,] FALSE   FALSE  FALSE
## [55,] FALSE   FALSE  FALSE
## [56,] FALSE    TRUE  FALSE
## [57,] FALSE    TRUE  FALSE
## [58,] FALSE   FALSE  FALSE
## [59,] FALSE    TRUE  FALSE
## [60,] FALSE    TRUE  FALSE
## [61,] FALSE    TRUE  FALSE
## [62,] FALSE   FALSE  FALSE
## [63,] FALSE   FALSE  FALSE
## [64,] FALSE   FALSE  FALSE
## [65,] FALSE    TRUE  FALSE
## [66,] FALSE   FALSE  FALSE
## [67,] FALSE   FALSE  FALSE
## [68,] FALSE   FALSE  FALSE
## [69,] FALSE   FALSE  FALSE
## [70,] FALSE   FALSE  FALSE
## [71,] FALSE   FALSE  FALSE
## [72,] FALSE    TRUE  FALSE
## [73,] FALSE   FALSE  FALSE
## [74,] FALSE   FALSE  FALSE
## [75,] FALSE   FALSE  FALSE
## [76,] FALSE    TRUE  FALSE
## [77,] FALSE   FALSE  FALSE
## [78,] FALSE   FALSE  FALSE
## [79,] FALSE   FALSE  FALSE
## [80,] FALSE    TRUE  FALSE
## [81,] FALSE   FALSE  FALSE
## [82,] FALSE   FALSE  FALSE
## [83,] FALSE   FALSE  FALSE
```
##### You can see where NA occurs, because the function is.na marks true for every place that has NA.

5. Show a list of the column names is this data frame.

```r
colnames(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
filter(sleep, vore=="herbi")
```

```
##                              name        genus  vore          order
## 1                 Mountain beaver   Aplodontia herbi       Rodentia
## 2                             Cow          Bos herbi   Artiodactyla
## 3                Three-toed sloth     Bradypus herbi         Pilosa
## 4                        Roe deer    Capreolus herbi   Artiodactyla
## 5                            Goat        Capri herbi   Artiodactyla
## 6                      Guinea pig        Cavis herbi       Rodentia
## 7                      Chinchilla   Chinchilla herbi       Rodentia
## 8                      Tree hyrax  Dendrohyrax herbi     Hyracoidea
## 9                  Asian elephant      Elephas herbi    Proboscidea
## 10                          Horse        Equus herbi Perissodactyla
## 11                         Donkey        Equus herbi Perissodactyla
## 12      Western american chipmunk     Eutamias herbi       Rodentia
## 13                        Giraffe      Giraffa herbi   Artiodactyla
## 14                     Gray hyrax  Heterohyrax herbi     Hyracoidea
## 15                 Mongoose lemur        Lemur herbi       Primates
## 16               African elephant    Loxodonta herbi    Proboscidea
## 17               Mongolian gerbil     Meriones herbi       Rodentia
## 18                 Golden hamster Mesocricetus herbi       Rodentia
## 19                          Vole      Microtus herbi       Rodentia
## 20                    House mouse          Mus herbi       Rodentia
## 21           Round-tailed muskrat     Neofiber herbi       Rodentia
## 22                           Degu      Octodon herbi       Rodentia
## 23                         Rabbit  Oryctolagus herbi     Lagomorpha
## 24                          Sheep         Ovis herbi   Artiodactyla
## 25                        Potoroo     Potorous herbi  Diprotodontia
## 26                 Laboratory rat       Rattus herbi       Rodentia
## 27                     Cotton rat     Sigmodon herbi       Rodentia
## 28         Arctic ground squirrel Spermophilus herbi       Rodentia
## 29 Thirteen-lined ground squirrel Spermophilus herbi       Rodentia
## 30 Golden-mantled ground squirrel Spermophilus herbi       Rodentia
## 31      Eastern american chipmunk       Tamias herbi       Rodentia
## 32                Brazilian tapir      Tapirus herbi Perissodactyla
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
## 1            nt        14.4       2.4          NA   9.6      NA    1.350
## 2  domesticated         4.0       0.7   0.6666667  20.0 0.42300  600.000
## 3          <NA>        14.4       2.2   0.7666667   9.6      NA    3.850
## 4            lc         3.0        NA          NA  21.0 0.09820   14.800
## 5            lc         5.3       0.6          NA  18.7 0.11500   33.500
## 6  domesticated         9.4       0.8   0.2166667  14.6 0.00550    0.728
## 7  domesticated        12.5       1.5   0.1166667  11.5 0.00640    0.420
## 8            lc         5.3       0.5          NA  18.7 0.01230    2.950
## 9            en         3.9        NA          NA  20.1 4.60300 2547.000
## 10 domesticated         2.9       0.6   1.0000000  21.1 0.65500  521.000
## 11 domesticated         3.1       0.4          NA  20.9 0.41900  187.000
## 12         <NA>        14.9        NA          NA   9.1      NA    0.071
## 13           cd         1.9       0.4          NA  22.1      NA  899.995
## 14           lc         6.3       0.6          NA  17.7 0.01227    2.625
## 15           vu         9.5       0.9          NA  14.5      NA    1.670
## 16           vu         3.3        NA          NA  20.7 5.71200 6654.000
## 17           lc        14.2       1.9          NA   9.8      NA    0.053
## 18           en        14.3       3.1   0.2000000   9.7 0.00100    0.120
## 19         <NA>        12.8        NA          NA  11.2      NA    0.035
## 20           nt        12.5       1.4   0.1833333  11.5 0.00040    0.022
## 21           nt        14.6        NA          NA   9.4      NA    0.266
## 22           lc         7.7       0.9          NA  16.3      NA    0.210
## 23 domesticated         8.4       0.9   0.4166667  15.6 0.01210    2.500
## 24 domesticated         3.8       0.6          NA  20.2 0.17500   55.500
## 25         <NA>        11.1       1.5          NA  12.9      NA    1.100
## 26           lc        13.0       2.4   0.1833333  11.0 0.00190    0.320
## 27         <NA>        11.3       1.1   0.1500000  12.7 0.00118    0.148
## 28           lc        16.6        NA          NA   7.4 0.00570    0.920
## 29           lc        13.8       3.4   0.2166667  10.2 0.00400    0.101
## 30           lc        15.9       3.0          NA   8.1      NA    0.205
## 31         <NA>        15.8        NA          NA   8.2      NA    0.112
## 32           vu         4.4       1.0   0.9000000  19.6 0.16900  207.501
```
##### There are 32 herbivores.

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 19kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small_mammals <- data.frame(filter(sleep, bodywt<=19))
large_mammals <- data.frame(filter(sleep, bodywt>=200))
small_mammals
```

```
##                              name         genus    vore           order
## 1                      Owl monkey         Aotus    omni        Primates
## 2                 Mountain beaver    Aplodontia   herbi        Rodentia
## 3      Greater short-tailed shrew       Blarina    omni    Soricomorpha
## 4                Three-toed sloth      Bradypus   herbi          Pilosa
## 5                    Vesper mouse       Calomys    <NA>        Rodentia
## 6                             Dog         Canis   carni       Carnivora
## 7                        Roe deer     Capreolus   herbi    Artiodactyla
## 8                      Guinea pig         Cavis   herbi        Rodentia
## 9                          Grivet Cercopithecus    omni        Primates
## 10                     Chinchilla    Chinchilla   herbi        Rodentia
## 11                Star-nosed mole     Condylura    omni    Soricomorpha
## 12      African giant pouched rat    Cricetomys    omni        Rodentia
## 13      Lesser short-tailed shrew     Cryptotis    omni    Soricomorpha
## 14           Long-nosed armadillo       Dasypus   carni       Cingulata
## 15                     Tree hyrax   Dendrohyrax   herbi      Hyracoidea
## 16         North American Opossum     Didelphis    omni Didelphimorphia
## 17                  Big brown bat     Eptesicus insecti      Chiroptera
## 18              European hedgehog     Erinaceus    omni  Erinaceomorpha
## 19                   Patas monkey  Erythrocebus    omni        Primates
## 20      Western american chipmunk      Eutamias   herbi        Rodentia
## 21                   Domestic cat         Felis   carni       Carnivora
## 22                         Galago        Galago    omni        Primates
## 23                     Gray hyrax   Heterohyrax   herbi      Hyracoidea
## 24                 Mongoose lemur         Lemur   herbi        Primates
## 25           Thick-tailed opposum    Lutreolina   carni Didelphimorphia
## 26                        Macaque        Macaca    omni        Primates
## 27               Mongolian gerbil      Meriones   herbi        Rodentia
## 28                 Golden hamster  Mesocricetus   herbi        Rodentia
## 29                          Vole       Microtus   herbi        Rodentia
## 30                    House mouse           Mus   herbi        Rodentia
## 31               Little brown bat        Myotis insecti      Chiroptera
## 32           Round-tailed muskrat      Neofiber   herbi        Rodentia
## 33                     Slow loris     Nyctibeus   carni        Primates
## 34                           Degu       Octodon   herbi        Rodentia
## 35     Northern grasshopper mouse     Onychomys   carni        Rodentia
## 36                         Rabbit   Oryctolagus   herbi      Lagomorpha
## 37                Desert hedgehog   Paraechinus    <NA>  Erinaceomorpha
## 38                          Potto  Perodicticus    omni        Primates
## 39                     Deer mouse    Peromyscus    <NA>        Rodentia
## 40                      Phalanger     Phalanger    <NA>   Diprotodontia
## 41                        Potoroo      Potorous   herbi   Diprotodontia
## 42                     Rock hyrax      Procavia    <NA>      Hyracoidea
## 43                 Laboratory rat        Rattus   herbi        Rodentia
## 44          African striped mouse     Rhabdomys    omni        Rodentia
## 45                Squirrel monkey       Saimiri    omni        Primates
## 46          Eastern american mole      Scalopus insecti    Soricomorpha
## 47                     Cotton rat      Sigmodon   herbi        Rodentia
## 48                       Mole rat        Spalax    <NA>        Rodentia
## 49         Arctic ground squirrel  Spermophilus   herbi        Rodentia
## 50 Thirteen-lined ground squirrel  Spermophilus   herbi        Rodentia
## 51 Golden-mantled ground squirrel  Spermophilus   herbi        Rodentia
## 52                     Musk shrew        Suncus    <NA>    Soricomorpha
## 53            Short-nosed echidna  Tachyglossus insecti     Monotremata
## 54      Eastern american chipmunk        Tamias   herbi        Rodentia
## 55                         Tenrec        Tenrec    omni    Afrosoricida
## 56                     Tree shrew        Tupaia    omni      Scandentia
## 57                          Genet       Genetta   carni       Carnivora
## 58                     Arctic fox        Vulpes   carni       Carnivora
## 59                        Red fox        Vulpes   carni       Carnivora
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt bodywt
## 1          <NA>        17.0       1.8          NA   7.0 0.01550  0.480
## 2            nt        14.4       2.4          NA   9.6      NA  1.350
## 3            lc        14.9       2.3   0.1333333   9.1 0.00029  0.019
## 4          <NA>        14.4       2.2   0.7666667   9.6      NA  3.850
## 5          <NA>         7.0        NA          NA  17.0      NA  0.045
## 6  domesticated        10.1       2.9   0.3333333  13.9 0.07000 14.000
## 7            lc         3.0        NA          NA  21.0 0.09820 14.800
## 8  domesticated         9.4       0.8   0.2166667  14.6 0.00550  0.728
## 9            lc        10.0       0.7          NA  14.0      NA  4.750
## 10 domesticated        12.5       1.5   0.1166667  11.5 0.00640  0.420
## 11           lc        10.3       2.2          NA  13.7 0.00100  0.060
## 12         <NA>         8.3       2.0          NA  15.7 0.00660  1.000
## 13           lc         9.1       1.4   0.1500000  14.9 0.00014  0.005
## 14           lc        17.4       3.1   0.3833333   6.6 0.01080  3.500
## 15           lc         5.3       0.5          NA  18.7 0.01230  2.950
## 16           lc        18.0       4.9   0.3333333   6.0 0.00630  1.700
## 17           lc        19.7       3.9   0.1166667   4.3 0.00030  0.023
## 18           lc        10.1       3.5   0.2833333  13.9 0.00350  0.770
## 19           lc        10.9       1.1          NA  13.1 0.11500 10.000
## 20         <NA>        14.9        NA          NA   9.1      NA  0.071
## 21 domesticated        12.5       3.2   0.4166667  11.5 0.02560  3.300
## 22         <NA>         9.8       1.1   0.5500000  14.2 0.00500  0.200
## 23           lc         6.3       0.6          NA  17.7 0.01227  2.625
## 24           vu         9.5       0.9          NA  14.5      NA  1.670
## 25           lc        19.4       6.6          NA   4.6      NA  0.370
## 26         <NA>        10.1       1.2   0.7500000  13.9 0.17900  6.800
## 27           lc        14.2       1.9          NA   9.8      NA  0.053
## 28           en        14.3       3.1   0.2000000   9.7 0.00100  0.120
## 29         <NA>        12.8        NA          NA  11.2      NA  0.035
## 30           nt        12.5       1.4   0.1833333  11.5 0.00040  0.022
## 31         <NA>        19.9       2.0   0.2000000   4.1 0.00025  0.010
## 32           nt        14.6        NA          NA   9.4      NA  0.266
## 33         <NA>        11.0        NA          NA  13.0 0.01250  1.400
## 34           lc         7.7       0.9          NA  16.3      NA  0.210
## 35           lc        14.5        NA          NA   9.5      NA  0.028
## 36 domesticated         8.4       0.9   0.4166667  15.6 0.01210  2.500
## 37           lc        10.3       2.7          NA  13.7 0.00240  0.550
## 38           lc        11.0        NA          NA  13.0      NA  1.100
## 39         <NA>        11.5        NA          NA  12.5      NA  0.021
## 40         <NA>        13.7       1.8          NA  10.3 0.01140  1.620
## 41         <NA>        11.1       1.5          NA  12.9      NA  1.100
## 42           lc         5.4       0.5          NA  18.6 0.02100  3.600
## 43           lc        13.0       2.4   0.1833333  11.0 0.00190  0.320
## 44         <NA>         8.7        NA          NA  15.3      NA  0.044
## 45         <NA>         9.6       1.4          NA  14.4 0.02000  0.743
## 46           lc         8.4       2.1   0.1666667  15.6 0.00120  0.075
## 47         <NA>        11.3       1.1   0.1500000  12.7 0.00118  0.148
## 48         <NA>        10.6       2.4          NA  13.4 0.00300  0.122
## 49           lc        16.6        NA          NA   7.4 0.00570  0.920
## 50           lc        13.8       3.4   0.2166667  10.2 0.00400  0.101
## 51           lc        15.9       3.0          NA   8.1      NA  0.205
## 52         <NA>        12.8       2.0   0.1833333  11.2 0.00033  0.048
## 53         <NA>         8.6        NA          NA  15.4 0.02500  4.500
## 54         <NA>        15.8        NA          NA   8.2      NA  0.112
## 55         <NA>        15.6       2.3          NA   8.4 0.00260  0.900
## 56         <NA>         8.9       2.6   0.2333333  15.1 0.00250  0.104
## 57         <NA>         6.3       1.3          NA  17.7 0.01750  2.000
## 58         <NA>        12.5        NA          NA  11.5 0.04450  3.380
## 59         <NA>         9.8       2.4   0.3500000  14.2 0.05040  4.230
```

```r
large_mammals
```

```
##               name         genus  vore          order conservation sleep_total
## 1              Cow           Bos herbi   Artiodactyla domesticated         4.0
## 2   Asian elephant       Elephas herbi    Proboscidea           en         3.9
## 3            Horse         Equus herbi Perissodactyla domesticated         2.9
## 4          Giraffe       Giraffa herbi   Artiodactyla           cd         1.9
## 5      Pilot whale Globicephalus carni        Cetacea           cd         2.7
## 6 African elephant     Loxodonta herbi    Proboscidea           vu         3.3
## 7  Brazilian tapir       Tapirus herbi Perissodactyla           vu         4.4
##   sleep_rem sleep_cycle awake brainwt   bodywt
## 1       0.7   0.6666667 20.00   0.423  600.000
## 2        NA          NA 20.10   4.603 2547.000
## 3       0.6   1.0000000 21.10   0.655  521.000
## 4       0.4          NA 22.10      NA  899.995
## 5       0.1          NA 21.35      NA  800.000
## 6        NA          NA 20.70   5.712 6654.000
## 7       1.0   0.9000000 19.60   0.169  207.501
```

8. What is the mean weight for both the small and large mammals?

```r
mean (small_mammals[,11])
```

```
## [1] 1.797847
```


```r
mean (large_mammals[,11])
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

```r
mean(small_mammals[,6])
```

```
## [1] 11.78644
```


```r
mean(large_mammals[,6])
```

```
## [1] 3.3
```
##### Small animals sleep more on average. 

10. Which animal is the sleepiest among the entire dataframe?

```r
sleep
```

```
##                              name         genus    vore           order
## 1                         Cheetah      Acinonyx   carni       Carnivora
## 2                      Owl monkey         Aotus    omni        Primates
## 3                 Mountain beaver    Aplodontia   herbi        Rodentia
## 4      Greater short-tailed shrew       Blarina    omni    Soricomorpha
## 5                             Cow           Bos   herbi    Artiodactyla
## 6                Three-toed sloth      Bradypus   herbi          Pilosa
## 7               Northern fur seal   Callorhinus   carni       Carnivora
## 8                    Vesper mouse       Calomys    <NA>        Rodentia
## 9                             Dog         Canis   carni       Carnivora
## 10                       Roe deer     Capreolus   herbi    Artiodactyla
## 11                           Goat         Capri   herbi    Artiodactyla
## 12                     Guinea pig         Cavis   herbi        Rodentia
## 13                         Grivet Cercopithecus    omni        Primates
## 14                     Chinchilla    Chinchilla   herbi        Rodentia
## 15                Star-nosed mole     Condylura    omni    Soricomorpha
## 16      African giant pouched rat    Cricetomys    omni        Rodentia
## 17      Lesser short-tailed shrew     Cryptotis    omni    Soricomorpha
## 18           Long-nosed armadillo       Dasypus   carni       Cingulata
## 19                     Tree hyrax   Dendrohyrax   herbi      Hyracoidea
## 20         North American Opossum     Didelphis    omni Didelphimorphia
## 21                 Asian elephant       Elephas   herbi     Proboscidea
## 22                  Big brown bat     Eptesicus insecti      Chiroptera
## 23                          Horse         Equus   herbi  Perissodactyla
## 24                         Donkey         Equus   herbi  Perissodactyla
## 25              European hedgehog     Erinaceus    omni  Erinaceomorpha
## 26                   Patas monkey  Erythrocebus    omni        Primates
## 27      Western american chipmunk      Eutamias   herbi        Rodentia
## 28                   Domestic cat         Felis   carni       Carnivora
## 29                         Galago        Galago    omni        Primates
## 30                        Giraffe       Giraffa   herbi    Artiodactyla
## 31                    Pilot whale Globicephalus   carni         Cetacea
## 32                      Gray seal  Haliochoerus   carni       Carnivora
## 33                     Gray hyrax   Heterohyrax   herbi      Hyracoidea
## 34                          Human          Homo    omni        Primates
## 35                 Mongoose lemur         Lemur   herbi        Primates
## 36               African elephant     Loxodonta   herbi     Proboscidea
## 37           Thick-tailed opposum    Lutreolina   carni Didelphimorphia
## 38                        Macaque        Macaca    omni        Primates
## 39               Mongolian gerbil      Meriones   herbi        Rodentia
## 40                 Golden hamster  Mesocricetus   herbi        Rodentia
## 41                          Vole       Microtus   herbi        Rodentia
## 42                    House mouse           Mus   herbi        Rodentia
## 43               Little brown bat        Myotis insecti      Chiroptera
## 44           Round-tailed muskrat      Neofiber   herbi        Rodentia
## 45                     Slow loris     Nyctibeus   carni        Primates
## 46                           Degu       Octodon   herbi        Rodentia
## 47     Northern grasshopper mouse     Onychomys   carni        Rodentia
## 48                         Rabbit   Oryctolagus   herbi      Lagomorpha
## 49                          Sheep          Ovis   herbi    Artiodactyla
## 50                     Chimpanzee           Pan    omni        Primates
## 51                          Tiger      Panthera   carni       Carnivora
## 52                         Jaguar      Panthera   carni       Carnivora
## 53                           Lion      Panthera   carni       Carnivora
## 54                         Baboon         Papio    omni        Primates
## 55                Desert hedgehog   Paraechinus    <NA>  Erinaceomorpha
## 56                          Potto  Perodicticus    omni        Primates
## 57                     Deer mouse    Peromyscus    <NA>        Rodentia
## 58                      Phalanger     Phalanger    <NA>   Diprotodontia
## 59                   Caspian seal         Phoca   carni       Carnivora
## 60                Common porpoise      Phocoena   carni         Cetacea
## 61                        Potoroo      Potorous   herbi   Diprotodontia
## 62                Giant armadillo    Priodontes insecti       Cingulata
## 63                     Rock hyrax      Procavia    <NA>      Hyracoidea
## 64                 Laboratory rat        Rattus   herbi        Rodentia
## 65          African striped mouse     Rhabdomys    omni        Rodentia
## 66                Squirrel monkey       Saimiri    omni        Primates
## 67          Eastern american mole      Scalopus insecti    Soricomorpha
## 68                     Cotton rat      Sigmodon   herbi        Rodentia
## 69                       Mole rat        Spalax    <NA>        Rodentia
## 70         Arctic ground squirrel  Spermophilus   herbi        Rodentia
## 71 Thirteen-lined ground squirrel  Spermophilus   herbi        Rodentia
## 72 Golden-mantled ground squirrel  Spermophilus   herbi        Rodentia
## 73                     Musk shrew        Suncus    <NA>    Soricomorpha
## 74                            Pig           Sus    omni    Artiodactyla
## 75            Short-nosed echidna  Tachyglossus insecti     Monotremata
## 76      Eastern american chipmunk        Tamias   herbi        Rodentia
## 77                Brazilian tapir       Tapirus   herbi  Perissodactyla
## 78                         Tenrec        Tenrec    omni    Afrosoricida
## 79                     Tree shrew        Tupaia    omni      Scandentia
## 80           Bottle-nosed dolphin      Tursiops   carni         Cetacea
## 81                          Genet       Genetta   carni       Carnivora
## 82                     Arctic fox        Vulpes   carni       Carnivora
## 83                        Red fox        Vulpes   carni       Carnivora
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
## 1            lc        12.1        NA          NA 11.90      NA   50.000
## 2          <NA>        17.0       1.8          NA  7.00 0.01550    0.480
## 3            nt        14.4       2.4          NA  9.60      NA    1.350
## 4            lc        14.9       2.3   0.1333333  9.10 0.00029    0.019
## 5  domesticated         4.0       0.7   0.6666667 20.00 0.42300  600.000
## 6          <NA>        14.4       2.2   0.7666667  9.60      NA    3.850
## 7            vu         8.7       1.4   0.3833333 15.30      NA   20.490
## 8          <NA>         7.0        NA          NA 17.00      NA    0.045
## 9  domesticated        10.1       2.9   0.3333333 13.90 0.07000   14.000
## 10           lc         3.0        NA          NA 21.00 0.09820   14.800
## 11           lc         5.3       0.6          NA 18.70 0.11500   33.500
## 12 domesticated         9.4       0.8   0.2166667 14.60 0.00550    0.728
## 13           lc        10.0       0.7          NA 14.00      NA    4.750
## 14 domesticated        12.5       1.5   0.1166667 11.50 0.00640    0.420
## 15           lc        10.3       2.2          NA 13.70 0.00100    0.060
## 16         <NA>         8.3       2.0          NA 15.70 0.00660    1.000
## 17           lc         9.1       1.4   0.1500000 14.90 0.00014    0.005
## 18           lc        17.4       3.1   0.3833333  6.60 0.01080    3.500
## 19           lc         5.3       0.5          NA 18.70 0.01230    2.950
## 20           lc        18.0       4.9   0.3333333  6.00 0.00630    1.700
## 21           en         3.9        NA          NA 20.10 4.60300 2547.000
## 22           lc        19.7       3.9   0.1166667  4.30 0.00030    0.023
## 23 domesticated         2.9       0.6   1.0000000 21.10 0.65500  521.000
## 24 domesticated         3.1       0.4          NA 20.90 0.41900  187.000
## 25           lc        10.1       3.5   0.2833333 13.90 0.00350    0.770
## 26           lc        10.9       1.1          NA 13.10 0.11500   10.000
## 27         <NA>        14.9        NA          NA  9.10      NA    0.071
## 28 domesticated        12.5       3.2   0.4166667 11.50 0.02560    3.300
## 29         <NA>         9.8       1.1   0.5500000 14.20 0.00500    0.200
## 30           cd         1.9       0.4          NA 22.10      NA  899.995
## 31           cd         2.7       0.1          NA 21.35      NA  800.000
## 32           lc         6.2       1.5          NA 17.80 0.32500   85.000
## 33           lc         6.3       0.6          NA 17.70 0.01227    2.625
## 34         <NA>         8.0       1.9   1.5000000 16.00 1.32000   62.000
## 35           vu         9.5       0.9          NA 14.50      NA    1.670
## 36           vu         3.3        NA          NA 20.70 5.71200 6654.000
## 37           lc        19.4       6.6          NA  4.60      NA    0.370
## 38         <NA>        10.1       1.2   0.7500000 13.90 0.17900    6.800
## 39           lc        14.2       1.9          NA  9.80      NA    0.053
## 40           en        14.3       3.1   0.2000000  9.70 0.00100    0.120
## 41         <NA>        12.8        NA          NA 11.20      NA    0.035
## 42           nt        12.5       1.4   0.1833333 11.50 0.00040    0.022
## 43         <NA>        19.9       2.0   0.2000000  4.10 0.00025    0.010
## 44           nt        14.6        NA          NA  9.40      NA    0.266
## 45         <NA>        11.0        NA          NA 13.00 0.01250    1.400
## 46           lc         7.7       0.9          NA 16.30      NA    0.210
## 47           lc        14.5        NA          NA  9.50      NA    0.028
## 48 domesticated         8.4       0.9   0.4166667 15.60 0.01210    2.500
## 49 domesticated         3.8       0.6          NA 20.20 0.17500   55.500
## 50         <NA>         9.7       1.4   1.4166667 14.30 0.44000   52.200
## 51           en        15.8        NA          NA  8.20      NA  162.564
## 52           nt        10.4        NA          NA 13.60 0.15700  100.000
## 53           vu        13.5        NA          NA 10.50      NA  161.499
## 54         <NA>         9.4       1.0   0.6666667 14.60 0.18000   25.235
## 55           lc        10.3       2.7          NA 13.70 0.00240    0.550
## 56           lc        11.0        NA          NA 13.00      NA    1.100
## 57         <NA>        11.5        NA          NA 12.50      NA    0.021
## 58         <NA>        13.7       1.8          NA 10.30 0.01140    1.620
## 59           vu         3.5       0.4          NA 20.50      NA   86.000
## 60           vu         5.6        NA          NA 18.45      NA   53.180
## 61         <NA>        11.1       1.5          NA 12.90      NA    1.100
## 62           en        18.1       6.1          NA  5.90 0.08100   60.000
## 63           lc         5.4       0.5          NA 18.60 0.02100    3.600
## 64           lc        13.0       2.4   0.1833333 11.00 0.00190    0.320
## 65         <NA>         8.7        NA          NA 15.30      NA    0.044
## 66         <NA>         9.6       1.4          NA 14.40 0.02000    0.743
## 67           lc         8.4       2.1   0.1666667 15.60 0.00120    0.075
## 68         <NA>        11.3       1.1   0.1500000 12.70 0.00118    0.148
## 69         <NA>        10.6       2.4          NA 13.40 0.00300    0.122
## 70           lc        16.6        NA          NA  7.40 0.00570    0.920
## 71           lc        13.8       3.4   0.2166667 10.20 0.00400    0.101
## 72           lc        15.9       3.0          NA  8.10      NA    0.205
## 73         <NA>        12.8       2.0   0.1833333 11.20 0.00033    0.048
## 74 domesticated         9.1       2.4   0.5000000 14.90 0.18000   86.250
## 75         <NA>         8.6        NA          NA 15.40 0.02500    4.500
## 76         <NA>        15.8        NA          NA  8.20      NA    0.112
## 77           vu         4.4       1.0   0.9000000 19.60 0.16900  207.501
## 78         <NA>        15.6       2.3          NA  8.40 0.00260    0.900
## 79         <NA>         8.9       2.6   0.2333333 15.10 0.00250    0.104
## 80         <NA>         5.2        NA          NA 18.80      NA  173.330
## 81         <NA>         6.3       1.3          NA 17.70 0.01750    2.000
## 82         <NA>        12.5        NA          NA 11.50 0.04450    3.380
## 83         <NA>         9.8       2.4   0.3500000 14.20 0.05040    4.230
```
##### The little brown bat is the sleepiest with 19.9 value in sleep_total. 

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
