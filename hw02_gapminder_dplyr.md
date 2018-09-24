Alejandra\_hw02\_gapminder\_dplyr
================

\#Gapminder exploration and use of dplyr This is an R Markdown document
used for exploring the gapminder dataset through the functions of the
dplyr package. This document is intented to serve as a “cheatsheet” for
future work on R.

\#\#Loading data

[Readme](hw02_gapminder_dplyr.md)

``` r
library(gapminder)
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(knitr)
```

## Smell test the data

Is it a data.frame, a matrix, a vector, a list?

``` r
gapminder
```

    ## # A tibble: 1,704 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 1,694 more rows

``` r
typeof(gapminder)
```

    ## [1] "list"

When printing `gapminder` I can see the variables of the data set are
arranged in columns and the observations of these variables are arranged
in rows. This is a two dimensional object that holds different types of
data. I know this object is a data frame.

The function `typeof()` determines the (R internal) type or storage mode
of an object. The type of this data set is a **list**. A list in R
allows to gather a variety of objects under one name in an ordered way.
These objects can be matrices, vectors, data frames, other lists, etc.

What is its class?

``` r
class(gapminder)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

The `class()`function confirms that gapminder is a data frame. :+1:

How many variables/columns?

``` r
ncol(gapminder)
```

    ## [1] 6

How many rows/observations?

``` r
nrow(gapminder)
```

    ## [1] 1704

`ncol()` and `nrow()` functions return the number of columns and rows,
respectively.

Can you get these facts about “extent” or “size” in more than one way?
Can you imagine different functions being useful in different contexts?

``` r
dim(gapminder)
```

    ## [1] 1704    6

I can get the same information with `dim()` function, the output is
(number of rows, number of columns).

What data type is each variable?

``` r
lapply(gapminder,class)
```

    ## $country
    ## [1] "factor"
    ## 
    ## $continent
    ## [1] "factor"
    ## 
    ## $year
    ## [1] "integer"
    ## 
    ## $lifeExp
    ## [1] "numeric"
    ## 
    ## $pop
    ## [1] "integer"
    ## 
    ## $gdpPercap
    ## [1] "numeric"

I can also get all the above information with `str()`
    function:

``` r
str(gapminder)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

## Explore individual variables

Here I explore the categorical variable `country` and the quantitative
variable `lifeExp`.

### country

I would like to know how many countries are stored within `country`:

``` r
n_distinct(gapminder$country)
```

    ## [1] 142

Another way to do this is and to get all the unique values (all
countries) is:

``` r
unique(gapminder$country)
```

    ##   [1] Afghanistan              Albania                 
    ##   [3] Algeria                  Angola                  
    ##   [5] Argentina                Australia               
    ##   [7] Austria                  Bahrain                 
    ##   [9] Bangladesh               Belgium                 
    ##  [11] Benin                    Bolivia                 
    ##  [13] Bosnia and Herzegovina   Botswana                
    ##  [15] Brazil                   Bulgaria                
    ##  [17] Burkina Faso             Burundi                 
    ##  [19] Cambodia                 Cameroon                
    ##  [21] Canada                   Central African Republic
    ##  [23] Chad                     Chile                   
    ##  [25] China                    Colombia                
    ##  [27] Comoros                  Congo, Dem. Rep.        
    ##  [29] Congo, Rep.              Costa Rica              
    ##  [31] Cote d'Ivoire            Croatia                 
    ##  [33] Cuba                     Czech Republic          
    ##  [35] Denmark                  Djibouti                
    ##  [37] Dominican Republic       Ecuador                 
    ##  [39] Egypt                    El Salvador             
    ##  [41] Equatorial Guinea        Eritrea                 
    ##  [43] Ethiopia                 Finland                 
    ##  [45] France                   Gabon                   
    ##  [47] Gambia                   Germany                 
    ##  [49] Ghana                    Greece                  
    ##  [51] Guatemala                Guinea                  
    ##  [53] Guinea-Bissau            Haiti                   
    ##  [55] Honduras                 Hong Kong, China        
    ##  [57] Hungary                  Iceland                 
    ##  [59] India                    Indonesia               
    ##  [61] Iran                     Iraq                    
    ##  [63] Ireland                  Israel                  
    ##  [65] Italy                    Jamaica                 
    ##  [67] Japan                    Jordan                  
    ##  [69] Kenya                    Korea, Dem. Rep.        
    ##  [71] Korea, Rep.              Kuwait                  
    ##  [73] Lebanon                  Lesotho                 
    ##  [75] Liberia                  Libya                   
    ##  [77] Madagascar               Malawi                  
    ##  [79] Malaysia                 Mali                    
    ##  [81] Mauritania               Mauritius               
    ##  [83] Mexico                   Mongolia                
    ##  [85] Montenegro               Morocco                 
    ##  [87] Mozambique               Myanmar                 
    ##  [89] Namibia                  Nepal                   
    ##  [91] Netherlands              New Zealand             
    ##  [93] Nicaragua                Niger                   
    ##  [95] Nigeria                  Norway                  
    ##  [97] Oman                     Pakistan                
    ##  [99] Panama                   Paraguay                
    ## [101] Peru                     Philippines             
    ## [103] Poland                   Portugal                
    ## [105] Puerto Rico              Reunion                 
    ## [107] Romania                  Rwanda                  
    ## [109] Sao Tome and Principe    Saudi Arabia            
    ## [111] Senegal                  Serbia                  
    ## [113] Sierra Leone             Singapore               
    ## [115] Slovak Republic          Slovenia                
    ## [117] Somalia                  South Africa            
    ## [119] Spain                    Sri Lanka               
    ## [121] Sudan                    Swaziland               
    ## [123] Sweden                   Switzerland             
    ## [125] Syria                    Taiwan                  
    ## [127] Tanzania                 Thailand                
    ## [129] Togo                     Trinidad and Tobago     
    ## [131] Tunisia                  Turkey                  
    ## [133] Uganda                   United Kingdom          
    ## [135] United States            Uruguay                 
    ## [137] Venezuela                Vietnam                 
    ## [139] West Bank and Gaza       Yemen, Rep.             
    ## [141] Zambia                   Zimbabwe                
    ## 142 Levels: Afghanistan Albania Algeria Angola Argentina ... Zimbabwe

### lifeExp

For the life expectancy variable I would like to know the range of the
values first:

``` r
range(gapminder$lifeExp)
```

    ## [1] 23.599 82.603

The range, quartiles, median, and mean can all be obtained through
`summary()`:

``` r
summary(gapminder$lifeExp)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   23.60   48.20   60.71   59.47   70.85   82.60

Know I want to look at the distribution of `lifeExp` by combining the
histogram of the variable and a Kernel density plot.

``` r
ggplot(gapminder, aes(lifeExp)) +
         geom_histogram(aes(y=..density..),fill = "cornflowerblue", bins=50) +
  geom_density()
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## Explore various plot types

``` r
  ggplot(gapminder, aes(lifeExp,gdpPercap, color=continent)) +
    geom_point(aes(alpha=year)) +
    scale_y_log10()
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
a<-gapminder%>%
  filter(year > 2000)
a
```

    ## # A tibble: 284 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       2002    42.1 25268405      727.
    ##  2 Afghanistan Asia       2007    43.8 31889923      975.
    ##  3 Albania     Europe     2002    75.7  3508512     4604.
    ##  4 Albania     Europe     2007    76.4  3600523     5937.
    ##  5 Algeria     Africa     2002    71.0 31287142     5288.
    ##  6 Algeria     Africa     2007    72.3 33333216     6223.
    ##  7 Angola      Africa     2002    41.0 10866106     2773.
    ##  8 Angola      Africa     2007    42.7 12420476     4797.
    ##  9 Argentina   Americas   2002    74.3 38331121     8798.
    ## 10 Argentina   Americas   2007    75.3 40301927    12779.
    ## # ... with 274 more rows

``` r
ggplot(a, aes(lifeExp,gdpPercap, color=continent)) +
    geom_point(aes(size=year)) +
    scale_y_log10()
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
