# Cumulative count of set union

Create [cumulative_sets_bars.md](cumulative_sets_bars.md): `R -e 'library(knitr); knit("cumulative_sets_bars.Rmd")'`

Required libraries:

```r
library(plyr)
library(dplyr)
library(ggplot2)
```

## Basix example

Create data frame with two columns for set IDs and elements:


```r
element <- c("A","B","C","A","A","D","E","F")
id <- c("Set1", "Set1", "Set1", "Set3", "Set2", "Set3","Set3","Set4")
df <- data.frame(id, element)
df
```

```
##     id element
## 1 Set1       A
## 2 Set1       B
## 3 Set1       C
## 4 Set3       A
## 5 Set2       A
## 6 Set3       D
## 7 Set3       E
## 8 Set4       F
```

Order by set id and increase counter for each additional element to generate a cumulative count of unique elements:


```r
df.cc <- mutate(df[order(df$id),],cc=cumsum(!duplicated(unlist(element))))
df.cc
```

```
##     id element cc
## 1 Set1       A  1
## 2 Set1       B  2
## 3 Set1       C  3
## 4 Set2       A  3
## 5 Set3       A  3
## 6 Set3       D  4
## 7 Set3       E  5
## 8 Set4       F  6
```

Now, collapse the data frame by `id` and assign highest value of `cc` to the new column `ccmax`.
Additionally, add a count for the number of elements in each set and a string with all element names.


```r
df.cc.max <- ddply(df.cc, .(id), summarise, n_elements=length(element), ccmax=max(cc), elements=paste(element,collapse=" "))
df.cc.max
```

```
##     id n_elements ccmax elements
## 1 Set1          3     3    A B C
## 2 Set2          1     3        A
## 3 Set3          3     5    A D E
## 4 Set4          1     6        F
```

Plot bar chart with set sizes and cumulative sum of set union size (`ccmax`):

```r
ggplot(df.cc.max,aes(x=id,fill=id)) +
  geom_bar(aes(y=n_elements),stat="identity") +
  geom_point(aes(y=ccmax, fill=id),shape=16,size=5) +
  geom_text(y=0.1,aes(label=elements)) +
  theme_bw() + theme(legend.position="none",axis.ticks=element_blank(),panel.grid.minor.y=element_blank()) +
  xlab("") +
  scale_y_continuous("# elements in each set (bars) and cumulative set union (points)",breaks=seq(0,6,1),labels=(seq(0,6,1)))
```

![plot of chunk cumulative_sets_bars](figure/cumulative_sets_bars-1.png)

