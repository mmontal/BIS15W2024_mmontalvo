---
title: "Lab 4 Homework"
author: "Mariana Montalvo"
date: "2024-01-29"
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

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
dim(homerange)
```

```
## [1] 569  24
```


```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


```r
select_if(homerange, is.character)
```

```
## # A tibble: 569 × 16
##    taxon        common.name class order family genus species primarymethod N    
##    <chr>        <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake fishes  american e… acti… angu… angui… angu… rostra… telemetry     16   
##  2 river fishes blacktail … acti… cypr… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 river fishes central st… acti… cypr… cypri… camp… anomal… mark-recaptu… 20   
##  4 river fishes rosyside d… acti… cypr… cypri… clin… fundul… mark-recaptu… 26   
##  5 river fishes longnose d… acti… cypr… cypri… rhin… catara… mark-recaptu… 17   
##  6 river fishes muskellunge acti… esoc… esoci… esox  masqui… telemetry     5    
##  7 marine fish… pollack     acti… gadi… gadid… poll… pollac… telemetry     2    
##  8 marine fish… saithe      acti… gadi… gadid… poll… virens  telemetry     2    
##  9 marine fish… lined surg… acti… perc… acant… acan… lineat… direct obser… <NA> 
## 10 marine fish… orangespin… acti… perc… acant… naso  litura… telemetry     8    
## # ℹ 559 more rows
## # ℹ 7 more variables: alternative.mass.reference <chr>, hra.reference <chr>,
## #   realm <chr>, thermoregulation <chr>, locomotion <chr>, trophic.guild <chr>,
## #   prey.size.reference <chr>
```


```r
select_if(homerange, is.numeric)
```

```
## # A tibble: 569 × 8
##    mean.mass.g log10.mass mean.hra.m2 log10.hra dimension preymass
##          <dbl>      <dbl>       <dbl>     <dbl>     <dbl>    <dbl>
##  1        887       2.95     282750        5.45         3    NA   
##  2        562       2.75        282.       2.45         2    NA   
##  3         34       1.53        116.       2.06         2    NA   
##  4          4       0.602       126.       2.10         2    NA   
##  5          4       0.602        87.1      1.94         2    NA   
##  6       3525       3.55      39344.       4.59         2    NA   
##  7        737.      2.87       9056.       3.96         2     1.39
##  8        449.      2.65      44516.       4.65         2    NA   
##  9        109.      2.04         11.1      1.05         2    NA   
## 10        772.      2.89      32093.       4.51         2    NA   
## # ℹ 559 more rows
## # ℹ 2 more variables: log10.preymass <dbl>, PPMR <dbl>
```


```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild        dimension        preymass         log10.preymass   
##  Length:569         Min.   :2.000   Min.   :     0.67   Min.   :-0.1739  
##  Class :character   1st Qu.:2.000   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Median :2.000   Median :    53.75   Median : 1.7304  
##                     Mean   :2.218   Mean   :  3989.88   Mean   : 2.0188  
##                     3rd Qu.:2.000   3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                     Max.   :3.000   Max.   :130233.20   Max.   : 5.1147  
##                                     NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
levels(as.factor(homerange$taxon))
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```


```r
levels(as.factor(homerange$order))
```

```
##  [1] "accipitriformes"       "afrosoricida"          "anguilliformes"       
##  [4] "anseriformes"          "apterygiformes"        "artiodactyla"         
##  [7] "caprimulgiformes"      "carnivora"             "charadriiformes"      
## [10] "columbidormes"         "columbiformes"         "coraciiformes"        
## [13] "cuculiformes"          "cypriniformes"         "dasyuromorpha"        
## [16] "dasyuromorpia"         "didelphimorphia"       "diprodontia"          
## [19] "diprotodontia"         "erinaceomorpha"        "esociformes"          
## [22] "falconiformes"         "gadiformes"            "galliformes"          
## [25] "gruiformes"            "lagomorpha"            "macroscelidea"        
## [28] "monotrematae"          "passeriformes"         "pelecaniformes"       
## [31] "peramelemorphia"       "perciformes"           "perissodactyla"       
## [34] "piciformes"            "pilosa"                "proboscidea"          
## [37] "psittaciformes"        "rheiformes"            "roden"                
## [40] "rodentia"              "salmoniformes"         "scorpaeniformes"      
## [43] "siluriformes"          "soricomorpha"          "squamata"             
## [46] "strigiformes"          "struthioniformes"      "syngnathiformes"      
## [49] "testudines"            "tetraodontiformes\xa0" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- select(homerange, taxon:species)
taxa
```

```
## # A tibble: 569 × 7
##    taxon         common.name             class        order family genus species
##    <chr>         <chr>                   <chr>        <chr> <chr>  <chr> <chr>  
##  1 lake fishes   american eel            actinoptery… angu… angui… angu… rostra…
##  2 river fishes  blacktail redhorse      actinoptery… cypr… catos… moxo… poecil…
##  3 river fishes  central stoneroller     actinoptery… cypr… cypri… camp… anomal…
##  4 river fishes  rosyside dace           actinoptery… cypr… cypri… clin… fundul…
##  5 river fishes  longnose dace           actinoptery… cypr… cypri… rhin… catara…
##  6 river fishes  muskellunge             actinoptery… esoc… esoci… esox  masqui…
##  7 marine fishes pollack                 actinoptery… gadi… gadid… poll… pollac…
##  8 marine fishes saithe                  actinoptery… gadi… gadid… poll… virens 
##  9 marine fishes lined surgeonfish       actinoptery… perc… acant… acan… lineat…
## 10 marine fishes orangespine unicornfish actinoptery… perc… acant… naso  litura…
## # ℹ 559 more rows
```

**5. The variable `taxon` identifies the common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- filter(homerange, trophic.guild=="carnivore")
carnivores
```

```
## # A tibble: 342 × 24
##    taxon        common.name class order family genus species primarymethod N    
##    <chr>        <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake fishes  american e… acti… angu… angui… angu… rostra… telemetry     16   
##  2 river fishes blacktail … acti… cypr… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 river fishes central st… acti… cypr… cypri… camp… anomal… mark-recaptu… 20   
##  4 river fishes rosyside d… acti… cypr… cypri… clin… fundul… mark-recaptu… 26   
##  5 river fishes longnose d… acti… cypr… cypri… rhin… catara… mark-recaptu… 17   
##  6 river fishes muskellunge acti… esoc… esoci… esox  masqui… telemetry     5    
##  7 marine fish… pollack     acti… gadi… gadid… poll… pollac… telemetry     2    
##  8 marine fish… saithe      acti… gadi… gadid… poll… virens  telemetry     2    
##  9 marine fish… giant trev… acti… perc… caran… cara… ignobi… telemetry     4    
## 10 lake fishes  rock bass   acti… perc… centr… ambl… rupest… mark-recaptu… 16   
## # ℹ 332 more rows
## # ℹ 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <dbl>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```


```r
herbivores <- filter(homerange, trophic.guild=="herbivore")
herbivores
```

```
## # A tibble: 227 × 24
##    taxon        common.name class order family genus species primarymethod N    
##    <chr>        <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 marine fish… lined surg… acti… perc… acant… acan… lineat… direct obser… <NA> 
##  2 marine fish… orangespin… acti… perc… acant… naso  litura… telemetry     8    
##  3 marine fish… bluespine … acti… perc… acant… naso  unicor… telemetry     7    
##  4 marine fish… redlip ble… acti… perc… blenn… ophi… atlant… direct obser… 20   
##  5 marine fish… bermuda ch… acti… perc… kypho… kyph… sectat… telemetry     11   
##  6 marine fish… cherubfish  acti… perc… pomac… cent… argi    direct obser… <NA> 
##  7 marine fish… damselfish  acti… perc… pomac… chro… chromis direct obser… <NA> 
##  8 marine fish… twinspot d… acti… perc… pomac… chry… biocel… direct obser… 18   
##  9 marine fish… wards dams… acti… perc… pomac… poma… wardi   direct obser… <NA> 
## 10 marine fish… australian… acti… perc… pomac… steg… apical… direct obser… <NA> 
## # ℹ 217 more rows
## # ℹ 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <dbl>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```


**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**

```r
mean(carnivores$mean.hra.m2)
```

```
## [1] 13039918
```


```r
mean(herbivores$mean.hra.m2)
```

```
## [1] 34137012
```



## Herbivores have on average a larger mean.

**9. Make a new dataframe `owls` that is limited to the mean mass, log10 mass, family, genus, and species of owls in the database. Which is the smallest owl? What is its common name? Do a little bit of searching online to see what you can learn about this species and provide a link below** 

```r
owls <- homerange %>% 
  select(mean.mass.g, log10.mass, family, genus, species, common.name) %>% 
  filter(family=="strigidae") %>% 
  arrange(mean.mass.g)
owls
```

```
## # A tibble: 8 × 6
##   mean.mass.g log10.mass family    genus      species     common.name       
##         <dbl>      <dbl> <chr>     <chr>      <chr>       <chr>             
## 1        61.3       1.79 strigidae glaucidium passerinum  Eurasian pygmy owl
## 2       119         2.08 strigidae aegolius   funereus    boreal owl        
## 3       156.        2.19 strigidae athene     noctua      little owl        
## 4       252         2.40 strigidae asio       otus        long-eared owl    
## 5       519         2.72 strigidae strix      aluco       tawny owl         
## 6      1510         3.18 strigidae bubo       virginianus great horned owl  
## 7      1920         3.28 strigidae nyctea     scandiaca   snowy owl         
## 8      2191         3.34 strigidae bubo       bubo        Eurasian eagle-owl
```
## The smallest owl is passerinum, its common name is Eurasian pygmy owl. These owls are native to the central Paleartic region and are found in coniferous forests. More information about this owl can be found here: https://animaldiversity.org/accounts/Glaucidium_passerinum/

**10. As measured by the data, which bird species has the largest homerange? Show all of your work, please. Look this species up online and tell me about it!**.  

```r
largest_homerange <- select (homerange, "class", "mean.hra.m2", "species", "common.name")
```


```r
largest_homerange %>% 
  filter(class =="aves") %>% 
  arrange(desc(mean.hra.m2))
```

```
## # A tibble: 140 × 4
##    class mean.hra.m2 species      common.name           
##    <chr>       <dbl> <chr>        <chr>                 
##  1 aves    241000000 cheriway     caracara              
##  2 aves    200980000 pygargus     Montagu's harrier     
##  3 aves    153860000 peregrinus   peregrine falcon      
##  4 aves    117300000 pennatus     booted eagle          
##  5 aves     84300000 camelus      ostrich               
##  6 aves     78500000 gallicus     short-toed snake eagle
##  7 aves     63585000 turtur       European turtle dove  
##  8 aves     63570000 percnopterus Egyptian vulture      
##  9 aves     50240000 buteo        common buzzard        
## 10 aves     50000000 biarmicus    lanner falcon         
## # ℹ 130 more rows
```
## The caracara has the largest homerange. They are black and white birds with yellow-orange bills. They live in open country and perch on trees and poles and fences. More information can be found at the following website: https://www.allaboutbirds.org/guide/Crested_Caracara/id#

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
