# Bellabeat Data Analysis Case Study

## Table of Contents

1. [Introduction](#introduction)
2. [Business Task](#business-task)
3. [The Data](#the-data)
4. [Data Processing and Cleaning](#data-processing-and-cleaning)
5. [Analysis and Insights](#analysis-and-vizualization)
6. [Conclusion](#conclusion-and-recommendation)

## Introduction

This case study delves into the world of Bellabeat, a high-tech manufacturer of health-focused products for women. As a Junior Data Analyst on the team, my task is to analyze smart device usage data for Bellabeat. By exploring and understanding customer interactions with smart devices, I aim to uncover valuable insights that will drive Bellabeat's marketing strategy and propel the company towards greater success in the global smart device market.
## Business Task

The business task involves analyzing smart device usage data and answering the following questions:

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

## The Data

The data used for this case study is sourced from the Fitbit Fitness Tracker Data, a publicly available dataset on Kaggle. It contains personal fitness tracker information from 30 FitBit users who provided their consent for data submission, including minute-level output for physical activity, heart rate, and sleep monitoring. The dataset consists of 18 CSV files, which have been downloaded and stored in R Studio for analysis.

**Limitations**

+ Sample size: With only 30 users, the dataset may not be fully representative of all FitBit users.
+ Outdated: The data covers a one-month period in 2016, which may not reflect current trends and user habits.
+ Limited demographics: The dataset lacks information such as gender, age, and location, which could be useful for targeted marketing strategies.

Despite these limitations, the dataset offers valuable insights into user habits and will serve as the foundation for the analysis in this case study.

## Data Processing and Cleaning

As a Junior Data Analyst at Bellabeat, I performed the following data processing and cleaning steps to prepare the Fitbit smart device usage data for analysis:

### Loading Packages
I began by installing and loading the necessary R packages for data processing and visualization:
```
# Install packages
install.packages("tidyverse")
install.packages("tidyr")
install.packages("dplyr")
install.packages("lubridate")
install.packages("ggplot2")

# Load the libraries into the current R session
library(tidyverse)
library(tidyr)
library(dplyr)
library(lubridate)
library(ggplot2)
````
### Data Import
Next, I imported the CSV files into R for analysis:
```
# Data Import
DailyActivity <- read.csv("dailyActivity_merged.csv")
Calories <- read.csv("hourlyCalories_merged.csv")
Intensities <- read.csv("hourlyIntensities_merged.csv")
Sleep <- read.csv("sleepDay_merged.csv")
Weight <- read.csv("weightLogInfo_merged.csv")
````
### Data Cleaning
I then performed data cleaning to ensure the data was consistent and ready for analysis. I handled missing values and transformed date and time columns using the lubridate package:
```
# Data Cleaning
# Intensities
Intensities$ActivityHour <- parse_date_time(Intensities$ActivityHour, "%m/%d/%Y %I:%M:%S %p")
Intensities$date <- as.Date(Intensities$ActivityHour, format = "%d/%m/%Y")
Intensities$time <- format(as.POSIXct(Intensities$ActivityHour), format = "%H:%M:%S")

# Weight
Weight$Date <- parse_date_time(Weight$Date, "%m/%d/%Y %I:%M:%S %p")
Weight$date <- as.Date(Weight$Date, format = "%d/%m/%Y")
Weight$time <- format(as.POSIXct(Weight$Date), format = "%H:%M:%S")

# Sleep
Sleep$SleepDay <- parse_date_time(Sleep$SleepDay, "%m/%d/%Y %I:%M:%S %p")
Sleep$date <- as.Date(Sleep$SleepDay, format = "%d/%m/%Y")
Sleep$time <- format(as.POSIXct(Sleep$SleepDay), format = "%H:%M:%S")

# Calories
Calories$ActivityHour <- parse_date_time(Calories$ActivityHour, "%m/%d/%Y %I:%M:%S")
Calories$date <- as.Date(Calories$ActivityHour, format = "%d/%m/%Y")
Calories$time <- format(as.POSIXct(Calories$ActivityHour), format = "%H:%M:%S")
````
### Data Exploration
To gain an overview of the data structure, I used the View() and glimpse() functions:
```
# View the datasets
View(DailyActivity)
View(Calories)
View(Intensities)
View(Sleep)
View(Weight)

# Glimpse the datasets
glimpse(DailyActivity)
glimpse(Calories)
glimpse(Intensities)
glimpse(Sleep)
glimpse(Weight)
```
### Data Summary
To get an understanding of the dataset's key statistics, I generated summary statistics for relevant variables:
```
# Summary statistics
# Daily Activity
summary(DailyActivity$TotalSteps)
summary(DailyActivity$TotalDistance)
summary(DailyActivity$SedentaryMinutes)
summary(DailyActivity$Calories)

# Calories
summary(Calories$Calories)

# Sleep
summary(Sleep$TotalMinutesAsleep)
summary(Sleep$TotalTimeInBed)

# Weight
summary(Weight$WeightPounds)
summary(Weight$BMI)
```
### Data Merging and Viz
Lastly, I merged relevant datasets and created visualizations to gain insights into smart device usage:
```
# Merging two data sets
DailyActivity$date <- as.Date(DailyActivity$ActivityDate, format = "%m/%d/%Y")
sleep_vs_activity <- merge(Sleep, DailyActivity, by = c("Id", "date"))

# Total Steps Vs. Calories
ggplot(data = DailyActivity) +
  geom_point(mapping = aes(x = TotalSteps, y = Calories)) +
  geom_smooth(mapping = aes(x = TotalSteps, y = Calories)) +
  labs(title = "TotalSteps Vs. Calories") +
  theme(plot.title = element_text(hjust = 0.5))

# Total Intensity Vs. Time
ggplot(data = Intensities, aes(x = time, y = TotalIntensity)) +
  geom_bar(stat = "identity", fill = 'blue') +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title = "Total Intensity vs. Time") +
  theme(plot.title = element_text(hjust = 0.5))

# Time Minutes Asleep Vs. Total Steps
ggplot(data = sleep_vs_activity) +
  geom_point(mapping = aes(x = TotalMinutesAsleep, y = SedentaryMinutes)) +
  geom_smooth(mapping = aes(x = TotalMinutesAsleep, y = SedentaryMinutes)) +
  labs(title = "Time Minutes Asleep vs. Sedentary Minutes") +
  theme(plot.title = element_text(hjust = 0.5))
```

## Analysis and Visualizations

In this section, I will present the results of my analysis and the insights gained from the smart device usage data. I will highlight trends and patterns that could be relevant to Bellabeat customers and their usage of smart devices.


## Conclusion and Recommendations

In the conclusion, I will summarize the key findings from the analysis and reiterate the recommendations for Bellabeat's marketing strategy based on the smart device usage data.

