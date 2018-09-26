Alejandra\_hw02\_gapminder\_dplyr
================

## Gapminder exploration and use of dplyr

This is an R Markdown document used for exploring the gapminder dataset
through the functions of the dplyr and ggplot packages. This document is
intented to serve as a “cheatsheet” for future work on R.

## Loading data

``` r
library(gapminder)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(knitr)
library(kableExtra)
```

## Smell test the data

**Is it a data.frame, a matrix, a vector, a list?**

``` r
kable(head(gapminder)) %>%
kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

country

</th>

<th style="text-align:left;">

continent

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

lifeExp

</th>

<th style="text-align:right;">

pop

</th>

<th style="text-align:right;">

gdpPercap

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

28.801

</td>

<td style="text-align:right;">

8425333

</td>

<td style="text-align:right;">

779.4453

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

30.332

</td>

<td style="text-align:right;">

9240934

</td>

<td style="text-align:right;">

820.8530

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

31.997

</td>

<td style="text-align:right;">

10267083

</td>

<td style="text-align:right;">

853.1007

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

34.020

</td>

<td style="text-align:right;">

11537966

</td>

<td style="text-align:right;">

836.1971

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

36.088

</td>

<td style="text-align:right;">

13079460

</td>

<td style="text-align:right;">

739.9811

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

38.438

</td>

<td style="text-align:right;">

14880372

</td>

<td style="text-align:right;">

786.1134

</td>

</tr>

</tbody>

</table>

``` r
typeof(gapminder)
```

    ## [1] "list"

When printing `head(gapminder)` I can see the variables of the data set
and the first rows. The data is arranged in columns, and the
observations of these variables are arranged in rows. This is a two
dimensional object that holds different types of data. I know this
object is a data frame.

The function `typeof()` determines the (R internal) type or storage mode
of an object. The type of this data set is a **list**. A list in R
allows to gather a variety of objects under one name in an ordered way.
These objects can be matrices, vectors, data frames, other lists, etc.

**What is its class?**

``` r
class(gapminder)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

The `class()`function confirms that gapminder is a data frame. :+1:

**How many variables/columns?**

``` r
ncol(gapminder)
```

    ## [1] 6

**How many rows/observations?**

``` r
nrow(gapminder)
```

    ## [1] 1704

`ncol()` and `nrow()` functions return the number of columns and rows,
respectively.

**Can you get these facts about “extent” or “size” in more than one way?
Can you imagine different functions being useful in different
contexts?**

``` r
dim(gapminder) #rows, columns
```

    ## [1] 1704    6

``` r
length(gapminder) #Number of columns
```

    ## [1] 6

I can get the same information with `dim()` function, the output is
(number of rows, number of columns). Functions are useful depending on
what we want to look at. If I just want to know how big is my dataset
dim() is great, if I for example want to iterate, on either columns or
rows, then ncol or nrow would be sufficient.

**What data type is each variable?**

``` r
lapply(gapminder,class) %>%
  as.matrix() %>%
  kable(col.names = "Data type",align = 'lc') %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

</th>

<th style="text-align:left;">

Data type

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

country

</td>

<td style="text-align:left;">

factor

</td>

</tr>

<tr>

<td style="text-align:left;">

continent

</td>

<td style="text-align:left;">

factor

</td>

</tr>

<tr>

<td style="text-align:left;">

year

</td>

<td style="text-align:left;">

integer

</td>

</tr>

<tr>

<td style="text-align:left;">

lifeExp

</td>

<td style="text-align:left;">

numeric

</td>

</tr>

<tr>

<td style="text-align:left;">

pop

</td>

<td style="text-align:left;">

integer

</td>

</tr>

<tr>

<td style="text-align:left;">

gdpPercap

</td>

<td style="text-align:left;">

numeric

</td>

</tr>

</tbody>

</table>

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

``` r
#Work around to show the data in a nice table:
data.frame(variable = names(gapminder),
          classe = sapply(gapminder, typeof),
          first_values = sapply(gapminder, function(x) paste0(head(x),  collapse = ", ")),
          row.names = NULL) %>% 
          kable() %>%
          kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

variable

</th>

<th style="text-align:left;">

classe

</th>

<th style="text-align:left;">

first\_values

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

country

</td>

<td style="text-align:left;">

integer

</td>

<td style="text-align:left;">

Afghanistan, Afghanistan, Afghanistan, Afghanistan, Afghanistan,
Afghanistan

</td>

</tr>

<tr>

<td style="text-align:left;">

continent

</td>

<td style="text-align:left;">

integer

</td>

<td style="text-align:left;">

Asia, Asia, Asia, Asia, Asia, Asia

</td>

</tr>

<tr>

<td style="text-align:left;">

year

</td>

<td style="text-align:left;">

integer

</td>

<td style="text-align:left;">

1952, 1957, 1962, 1967, 1972, 1977

</td>

</tr>

<tr>

<td style="text-align:left;">

lifeExp

</td>

<td style="text-align:left;">

double

</td>

<td style="text-align:left;">

28.801, 30.332, 31.997, 34.02, 36.088, 38.438

</td>

</tr>

<tr>

<td style="text-align:left;">

pop

</td>

<td style="text-align:left;">

integer

</td>

<td style="text-align:left;">

8425333, 9240934, 10267083, 11537966, 13079460, 14880372

</td>

</tr>

<tr>

<td style="text-align:left;">

gdpPercap

</td>

<td style="text-align:left;">

double

</td>

<td style="text-align:left;">

779.4453145, 820.8530296, 853.10071, 836.1971382, 739.9811058, 786.11336

</td>

</tr>

</tbody>

</table>

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
countries) is by using `unique()`:

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
values:

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

The histogram shows two main peaks, where the most values are found: the
biggest one around 70 years and the other one, close to 50 years.

## Explore various plot types

### A scatterplot of two quantitative variables

In this section I will explore gdpPercap as a function of lifeExp:

``` r
  ggplot(gapminder, aes(lifeExp,gdpPercap, color=continent)) +
    geom_point(aes(alpha=year)) +
    scale_y_log10()
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

In the above graph I used transparency to differentiate the year of the
data. Now, I will just use the last year of data, which is 2007:

``` r
a<-gapminder%>%
  filter(year == 2007)
a
```

    ## # A tibble: 142 x 6
    ##    country     continent  year lifeExp       pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
    ##  1 Afghanistan Asia       2007    43.8  31889923      975.
    ##  2 Albania     Europe     2007    76.4   3600523     5937.
    ##  3 Algeria     Africa     2007    72.3  33333216     6223.
    ##  4 Angola      Africa     2007    42.7  12420476     4797.
    ##  5 Argentina   Americas   2007    75.3  40301927    12779.
    ##  6 Australia   Oceania    2007    81.2  20434176    34435.
    ##  7 Austria     Europe     2007    79.8   8199783    36126.
    ##  8 Bahrain     Asia       2007    75.6    708573    29796.
    ##  9 Bangladesh  Asia       2007    64.1 150448339     1391.
    ## 10 Belgium     Europe     2007    79.4  10392226    33693.
    ## # ... with 132 more rows

``` r
ggplot(a, aes(lifeExp,gdpPercap, color=continent)) +
    geom_point() +
    scale_y_log10()
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

This graph presents data from only one year. It is easily appreciated
the difference overall between lifeExp and gdpPercap in African
countries where it is lower than in Europe and Oceania.

### A plot of one quantitative variable.

Now, I will look at the lifeExp data arranged by continent with a
frequency polygon:

``` r
ggplot(gapminder, aes(lifeExp, color=continent)) +
  geom_freqpoly()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

This plot shows the frequency of life expectancy values within the
dataset. We can see a discrepancy between the most common values for
life expectancy in Africa and Europe.

And now lets look at the distribution of this same variable among
continents.

``` r
ggplot(gapminder, aes(lifeExp, fill=continent)) +
  geom_density(alpha=0.3)
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

In this plot it is easily appreciated that Europe and Oceania present a
more homogenous distribution of life expectancy, while the rest of the
continents have a wider range of values, probably because of bigger
socio-economic gaps between countries.

### Plot of one quantitative variable and one categorical.

First, I will arrange my data:

``` r
# for the gapminder, group by continent
# to show the average Life Expectancy (LE) per continent
LEbyCont <- gapminder %>% 
  group_by(continent) %>% 
  summarize(avgLE = mean(lifeExp)) 

kable(LEbyCont, #Table to plot
      col.names = c('Continent', 'Avg. Life Expectancy'), #name the columns
      digits = 1, #round to 1 decimal
      align = 'lc') #Column alignment 'lc' is for the first column left aligned 'l', and second centered 'c'
```

<table>

<thead>

<tr>

<th style="text-align:left;">

Continent

</th>

<th style="text-align:center;">

Avg. Life Expectancy

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:center;">

48.9

</td>

</tr>

<tr>

<td style="text-align:left;">

Americas

</td>

<td style="text-align:center;">

64.7

</td>

</tr>

<tr>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:center;">

60.1

</td>

</tr>

<tr>

<td style="text-align:left;">

Europe

</td>

<td style="text-align:center;">

71.9

</td>

</tr>

<tr>

<td style="text-align:left;">

Oceania

</td>

<td style="text-align:center;">

74.3

</td>

</tr>

</tbody>

</table>

``` r
#LEbyCont
```

Now I will do the plot to present average of life expectancy in each
continent.

``` r
# plot the avg Life Expectancy for each continent

LEbyCont <- LEbyCont %>% mutate(avgLE = round(avgLE, 1)) #round numbers of Avg. Life Expectancy
ggplot(LEbyCont, #table to plot
  aes(x = continent, y = avgLE, color = continent, label = avgLE)) +  #aesthetic mappings and select the AvgLE as the     labels on the plot
  geom_point(size=4) + #size of points in the plot
  ggtitle("Life Expectancy by Continent") +  #title of the plot
  xlab("Continent") + ylab("Avg. Life Expectancy (years)") + #x and y labels
  geom_text(aes(label=avgLE),hjust=-0.3, vjust=0)#adjust the place of the labels
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

Here, it is easily appreciated the overall difference of the average
life expectancy between continents.

``` r
g1 <- ggplot(gapminder, aes(continent, pop, fill = continent)) +
      geom_boxplot() +
      scale_y_log10() +
      theme(legend.position = "none", axis.text.x = element_text(angle = 45, hjust = 1)) +
      xlab("")
g2 <- ggplot(gapminder, aes(continent, lifeExp, fill = continent)) +
      geom_boxplot()+
      theme(legend.title = element_blank(), legend.position="none", axis.text.x = element_text(angle = 45, hjust = 1))+
      xlab("") +
      scale_y_continuous(breaks = seq(0,90,by=10))
plot_grid(g1, g2) #used plot_grid to put plots together
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

The boxplots show population and life expectancy variables for all the
continents. We can see a wide spread distribution of both variables
mostly in Asia and Africa. Although there are distinct outliers in both
plots, I was particularly interested on the outlier in Africa, where
life expectancy is very low.

## Use filter(), select() and %\>%

So let’s try to find that data point:

``` r
x<-gapminder %>%
  filter(continent %in% "Africa", lifeExp<30) %>%
  select(country,lifeExp, year)
x
```

    ## # A tibble: 1 x 3
    ##   country lifeExp  year
    ##   <fct>     <dbl> <int>
    ## 1 Rwanda     23.6  1992

After finding this very low value of life expectancy in Rwanda in 1992,
I researched a bit for an explanation of this, and learnt about the
Rwandan Civil War during which many people were killed and therefore,
the impact on this variable.

I want to see how life expectancy in Rwanda behaved before and after
this event:

``` r
gapminder%>%
  filter(country %in% c("Rwanda")) %>%
  select(lifeExp, year) %>%
  ggplot(aes(year,lifeExp)) +
  ggtitle("Historical Life Expectancy in Rwanda") +
  geom_point(color="red") +
  geom_path(alpha=0.2)
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->
We can see that before this event, the trend of life expectancy was to
go higher, but becaise of this terrible event, life expectancy drop
dramatically and took about 20 years to recover.

I suspect this event also impacted the variable of population:

``` r
gapminder%>%
  filter(country %in% c("Rwanda")) %>%
  select(pop, year) %>%
  ggplot(aes(year,pop)) +
  ggtitle("Historical population in Rwanda") +
  geom_point(color="red") +
  geom_path(alpha=0.2)
```

![](hw02_gapminder_dplyr_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->
In this graph we can see that not only the population didn’t grow but it
even decreased during this period of Rwanda’s history.

## But I want to do more\!

Evaluate this code and describe the result. Presumably the analyst’s
intent was to get the data for Rwanda and Afghanistan. Did they succeed?
Why or why not? If not, what is the correct way to do this?

``` r
filter(gapminder, country == c("Rwanda", "Afghanistan")) %>%
  kable(.)
```

<table>

<thead>

<tr>

<th style="text-align:left;">

country

</th>

<th style="text-align:left;">

continent

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

lifeExp

</th>

<th style="text-align:right;">

pop

</th>

<th style="text-align:right;">

gdpPercap

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

30.332

</td>

<td style="text-align:right;">

9240934

</td>

<td style="text-align:right;">

820.8530

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

34.020

</td>

<td style="text-align:right;">

11537966

</td>

<td style="text-align:right;">

836.1971

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

38.438

</td>

<td style="text-align:right;">

14880372

</td>

<td style="text-align:right;">

786.1134

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

40.822

</td>

<td style="text-align:right;">

13867957

</td>

<td style="text-align:right;">

852.3959

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

41.763

</td>

<td style="text-align:right;">

22227415

</td>

<td style="text-align:right;">

635.3414

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

43.828

</td>

<td style="text-align:right;">

31889923

</td>

<td style="text-align:right;">

974.5803

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

40.000

</td>

<td style="text-align:right;">

2534927

</td>

<td style="text-align:right;">

493.3239

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

43.000

</td>

<td style="text-align:right;">

3051242

</td>

<td style="text-align:right;">

597.4731

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

44.600

</td>

<td style="text-align:right;">

3992121

</td>

<td style="text-align:right;">

590.5807

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

46.218

</td>

<td style="text-align:right;">

5507565

</td>

<td style="text-align:right;">

881.5706

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

23.599

</td>

<td style="text-align:right;">

7290203

</td>

<td style="text-align:right;">

737.0686

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

43.413

</td>

<td style="text-align:right;">

7852401

</td>

<td style="text-align:right;">

785.6538

</td>

</tr>

</tbody>

</table>

When running this code, I can see that some years of data are missing,
since == is a logical operator that compares two things that are the
same length. So in this sense, as we have 12 years of data 6 and 6 go to
each country. So if we add a third country, 4 years would be displayed
for each one as shown below:

``` r
filter(gapminder, country == c("Rwanda", "Afghanistan", "Mexico")) %>%
  kable(.)
```

<table>

<thead>

<tr>

<th style="text-align:left;">

country

</th>

<th style="text-align:left;">

continent

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

lifeExp

</th>

<th style="text-align:right;">

pop

</th>

<th style="text-align:right;">

gdpPercap

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

30.332

</td>

<td style="text-align:right;">

9240934

</td>

<td style="text-align:right;">

820.8530

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

36.088

</td>

<td style="text-align:right;">

13079460

</td>

<td style="text-align:right;">

739.9811

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

40.822

</td>

<td style="text-align:right;">

13867957

</td>

<td style="text-align:right;">

852.3959

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

42.129

</td>

<td style="text-align:right;">

25268405

</td>

<td style="text-align:right;">

726.7341

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:left;">

Americas

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

58.299

</td>

<td style="text-align:right;">

41121485

</td>

<td style="text-align:right;">

4581.6094

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:left;">

Americas

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

65.032

</td>

<td style="text-align:right;">

63759976

</td>

<td style="text-align:right;">

7674.9291

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:left;">

Americas

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

71.455

</td>

<td style="text-align:right;">

88111030

</td>

<td style="text-align:right;">

9472.3843

</td>

</tr>

<tr>

<td style="text-align:left;">

Mexico

</td>

<td style="text-align:left;">

Americas

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

76.195

</td>

<td style="text-align:right;">

108700891

</td>

<td style="text-align:right;">

11977.5750

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

40.000

</td>

<td style="text-align:right;">

2534927

</td>

<td style="text-align:right;">

493.3239

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

44.100

</td>

<td style="text-align:right;">

3451079

</td>

<td style="text-align:right;">

510.9637

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

46.218

</td>

<td style="text-align:right;">

5507565

</td>

<td style="text-align:right;">

881.5706

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

36.087

</td>

<td style="text-align:right;">

7212583

</td>

<td style="text-align:right;">

589.9445

</td>

</tr>

</tbody>

</table>

To extract all the data from Rwanda and Afghanistan, the analyst should
use `%in%` operator instead. Let’s try it:

``` r
filter(gapminder, country %in% c("Rwanda", "Afghanistan")) %>%
  kable(.)
```

<table>

<thead>

<tr>

<th style="text-align:left;">

country

</th>

<th style="text-align:left;">

continent

</th>

<th style="text-align:right;">

year

</th>

<th style="text-align:right;">

lifeExp

</th>

<th style="text-align:right;">

pop

</th>

<th style="text-align:right;">

gdpPercap

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

28.801

</td>

<td style="text-align:right;">

8425333

</td>

<td style="text-align:right;">

779.4453

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

30.332

</td>

<td style="text-align:right;">

9240934

</td>

<td style="text-align:right;">

820.8530

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

31.997

</td>

<td style="text-align:right;">

10267083

</td>

<td style="text-align:right;">

853.1007

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

34.020

</td>

<td style="text-align:right;">

11537966

</td>

<td style="text-align:right;">

836.1971

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

36.088

</td>

<td style="text-align:right;">

13079460

</td>

<td style="text-align:right;">

739.9811

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

38.438

</td>

<td style="text-align:right;">

14880372

</td>

<td style="text-align:right;">

786.1134

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

39.854

</td>

<td style="text-align:right;">

12881816

</td>

<td style="text-align:right;">

978.0114

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

40.822

</td>

<td style="text-align:right;">

13867957

</td>

<td style="text-align:right;">

852.3959

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

41.674

</td>

<td style="text-align:right;">

16317921

</td>

<td style="text-align:right;">

649.3414

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

41.763

</td>

<td style="text-align:right;">

22227415

</td>

<td style="text-align:right;">

635.3414

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

42.129

</td>

<td style="text-align:right;">

25268405

</td>

<td style="text-align:right;">

726.7341

</td>

</tr>

<tr>

<td style="text-align:left;">

Afghanistan

</td>

<td style="text-align:left;">

Asia

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

43.828

</td>

<td style="text-align:right;">

31889923

</td>

<td style="text-align:right;">

974.5803

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1952

</td>

<td style="text-align:right;">

40.000

</td>

<td style="text-align:right;">

2534927

</td>

<td style="text-align:right;">

493.3239

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1957

</td>

<td style="text-align:right;">

41.500

</td>

<td style="text-align:right;">

2822082

</td>

<td style="text-align:right;">

540.2894

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1962

</td>

<td style="text-align:right;">

43.000

</td>

<td style="text-align:right;">

3051242

</td>

<td style="text-align:right;">

597.4731

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1967

</td>

<td style="text-align:right;">

44.100

</td>

<td style="text-align:right;">

3451079

</td>

<td style="text-align:right;">

510.9637

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1972

</td>

<td style="text-align:right;">

44.600

</td>

<td style="text-align:right;">

3992121

</td>

<td style="text-align:right;">

590.5807

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1977

</td>

<td style="text-align:right;">

45.000

</td>

<td style="text-align:right;">

4657072

</td>

<td style="text-align:right;">

670.0806

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1982

</td>

<td style="text-align:right;">

46.218

</td>

<td style="text-align:right;">

5507565

</td>

<td style="text-align:right;">

881.5706

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1987

</td>

<td style="text-align:right;">

44.020

</td>

<td style="text-align:right;">

6349365

</td>

<td style="text-align:right;">

847.9912

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1992

</td>

<td style="text-align:right;">

23.599

</td>

<td style="text-align:right;">

7290203

</td>

<td style="text-align:right;">

737.0686

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

1997

</td>

<td style="text-align:right;">

36.087

</td>

<td style="text-align:right;">

7212583

</td>

<td style="text-align:right;">

589.9445

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

2002

</td>

<td style="text-align:right;">

43.413

</td>

<td style="text-align:right;">

7852401

</td>

<td style="text-align:right;">

785.6538

</td>

</tr>

<tr>

<td style="text-align:left;">

Rwanda

</td>

<td style="text-align:left;">

Africa

</td>

<td style="text-align:right;">

2007

</td>

<td style="text-align:right;">

46.242

</td>

<td style="text-align:right;">

8860588

</td>

<td style="text-align:right;">

863.0885

</td>

</tr>

</tbody>

</table>

Now we have all the data from both countries :thumbsup:
