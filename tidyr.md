# Reference for 'tidyr' package
November 2, 2015  

  > *__tidy data__: every column in the data frame represents a variable and every row represents an observation --- also referred to as __long format__*
  
  
## Overview

* `gather()` and `spread()` convert between long and wide data
* `separate()` splits a single column into servaral variables and is commonly used in conjunction with `gather()`


```r
library(tidyr); library(dplyr)

set.seed(1)
tidyr.ex <- data.frame(
  participant = c("p1", "p2", "p3", "p4", "p5", "p6"), 
  info = c("g1m", "g1m", "g1f", "g2m", "g2m", "g2m"),
  day1score = rnorm(n = 6, mean = 80, sd = 15), 
  day2score = rnorm(n = 6, mean = 88, sd = 8)
)
```


```r
print(tidyr.ex)
```

```
##   participant info day1score day2score
## 1          p1  g1m  70.60319  91.89943
## 2          p2  g1m  82.75465  93.90660
## 3          p3  g1f  67.46557  92.60625
## 4          p4  g2m 103.92921  85.55689
## 5          p5  g2m  84.94262 100.09425
## 6          p6  g2m  67.69297  91.11875
```

### gather: convert wide data to long data

* takes columns and gathers them into new variable
* `gather(df, newVar1, newVar2, vector1, vector2)`


```r
tidyr.ex %>%
  gather(day, score, c(day1score, day2score))
```

```
##    participant info       day     score
## 1           p1  g1m day1score  70.60319
## 2           p2  g1m day1score  82.75465
## 3           p3  g1f day1score  67.46557
## 4           p4  g2m day1score 103.92921
## 5           p5  g2m day1score  84.94262
## 6           p6  g2m day1score  67.69297
## 7           p1  g1m day2score  91.89943
## 8           p2  g1m day2score  93.90660
## 9           p3  g1f day2score  92.60625
## 10          p4  g2m day2score  85.55689
## 11          p5  g2m day2score 100.09425
## 12          p6  g2m day2score  91.11875
```


### spread: convert long data to wide data

* takes different levels of a factor and spreads them out into different columns
* `spread(df, var1, var2)`


```r
# returns back original data frame
tidyr.ex %>%
  gather(day, score, c(day1score, day2score)) %>%
  spread(day, score)
```

```
##   participant info day1score day2score
## 1          p1  g1m  70.60319  91.89943
## 2          p2  g1m  82.75465  93.90660
## 3          p3  g1f  67.46557  92.60625
## 4          p4  g2m 103.92921  85.55689
## 5          p5  g2m  84.94262 100.09425
## 6          p6  g2m  67.69297  91.11875
```


### separate: separates one column into many

* takes values in a column and creates new columns
* `separate(df, col, into, sep)`


```r
# sep can be numeric (length) or character
tidyr.ex %>%
  gather(day, score, c(day1score, day2score)) %>%
  separate(col = info, into = c("group", "gender"), sep = 2)
```

```
##    participant group gender       day     score
## 1           p1    g1      m day1score  70.60319
## 2           p2    g1      m day1score  82.75465
## 3           p3    g1      f day1score  67.46557
## 4           p4    g2      m day1score 103.92921
## 5           p5    g2      m day1score  84.94262
## 6           p6    g2      m day1score  67.69297
## 7           p1    g1      m day2score  91.89943
## 8           p2    g1      m day2score  93.90660
## 9           p3    g1      f day2score  92.60625
## 10          p4    g2      m day2score  85.55689
## 11          p5    g2      m day2score 100.09425
## 12          p6    g2      m day2score  91.11875
```


### unite: opposite of separate

* not used frequently
* `unit(df, newVarName, col1, col2)`


```r
# default combines with "_"
tidyr.ex %>%
  gather(day, score, c(day1score, day2score)) %>%
  separate(col = info, into = c("group", "gender"), sep = 2) %>%
  unite(infoAgain, group, gender)
```

```
##    participant infoAgain       day     score
## 1           p1      g1_m day1score  70.60319
## 2           p2      g1_m day1score  82.75465
## 3           p3      g1_f day1score  67.46557
## 4           p4      g2_m day1score 103.92921
## 5           p5      g2_m day1score  84.94262
## 6           p6      g2_m day1score  67.69297
## 7           p1      g1_m day2score  91.89943
## 8           p2      g1_m day2score  93.90660
## 9           p3      g1_f day2score  92.60625
## 10          p4      g2_m day2score  85.55689
## 11          p5      g2_m day2score 100.09425
## 12          p6      g2_m day2score  91.11875
```

