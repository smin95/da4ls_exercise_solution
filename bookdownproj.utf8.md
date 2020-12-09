--- 
title: "Data Analysis for the Life Sciences with R: Exercise Solutions"
author: "Seung Hyun (Sam) Min"
date: "2020-12-09"
site: bookdown::bookdown_site
---

# Welcome {-}

<img src="./img/book_cover.jpg" width="250" height="375" alt="Cover image" align="right" style="margin: 0 1em 0 1em"/>

This book contains **unofficial exercise solutions** for the book [Data Analysis for the Life Sciences with R](https://leanpub.com/dataanalysisforthelifesciences) by Rafael A. Irizarry and Michael I. Love. The PDF copy of the book is available for free and the physical copy is available in [Amazon](https://www.amazon.com/Data-Analysis-Life-Sciences-R/dp/1498775675/ref=sr_1_2?dchild=1&keywords=Data+Analysis+for+the+Life+Sciences&qid=1607484629&sr=8-2).

## Acknowledgment {-}
I would like to thank Rafael A. Irizarry and Michael I. Love for writing this wonderful book. 

<!--chapter:end:index.Rmd-->

# Getting started

Since this chapter does not deal with statistics, I have decided to skip this chapter altogether.

<!--chapter:end:getting_started.Rmd-->

---
output: html_document
chapter: Inference
editor_options:
  chunk_output_type: console
---
# Inference
First, upload necessary package(s).

```r
library(tidyverse) #also uploads  dplyr
library(rafalib) # important for plotting with base R
```
## 2.7 Exercises 

If you have not downloaded the data before,

```r
dir <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/"
filename <- "femaleControlsPopulation.csv"
url <- paste0(dir, filename)
x <- unlist(read.csv(url))
```

Or if you already have downloaded the data, then just upload it.


```r
dat <- read.csv('femaleControlsPopulation.csv')
bodyweight <- select(dat, Bodyweight)
x <- unlist(bodyweight)

# or use pipe %>%
x <- read.csv('femaleControlsPopulation.csv') %>% select(Bodyweight) %>% unlist()
```

Check out what `unlist` does by typing `?unlist` in the command. The second method is more concise because of the pipe `%>%`, which allows multiple lines of commands to be in one continuous line. 

### Question 1

<div class="question">
What is the average of these weights?
</div>

<div class="answer">


```r
mean(x)
```

```
## [1] 23.89338
```

### Question 2
<div class="question">
After setting the seed at 1, `set.seed(1)` take a random sample size 5. What is the absolute value (use `abs`) of the difference between the average of the sample and the average of all the values?
</div>

<div class="answer">


```r
set.seed(1)
avg_sample <- mean(sample(x,5)) # average of the sample of 5
avg_pop <- mean(x) # average of all values
abs(avg_sample - avg_pop) # absolute difference
```

```
## [1] 0.2706222
```

### Question 3
<div class="question">
After setting the seed at 5, `set.seed(5)` take a random sample size 5. What is the absolute value (use `abs`) of the difference between the average of the sample and the average of all the values?
</div>

<div class="answer">


```r
set.seed(5)
avg_sample <- mean(sample(x,5)) # average of the sample of 5
avg_pop <- mean(x) # average of all values
abs(avg_sample - avg_pop) # absolute difference
```

```
## [1] 1.433378
```

### Question 4
<div class="question">
Why are the answers from 2 and 3 different?
</div>

<div class="answer">

```r
set.seed(1) # question 2
a <- sample(x,5)
a
```

```
##  Bodyweight60  Bodyweight84 Bodyweight128 Bodyweight202 
##         21.51         28.14         24.04         23.45 
##  Bodyweight45 
##         23.68
```

```r
set.seed(5) # question 3
b <- sample(x,5)
b
```

```
##  Bodyweight46 Bodyweight154 Bodyweight205  Bodyweight64 
##         21.86         20.30         22.95         21.92 
##  Bodyweight24 
##         25.27
```

```r
identical(a,b) # these two samples are not identical
```

```
## [1] FALSE
```
Notice that samples `a` and `b` differ. Since the seeds were different (`1` vs `5`), different random numbers were generated. Therefore, the answer is **C: Because the average of the samples is a random variable.**

### Question 5
<div class="question">
Set the seed at 1, then using a for-loop take a random sample of 5 mice in 1,000 times. Save these averages. What percent of these 1,000 averages are more than 1 gram away from the average of `x`?
</div>

<div class="answer">


```r
set.seed(1)
n <- 1000
res <- vector('double',n) 

for (i in seq(n)) {
  avg_sample <- mean(sample(x,5))
  res[[i]] <- avg_sample
}

mean(abs(res-mean(x)) > 1)
```

```
## [1] 0.498
```
To make a for loop work in R, an empty vector needs to be created first. This can be achieved with the function `vector`. In this example, the empty vector is `res` (short for result). In the for loop, each average (`avg_sample`) from one repetition gets stored in `res`. 

### Question 6
<div class="question">
We are now going to increase the number of times we redo the sample from 1,000 to 10,000. Set the seed at 1, then using a for-loop take a random sample of 5 mice 10,000 times. Save these averages. What percent of these 10,000 averages are more than 1 gram away from the average of `x`?
</div>

<div class="answer">

```r
set.seed(1)
n <- 10000
res <- vector('double',n)
for (i in seq(n)) {
  avg_sample <- mean(sample(x,5))
  res[[i]] <- avg_sample
}
mean(abs(res-mean(x)) > 1)
```

```
## [1] 0.4976
```

### Question 7
<div class="question">
Note that the answers to 5 and 6 barely changed. This is expected. The way we think about the random value distributions is as the distribution of the list of values obtained if we repeated the experiment an infinite number of times. On a computer, we can't perform an infinite number of iterations so instead, for our examples, we consider 1,000 to be large enough, thus 10,000 is as well. Now if instead we change the sample size, then we change the random variable and thus its distribution.

Set the seed at 1, then using a for-loop take a random sample of 50 mice 1,000 times. Save these averages. What percent of these 1,000 averages are more than 1 gram away from the average of `x`?
</div>


```r
set.seed(1)
n <- 1000
res <- vector('double',n)
for (i in seq(n)) {
  avg_sample <- mean(sample(x,50))
  res[[i]] <- avg_sample
}
mean(abs(res-mean(x)) > 1)
```

```
## [1] 0.019
```

### Question 8

<div class="question">
Use a histogram to "look" at the distribution of averages we get with a sample size of 5 and sample size of 50. How would you say they differ?
</div>

<div class="answer">


```r
# sample size = 5
set.seed(1)
n <- 1000
res5 <- vector('double',n)
for (i in seq(n)) {
  avg_sample <- mean(sample(x,5))
  res5[[i]] <- avg_sample
}
sd(res5) # standard deviation = spread of the histogram
```

```
## [1] 1.52445
```


```r
# sample size = 50
set.seed(1)
n <- 1000
res50 <- vector('double',n)
for (i in seq(n)) {
  avg_sample <- mean(sample(x,50))
  res50[[i]] <- avg_sample
}
sd(res50) # standard deviation = spread of the histogram
```

```
## [1] 0.4260116
```


```r
mypar(1,2) # plot histograms
hist(res5)
hist(res50)
```

<img src="bookdownproj_files/figure-html/unnamed-chunk-15-1.png" width="672" />

`mypar` is a function from the package `rafalib`. It helps to align multiple plots in a single plot. `mypar(1,1)` contains one panel only, `mypar(2,1)` contains 2 rows of panels and 1 column, `mypar(1,2)` contains 1 row of panels and 2 columns, etc. Type `?mypar` for more information. `hist` plots a  histogram.

The answer is **B: They both look normal, but with a sample size of 50 the spread is smaller.**

### Question 9
<div class="question">
For the last set of averages, the ones obtained from a sample size of 50, what percent are between 23 and 25?
</div>

<div class="answer">

```r
mean((res50 >=23) & (res50 <= 25))
```

```
## [1] 0.976
```

### Question 10
<div class="question">
Now ask the same question of a normal distribution with average 23.9 and standard deviation 0.43.
</div>

<div class="answer">


```r
pnorm(25,23.9,0.43) - pnorm(23,23.9,0.43)
```

```
## [1] 0.9765648
```

The answers to 9 and 10 were very similar. This is because we can approximate the distribution of the sample average with a normal distribution. We will learn more about the reason for this next.

## 2.9 Exercises

If you have not downloaded the data before:

```r
dir <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/"
filename <- "mice_pheno.csv"
url <- paste0(dir, filename)
dat <- read.csv(url)
dat <- na.omit(dat)
```

If you have  the data already in your directory:

```r
raw_data <- read.csv('mice_pheno.csv')
dat <- na.omit(raw_data)
```
### Question 1
<div class="question">
Use `dplyr` to create a vector x with the body weight of all males on the control (chow) diet. What is this population's average?
</div>

<div class="answer">


```r
x <- dat %>% filter(Sex == 'M' & Diet == 'chow') %>%
  select(Bodyweight) %>% unlist()
mean(x)
```

```
## [1] 30.96381
```

Throughout the book, I will be using `%>%` for brevity. If you don't understand it, please check out Chapter 18 of [*R for Data Science](https://r4ds.had.co.nz/pipes.html).

### Question 2
<div class="question">
Now use the `rafalib` package and use the `popsd` function to compute the population standard deviation.
</div>

<div class="answer">


```r
popsd(x)
```

```
## [1] 4.420501
```

### Question 3
<div class="question">
Set the seed at 1. Take a random sample X of size 25 from x. What is the sample average?
</div>

<div class="answer">


```r
set.seed(1)
samp_x <- sample(x,25) # sample of x
mean(samp_x)
```

```
## [1] 32.0956
```

### Question 4
<div class="question">
Use `dplyr` to create a vector `y` with the body weight of all males on the high fat (hf) diet. What is this population's average?
</div>

<div class="answer">


```r
y <- dat %>% filter(Sex == 'M' & Diet == 'hf') %>%
  select(Bodyweight) %>% unlist()
mean(y)
```

```
## [1] 34.84793
```

### Question 5
<div class="question">
Now use the `rafalib` package and use the `popsd` function to compute the population standard deviation.
</div>

<div class="answer">


```r
popsd(y)
```

```
## [1] 5.574609
```


### Question 6
<div class="question">
Set the seed at 1. Take a random sample Y of size 25 from y. What is the sample average?
</div>

<div class="answer">


```r
set.seed(1)
samp_y <- sample(y,25)
mean(samp_y)
```

```
## [1] 34.768
```

### Question 7
<div class="question">
What is the difference in absolute value between $\bar{y}-\bar{x}$ and $\bar{Y}-\bar{X}$?
</div>

<div class="answer">


```r
pop_diff <- mean(y) - mean(x)
sample_diff <- mean(samp_y) - mean(samp_x)
abs(sample_diff - pop_diff)
```

```
## [1] 1.211716
```

### Question 8
<div class="question">
Repeat the above for females. Make sure to set the seed to 1 before each `sample` call. What is the difference in absolute value between $\bar{y}-\bar{x}$ and $\bar{Y}-\bar{X}$?
</div>

<div class="answer">


```r
chow_f_pop <- dat %>% filter(Sex == 'F' & Diet == 'chow') %>%
  select(Bodyweight) %>% unlist() # x
hf_f_pop <- dat %>% filter(Sex == 'F' & Diet == 'hf') %>%
  select(Bodyweight) %>% unlist() # y

set.seed(1)
sample_chow_f_pop <- sample(chow_f_pop, 25) # X
sample_hf_f_pop <- sample(hf_f_pop,25) # Y

pop_diff <- mean(hf_f_pop) - mean(chow_f_pop) # y - x
sample_diff <- mean(sample_hf_f_pop) - mean(sample_chow_f_pop) # Y - X

abs(sample_diff - pop_diff)
```

```
## [1] 0.07888278
```

### Question 9
<div class="question">
For the females, our sample estimates were closer to the population difference than with males. What is a possible explanation for this?
</div>

<div class="answer">

```r
ans <- c(popsd(hf_f_pop), popsd(chow_f_pop), popsd(y), popsd(x))
names(ans) <- c('hf female', 'chow female', 'hf male', 'chow male')
ans
```

```
##   hf female chow female     hf male   chow male 
##    5.069870    3.416438    5.574609    4.420501
```

The answer is **A: The population variance of the females is smaller than that of the males; thus, the sample variable has less variability.**

<!--chapter:end:inference.Rmd-->
