---
title: "Bellabeat_Case_Study"
author: "Barbara Barton"
date: "2025-11-07"
output:
  md_document:
    variant: markdown_github
  ---


## Introduction

The case study follows the six phases of the data analysis process:

![](images/ASK.png)

![](images/prepare.png)

![](images/process.png)

![](images/analyze.png)

![](images/share.png)
 
![](images/act.png)

![](images/Bellabeat2.png) 

a high-tech company that manufactures health-focused smart products that are beautifully designed technology that informs and inspires women around the world. Bellabeat collects data on activity, sleep, stress, and reproductive health with goal of empowering women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. They have task the market analytics team to analyze smart device data to gain insight into how consumers are using their smart devices and make high-level recommendations that will allow Bellabeat to make data driven decision that will improve their marketing strategy and potentially increase their share in the global market.

![](images/ASK.png)

**Business Task**

To analyze Fitbit customers smart devices data usage and make high-level recommendations to improve Bellabeat marketing strategy.

Stakeholders
•	Urška Sršen:Bellabeat’s cofounder and Chief Creative Officer 
•	Sando Mur:Mathematician and Bellabeat’s cofounder  
•	Bellabeat marketing analytics team, data analyst team  

![](images/prepare.png)

Import datasets, become familiar with the data, check for errors, check the credibility of the data and transform the data to make more complete analysis.


``` r
daily_activity <- read.csv("daily_activity_04-12-2016.csv")
daily_sleep <- read.csv("sleep_day_04-12-2016.csv")
daily_calories <- read.csv("daily_calories_04-12-2016.csv")
daily_steps <- read.csv("daily_steps_04-12-2016.csv")
weight_info <- read.csv("weight_loginfo_04-12-2016.csv")
```


``` r
activity_sleep <- merge(daily_activity, daily_sleep, by="Id")
```


``` r
colnames(activity_sleep)
```

```
##  [1] "Id"                       "ActivityDate"             "DayofWeek.x"              "TotalSteps"               "TotalDistance"           
##  [6] "TrackerDistance"          "LoggedActivitiesDistance" "VeryActiveDistance"       "ModeratelyActiveDistance" "LightActiveDistance"     
## [11] "SedentaryActiveDistance"  "VeryActiveMinutes"        "FairlyActiveMinutes"      "LightlyActiveMinutes"     "SedentaryMinutes"        
## [16] "Calories"                 "SleepDay"                 "DayofWeek.y"              "TotalSleepRecords"        "TotalMinutesAsleep"      
## [21] "TotalTimeInBed"
```

``` r
str(activity_sleep)
```

```
## 'data.frame':	12441 obs. of  21 variables:
##  $ Id                      : num  1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
##  $ ActivityDate            : chr  "5/7/2016" "5/7/2016" "5/7/2016" "5/7/2016" ...
##  $ DayofWeek.x             : chr  "Saturday" "Saturday" "Saturday" "Saturday" ...
##  $ TotalSteps              : int  11992 11992 11992 11992 11992 11992 11992 11992 11992 11992 ...
##  $ TotalDistance           : num  7.71 7.71 7.71 7.71 7.71 ...
##  $ TrackerDistance         : num  7.71 7.71 7.71 7.71 7.71 ...
##  $ LoggedActivitiesDistance: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ VeryActiveDistance      : num  2.46 2.46 2.46 2.46 2.46 ...
##  $ ModeratelyActiveDistance: num  2.12 2.12 2.12 2.12 2.12 ...
##  $ LightActiveDistance     : num  3.13 3.13 3.13 3.13 3.13 ...
##  $ SedentaryActiveDistance : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ VeryActiveMinutes       : int  37 37 37 37 37 37 37 37 37 37 ...
##  $ FairlyActiveMinutes     : int  46 46 46 46 46 46 46 46 46 46 ...
##  $ LightlyActiveMinutes    : int  175 175 175 175 175 175 175 175 175 175 ...
##  $ SedentaryMinutes        : int  833 833 833 833 833 833 833 833 833 833 ...
##  $ Calories                : int  1821 1821 1821 1821 1821 1821 1821 1821 1821 1821 ...
##  $ SleepDay                : chr  "4/12/2016 0:00" "4/13/2016 0:00" "4/15/2016 0:00" "4/16/2016 0:00" ...
##  $ DayofWeek.y             : chr  "Tuesday" "Wednesday" "Friday" "Saturday" ...
##  $ TotalSleepRecords       : int  1 2 1 2 1 1 1 1 1 1 ...
##  $ TotalMinutesAsleep      : int  327 384 412 340 700 304 360 325 361 430 ...
##  $ TotalTimeInBed          : int  346 407 442 367 712 320 377 364 384 449 ...
```

**Does data ROCCC?**
The Bellabeat case study uses FitBit Fitness Tracker Data (CC0: Public Domain, dataset made available through Mobius). This Kaggle data set contains personal fitness tracker from thirty Fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits.
The data was uploaded and stored in R Studio, sorted, and checked for credibility.
*Reliable: data collected from 30 Fitbit users who consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring.
*Original: data collected from 30 eligible Fitbit users who consented to the submission of personal tracker data 
*Comprehensive: health and fitness minute-level data is available for measures of physical activity, heart rate, and sleep monitoring but the data was collected from only 30 eligible Fitbit users who responded to a survey 
*Current: available data is from 03-12.2016 to 05.12.2016. This data is not current and so the ways Fitbit user interact with their device and available Fitbit features may have changed
*Cited: Unknown

**Data Limitations**

Data was collected from only 30 Fitbit users. This small sample may not be representative of the Fitbit population thereby hindering the generalizability of the findings. The short window of collection is also a limitation. People might use their devices differently during different times of the year. 

## Process


``` r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ────────────────────────────────────────────────────────────────────────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.2
## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
## ✔ purrr     1.1.0     
## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library("lubridate")
library("dplyr")
library("here")
```

```
## here() starts at /cloud/project
```

``` r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

``` r
library("skimr")
library("tidyr")
library("ggplot2")
```

Transform data for more comprehensive analysis


``` r
activity_sleep <- merge(daily_activity, daily_sleep, by="Id")
```


``` r
activity_sleep$DayofWeek.y <- factor(activity_sleep$DayofWeek.y, 
    levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"),
         ordered = TRUE)

activity_sleep_sorted <- activity_sleep[order(activity_sleep$DayofWeek.y),]
```


``` r
daily_steps$DayofWeek <- factor(daily_steps$DayofWeek, 
      levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"),
          ordered = TRUE)

daily_steps_sorted <- daily_steps[order(daily_steps$DayofWeek),]
```
 

``` r
daily_calories$DayofWeek <- factor(daily_calories$DayofWeek, 
      levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"),
           ordered = TRUE)

daily_calories_sorted <- daily_calories[order(daily_calories$DayofWeek),]
```


``` r
daily_sleep$DayofWeek <- factor(daily_sleep$DayofWeek, 
      levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"),
          ordered = TRUE)

daily_sleep_sorted <- daily_sleep[order(daily_sleep$DayofWeek),]
```

Overview of the datasets
*The data description stated that the dataset included 30 eligible Fibit users   but a check using n_distinct () showed that the datasets for activity, calories, and steps included 33 Fitbit users, sleep included 24 users and weight, 8 users. 


``` r
n_distinct(daily_activity$Id)
```

```
## [1] 33
```

``` r
n_distinct(activity_sleep_sorted$Id)
```

```
## [1] 24
```

``` r
n_distinct(daily_sleep$Id)
```

```
## [1] 24
```

``` r
n_distinct(daily_calories$Id)
```

```
## [1] 33
```

``` r
n_distinct(weight_info$Id)
```

```
## [1] 8
```

``` r
n_distinct(daily_steps$Id)
```

```
## [1] 33
```

![](images/analyze.png)

**Summary**

The min, max, and mean were as follows:
*The  average steps was determined to be 8,117 and the maximum was 22,988. 
*Average calories burned was 2,329 and the maximum was 4,900.
*The average minutes  asleep was 419 and the max 796.


``` r
activity_sleep_sorted %>%
   select(TotalSteps,
          SedentaryMinutes,
          VeryActiveMinutes,
          FairlyActiveMinutes,
          LightlyActiveMinutes,
          Calories,
          TotalMinutesAsleep,
          TotalTimeInBed) %>%
   summary()
```

```
##    TotalSteps    SedentaryMinutes VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes    Calories    TotalMinutesAsleep TotalTimeInBed 
##  Min.   :    0   Min.   :   0.0   Min.   :  0.00    Min.   :  0.00      Min.   :  0.0        Min.   :   0   Min.   : 58.0      Min.   : 61.0  
##  1st Qu.: 4660   1st Qu.: 659.0   1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:144.0        1st Qu.:1783   1st Qu.:361.0      1st Qu.:402.0  
##  Median : 8596   Median : 734.0   Median :  8.00    Median : 10.00      Median :200.0        Median :2162   Median :432.0      Median :463.0  
##  Mean   : 8117   Mean   : 799.2   Mean   : 23.97    Mean   : 17.35      Mean   :199.9        Mean   :2329   Mean   :419.4      Mean   :458.4  
##  3rd Qu.:11317   3rd Qu.: 853.0   3rd Qu.: 36.00    3rd Qu.: 24.00      3rd Qu.:258.0        3rd Qu.:2865   3rd Qu.:492.0      3rd Qu.:526.0  
##  Max.   :22988   Max.   :1440.0   Max.   :210.00    Max.   :143.00      Max.   :518.0        Max.   :4900   Max.   :796.0      Max.   :961.0
```


``` r
avg_daily_steps <- 
  daily_steps_sorted %>%
  group_by(DayofWeek) %>%
  summarize(
    avg_steps = mean(StepTotal, na.rm = TRUE))
```


``` r
avg_daily_calories <- 
  daily_calories_sorted %>%
  group_by(DayofWeek) %>%
  summarize(
    avg_calories = mean(Calories, na.rm = TRUE))

print(avg_daily_calories)
```

```
## # A tibble: 7 × 2
##   DayofWeek avg_calories
##   <ord>            <dbl>
## 1 Sunday           2263 
## 2 Monday           2324.
## 3 Tuesday          2356.
## 4 Wednesday        2303.
## 5 Thursday         2200.
## 6 Friday           2332.
## 7 Saturday         2355.
```


``` r
avg_daily_sleep <- 
  daily_sleep_sorted %>%
  group_by(DayofWeek) %>%
  summarize(
    avg_sleep = mean(TotalMinutesAsleep, na.rm = TRUE))

print(avg_daily_sleep)
```

```
## # A tibble: 7 × 2
##   DayofWeek avg_sleep
##   <ord>         <dbl>
## 1 Sunday         453.
## 2 Monday         419.
## 3 Tuesday        405.
## 4 Wednesday      435.
## 5 Thursday       402.
## 6 Friday         405.
## 7 Saturday       421.
```

After summarizing the data, the relationship between day of the week and steps, calories and sleep were highlighted using bar graphs. From April 12th - May 12th, Fitbit users reported the most steps on Saturdays and Tuesdays and least steps on Sundays and Thursdays.
Users reported burning most calories on Tuesdays and the fewest on Thursdays. The most minutes of sleep were reported on Sundays and the fewest on Thursdays.


``` r
ggplot(data = avg_daily_steps, aes(x=DayofWeek, y= avg_steps, fill = DayofWeek)) +
   geom_bar(stat = "identity") +
   labs(title = "Average Daily Steps by Day of the Week", subtitle = "April 12 - May 12, 2016")
```

![plot of chunk plotting steps by Day of Week](figure/plotting steps by Day of Week-1.png)


``` r
ggplot(data = avg_daily_calories, aes(x=DayofWeek, y= avg_calories, fill = DayofWeek)) +
   geom_bar(stat = "identity") +
   labs(title = "Average Calories by Day of the Week", subtitle = "April 12 - May 12, 2016")
```

![plot of chunk plotting calories burned by day of week](figure/plotting calories burned by day of week-1.png)


``` r
ggplot(data = avg_daily_sleep, aes(x=DayofWeek, y= avg_sleep, fill = DayofWeek)) +
   geom_bar(stat = "identity") +
   labs(title = "Average Minutes Asleep by Day of the Week", subtitle = "April 12 - May 12, 2016")
```

![plot of chunk plotting minutes of sleep by day of week](figure/plotting minutes of sleep by day of week-1.png)


``` r
ggplot(data = activity_sleep_sorted) + 
     geom_jitter(mapping = aes(x = TotalMinutesAsleep, y = TotalTimeInBed, color = "orange")) +
   labs(title = "Time in Bed vs Minutes of Sleep", subtitle = "April 12 - May 12, 2016")
```

![plot of chunk plotting relationship between minutes of sleep and time in bed](figure/plotting relationship between minutes of sleep and time in bed-1.png)


``` r
ggplot(data = activity_sleep_sorted) +
  geom_smooth(mapping = aes(x=LightlyActiveMinutes, y = Calories)) +
  geom_jitter(mapping = aes(x=LightlyActiveMinutes, y=Calories), color = "orange") +
  labs(title = "Lightly Active Minutes and Calories Burned", subtitle = "April 12 - May 12, 2016")
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

![plot of chunk plotting relationship between active minutes and calories](figure/plotting relationship between active minutes and calories-1.png)


``` r
ggplot(data = activity_sleep_sorted) +
   geom_point(mapping = aes(x=VeryActiveMinutes, y=TotalMinutesAsleep), color ="orange") + 
   labs(title = "Very Active Minutes and Total Minutes Asleep", subtitle = "April 12 - May 12, 2016")
```

![plot of chunk plotting the relationship between active minutes and sleep](figure/plotting the relationship between active minutes and sleep-1.png)

![](images/share.png)

![](images/Dashboard .png) 

![](images/act.png) 

**Insight**
*Data showed that only 3% of activity minutes reported by Fitbit users were very or fairly active minutes while over 80% of minutes reported were sedentary. Sedentary minutes are minutes with little to no movement including time being seated, lying down, or any other activity that doesn't involve significant movement or increased heart rate (source: Delobelle J, Lebuf E, Dyck DV, et al. Fitbit’s accuracy to measure short bouts of stepping and sedentary behaviour: validation, sensitivity and specificity study. DIGITAL HEALTH. 2024;10. doi:10.1177/20552076241262710. 
*Participants reported the most steps and  calories burned Tuesdays and Saturdays and the least on Thursdays and Sundays.
*Participants reported the highest average number of minutes asleep on Sundays and Wednesdays.
*Data showed a strong correlation between time in bed and total minutes asleep.
*Data did not show any correlation between sleep and activity.

**Recommendations**
In order to help Bellabeat make data driven decisions that could launch them as global players in health and fitness smart device market more data is needed. A larger population sample as well as more demographic information about participants would be helpful to the marketing team. Collect data on the goals of users and use this data to develop strategies to motivate them to be less sedentary. 
Based on the available data, I recommend helping to motivate users by sending applauds when they are doing well and when they not doing as well, reminders that even light activity can be beneficial. Provide a support system for users by creating community/group activities. Provide holistic health analysis (heart health, sleep, diet, and stress analyses) that help users get better sleep, reduce their stress and be more active participants in maintaining their health.
