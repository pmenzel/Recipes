# Population pyramid with bi-directional bars

Create [population_pyramid_bars.md](population_pyramid_bars.md): `R -e 'library(knitr); knit("population_pyramid_bars.Rmd")'`

Required libraries:

```r
library(ggplot2)
```

## Basic case

Create data frame with columns for age group, gender, and count:

```r
set.seed(100)
df <- data.frame(count=sample(1:20,40,replace=TRUE), gender=c(rep('M',20),rep('F',20)), age=factor(rep(1:20,2)))
df
```

```
##    count gender age
## 1      7      M   1
## 2      6      M   2
## 3     12      M   3
## 4      2      M   4
## 5     10      M   5
## 6     10      M   6
## 7     17      M   7
## 8      8      M   8
## 9     11      M   9
## 10     4      M  10
## 11    13      M  11
## 12    18      M  12
## 13     6      M  13
## 14     8      M  14
## 15    16      M  15
## 16    14      M  16
## 17     5      M  17
## 18     8      M  18
## 19     8      M  19
## 20    14      M  20
## 21    11      F   1
## 22    15      F   2
## 23    11      F   3
## 24    15      F   4
## 25     9      F   5
## 26     4      F   6
## 27    16      F   7
## 28    18      F   8
## 29    11      F   9
## 30     6      F  10
## 31    10      F  11
## 32    19      F  12
## 33     7      F  13
## 34    20      F  14
## 35    14      F  15
## 36    18      F  16
## 37     4      F  17
## 38    13      F  18
## 39    20      F  19
## 40     3      F  20
```

The trick for making bi-directional bars is to convert the counts of one gender to negative values and change the axis labels to postive values on both sides:

```r
ggplot(data=df,aes(x=age, fill=gender)) +
  geom_bar(data=subset(df,gender=='M'),aes(y=count),stat="identity") +
  geom_bar(data=subset(df,gender=='F'),aes(y=-1*count),stat="identity") +
  coord_flip() +
  theme_bw() +
  theme(legend.position="bottom") +
  scale_y_continuous(limits=c(-20,20),breaks=seq(-20,20,5),labels=abs(seq(-20,20,5)))
```

![plot of chunk population_pyramid_bars](figure/population_pyramid_bars-1.png)

