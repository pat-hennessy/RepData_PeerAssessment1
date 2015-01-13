# Reproducible Research: Peer Assessment 1
<br/>
<br/>


<br/>
<br/>

## Loading and preprocessing the data


```r
data <- read.csv("activity/activity.csv")

stepsPerDay <- data %>%
               group_by(date = as.Date(date)) %>%
               summarise(steps = sum(steps))
```
<br/>

## What is the mean total number of steps taken per day?
<br/>

#### Part 1
Below is a histogram showing the frequency of occurrence of counts of the total steps taken per day.  This is in accordance with David Cook's (Community TA) [clarification of this part of the assignment](https://class.coursera.org/repdata-010/forum/thread?thread_id=9).


```r
g <- ggplot(stepsPerDay, aes(x = steps)) +
    geom_histogram(binwidth = 500) +
    xlab("Steps Per Day")

print(g)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 


#### _Part 1a_
_Below is a bar chart of the total steps taken each day.  This is an additional view, not asked for in the assignment, but supplied here in case graders aren't familiar with Davis Cook's explanation cited above.  This might be construed as answering the question posed in the assignment._


```r
g <- ggplot(stepsPerDay, aes(x = date, y = steps)) +
    geom_bar(stat = "identity") +
    scale_x_date(breaks = date_breaks(width = "1 week")) +
    theme(axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5))

print(g)
```

```
## Warning: Removed 8 rows containing missing values (position_stack).
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 
<br/>

#### Part 2a
Below is calculated the mean total number of steps taken per day.


```r
meanStepsPerDay <- mean(stepsPerDay$steps, na.rm = TRUE)
print(meanStepsPerDay)
```

```
## [1] 10766.19
```
<br/>

#### Part 2b
Below is calculated the median total number of steps taken per day.

```r
medianStepsPerDay <- median(stepsPerDay$steps, na.rm = TRUE)
print(medianStepsPerDay)
```

```
## [1] 10765
```
<br/>
<br/>

## What is the average daily activity pattern?
<br/>

#### Part 1
Below is a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis).


```r
data$interval <- as.POSIXct(paste(data$interval %/% 100, data$interval %% 100), format = "%H %M")

meanStepsPerInterval <- data %>%
    group_by(interval) %>%
    summarise(intervalMean = mean(steps, na.rm = TRUE))

g <- ggplot(meanStepsPerInterval, aes(interval, intervalMean)) +
    geom_line() +
    scale_x_datetime(labels = date_format("%H:%M"), breaks = "2 hour") +
    ylab("Mean Number of Steps") +
    xlab("Interval")
    

print(g)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 
<br/>

#### Part 2
Below is calculated the 5-minute interval which, on average across all the days in the dataset, contains the maximum number of steps.


```r
max(meanStepsPerInterval$intervalMean)
```

```
## [1] 206.1698
```
<br/>
<br/>

## Imputing missing values
<br/>
<br/>

## Are there differences in activity patterns between weekdays and weekends?
