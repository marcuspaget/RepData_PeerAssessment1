---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

- Check if datafile exists, if not then where zip is available unzip
- As activity.zip is the git hub source - not downloading it
- Create sum by date and store in total


```r
datafile<-"activity.csv"
zipfile<-"activity.zip"


if(!(file.exists(datafile) && file.exists(zipfile))) {

  unzip(zipfile, exdir='.')

}
x<-read.csv("activity.csv")

total<-aggregate(steps ~ date,data=x,sum)
```


## What is mean total number of steps taken per day?
- Histogram of total number of steps
- Mean and Median by day
- Mean and Median total data



```r
hist(total$steps,main="Steps totalled by day",xlab="Steps")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 



```r
means<-aggregate(steps ~ date, subset(x,steps > 0, select = c(steps,date)), mean)
plot(means,main="Average steps per day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

```r
medians<-aggregate(steps ~ date, subset(x,steps > 0, select = c(steps,date)), median)
plot(medians,main="Median steps per day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-2.png) 

```r
print(paste("Mean for all days:",format(mean(total$steps),digits=1)))
```

```
## [1] "Mean for all days: 10766"
```

```r
print(paste("Median for all days:",format(median(total$steps),digits=1)))
```

```
## [1] "Median for all days: 10765"
```


## What is the average daily activity pattern?


```r
aves<-aggregate(steps ~ interval, data=x, mean)
plot(steps ~ interval, data=aves,type="l")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

## Inputing missing values
1. Calculate and report number of missing values for steps in the dataset


```r
print(paste("Number of missing values in dataset:",dim(x[is.na(x$steps),])[[1]]))
```

```
## [1] "Number of missing values in dataset: 2304"
```
2. Strategy to fill them in - replace with mean for interval

```r
nr=nrow(x)
y<-x[0,]

for(i in 1:nr) {

  thisstep<-x[i,"steps"]
  thisdate<-x[i,"date"]
  thisint<-x[i,"interval"]
  
  if(is.na(x[i,"steps"])) {
    thisstep<-aves[aves$interval==thisint,"steps"]
  }
  lst<-list(steps=thisstep,date=thisdate,interval=thisint)
  y<-rbind(y,as.data.frame(lst))
}

avey<-aggregate(steps ~ interval, data=y, mean)
plot(steps ~ interval, data=avey,type="l")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 
## Are there differences in activity patterns between weekdays and weekends?

Ran out of time on this one.


