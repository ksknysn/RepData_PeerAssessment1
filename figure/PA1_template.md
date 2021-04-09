---
title: "PA1_template"
output:
  html_document: default
  pdf_document: default
  word_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```{r part1}
library(ggplot2)
act = read.csv("C:/Users/keskiny/Desktop/coursera/activity/activity.csv")
totalstepsperday<-aggregate(steps~date, data=act, FUN= sum, na.rm=TRUE)
hist(totalstepsperday$steps, breaks=dim(totalstepsperday)[1], 
     main="Total steps per day",
     xlab="Number of steps per day",
     ylab="Interval",
     col="orange")


mean(totalstepsperday$steps)
median(totalstepsperday$steps)
##---
fivemin<-aggregate(steps~interval, data=act, FUN=mean, na.rm=TRUE)
plot(x=fivemin$interval, y=fivemin$steps, type="l",col="orange", xlab="5-minute intervals", ylab="Average steps taken ~ Days", main="Average Daily Activity Pattern")
fivemin$interval[which.max(fivemin$steps)]
##--
act2<-act
nas<-is.na(act2$steps)
avg_interval<-tapply(act2$steps, act2$interval, mean, na.rm=TRUE, simplify = TRUE)
act2$steps[nas]<-avg_interval[as.character(act2$interval[nas])]
##--
par(mfrow=c(1,2))
totalstepsperday2 <- aggregate(steps~date, data=act2, FUN =sum, na.rm=TRUE)
##--
act2$date<-as.Date(act$date, "%Y-%m-%d")
act2$wday<- ifelse(weekdays(act2$date)=="Saturday" | weekdays(act2$date)=="Sunday", "Weekend", "Weekday")
fivemin2<- aggregate(steps~interval, data=act2, FUN=mean, na.rm=TRUE)
ggplot(act2, aes(x =interval , y=steps, color=wday)) +
  geom_line() +
  labs(title = "Ave Daily Steps (type of day)", x = "Interval", y = "Total Number of Steps") +
  facet_wrap(~ wday, ncol = 1, nrow=2)

```



## Including Plots

You can also embed plots, for example:


Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

