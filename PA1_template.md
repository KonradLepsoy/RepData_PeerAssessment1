---
title: "Peer Assessment #1"
author: "Lars Konrad Andreassen"
date: "Thursday, May 07, 2015"
output: html_document
---

Loading data from the provided URL into a dataframe and then format the data to a suitable format


```r
require("utils")
library(utils)
library(data.table)
library(ggplot2)

url<-"https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"

if (!file.exists("Assignment 1")) {
  dir.create("Assignment 1")
}
setwd("./Assignment 1")

if (!file.exists("activity.csv")) {
  download.file(url, destfile="./repdata-data-activity.zip")
  file<-unzip(zipfile="repdata-data-activity.zip") 
}

act <- read.csv(file="activity.csv", header=TRUE, sep=",")
act_noNA <- subset(act, !(is.na(act$steps) ) )
act$date <- as.POSIXlt( act$date, format="%Y-%m-%d")
act_noNA$date_type <- as.POSIXlt( act_noNA$date, format="%Y-%m-%d")
act <- data.table(act)
act_noNA <- data.table(act_noNA)
```

# Question number 1 : Steps taken per day
What is mean total number of steps taken per day?

```r
Total_number_of_steps_per_day <- act_noNA[ , sum(steps), by = date]
head(Total_number_of_steps_per_day)
```

```
##          date    V1
## 1: 2012-10-02   126
## 2: 2012-10-03 11352
## 3: 2012-10-04 12116
## 4: 2012-10-05 13294
## 5: 2012-10-06 15420
## 6: 2012-10-07 11015
```

Make a histogram of the total number of steps taken each day

```r
ggplot(Total_number_of_steps_per_day, aes(x=date,  y=V1 ) ) + geom_histogram (stat="identity")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

Calculate the mean of the total number of steps taken per day

```r
my_mean <- mean(Total_number_of_steps_per_day$V1)
```

Calculate the median of the total number of steps taken per day

```r
my_median <- median(Total_number_of_steps_per_day$V1)
```


**The mean number of steps taken per day is 1.0766189 &times; 10<sup>4</sup> and the median number of steps taken per day is 10765.**


# Question number 2 : Daily interval pattern
What is the average daily activity pattern?

```r
Avg_number_of_steps_per_interval <- act_noNA[ , mean(steps), by = interval]
```

Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
plot (Avg_number_of_steps_per_interval$interval, 
      Avg_number_of_steps_per_interval$V1, type="l", xlab= "Interval", ylab="Avgerage number of steps")
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png) 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
Max_Avg_number_of_steps_per_interval <- Avg_number_of_steps_per_interval[order(-rank(V1))]
max_steps <- Max_Avg_number_of_steps_per_interval[1, interval]
```


**The interval with the max number of steps is 835.**


# Question number 3 : Missing values
Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
sum(is.na(act$steps))
```

```
## [1] 2304
```

Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

Create a new dataset that is equal to the original dataset but with the missing data filled in.

Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


Resetting the users environment and returning to original directory

```r
setwd("..") 
```
