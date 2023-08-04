# Bellabeat Data Analysis Case Study

## Table of Contents

1. [Introduction](#introduction)
2. [Business Task](#business-task)
3. [The Data](#the-data)
4. [Data Processing and Cleaning](#data-processing-and-cleaning)
5. [Analysis and Visualizations](#analysis-and-visualizations)
6. [Conclusion and Recommendations](#conclusion-and-recommendations)

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

### Number of Participants

In the initial exploration, I determined the number of unique participants in each dataset:

- Daily Activity: 33 participants
- Weight: 8 participants
- Sleep: 24 participants
- Calories: 33 participants
- Intensities: 33 participants

It is important to note that the dataset has some limitations in terms of sample size and demographics, which may affect the generalizability of the insights.

### Trends in Smart Device Usage

#### Total Steps Vs. Calories

To understand the relationship between physical activity and calorie expenditure, I created a scatter plot comparing "TotalSteps" and "Calories" using the `ggplot2` library:

```
ggplot(data = DailyActivity) +
    geom_point(mapping = aes(x = TotalSteps, y = Calories)) +
    geom_smooth(mapping = aes(x = TotalSteps, y = Calories)) +
    labs(title = "TotalSteps Vs. Calories") +
    theme(plot.title = element_text(hjust = 0.5))
```
![Totals Stepvs vs Calories](https://github.com/mkj89/bellabeat-fitness-analysis/blob/main/Viz/Steps%20vs%20Calories.png)

The graph shows a positive trend, indicating that as the total number of steps increases, so does the number of calories burned. However, there are outliers on the graph, suggesting that there are other factors influencing calorie expenditure beyond just the number of steps taken. For instance, activities like swimming or bodybuilding might burn calories without requiring many steps.

#### Total Intensity Vs. Time

To explore the intensity of smart device usage over time, I created a bar chart representing "TotalIntensity" versus "Time" using the `ggplot2` library:

```
ggplot(data = Intensities, aes(x = time, y = TotalIntensity)) +
    geom_bar(stat = "identity", fill = 'blue') +
    theme(axis.text.x = element_text(angle = 90)) +
    labs(title = "Total Intensity vs. Time") +
    theme(plot.title = element_text(hjust = 0.5))
```
![Itensity vs time](https://github.com/mkj89/bellabeat-fitness-analysis/blob/main/Viz/itensity%20vs%20time.png)

The intensity of smart device usage shows a distinct pattern, starting around 5 am and experiencing its first, albeit smaller, peak around noon. After a slight dip at 3 pm, it then exhibits a prominent 3-hour peak from 5 to 7 pm. Subsequently, it undergoes a significant decline until 8 pm and gradually slows down until 11 pm. Finally, it reaches its lowest point at 3 am. This trend suggests that consumers are more physically active during the early morning and afternoon hours, with another surge in the evening, possibly during their lunch breaks or after their typical 9-5 working hours.

#### Time Minutes Asleep Vs. Total Steps

To analyze the relationship between sleep and activity levels, I created a scatter plot comparing "TotalMinutesAsleep" and "SedentaryMinutes" using the `ggplot2` library:

```
ggplot(data = sleep_vs_activity) +
    geom_point(mapping = aes(x = TotalMinutesAsleep, y = SedentaryMinutes)) +
    geom_smooth(mapping = aes(x = TotalMinutesAsleep, y = SedentaryMinutes)) +
    labs(title = "Time Minutes Asleep vs. Sedentary Minutes") +
    theme(plot.title = element_text(hjust = 0.5))
```
![Asleep vs Sedentary](https://github.com/mkj89/bellabeat-fitness-analysis/blob/main/Viz/asleep%20vs%20sedentary.png)

The graph demonstrates a clear negative trend, indicating that individuals who lead a sedentary lifestyle tend to have shorter and less restful sleep, whereas those who engage in more physical activity enjoy longer and more restorative sleep. The data points to a significant difference in sleep duration, with less active individuals sleeping for as little as 50 minutes, while highly active individuals can enjoy up to 13.33 hours of sleep. This observation underscores the importance of physical activity in promoting better sleep quality and overall well-being.


## Conclusion and Recommendations


After a thorough analysis of the 2016 dataset from Fitbit Fitness Tracker, several trends in smart device usage have been identified, providing valuable insights for Bellabeat's marketing strategy. Despite the limitations of the dataset, which covers a one-month period and comprises a small sample size of 30 users, the findings can still offer meaningful implications for the company.

### Trends in Smart Device Usage:
***1. Positive Correlation:*** The graph comparing Total Steps to Calories burned indicates a positive trend, suggesting that as individuals take more steps to achieve their fitness goals, they burn more calories. Although there are outliers, the overall pattern implies a positive connection between physical activity and calorie expenditure.

***2. Usage Patterns:*** The Total Intensity vs. Time graph illustrates two significant peaks around 12 pm (noon) and 5 pm to 7 pm. These patterns indicate that users are more physically active during these periods, possibly during their lunch breaks or after their typical 9-5 working hours.

***3. Sleep and Activity Link:*** The graph comparing Time Minutes Asleep to Total Steps reveals a negative trend, showing that individuals who are less active throughout the day tend to have less restful sleep. Conversely, those who engage in more physical activity tend to have longer periods of sleep. The difference in the range suggests that less active individuals may get as little as 50 minutes of sleep, while highly active individuals may sleep up to 13.33 hours.

### Recommendations for Bellabeat's Marketing Strategy:

***1. Target Audience:*** Based on the analysis, Bellabeat's primary target audience should be health-conscious, career-driven individuals with 9-5 jobs. The increased device usage rate after work hours indicates that these individuals prioritize their health and fitness, making them an ideal market for Bellabeat's smart devices.

***2. Influencer Partnerships:*** To effectively reach the target audience, Bellabeat should consider collaborating with health and fitness influencers on social media. Partnering with influencers will allow Bellabeat to showcase its products to a niche audience that is more likely to make fitness-related purchases.

***3. Enhanced Features:*** Bellabeat's smart devices should be equipped with personalized reminders and recommendations. These features can include reminders for taking breaks, walking, and engaging in breathing exercises. Additionally, providing food recommendations to improve fat burning and enhance sleep quality can further cater to the target audience's health needs.

***4. Spokespersons:*** Utilizing spokespersons who have experience using Bellabeat's smart devices and have achieved success in their fitness journey can be a powerful marketing strategy. Athletes or fitness enthusiasts who can share their positive experiences with the products can inspire and influence potential customers.

***5. Innovation:*** Bellabeat should invest in research and development to introduce innovative technology that can differentiate its smart devices from competitors. Exploring technologies that track brain patterns or immune system activity related to active and sedentary lifestyles could potentially disrupt the market and attract more customers.

***6. Ongoing Research:*** Given the limitations of the dataset, it is essential for Bellabeat to conduct further research and collect more recent data to ensure that its marketing strategies align with current trends and consumer preferences.

By implementing these recommendations and gaining a better understanding of the target audience, Bellabeat can create a more effective and targeted marketing strategy. Ultimately, this will empower individuals to lead healthier and more active lives with the support of Bellabeat's innovative smart devices.
