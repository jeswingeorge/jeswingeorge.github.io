# Explore and Summarize Data of Red Wine using R

## Aim of this project

To find out which chemical properties influence the quality of red wines. For
this purpose, the given dataset for red wines was explored using R.

```R
library(ggplot2)
library(tidyr)
library(dplyr)
library(GGally)
library(gridExtra)
library(corrplot)
library(lattice)
```

```R
# Load the Data
wine_df <- read.csv('wineQualityReds.csv', header = TRUE, sep = ',')
```

## Univariate Plots Section

Size of dataset

```
dim(wine_df)
```

```
[1] 1599   13
```

The dataset consists of 13 variables and 1599 observations.

Structure of dataset

```
str(wine_df)
```

```
'data.frame':	1599 obs. of  13 variables:
 $ X                   : int  1 2 3 4 5 6 7 8 9 10 ...
 $ fixed.acidity       : num  7.4 7.8 7.8 11.2 7.4 7.4 7.9 7.3 7.8 7.5 ...
 $ volatile.acidity    : num  0.7 0.88 0.76 0.28 0.7 0.66 0.6 0.65 0.58 0.5 ...
 $ citric.acid         : num  0 0 0.04 0.56 0 0 0.06 0 0.02 0.36 ...
 $ residual.sugar      : num  1.9 2.6 2.3 1.9 1.9 1.8 1.6 1.2 2 6.1 ...
 $ chlorides           : num  0.076 0.098 0.092 0.075 0.076 0.075 0.069 0.065 0.073 0.071 ...
 $ free.sulfur.dioxide : num  11 25 15 17 11 13 15 15 9 17 ...
 $ total.sulfur.dioxide: num  34 67 54 60 34 40 59 21 18 102 ...
 $ density             : num  0.998 0.997 0.997 0.998 0.998 ...
 $ pH                  : num  3.51 3.2 3.26 3.16 3.51 3.51 3.3 3.39 3.36 3.35 ...
 $ sulphates           : num  0.56 0.68 0.65 0.58 0.56 0.56 0.46 0.47 0.57 0.8 ...
 $ alcohol             : num  9.4 9.8 9.8 9.8 9.4 9.4 9.4 10 9.5 10.5 ...
 $ quality             : int  5 5 5 6 5 5 5 7 7 5 ...
```

Converted the quality variable from type integer to type factor, as it denotes
the ratings received by the wine.


Statistical summary of the given dataset

```
summary(wine_df)
```

```
        X          fixed.acidity   volatile.acidity  citric.acid
  Min.   :   1.0   Min.   : 4.60   Min.   :0.1200   Min.   :0.000
  1st Qu.: 400.5   1st Qu.: 7.10   1st Qu.:0.3900   1st Qu.:0.090
  Median : 800.0   Median : 7.90   Median :0.5200   Median :0.260
  Mean   : 800.0   Mean   : 8.32   Mean   :0.5278   Mean   :0.271
  3rd Qu.:1199.5   3rd Qu.: 9.20   3rd Qu.:0.6400   3rd Qu.:0.420
  Max.   :1599.0   Max.   :15.90   Max.   :1.5800   Max.   :1.000
  residual.sugar     chlorides       free.sulfur.dioxide
  Min.   : 0.900   Min.   :0.01200   Min.   : 1.00
  1st Qu.: 1.900   1st Qu.:0.07000   1st Qu.: 7.00
  Median : 2.200   Median :0.07900   Median :14.00
  Mean   : 2.539   Mean   :0.08747   Mean   :15.87
  3rd Qu.: 2.600   3rd Qu.:0.09000   3rd Qu.:21.00
  Max.   :15.500   Max.   :0.61100   Max.   :72.00
  total.sulfur.dioxide    density             pH          sulphates
  Min.   :  6.00       Min.   :0.9901   Min.   :2.740   Min.   :0.3300
  1st Qu.: 22.00       1st Qu.:0.9956   1st Qu.:3.210   1st Qu.:0.5500
  Median : 38.00       Median :0.9968   Median :3.310   Median :0.6200
  Mean   : 46.47       Mean   :0.9967   Mean   :3.311   Mean   :0.6581
  3rd Qu.: 62.00       3rd Qu.:0.9978   3rd Qu.:3.400   3rd Qu.:0.7300
  Max.   :289.00       Max.   :1.0037   Max.   :4.010   Max.   :2.0000
     alcohol         quality
  Min.   : 8.40   Min.   :3.000
  1st Qu.: 9.50   1st Qu.:5.000
  Median :10.20   Median :6.000
  Mean   :10.42   Mean   :5.636
  3rd Qu.:11.10   3rd Qu.:6.000
  Max.   :14.90   Max.   :8.000
```

### Analyzing variable : Quality

To check out the most frequent quality of wines available in the dataset.

```
table(wine_df$quality)
```

```
  3   4   5   6   7   8
 10  53 681 638 199  18
```

The above stats shows the available number of different qualities of red wine
present in the given dataset. The best quality of red wine available in the
given dataset is __Quality 8__  and it is totally 18 in number. Red wine with
__Quality 7__ is 199 in number and the most avaliable qualities of red wine are
__Quality 5__ and __Quality 6__ with 681 and 638 numbers respectively.

```
p1 <- ggplot(data = wine_df, aes(x=factor(quality))) +
  geom_bar()+
  labs(x = 'Quality of wines', y = 'Frequency',
       title = 'Bargraph of frequency vs Quality of available wines')
p1
```

![Hist1](../../images/Projects/red_wine_quality/1.png)

So from the above two analysis: it is observed that wines with quality rating 5
and 6 is the most available in the given dataset. Followed by quality 7 and
quality 4. Quality 8 followed by quality 3 are the least available.

***

### Analyzing variable : fixed acidity

__Description of fixed acidity:__ most acids involved with wine or fixed or
nonvolatile (do not evaporate readily).

```
ggplot(data = wine_df, aes(x=fixed.acidity))+
  geom_histogram(binwidth = 0.5, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(4.5,16), breaks = seq(4.5,16,0.5))+
  labs(title = 'Histogram of fixed acidity')
```

![Hist2](../../images/Projects/red_wine_quality/2.png)

The above distribution is right-skewed and most of the wines in the given
dataset have fixed acidity between 6.5 and 8 g/dm<super>3</super>.

```
ggplot(data = wine_df, aes(x=fixed.acidity))+
  geom_histogram(binwidth = 1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(4.5,16), breaks = seq(4.5,16,1))+
  facet_wrap(  ~ quality, ncol=2, scales = 'free_y')+
  labs(title = 'Faceting the distribution of fixed.acidity based on wine quality')
```

![Hist3](../../images/Projects/red_wine_quality/3.png)

The above facetted graph, confirms the previous two analysis that the red wines
with __Quality 5 and 6__ are the most available in the dataset and their fixed
acidity mostly ranges between 6.5 g/dm<super>3</super>  and 8 g/dm<super>3</super>  respectively. And the
best two  available qualities of wine __Quality 7 and 8__ have a very low count
and their  value of fixed acidity usually varies between 6.5 and 11.5 g/dm<super>3</super> .
Also __Quality 3 and 4__ wines have value of fixed acidity varying between 6.5
and 8.5 g/dm<super>3</super>.

```
by(wine_df$fixed.acidity, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  6.700   7.150   7.500   8.360   9.875  11.600
-----------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  4.600   6.800   7.500   7.779   8.400  12.500
-----------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.000   7.100   7.800   8.167   8.900  15.900
-----------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  4.700   7.000   7.900   8.347   9.400  14.300
-----------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  4.900   7.400   8.800   8.872  10.100  15.600
-----------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.000   7.250   8.250   8.567  10.225  12.600
```

The above statistics show the summary of fixed acidity present in different
qualities of red wine.

```
ggplot(data = wine_df, aes(x=quality, y=fixed.acidity))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(6.5,10.5))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title = 'Boxplot of fixed acidity vs Quality of red wine')
```

![Hist4](../../images/Projects/red_wine_quality/4.png)

The __Quality 8__ of red wine has median fixed acidity  of 8.25 g/dm<super>3</super>  and
mean fixed acidity of 8.567 g/dm<super>3</super> . The __Quality 7__ of red wine has median
fixed acidity of 8.8 g/dm<super>3</super>  and mean fixed acidity of 8.872 g/dm<super>3</super> .

The most available __Quality 5__ of red wine has median fixed-acidity of 7.8
g/dm<super>3</super>  and mean fixed-acidity of 8.167 g/dm<super>3</super> . And __Quality 6__ of red wine
has median fixed-acidity of 7.9 g/dm<super>3</super>  and mean fixed-acidity of 8.347 g/dm<super>3</super> .

And the available poor qualities of red wine namely __Quality 3 and Quality 4__
have median fixed acidity of 7.5 g/dm<super>3</super>  each and their mean value of fixed
acidity is 8.36 and 7.79 g/dm<super>3</super>  respectively.

The red point shows the mean value of the fixed acidity.

So from the above anlaysis, fixed acidity volume does somewhat improve the
quality of the condidered red wine.

***

### Analyzing variable : volatile acidity

__Description of volatile acidity:__ the amount of acetic acid in wine, which at
too high of levels can lead to an unpleasant, vinegar taste

```
ggplot(data = wine_df, aes(x=volatile.acidity))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0,1.6), breaks = seq(0,1.6,0.1))+
  labs(title = 'Histogram of volatile acidity content in the available red wines')
```

![Hist5](../../images/Projects/red_wine_quality/5.png)

From the above histogram of volatile acidity - most of the wines have volatile
acidity in the range of 0.3 to 0.7 g/dm<super>3</super> .

```
ggplot(data = wine_df, aes(x=volatile.acidity))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0,1.6), breaks = seq(0,1.6,0.1))+
  facet_wrap(  ~ quality, ncol=2, scales = 'free_y')+
  labs(title = 'Faceting the distribution of volatile.acidity based on wine quality')
```

![Hist6](../../images/Projects/red_wine_quality/6.png)


Most of the red wines with __Quality 8 and Quality 7__ have volatile acidity
usually  ranging from 0.2 to 0.4 g/dm<super>3</super> . Also their distribution is
right-skewed,so we can infer from this that good quality red wines have usually
low values of volatile acidity.

Red wines of __Quality 6 and 5__ are roughly normally distributed and their
values usually range from 0.2 to 0.7 g/dm<super>3</super> .

Poor quality red wines of __Quality 3 and 4__ have their values ranging from
0.4 to 1 g/dm<super>3</super> .

```
by(wine_df$volatile.acidity, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.4400  0.6475  0.8450  0.8845  1.0100  1.5800
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  0.230   0.530   0.670   0.694   0.870   1.130
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  0.180   0.460   0.580   0.577   0.670   1.330
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.1600  0.3800  0.4900  0.4975  0.6000  1.0400
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.1200  0.3000  0.3700  0.4039  0.4850  0.9150
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.2600  0.3350  0.3700  0.4233  0.4725  0.8500
```
The above statistics show the summary of volatile acidity present in different
qualities of red wine.

```{r}
ggplot(data = wine_df, aes(x=quality, y = volatile.acidity))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(0.25,1.05))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title = 'Boxplot of volatile acidity vs Quality of red wine')
```

![Hist7](../../images/Projects/red_wine_quality/7.png)

The above box plot shows that good quality red wines have lower amounts of
volatile acids present in them.

The best available red wine of __Quality 8 and Quality 7__ have median volatile
acidity of 0.37 g/dm<super>3</super>  each and mean volatile acidity of 0.4233 and 0.4033
g/dm<super>3</super> . And as the quality of red wine degrades amount of volatile acidity
present in them also increases. The red point shows the mean value of the
volatile acidity.

***

### Analyzing variable : Citric Acid

__Description of Citric acid:__  found in small quantities, citric acid can add
'freshness' and flavor to wines.

```
ggplot(data = wine_df, aes(x=citric.acid))+
  geom_histogram(binwidth = 0.05, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0,1), breaks = seq(0,1,0.1))+
  labs(title = 'Histogram of Citric acid content in the available red wines')
```

![Hist8](../../images/Projects/red_wine_quality/8.png)


The above histogram on amount of citric acid in the red wines is right-skewed.


```
ggplot(data = wine_df, aes(x=citric.acid))+
  geom_histogram(binwidth = 0.05, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0,1), breaks = seq(0,1,0.1))+
  facet_wrap(  ~ quality, ncol=2, scales = 'free_y')+
  labs(title = 'Faceting the distribution of citric acid based on wine quality')
```

![Hist9](../../images/Projects/red_wine_quality/9.png)


The above faceted graph shows the amount of citric acid present in different
qualities of red wine.
Good red wines of __Quality 8 and 7__ have citric acid in the range 0.3 to 0.55
g/dm<super>3</super> .
Red wines of __Quality 6, 5 and 4__ have  roughly right skewed distribution and
the amount of citric acid in them range from 0 t0 0.6 g/dm<super>3</super>   and their values
usually lie around 0.35 to 0.6 g/dm<super>3</super> .
Poor quality red wines of __Quality 3__ use citric acid in the range from
0 to 0.6 g/dm<super>3</super> .

```
by(wine_df$citric.acid, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.0050  0.0350  0.1710  0.3275  0.6600
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.0300  0.0900  0.1742  0.2700  1.0000
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.0900  0.2300  0.2437  0.3600  0.7900
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.0900  0.2600  0.2738  0.4300  0.7800
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.3050  0.4000  0.3752  0.4900  0.7600
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0300  0.3025  0.4200  0.3911  0.5300  0.7200
```
The above statistics show the summary of citric acid present in different
qualities of red wine.

```
ggplot(data = wine_df, aes(x=quality, y = citric.acid))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(0,0.55))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title = 'Boxplot of citric acid vs Quality of red wine')
```

![Hist10](../../images/Projects/red_wine_quality/10.png)


The above box plot shows that good quality red wines have higher amounts of
citric acids present in them.

The best available red wine of __Quality 8 and Quality 7__ have median citric
acid of 0.42 g/dm<super>3</super>  and .4 g/dm<super>3</super>  and mean volatile acidity of 0.3911 g/dm<super>3</super>
and 0.3752 g/dm<super>3</super> . And as the quality of red wine degrades amount of citric
acid present in them also deceases. The red point shows the mean value of the
citric acid.

***

### Analyzing variable : residual sugar

__Description of residual sugar:__ the amount of sugar remaining after
fermentation stops, it is rare to find wines with less than 1 gram/liter and
wines with greater than 45 grams/liter are considered sweet.

```
p <- ggplot(wine_df, aes(x=residual.sugar))+
  geom_histogram(binwidth = 0.5,color='black',fill='white')+
  scale_x_continuous(limits = c(0.5,16), breaks = seq(0.5,16,1))+
  labs(title = 'Histogram of residual sugar content in the available red wines')

p
```

![Hist11](../../images/Projects/red_wine_quality/11.png)

The distribution obtained above is right-skewed distribution. And most of the
available red wines have residual sugar in the range of 1.5 to 2.5 grams/liter.
Applying log scale to the residual sugar histogram.

```
  ggplot(wine_df, aes(x=residual.sugar))+
  geom_histogram(binwidth = 0.1,color='black',fill='white')+
  labs(title = 'Histogram of residual sugar content using log_scale')+
  scale_x_log10(limits = c(0.8,10), breaks = seq(0.8,10,1))
```

![Hist12](../../images/Projects/red_wine_quality/12.png)


```
ggplot(wine_df, aes(x=residual.sugar))+
  geom_histogram(binwidth = 0.1,color='black',fill='white')+
  scale_x_log10(limits = c(0.8,10), breaks = seq(0.8,10,1))+
  facet_wrap( ~ quality,ncol=2, scales = 'free_y')+
  labs(title = 'Faceting the distribution of residual sugar content based on wine quality in the available red wines')
```

![Hist13](../../images/Projects/red_wine_quality/13.png)


The above diagram shows that almost all qualities of red wines use residual
sugar in amounts between 1.5 to 2.5 grams/liter. And scarcely do the values of
the residual sugar level varies. So chances are that the presence of residual
sugar must be almost fixed in all red wines.

```
by(wine_df$residual.sugar, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  1.200   1.875   2.100   2.635   3.100   5.700
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  1.300   1.900   2.100   2.694   2.800  12.900
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  1.200   1.900   2.200   2.529   2.600  15.500
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  0.900   1.900   2.200   2.477   2.500  15.400
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  1.200   2.000   2.300   2.721   2.750   8.900
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  1.400   1.800   2.100   2.578   2.600   6.400
```

The above statistics show the summary of residual sugar present in different
qualities of red wine.

```
ggplot(wine_df, aes(x = quality, y = residual.sugar))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(1.5,3.3))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title = 'Boxplot of amount of residual sugar level vs Quality of red wines')
```

![Hist14](../../images/Projects/red_wine_quality/14.png)


The above graph using boxplots and the statistical summary above that proves
that in almost all qualities of red wines the median quantity of residual sugar
ranges from 2.1 to 2.3 grams/liter. And the mean quantity of residual sugar is
around  2.5 grams/liter. The red point shows the mean value of the residual
sugar.

The huge variation between mean and median is due to the presence of outliers.

***

### Analyzing variable : chlorides

__Description of chlorides:__ The amount of salt in the wine.

```
ggplot(wine_df, aes(x=chlorides))+
  geom_histogram(binwidth = 0.025,color = 'black', fill='white')+
  scale_x_continuous(limits = c(0,0.62), breaks = seq(0,0.62,0.05))+
  labs(title = 'Histogram of amount of chlorides in the available red wines')
```

![Hist15](../../images/Projects/red_wine_quality/15.png)


The above histogram has a right skewed distribution. And it can be concluded
from the distribution that chlorides added to the red wines are in the range
of 0.05 to 0.1 grams.

```
ggplot(wine_df, aes(x=chlorides))+
  geom_histogram(binwidth = 0.025,color = 'black', fill='white')+
  scale_x_continuous(limits = c(0,0.62), breaks = seq(0,0.62,0.05))+
  facet_wrap( ~ quality, ncol=2, scales = 'free_y')+
  labs(title = 'Faceting the distribution of chlorides based on wine quality in the available red wines')
```

![Hist16](../../images/Projects/red_wine_quality/16.png)

The above faceted graph shows almost all qualities of wine have their chlorides
amount in the range of 0.05 to 1 grams. So it can be inferred that it is
necessary to have chlorides in the abive mentioned range for the preapartaion
of the above wine.

```
by(wine_df$chlorides, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0610  0.0790  0.0905  0.1225  0.1430  0.2670
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.04500 0.06700 0.08000 0.09068 0.08900 0.61000
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.03900 0.07400 0.08100 0.09274 0.09400 0.61100
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.03400 0.06825 0.07800 0.08496 0.08800 0.41500
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.01200 0.06200 0.07300 0.07659 0.08700 0.35800
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.04400 0.06200 0.07050 0.06844 0.07550 0.08600
```

```
ggplot(wine_df, aes(x=quality,y=chlorides))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(0.05,0.14))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title='Boxplot of amount of chlorides vs the quality of red wine')
```

![Hist17](../../images/Projects/red_wine_quality/17.png)

Using the boxplot and statistical summary it is seen that chlorides are used in
small quanities of 0.05 to 0.15 grams. And the best available red wine of
__quality 8__ uses the least median amount of 0.0705 grams and as the quality
of red wine decreases the chloride content increases along with it. The red
point shows the mean value of the chlorides.

The huge difference between mean and median is due to the presence of these
outliers.

***

### Analyzing variable : free.sulfur.dioxide

__Description of free.sulfur.dioxide:__ The free form of SO2 exists in
equilibrium between molecular SO2 (as a dissolved gas) and bisulfite ion; it
prevents microbial growth and the oxidation of wine

```
ggplot(wine_df, aes(x=free.sulfur.dioxide))+
  geom_histogram(binwidth = 1, color = 'black', fill='white')+
  scale_x_continuous(limits = c(1,72), breaks = seq(1,72,2))+
  labs(title = 'Histogram of free.sulfur.dioxide content in the available red wines')
```

![Hist18](../../images/Projects/red_wine_quality/18.png)

The distribution of free.sulfur.dioxide content in the available red wines is
right-skewed in nature and its mode value at 5.

```
ggplot(wine_df, aes(x=free.sulfur.dioxide))+
  geom_histogram(binwidth = 2, color = 'black', fill='white')+
  scale_x_continuous(limits = c(1,72), breaks = seq(1,72,4))+
  facet_wrap( ~ quality, ncol = 2, scales='free_y') +
  labs(title = 'Faceting the distribution of free.sulfur.dioxide based on wine quality')
```

![Hist19](../../images/Projects/red_wine_quality/19.png)


The faceted distribution of free sulfur dioxide based on quality shows that
majority of almost all qualities of wine use free sulifur dioxide in 5 ppm
amounts.

```
by(wine_df$free.sulfur.dioxide, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
    3.0     5.0     6.0    11.0    14.5    34.0
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   3.00    6.00   11.00   12.26   15.00   41.00
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   3.00    9.00   15.00   16.98   23.00   68.00
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   1.00    8.00   14.00   15.71   21.00   72.00
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   3.00    6.00   11.00   14.05   18.00   54.00
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   3.00    6.00    7.50   13.28   16.50   42.00
```

```
ggplot(wine_df, aes(x=quality,y=free.sulfur.dioxide))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(0,25))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title='Boxplot of amount of free.sulfur.dioxide vs the quality of red wine')
```

![Hist20](../../images/Projects/red_wine_quality/20.png)

Using the boxplot and statistical summary it can be inferred that even though
the presence of free sulfur dioxide is essential in red quality wines  to
prevent the growth of microbes, its amount in the wine  does not have much
affect on its quality. The red point shows the mean value of the free sulfur
dioxide.

***

### Analyzing variable : total.sulfur.dioxide

__Description of total.sulfur.dioxide:__ Amount of free and bound forms of
S0 2  in low concentrations. S0 2  is mostly undetectable in wine but at free
S0 2  concentrations over 50 ppm, S0 2  becomes evident in the nose and taste
of wine.

```
ggplot(wine_df, aes(x=total.sulfur.dioxide))+
  geom_histogram(binwidth = 10,fill='white',color='black')+
  scale_x_continuous(limits = c(0,300), breaks = seq(0,300,10))+
  labs(title = 'Histogram of distribution of sulfur.dioxide content in the available red wines')
```

![Hist21](../../images/Projects/red_wine_quality/21.png)

The given histogram is right skewed in nature. And most of the available red
quality wines have total sulfur dioxide of amount about 20 to 30 ppm.

```
ggplot(wine_df, aes(x=total.sulfur.dioxide))+
  geom_histogram(binwidth = 10,fill='white',color='black')+
  scale_x_continuous(limits = c(0,180), breaks = seq(0,180,10))+
  facet_wrap( ~ quality, ncol = 2, scales='free_y') +
  labs(title = 'Faceting the distribution of total.sulfur.dioxide based on wine quality')
```

![Hist22](../../images/Projects/red_wine_quality/22.png)

The above faceted graph shows that majority of all qualities of wine have total
sulfur dioxide in the of 20 to 30 ppm. And the histogram corresponding to each
quality has a right-skewed distribution.

```
by(wine_df$total.sulfur.dioxide, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
    9.0    12.5    15.0    24.9    42.5    49.0
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   7.00   14.00   26.00   36.25   49.00  119.00
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   6.00   26.00   47.00   56.51   84.00  155.00
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   6.00   23.00   35.00   40.87   54.00  165.00
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   7.00   17.50   27.00   35.02   43.00  289.00
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  12.00   16.00   21.50   33.44   43.00   88.00
```

```
ggplot(wine_df, aes(x=quality,y=total.sulfur.dioxide))+
  geom_boxplot(aes(group=quality))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title='Boxplot of amount of total.sulfur.dioxide vs the quality of red wine')+
  coord_cartesian(ylim = c(5,90))
```

![Hist23](../../images/Projects/red_wine_quality/23.png)

From the statistical summary and boxplot it is seen that only __quality 5__ red
wines have the highest content of total sulfur dioxide with median value of 47
ppm and mean value of 56.51 ppm. Also the best quality of available red wine
which is __quality 8__ have low median content of total sulfur dioxide with
21.5 ppm and low mean content of total sulfur dioxide 33.44. And __quality 3__
which has low median content of 15 ppm and mean content of 24.9 ppm. So based
on the amount of total sulfur dioxide available in each quality of wine we
cannot decide whether quality of red wine is good or not. The red point shows
the mean value of the total sulfur dioxide.

***

### Analyzing variable : density

__Description of density:__ the density of water is close to that of water
depending on the percent alcohol and sugar content.

```
ggplot(wine_df, aes(x=density))+
  geom_histogram(binwidth = 0.001, color = 'black', fill = 'white')+
  labs(title='Histogram of density of red wines')+
  scale_x_continuous(breaks = seq(0.990,1.004,0.001), limits = c(0.990,1.004) )
```

![Hist24](../../images/Projects/red_wine_quality/24.png)

The above histogram on density of red wines has a normal distribution and most
of the available red wines have a density in the range of 0.995 to 0.997.

```
ggplot(wine_df, aes(x=density))+
  geom_histogram(binwidth = 0.001, color = 'black', fill = 'white')+
  labs(title='Faceting the distribution of density based on wine quality of the available red wines')+
  scale_x_continuous(breaks = seq(0.990,1.004,0.002), limits = c(0.990,1.004))+
  facet_wrap( ~ quality, ncol=2, scales = 'free_y')
```

![Hist25](../../images/Projects/red_wine_quality/25.png)

The faceted distribution shows that almost all qaulity of red wines have a
normal distribution with their mean value of density concentrated around the
0.994 to 0.996 region of density for __quality 7 and quality 8__ red wines. And
the other lower quality wines had their majority of the density values ranging
from 0.995 to 0.997.

```
by(wine_df$density, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9947  0.9961  0.9976  0.9975  0.9988  1.0008
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9934  0.9957  0.9965  0.9965  0.9974  1.0010
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9926  0.9962  0.9970  0.9971  0.9979  1.0031
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9901  0.9954  0.9966  0.9966  0.9979  1.0037
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9906  0.9948  0.9958  0.9961  0.9974  1.0032
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.9908  0.9942  0.9949  0.9952  0.9972  0.9988
```

```
ggplot(wine_df, aes(x=quality, y=density))+
  geom_boxplot(aes(group=quality))+
  labs(title='Boxplot of density vs quality of red wine')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  coord_cartesian(ylim = c(0.994,0.999))
```

![Hist26](../../images/Projects/red_wine_quality/26.png)

From the statistical summary and boxplot it can be seen that good quality red
wines having __quality 8 and 7__ have a low median value of density 0.9949 and
0.9958 respectively, and mean density value of 0.9952 and 0.9961. For other
lower qualities of wine such as __quality 3__ has median density value of
0.9976 and mean density value of 0.9975. So it can be said that lower density
of red wines denotes better quality. The red point shows the mean value of the
density.

***

### Analyzing variable : pH

__Description of pH:__ describes how acidic or basic a wine is on a scale from 0
(very acidic) to 14 (very basic); most wines are between 3-4 on the pH scale.

```
ggplot(wine_df, aes(x=pH))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(2.73, 4.02), breaks = seq(2.74,4.02,0.1))+
  labs(title = 'Histogram of pH values of red wine')
```

![Hist27](../../images/Projects/red_wine_quality/27.png)

The above histogram has a normal distribution and most of the red wines have pH
values in the range of 3.14 to 3.44.

```
ggplot(wine_df, aes(x=pH))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(2.73, 4.02), breaks = seq(2.74,4.02,0.1))+
  labs(title = 'Faceting the distribution of pH values based on wine quality in the available red wines')+
  facet_wrap( ~ quality, ncol=2, scales = 'free_y')
```

![Hist28](../../images/Projects/red_wine_quality/28.png)

From the above faceted graph it can be seen that __quality 5 to 8__ have normal
distribution and their values range from 3.14 to 3.44. The graph of
distribution of pH values for different qualities of red wine are not much
helpful in finding how pH values affect quality.


```
by(wine_df$pH, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  3.160   3.312   3.390   3.398   3.495   3.630
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  2.740   3.300   3.370   3.382   3.500   3.900
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  2.880   3.200   3.300   3.305   3.400   3.740
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  2.860   3.220   3.320   3.318   3.410   4.010
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  2.920   3.200   3.280   3.291   3.380   3.780
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  2.880   3.163   3.230   3.267   3.350   3.720
```


```
ggplot(wine_df, aes(x=quality,y=pH))+
  geom_boxplot(aes(group=quality))+
  labs(title = 'Boxplot of pH vs Quality for red wine')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  coord_cartesian(ylim = c(3.15,3.55))
```

![Hist29](../../images/Projects/red_wine_quality/29.png)

From the statistical summary and boxplot it is observed that median pH value of
__quality 8__ red wine is 3.23 and it is lower than other qualities of red wine
but its mean pH value is 3.267 which is higher than the mean pH values of other
quality of wines. So it cannot be generally stated that pH has a direct
relationship with the quality of wine. The red point shows the mean value of
the pH.

***

### Analyzing variable : sulphates

__Description of sulphates:__ a wine additive which can contribute to sulfur
dioxide gas (S02) levels, wich acts as an antimicrobial and antioxidant.

```
ggplot(wine_df, aes(x=sulphates))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0.3,2), breaks = seq(0.3,2,0.1))+
  labs(title = 'Histogram of distribution of sulphates for red wines')
```

![Hist30](../../images/Projects/red_wine_quality/30.png)

The distribution of sulphates in red wine has a right-skewed distribution with
the mode value of sulphates lying in the range from 0.5 to 0.6.


```
ggplot(wine_df, aes(x=sulphates))+
  geom_histogram(binwidth = 0.1, color = 'black', fill = 'white')+
  scale_x_continuous(limits = c(0.3,2), breaks = seq(0.3,2,0.1))+
  labs(title = 'Faceting the distribution of sulphates based on wine quality in the available red wines')+
  facet_wrap( ~ quality, ncol = 2, scales = 'free_y')
```

![Hist31](../../images/Projects/red_wine_quality/31.png)

From the faceted distribution of sulphates considering quality as the
categorical variable it is seen that that the amount of sulphates in higher
quality of wines such as __quality 8 and 7__ have slightly higher higher amount
of sulphates than those ranging from quality 3 to quality 6.


```
by(wine_df$sulphates, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.4000  0.5125  0.5450  0.5700  0.6150  0.8600
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.3300  0.4900  0.5600  0.5964  0.6000  2.0000
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  0.370   0.530   0.580   0.621   0.660   1.980
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.4000  0.5800  0.6400  0.6753  0.7500  1.9500
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.3900  0.6500  0.7400  0.7413  0.8300  1.3600
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.6300  0.6900  0.7400  0.7678  0.8200  1.1000
```

```
ggplot(wine_df, aes(x=quality,y=sulphates))+
  geom_boxplot(aes(group=quality))+
  labs(title='Boxplot of amount of sulphates vs Quality for red wine')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4) +
  coord_cartesian(ylim = c(0.48,0.84))
```

![Hist32](../../images/Projects/red_wine_quality/32.png)


From the statistical summary and boxplot of sulphates vs the quality of red
wines it is observed that better quality red wines such as __quality 8 and 7__
have higher median amd mean value of sulphates as compared to that of other
lower quality red wines. The red point shows the mean value of the sulphates.

***

### Analyzing variable : alcohol

__Description of alcohol:__ the percent alcohol content of the wine measured in
% by volume.

```
ggplot(wine_df, aes(x=alcohol))+
  geom_histogram(binwidth = 0.5,color='black',fill='white')+
  labs(title = 'Histogram of distribution of alcohol in red wines')+
  scale_x_continuous(limits = c(8,15), breaks = seq(8,15,0.5))
```

![Hist33](../../images/Projects/red_wine_quality/33.png)

The alcohol content in red wines have a right-skewed distribution with most of
the red wines having alcohol content in range from 9-10 % by volume.


```
ggplot(wine_df, aes(x=alcohol))+
  geom_histogram(binwidth = 0.5,color='black',fill='white')+
  labs(title = 'Faceting the distribution of alcohol based on wine quality in the available red wines')+
  scale_x_continuous(limits = c(8,15), breaks = seq(8,15,0.5))+
  facet_wrap( ~quality, ncol=2, scales = 'free_y')
```

![Hist34](../../images/Projects/red_wine_quality/34.png)

From the faceted distribution of alcohol based on quality it can be observed
that usually good quality red wines having quality ratings of
__quality 8 and 7__ have higher alcohol content than other quality of available
red wines.

```
by(wine_df$alcohol, wine_df$quality, summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  8.400   9.725   9.925   9.955  10.575  11.000
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   9.00    9.60   10.00   10.27   11.00   13.10
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
    8.5     9.4     9.7     9.9    10.2    14.9
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   8.40    9.80   10.50   10.63   11.30   14.00
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   9.20   10.80   11.50   11.47   12.10   14.00
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   9.80   11.32   12.15   12.09   12.88   14.00
```

```
ggplot(wine_df, aes(x=quality,y=alcohol))+
  geom_boxplot(aes(group=quality))+
  labs(title='Boxplot of alcohol content vs quality of red wine')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  coord_cartesian(ylim = c(9.2,13))
```

![Hist35](../../images/Projects/red_wine_quality/35.png)

From the statistical summary and boxplot above it can be assumed that high
quality of red wines have higher alcohol content % by volume as compared to
other lower quality wines. The red point shows the mean value of the alcohol
content in percentage by volume.

***

### Analyzing created variable : Total_acidity

__Description of Total_acidity :__ As there are three types of acidity in the
red wine so i was just curious to find out the net effect of all acidity on the
quality of red wine.

So i created a new variable called __Total_acidity__ (measured in g/dm<super>3</super> )
which is the sum of fixed  acidity, volatile acidity and citirc acid.

```
wine_df <- wine_df %>%
  mutate(Total_acidity = fixed.acidity + citric.acid + volatile.acidity)

ggplot(wine_df, aes(x=Total_acidity))+
  geom_histogram(binwidth = 0.5, color='black', fill='white')+
  scale_x_continuous(limits=c(5,17.5), breaks = seq(5,17.5,0.5))+
  labs(title='Histogram of distribution of total_acidity in red wines')
```

![Hist36](../../images/Projects/red_wine_quality/36.png)

The above histogram has a right-skewed distribution with majority of the red
wines having total acidity in the range of 7.5 to 9 g/dm<super>3</super> .

```
ggplot(wine_df, aes(x=Total_acidity))+
  geom_histogram(binwidth = 0.5, color='black', fill='white')+
  scale_x_continuous(limits=c(5,17.5), breaks = seq(5,17.5,1))+
  labs(title='Faceting the distribution of total_acidity in red wines based on quality')+
  facet_wrap( ~quality, ncol=2, scales = 'free_y')
```

![Hist37](../../images/Projects/red_wine_quality/37.png)

The faceted graph shows the distribution of total acidity in the available red
wine for different qualities.Quality 6 and 5 have a right-skewed distribution.
And the distibution for better qualilty 7 and 8 have uneven distribution.

```
by(wine_df$Total_acidity, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  7.480   8.101   8.883   9.415  10.780  12.840
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.270   7.570   8.300   8.647   9.300  13.450
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.770   7.880   8.600   8.988   9.830  16.910
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.390   7.710   8.640   9.118  10.186  15.350
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.320   8.105   9.470   9.651  10.980  17.045
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  5.660   7.933   9.095   9.381  11.002  13.630
```

```
ggplot(wine_df, aes(x=quality, y=Total_acidity))+
  geom_boxplot(aes(group=quality))+
  labs(title='Boxplot of total acidity vs quality of red wine')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  coord_cartesian(ylim = c(7.5,11))
```

![Hist38](../../images/Projects/red_wine_quality/38.png)

The statistical summary and boxplot of total acidity with respect to quality
does not show any type of relation between them. The red point shows the mean
value of the total acidity.

***

### Analyzing created variable : prop_free_so2

__Description of prop_free_so2 :__ It is the ratio of free.sulfur.dioxide to
total.sulfur.dioxide present in the concerned red wine. As the
total.sulfur.dioxide present in the red wine contains both free.sulfur.dioxide
and bound.sulfur.dioxide so to determine the proportion in which the
free.sulfur.dioxide is used and to check how this ratio affect the quality of
red wine we use this variable.

```
wine_df <- wine_df %>%
  mutate(prop_free_so2 = free.sulfur.dioxide/total.sulfur.dioxide)

ggplot(wine_df, aes(x=prop_free_so2))+
  geom_histogram(binwidth = 0.1, color='black',fill='white')+
  labs(title='Histogram of distribution of proportion of free SO2 in red wine')+
  scale_x_continuous(limits = c(0,1), breaks = seq(0,1,0.1))
```

![Hist39](../../images/Projects/red_wine_quality/39.png)

The above histogram shows distribution of proportion of free  SO<sub>2</sub>  in the red
wine and most of the red wines have the proportion usually between 0.1 to 0.5.

```
ggplot(wine_df, aes(x=prop_free_so2))+
  geom_histogram(binwidth = 0.1, color='black',fill='white')+
  labs(title='Faceting the distribution of proportion of free SO2 in red wine based on quality')+
  scale_x_continuous(limits = c(0,1), breaks = seq(0,1,0.1))+
  facet_wrap( ~ quality, ncol=2, scales = 'free_y')
```

![Hist40](../../images/Projects/red_wine_quality/40.png)

The above faceted distribution shows distribution of proportion of free  SO<sub>2</sub>
in various qualities of red wine.

```
by(wine_df$prop_free_so2, wine_df$quality,summary)
```

```
wine_df$quality: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.2128  0.3237  0.4541  0.4366  0.5502  0.7083
--------------------------------------------------------------------------------------------
wine_df$quality: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0531  0.2727  0.3913  0.3875  0.5000  0.7083
--------------------------------------------------------------------------------------------
wine_df$quality: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.08065 0.23077 0.32394 0.33917 0.42857 0.78261
--------------------------------------------------------------------------------------------
wine_df$quality: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
0.02273 0.28571 0.40362 0.40962 0.50000 0.85714
--------------------------------------------------------------------------------------------
wine_df$quality: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.1298  0.3333  0.4500  0.4361  0.5357  0.7429
--------------------------------------------------------------------------------------------
wine_df$quality: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.1364  0.3133  0.3823  0.4065  0.4926  0.7556
```

```
ggplot(wine_df, aes(x=quality, y=prop_free_so2))+
  geom_boxplot(aes(group=quality))+
  coord_cartesian(ylim = c(0.2,0.6))+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4) +
  labs(title='Boxplot of free sulfur dioxide to the quality of red wine')
```

![Hist41](../../images/Projects/red_wine_quality/41.png)


The statistical summary and boxplot gives the median and mean value of
proportion of free SO2  with respect to total sulfur dioxide. The red point
shows the mean value of the proportion of free sulfur dioxide.

## Univariate Analysis

### What is the structure of your dataset?

The above dataset consists of 1599 observations and 15 variables.
Also i added 2 variables to the dataset namely Total_acidity and prop_free_SO2.

### What is/are the main feature(s) of interest in your dataset?

* In the given dataset, 6 qualities of datset are available namely
_Quality 3, Quality 4, Quality 5, Quality 6, Quality 7 and Quality 8_.

* Of the available qualities of red wines _quality 5_ and _quality 6_ are the
majority 681 and 638 respectively.

### What other features in the dataset do you think will help support your investigation into your feature(s) of interest?

The following analysis made in the above univariate analysis might help in
determining the chemical property which help in influencing the quality of red
wines-

* Good quality red wines have lower amounts of volatile acids present in them.
* Citric acid content is higher in good quality wines.
* Amount of residual sugar content is almost same across all qualities of red
wine.
* Good quality of red wines have lower density.
* Sulphate content is higher in good quality red wines.
* Alcohol content is higher in good quality red wines.

### Did you create any new variables from existing variables in the dataset?

Yes, i created 2 new variables from the existing variables.

* __Total_acidity :__ As there are three types of acidity in the
red wine so i was just curious to find out the net effect of all acidity on the
quality of red wine. So i created this variable which is the sum of fixed
acidity, volatile acidity and citirc acid. (measured in g/dm<super>3</super> )

* __prop_free_so2 :__ It is the ratio of free.sulfur.dioxide to
total.sulfur.dioxide present in the concerned red wine. As the
total.sulfur.dioxide present in the red wine contains both free.sulfur.dioxide
and bound.sulfur.dioxide so to determine the proportion in which the
free.sulfur.dioxide is used and to check how this ratio affect the quality of
red wine we use this variable.

### Of the features you investigated, were there any unusual distributions? Did you perform any operation on the data to tidy, adjust, or change the form of the data? If so, why did you do this?

I tried to change the scale of  x-axis from continuous to logarithmic but there
was not much variation to the plot. No other transformation to the dataset and
plot was required as


## Bivariate Plots Section

In the bivariate analysis, i will start by finding the correlation coefficent
between quality and other variables and among the other supporting variables.

### Correlation matrix

```
cwine_df <- wine_df %>%
  select(c(2:15))

mcor <- cor(cwine_df)
round(mcor,digits = 3)
```

```
                      fixed.acidity volatile.acidity citric.acid
 fixed.acidity                1.000           -0.256       0.672
 volatile.acidity            -0.256            1.000      -0.552
 citric.acid                  0.672           -0.552       1.000
 residual.sugar               0.115            0.002       0.144
 chlorides                    0.094            0.061       0.204
 free.sulfur.dioxide         -0.154           -0.011      -0.061
 total.sulfur.dioxide        -0.113            0.076       0.036
 density                      0.668            0.022       0.365
 pH                          -0.683            0.235      -0.542
 sulphates                    0.183           -0.261       0.313
 alcohol                     -0.062           -0.202       0.110
 quality                      0.124           -0.391       0.226
 Total_acidity                0.996           -0.204       0.690
 prop_free_so2               -0.131           -0.073      -0.167

                      residual.sugar chlorides free.sulfur.dioxide
 fixed.acidity                 0.115     0.094              -0.154
 volatile.acidity              0.002     0.061              -0.011
 citric.acid                   0.144     0.204              -0.061
 residual.sugar                1.000     0.056               0.187
 chlorides                     0.056     1.000               0.006
 free.sulfur.dioxide           0.187     0.006               1.000
 total.sulfur.dioxide          0.203     0.047               0.668
 density                       0.355     0.201              -0.022
 pH                           -0.086    -0.265               0.070
 sulphates                     0.006     0.371               0.052
 alcohol                       0.042    -0.221              -0.069
 quality                       0.014    -0.129              -0.051
 Total_acidity                 0.124     0.117              -0.154
 prop_free_so2                -0.071    -0.105               0.327

                      total.sulfur.dioxide density     pH sulphates alcohol
 fixed.acidity                      -0.113   0.668 -0.683     0.183  -0.062
 volatile.acidity                    0.076   0.022  0.235    -0.261  -0.202
 citric.acid                         0.036   0.365 -0.542     0.313   0.110
 residual.sugar                      0.203   0.355 -0.086     0.006   0.042
 chlorides                           0.047   0.201 -0.265     0.371  -0.221
 free.sulfur.dioxide                 0.668  -0.022  0.070     0.052  -0.069
 total.sulfur.dioxide                1.000   0.071 -0.066     0.043  -0.206
 density                             0.071   1.000 -0.342     0.149  -0.496
 pH                                 -0.066  -0.342  1.000    -0.197   0.206
 sulphates                           0.043   0.149 -0.197     1.000   0.094
 alcohol                            -0.206  -0.496  0.206     0.094   1.000
 quality                            -0.185  -0.175 -0.058     0.251   0.476
 Total_acidity                      -0.096   0.676 -0.683     0.182  -0.067
 prop_free_so2                      -0.371  -0.265  0.185    -0.010   0.246

                      quality Total_acidity prop_free_so2
 fixed.acidity          0.124         0.996        -0.131
 volatile.acidity      -0.391        -0.204        -0.073
 citric.acid            0.226         0.690        -0.167
 residual.sugar         0.014         0.124        -0.071
 chlorides             -0.129         0.117        -0.105
 free.sulfur.dioxide   -0.051        -0.154         0.327
 total.sulfur.dioxide  -0.185        -0.096        -0.371
 density               -0.175         0.676        -0.265
 pH                    -0.058        -0.683         0.185
 sulphates              0.251         0.182        -0.010
 alcohol                0.476        -0.067         0.246
 quality                1.000         0.104         0.194
 Total_acidity          0.104         1.000        -0.149
 prop_free_so2          0.194        -0.149         1.000
```

### Plotting the Correlation matrix

```
corrplot(mcor)
```

![Hist42](../../images/Projects/red_wine_quality/42.png)

From the correlation matrix plot it can be observed that :

* Fixed acidity is highly correlated citric acid, density and Total
acidity.  Fixed acidity has high value of negative correlation with pH. And
correlation of fixed acidity with quality is very low.
* Volatile acidity is negatively correlated with citric acid and quality.
* Citric acid has high positive correlation with fixed acidity and total
acidity.
* Density has highly positively correlated with fixed acidity and total acidity.
* pH has negative correlation with density, fixed, citric and total acidity.
* Quality has negative correlation with density and positive correlation with
alcohol.

### Scatterplot matrix

```
splom(cwine_df[c(1,2,7,8,9,11,12)], data = cwine_df, groups = NULL,  axis.line.tck=0,axis.text.alpha = 0)

cwine_df <- cwine_df %>%
  select(1,2,7,8,9,11,12)
```

![Hist43](../../images/Projects/red_wine_quality/43.png)

The above scatter plot matrix has been made by selecting the variables from the
correlation matrix plot. This graph helps in getting the idea of scatterplot
that we will get when plotting different variables in the datset,

```
p2 <- ggpairs(data = cwine_df)+
  labs(title='Generalized Pairs Plot')

p2
```

![Hist44](../../images/Projects/red_wine_quality/44.png)

The generalized pairs plot shown above shows the correlation and scatterplot
between selected pairs of variables from the correlation and scatterplot matrix
shown above. The correlation values between variables are shown in the upper
triangular matrix. The diagonal matrix has the pairing between same variables.
The lower triangular shows the scatterplot between the pairs of variables.
The correlation values and scatterplot between variables helps in understanding
the relationship between the variables.

```
ggplot(wine_df, aes(x=pH, y=fixed.acidity))+
  geom_point(color='grey60')+
  stat_smooth(color='red',method='lm')+
  labs(title='Scatterplot of Fixed-acidity vs pH of red wine',y='fixed acidity (tartaric acid-g/dm<super>3</super>)')
```

![Hist45](../../images/Projects/red_wine_quality/45.png)

The above graph shows the negative correlation between fixed acidity and pH
value of red wine. Also a linear regression line is added to a scatterplot.

```
ggplot(wine_df, aes(x=pH, y=density))+
  geom_point(color='grey60')+
  stat_smooth(color='red')+
  labs(title='Scatterplot of density vs pH of red wine',y='density (g/cm 3)')
```

![Hist46](../../images/Projects/red_wine_quality/46.png)

The above graph shows the scatterplot between density and pH of red wine it
shows the negative corelation between density and pH. Also the regression line
used here is a loess curve.

```
ggplot(wine_df,aes(y=density,x=alcohol))+
  geom_point(color='grey60')+
  stat_smooth(method = 'loess',color='red', linetype='solid')+
  labs(title='Scatterplot of density vs alcohol content in red wine',
       x = 'alcohol (% by volume)', y='density (g/cm 3)')
```

![Hist47](../../images/Projects/red_wine_quality/47.png)

The above graph shows scatterplot of density vs alcohol content in red wine.
These parameters are negatively correlated and the negative correlation is
shown using a loess curve.

```
ggplot(wine_df,aes(x=factor(quality), y = alcohol))+
  geom_jitter(alpha=0.3)+
  geom_boxplot(alpha=0.8, color = 'blue')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title='Scatterplot and Boxplot of Quality vs alcohol content of red wine',
       y = 'alcohol (% by volume)')+
  coord_cartesian(ylim = c(9,13))
```

![Hist48](../../images/Projects/red_wine_quality/48.png)

The above scatterplot has quality as discrete variable and for higher quality
of red wine the alcohol content is also high which also proves the positive
correlation between them. The blue line denotes the mean value of alcohol
content across each quality. The red line denotes the median value of alcohol
content across each quality.

In the univariate analysis, boxplot graph was shown to prove the relation
between quality and alcohol content in red wine.

```
ggplot(wine_df, aes(x=factor(quality), y=volatile.acidity))+
  geom_jitter(alpha=1/3)+
  geom_boxplot(alpha=0.8, color = 'blue')+
  stat_summary(fun.y = 'mean', geom = 'point',color='red',shape=8,size=4)+
  labs(title='Scatterplot of Quality vs volatile acidity of red wine',
       y='volatile acidity (acetic acid - g/dm<super>3</super>)')+
  coord_cartesian(ylim = c(0.2,1.1))
```

![Hist49](../../images/Projects/red_wine_quality/49.png)


The above scatterplot has quality as discrete variable and for higher quality
of red wine the volatile acidity is very low which also proves the negative
correlation between them. The blue line denotes the mean value of volatile
acidity across each quality. The red line denotes the median value of volatile
acidity across each quality.

In the univariate analysis, boxplot graph was shown to prove the relation
between quality and volatile acidity in red wine.

### Bivariate Analysis

#### Talk about some of the relationships you observed in this part of the investigation. How did the feature(s) of interest vary with other features in the dataset?

The bivarite analysis gave an insight into the correlation  between quality and
other variables with the help of correlation coefficient and scatterplot. It was
observed that quality of red wine improves with the decreases in volatile
acidity. The alcohol content in red wine also improves the wine quality. From
the correlation matrix it was also observed that residual sugar and other
parameters don't have much effect on the quality of red wine.

#### Did you observe any interesting relationships between the other features (not the main feature(s) of interest)?

The relationship between pH and fixed/total acidity they have negative
correlation i.e., as pH value increases acidity content decreases. Also, pH is
negatively correlated with the density of the red wine. And density of the red
wine is also having negative correlation with the alcohol content in the red
wine. It is also found that citric acid is positively correlated with fixed
acidity of red wine but has a negative correlation with volatile acidity.

The bivariate analysis helped in improving the insight into the relationship
between quality and variables such as residual sugar,density and citric acid. It
was found that the presence of these variables don't have much impact on the
quality of red wines.

#### What was the strongest relationship you found?

The quality of red wine is highly dependant on volatile acidity and alcohol
content of the red wine. The quality and volatile acidity has  negative
correlation. The quality and alcohol content has a positive correlation between
them.


## Multivariate Plots Section


```
# Converting quality to ordered factors type
wine_df$quality <- ordered(wine_df$quality, levels=c(1:10))

ggplot(data = wine_df,aes(y=density, x=alcohol,color=quality))+
  geom_point(size=1,alpha=0.8)+
  geom_smooth(method = 'lm', se = FALSE, size=2)+
  scale_color_brewer(type='seq', guide = guide_legend(title = 'Quality'))+
  ggtitle('Alcohol and density of red wine based on Quality')+
  labs(x = 'Alcohol (% by volume)',y='density (g/cm 3)')
```

![Hist50](../../images/Projects/red_wine_quality/50.png)

Analyzing the above plot it's seen that as the quality of red wine improves
it's density decreases and alcohol content also increases as the points seem to
move towards the bottom right corner of the graph.

```
p3 <- ggplot(data = wine_df,aes(x=pH, y=fixed.acidity,color=quality))+
  geom_point(size=1,alpha=0.8)+
  geom_smooth(method = 'lm', se = FALSE, size=2)+
  scale_color_brewer(type='seq', guide = guide_legend(title = 'Quality'))+
  ggtitle('pH and fixed acidity of red wine based on Quality')+
  labs(y='fixed acidity (tartaric acid - g/dm<super>3</super>)')

p3
```

![Hist51](../../images/Projects/red_wine_quality/51.png)

It is observed from the scatterplot that the pH value of wine lies between 3
and 4 . Also for a good quality of red wine pH value must be less than 3.5 as
most points are concentrated there.
As it is observed from the  linear regression lines as pH decreases
fixed acidity content also improves.

I also referred the following blog to know more about the pH value of red wines.
[Blog on pH of wines](http://winemakersacademy.com/importance-ph-wine-making/)


```
p3 <- ggplot(wine_df, aes(x=alcohol,y=volatile.acidity,color=quality))+
  geom_point(size=1,alpha=0.8)+
  geom_smooth(method = 'lm', se = FALSE, size=2)+
  scale_color_brewer(type='seq', guide = guide_legend(title = 'Quality'))+
  ggtitle('volatile acidity vs alcohol content of red wine based on quality')+
  labs(x = 'Alcohol (% by volume)', y = 'volatile acidity (acetic acid - g/dm<super>3</super>)')
```

![Hist52](../../images/Projects/red_wine_quality/52.png)

Alcohol content and volatile acidity of wine are the two important factors that
affect the quality of red wine as from all the analysis done above. It is seen
that as volatile acidity decreases and alcohol content improves the quality of
wine improves gradually. So referring to the graph, as amount of volatile
acidity the quality of wine also improves but the only variation which is
observed is in case of Quality 8 red wine. It can be observed that the slope of
other quality of red wines is almost zero but for quality 8 red wine it can be
seen that slope has a low positive value. So a slight increase in the volatile
acidity content is marked by high increase in the alcohol content to maintain
the quality of red wine.


### Multivariate Analysis

#### Talk about some of the relationships you observed in this part of the investigation. Were there features that strengthened each other in terms of looking at your feature(s) of interest?

The relation between fixed acidity and pH by quality strengthens each other. As
its known that low pH value implies high acidity so from the graph it is seen
that at low pH values fixed acidity value is high. ANd the quality of wine also
improves  as the above relationship in the wine is satisfied.

#### Were there any interesting or surprising interactions between features?

One surprising feature is the relationship between density and alcohol content
by quality. It is seen that as the alcohol content  decreases the density of
the red wine has increased and the quality decreases that is the good quality
wines points tend to move towards the right bottom corner of the graph with low
density and high alcohol content.

------

## Final Plots and Summary

### Plot One

```
p1
```

![Hist1](../../images/Projects/red_wine_quality/1.png)

__Description One__

It is observed that wines with quality ratings 5and 6 is the most available in
the given dataset. Followed by quality 7 and quality 4. Quality 8 followed by
quality 3 are the least available. Data below gives the count of each quality
of red wine available in the given dataset.

```
table(wine_df$quality)
```

```
  1   2   3   4   5   6   7   8   9  10
  0   0  10  53 681 638 199  18   0   0
```

### Plot Two

__Plotting the Correlation matrix__

```
p2
```

![Hist44](../../images/Projects/red_wine_quality/44.png)

__Description__

The generalized pairs plot shown above shows the correlation and scatterplot
between selected pairs of variables from the correlation and scatterplot matrix
shown above. The correlation values between variables are shown in the upper
triangular matrix. The diagonal matrix has the pairing between same variables.
The lower triangular shows the scatterplot between the pairs of variables.
The correlation values and scatterplot between variables helps in understanding
the relationship between the variables.

It can be observed that :

* Fixed acidity is highly correlated citric acid, density and Total
acidity.  Fixed acidity has high value of negative correlation with pH. And
correlation of fixed acidity with quality is very low.
* Volatile acidity is negatively correlated with citric acid and quality.
* Citric acid has high positive correlation with fixed acidity and total
acidity.
* Density has highly positively correlated with fixed acidity and total acidity.
* pH has negative correlation with density, fixed, citric and total acidity.
* Alcohol has negative correlation with density and positive correlation with
alcohol.

### Plot Three
```
p3
```

![Hist52](../../images/Projects/red_wine_quality/52.png)

__Description Three__

Alcohol content and volatile acidity of wine are the two important factors that
affect the quality of red wine based on correlation values. It is seen
that as volatile acidity decreases and alcohol content improves the quality of
wine improves gradually. So referring to the graph, as amount of volatile
acidity the quality of wine also improves but the only variation which is
observed is in case of Quality 8 red wine. It can be observed that the slope of
other quality of red wines is almost zero but for quality 8 red wine it can be
seen that slope has a low positive value. So a slight increase in the volatile
acidity content is marked by high increase in the alcohol content to maintain
the quality of red wine.

------

## Reflection

The conclusions drawn by me in this analysis are:

1. In the given dataset for red wine, Quality 5 and quality 6 wines are the
most available.
2. Red wines of Quality 1, Quality 2, Quality 9 and Quality 10 are not
available.
3. Amount of residual sugar levels (in g/dm<super>3</super> ) in red wines have no affect on
quality of red wine,
4. As pH decreases fixed acidity amount improves.
5. Density of red wine decreases montonically as pH value increases.
6. Density of red wine decreases with increase in alcohol content.
7. Quality of red wine improves as alcohol content increases.
8. Quality of red wine improves as volatile acid's content decreases.

The analysis could have been performed better if there was more data on other
qualities of red wines available. If more data had been availabe results could
have been more accurate and new conclusions and inferences could have been made.

It was surprising to see that some chemical parameters such as residual sugar,
chlorides, free sulfur dioxide and total sulfur dioxide did not have much affect
on the quality of red wine.

For future explorations i can combine the red wine dataset with the dataset for
white wine to draw comparisons between them and analyze the differences in their
chemical constituents or parameters. Also the data for other qualities of red
wine would greatly help in improving this analysis.

***

Project can be found in this [github repo](https://github.com/jeswingeorge/EDA-project---wineQualityReds).
***
