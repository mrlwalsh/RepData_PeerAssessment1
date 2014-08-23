# Reproducible Research: Peer Assessment 1

## Setup  

Set the working directory, check and set directories, check and load packages,
load libraries.  

```r
setwd("~/GitHub/RepData_PeerAssessment1")

# check for directories and create if needed
if (!file.exists("data")) {
  dir.create("data")
}

if (!file.exists("figures")) {
  dir.create("figures")
}

# Check and load packages

install_required_libs<-function() {
    for(i in 1:length(required_lib)) {
        if(required_lib[i] %in% rownames(installed.packages()) == FALSE) {
            install.packages(required_lib[i])                            }
        library(required_lib[i], character.only = TRUE)
                                     }
                                  }

required_lib =c("ggplot2","ggvis","dplyr")

install_required_libs()
```

```
## 
## Attaching package: 'ggvis'
## 
## The following object is masked from 'package:ggplot2':
## 
##     resolution
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```
## Loading and preprocessing the data

* Show any code that is needed to Load the data (i.e. read.csv())  

* Process/transform the data (if necessary) into a format suitable for your analysis  


```r
a_data <- read.csv(file="./data/activity.csv",header=TRUE,fill=TRUE)

str(a_data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
summary(a_data)
```

```
##      steps               date          interval   
##  Min.   :  0.0   2012-10-01:  288   Min.   :   0  
##  1st Qu.:  0.0   2012-10-02:  288   1st Qu.: 589  
##  Median :  0.0   2012-10-03:  288   Median :1178  
##  Mean   : 37.4   2012-10-04:  288   Mean   :1178  
##  3rd Qu.: 12.0   2012-10-05:  288   3rd Qu.:1766  
##  Max.   :806.0   2012-10-06:  288   Max.   :2355  
##  NA's   :2304    (Other)   :15840
```

```r
# transform data

a_data$date <- as.Date(a_data$date, "%Y-%m-%d")
summary(a_data)
```

```
##      steps            date               interval   
##  Min.   :  0.0   Min.   :2012-10-01   Min.   :   0  
##  1st Qu.:  0.0   1st Qu.:2012-10-16   1st Qu.: 589  
##  Median :  0.0   Median :2012-10-31   Median :1178  
##  Mean   : 37.4   Mean   :2012-10-31   Mean   :1178  
##  3rd Qu.: 12.0   3rd Qu.:2012-11-15   3rd Qu.:1766  
##  Max.   :806.0   Max.   :2012-11-30   Max.   :2355  
##  NA's   :2304
```
## What is mean total number of steps taken per day?  

For this part of the assignment, you can ignore the missing values in the  
dataset.  

* Make a histogram of the total number of steps taken each day  

* Calculate and report the mean and median total number of steps taken per day  


```r
# Use dplyr and ggvis to group and display histogram of total steps
# steps <- 
# a_data %>%
#   group_by(date) %>%
#   summarize(nsteps = sum(steps)) %>%
#   ggvis(~nsteps) %>%
#   layer_histograms(binwidth = 1000) %>%
#   add_axis("x", title = "Number of Steps Taken") %>%
#   add_axis("x", orient = "top", ticks = 0, title = "Histogram of Steps Taken per Day",
#            properties = axis_props(
#              axis = list(stroke = "white"),
#              labels = list(fontSize = 0)))
# 
# steps

steps_plot <-
a_data %>%
  group_by(date) %>%
  summarize(nsteps = sum(steps)) %>%
  ggplot(aes(x=nsteps)) + geom_histogram(binwidth = 1000)

steps_plot + labs(title="Number of Steps per Day", x= "Number of Steps")
```

![plot of chunk NumberofStepsHistogram](./PA1_template_files/figure-html/NumberofStepsHistogram.png) 

```r
#Place plot in figures directory

png(file = "./figures/rrpa1_hist1.png")

steps_plot <-
a_data %>%
  group_by(date) %>%
  summarize(nsteps = sum(steps)) %>%
  ggplot(aes(x=nsteps)) + 
    geom_histogram(binwidth = 1000) +
    labs(title="Number of Steps per Day", x= "Number of Steps")

steps_plot

dev.off()
```

```
## pdf 
##   2
```

```r
# group data by day summing steps
meansteps <-
    a_data %>%
    group_by(date) %>%
    summarize(nsteps = sum(steps))

# find mean and median number of steps by day
meansteps
```

```
## Source: local data frame [61 x 2]
## 
##          date nsteps
## 1  2012-10-01     NA
## 2  2012-10-02    126
## 3  2012-10-03  11352
## 4  2012-10-04  12116
## 5  2012-10-05  13294
## 6  2012-10-06  15420
## 7  2012-10-07  11015
## 8  2012-10-08     NA
## 9  2012-10-09  12811
## 10 2012-10-10   9900
## 11 2012-10-11  10304
## 12 2012-10-12  17382
## 13 2012-10-13  12426
## 14 2012-10-14  15098
## 15 2012-10-15  10139
## 16 2012-10-16  15084
## 17 2012-10-17  13452
## 18 2012-10-18  10056
## 19 2012-10-19  11829
## 20 2012-10-20  10395
## 21 2012-10-21   8821
## 22 2012-10-22  13460
## 23 2012-10-23   8918
## 24 2012-10-24   8355
## 25 2012-10-25   2492
## 26 2012-10-26   6778
## 27 2012-10-27  10119
## 28 2012-10-28  11458
## 29 2012-10-29   5018
## 30 2012-10-30   9819
## 31 2012-10-31  15414
## 32 2012-11-01     NA
## 33 2012-11-02  10600
## 34 2012-11-03  10571
## 35 2012-11-04     NA
## 36 2012-11-05  10439
## 37 2012-11-06   8334
## 38 2012-11-07  12883
## 39 2012-11-08   3219
## 40 2012-11-09     NA
## 41 2012-11-10     NA
## 42 2012-11-11  12608
## 43 2012-11-12  10765
## 44 2012-11-13   7336
## 45 2012-11-14     NA
## 46 2012-11-15     41
## 47 2012-11-16   5441
## 48 2012-11-17  14339
## 49 2012-11-18  15110
## 50 2012-11-19   8841
## 51 2012-11-20   4472
## 52 2012-11-21  12787
## 53 2012-11-22  20427
## 54 2012-11-23  21194
## 55 2012-11-24  14478
## 56 2012-11-25  11834
## 57 2012-11-26  11162
## 58 2012-11-27  13646
## 59 2012-11-28  10183
## 60 2012-11-29   7047
## 61 2012-11-30     NA
```

```r
ms <- mean(meansteps$nsteps,na.rm=TRUE)
med <- median(meansteps$nsteps,na.rm=TRUE)

ms
```

```
## [1] 10766
```

```r
med
```

```
## [1] 10765
```
The mean number of steps taken per day is **10766** and the median 
number of steps taken per day is **10765**.  

## What is the average daily activity pattern?

Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis)  
and the average number of steps taken, averaged across all days (y-axis)  

Which 5-minute interval, on average across all the days in the dataset,  
contains the maximum number of steps?  

```r
# Find and remove NA rows

sum(is.na(a_data))
```

```
## [1] 2304
```

```r
for (i in 1:ncol(a_data)) {
  print(names(a_data)[i]);print(sum(is.na(a_data[,i])))
}
```

```
## [1] "steps"
## [1] 2304
## [1] "date"
## [1] 0
## [1] "interval"
## [1] 0
```

```r
bad_steps <- is.na(a_data$steps)
ga_data <- a_data[!bad_steps,]

summary(a_data)
```

```
##      steps            date               interval   
##  Min.   :  0.0   Min.   :2012-10-01   Min.   :   0  
##  1st Qu.:  0.0   1st Qu.:2012-10-16   1st Qu.: 589  
##  Median :  0.0   Median :2012-10-31   Median :1178  
##  Mean   : 37.4   Mean   :2012-10-31   Mean   :1178  
##  3rd Qu.: 12.0   3rd Qu.:2012-11-15   3rd Qu.:1766  
##  Max.   :806.0   Max.   :2012-11-30   Max.   :2355  
##  NA's   :2304
```

```r
summary(ga_data)
```

```
##      steps            date               interval   
##  Min.   :  0.0   Min.   :2012-10-02   Min.   :   0  
##  1st Qu.:  0.0   1st Qu.:2012-10-16   1st Qu.: 589  
##  Median :  0.0   Median :2012-10-29   Median :1178  
##  Mean   : 37.4   Mean   :2012-10-30   Mean   :1178  
##  3rd Qu.: 12.0   3rd Qu.:2012-11-16   3rd Qu.:1766  
##  Max.   :806.0   Max.   :2012-11-29   Max.   :2355
```

```r
# inter <-
#     ga_data %>%
#     group_by(interval) %>%
#     summarize(avgsteps = mean(steps)) %>%
#     ggvis(~interval,~avgsteps) %>%
#     layer_paths()
# 
# inter

# Plot time series avgsteps by interval

inter_ggp <-
    ga_data %>%
    group_by(interval) %>%
    summarize(avgsteps = mean(steps)) %>%
    ggplot(aes(x=interval,y=avgsteps)) +
           geom_line() +
           scale_x_continuous(breaks = seq(0, 2400, by = 200)) +
           labs(title="Time Series of Average Steps over Interval", 
                x= "Intra Day Interval")

inter_ggp
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

```r
# Save Plot to figures dirtectory

png(file = "./figures/rrpa1_interval.png")

inter_ggp <-
    ga_data %>%
    group_by(interval) %>%
    summarize(avgsteps = mean(steps)) %>%
    ggplot(aes(x=interval,y=avgsteps)) +
           geom_line() +
           scale_x_continuous(breaks = seq(0, 2400, by = 200)) +
           labs(title="Time Series of Average Steps over Interval", 
                x= "Intra Day Interval")

inter_ggp

dev.off()
```

```
## pdf 
##   2
```

```r
# order avgstep by interval data from High to Low

inter1 <-
    ga_data %>%
    group_by(interval) %>%
    summarize(avgsteps = mean(steps)) %>%
    arrange(desc(avgsteps))

inter1
```

```
## Source: local data frame [288 x 2]
## 
##    interval avgsteps
## 1       835    206.2
## 2       840    195.9
## 3       850    183.4
## 4       845    179.6
## 5       830    177.3
## 6       820    171.2
## 7       855    167.0
## 8       815    157.5
## 9       825    155.4
## 10      900    143.5
## ..      ...      ...
```

```r
inter1$interval[1]
```

```
## [1] 835
```

```r
inter1$avgsteps[1]
```

```
## [1] 206.2
```

```r
# inter2 <-
#     ga_data %>%
#     group_by(interval) %>%
#     summarize(avgsteps = mean(steps)) %>%
#     filter(interval >= 750 & interval <= 850) %>%
#     ggvis(~interval,~avgsteps) %>%
#     layer_paths()
# 
# inter2
```
Interval **835** has **206** average
steps, the maximun average number of steps.

## Imputing missing values

Note that there are a number of days/intervals where there are missing values  
(coded as NA). The presence of missing days may introduce bias into some  
calculations or summaries of the data. 

Calculate and report the total number of missing values in the dataset  
(i.e. the total number of rows with NAs)  

Devise a strategy for filling in all of the missing values in the dataset.  
The strategy does not need to be sophisticated. For example, you could use  
the mean/median for that day, or the mean for that 5-minute interval, etc.  

Create a new dataset that is equal to the original dataset but with the  
missing data filled in.  

Make a histogram of the total number of steps taken each day and Calculate  
and report the mean and median total number of steps taken per day. Do these  
values differ from the estimates from the first part of the assignment? What  
is the impact of imputing missing data on the estimates of the total daily  
number of steps?  

#### Strategy for Replacing NA Values  

The better approach for replacing missing step data is to use the average for
each interval across all days.  The range of step values seems to be narrower
for each interval across all days than for each day across all intervals.  I am
using the average of each interval across all days to replace missing step data
in the dataframe.  



```r
#Find missing data values
sum(is.na(a_data))
```

```
## [1] 2304
```

```r
for (i in 1:ncol(a_data)) {
  print(names(a_data)[i]);print(sum(is.na(a_data[,i])))
}
```

```
## [1] "steps"
## [1] 2304
## [1] "date"
## [1] 0
## [1] "interval"
## [1] 0
```

```r
bad_steps <- is.na(a_data$steps)
ba_data <- a_data[bad_steps,]

nrow(ba_data)
```

```
## [1] 2304
```

```r
# Use left join of original activity dataframe with interval averages data frame
tt <- left_join(a_data, inter1)
```

```
## Joining by: "interval"
```

```r
head(tt)
```

```
##   steps       date interval avgsteps
## 1    NA 2012-10-01        0  1.71698
## 2    NA 2012-10-01        5  0.33962
## 3    NA 2012-10-01       10  0.13208
## 4    NA 2012-10-01       15  0.15094
## 5    NA 2012-10-01       20  0.07547
## 6    NA 2012-10-01       25  2.09434
```

```r
tail(tt)
```

```
##       steps       date interval avgsteps
## 17563    NA 2012-11-30     2330   2.6038
## 17564    NA 2012-11-30     2335   4.6981
## 17565    NA 2012-11-30     2340   3.3019
## 17566    NA 2012-11-30     2345   0.6415
## 17567    NA 2012-11-30     2350   0.2264
## 17568    NA 2012-11-30     2355   1.0755
```

```r
tt$avgsteps <- as.integer(tt$avgsteps)

head(tt)
```

```
##   steps       date interval avgsteps
## 1    NA 2012-10-01        0        1
## 2    NA 2012-10-01        5        0
## 3    NA 2012-10-01       10        0
## 4    NA 2012-10-01       15        0
## 5    NA 2012-10-01       20        0
## 6    NA 2012-10-01       25        2
```

```r
tail(tt)
```

```
##       steps       date interval avgsteps
## 17563    NA 2012-11-30     2330        2
## 17564    NA 2012-11-30     2335        4
## 17565    NA 2012-11-30     2340        3
## 17566    NA 2012-11-30     2345        0
## 17567    NA 2012-11-30     2350        0
## 17568    NA 2012-11-30     2355        1
```

```r
ttt <- is.na(tt$steps)

# Replace NA state values with average across intervals
tt$steps[is.na(tt$steps)] <- tt$avgsteps
```

```
## Warning: number of items to replace is not a multiple of replacement
## length
```

```r
head(tt)
```

```
##   steps       date interval avgsteps
## 1     1 2012-10-01        0        1
## 2     0 2012-10-01        5        0
## 3     0 2012-10-01       10        0
## 4     0 2012-10-01       15        0
## 5     0 2012-10-01       20        0
## 6     2 2012-10-01       25        2
```

```r
tail(tt)
```

```
##       steps       date interval avgsteps
## 17563     2 2012-11-30     2330        2
## 17564     4 2012-11-30     2335        4
## 17565     3 2012-11-30     2340        3
## 17566     0 2012-11-30     2345        0
## 17567     0 2012-11-30     2350        0
## 17568     1 2012-11-30     2355        1
```

```r
summary(tt)
```

```
##      steps            date               interval       avgsteps    
##  Min.   :  0.0   Min.   :2012-10-01   Min.   :   0   Min.   :  0.0  
##  1st Qu.:  0.0   1st Qu.:2012-10-16   1st Qu.: 589   1st Qu.:  2.0  
##  Median :  0.0   Median :2012-10-31   Median :1178   Median : 33.5  
##  Mean   : 37.3   Mean   :2012-10-31   Mean   :1178   Mean   : 37.0  
##  3rd Qu.: 27.0   3rd Qu.:2012-11-15   3rd Qu.:1766   3rd Qu.: 52.2  
##  Max.   :806.0   Max.   :2012-11-30   Max.   :2355   Max.   :206.0
```

```r
# Plot histogram with missing values replaced with averages
steps_plot_revised <-
tt %>%
  group_by(date) %>%
  summarize(nsteps = sum(steps)) %>%
  ggplot(aes(x=nsteps)) + geom_histogram(binwidth = 1000)

         
steps_plot_revised + labs(title="Number of Steps per Day", x= "Number of Steps")
```

![plot of chunk unnamed-chunk-4](./PA1_template_files/figure-html/unnamed-chunk-4.png) 

```r
png(file = "./figures/rrpa1_hist1_rev.png")

steps_plot_revised <-
tt %>%
  group_by(date) %>%
  summarize(nsteps = sum(steps)) %>%
  ggplot(aes(x=nsteps)) + 
    geom_histogram(binwidth = 1000) +
    labs(title="Number of Steps per Day", x= "Number of Steps")

steps_plot_revised

dev.off()
```

```
## pdf 
##   2
```

```r
# group data by day summing steps
meansteps_revised <-
    tt %>%
    group_by(date) %>%
    summarize(nsteps = sum(steps))

# find mean and median number of steps by day
# meansteps

ms_rev <- mean(meansteps_revised$nsteps,na.rm=TRUE)
med_rev <- median(meansteps_revised$nsteps,na.rm=TRUE)

ms_rev
```

```
## [1] 10750
```

```r
med_rev
```

```
## [1] 10641
```
The mean number of steps taken per day is **10749** and the median 
number of steps taken per day is **10641**, after replacing missing values
for steps with the average number of steps for each interval.

## Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the  
dataset with the filled-in missing values for this part.  

Create a new factor variable in the dataset with two levels - "weekday" and  
"weekend" indicating whether a given date is a weekday or weekend day.  

Make a panel plot containing a time series plot (i.e. type = "l") of the  
5-minute interval (x-axis) and the average number of steps taken, averaged  
across all weekday days or weekend days (y-axis). The plot should look  
something like the following, which was creating using simulated data:  

![Sample panel plot](figures/sample_plot_1.png)

Your plot will look different from the one above because you will be using the  
activity monitor data. Note that the above plot was made using the lattice  
system but you can make the same version of the plot using any plotting system  
you choose.


```r
inter_by_day_type <-
    tt %>%
    mutate(day_name = weekdays(date),
           day_type = ifelse(day_name == "Saturday" | 
                             day_name == "Sunday","Weekend","Weekday")) %>%
    group_by(interval,day_type) %>%
    summarize(avgsteps_day = mean(steps)) %>%
    ggplot(aes(x=interval,y=avgsteps_day)) +
           geom_line() +
           facet_grid(day_type ~ .) +
           scale_x_continuous(breaks = seq(0, 2400, by = 200)) +
           labs(title="Time Series of Average Steps over Interval", 
                x= "Intra Day Interval")

inter_by_day_type
```

![plot of chunk unnamed-chunk-5](./PA1_template_files/figure-html/unnamed-chunk-5.png) 

```r
png(file = "./figures/rrpa1_ts_day_type.png")

inter_by_day_type <-
    tt %>%
    mutate(day_name = weekdays(date),
           day_type = ifelse(day_name == "Saturday" | 
                             day_name == "Sunday","Weekend","Weekday")) %>%
    group_by(interval,day_type) %>%
    summarize(avgsteps_day = mean(steps)) %>%
    ggplot(aes(x=interval,y=avgsteps_day)) +
           geom_line() +
           facet_grid(day_type ~ .) +
           scale_x_continuous(breaks = seq(0, 2400, by = 200)) +
           labs(title="Time Series of Average Steps over Interval", 
                x= "Intra Day Interval")

inter_by_day_type


dev.off()
```

```
## pdf 
##   2
```
The panel plot does show differences in the time series of steps on weekdays
versus weekends.  
