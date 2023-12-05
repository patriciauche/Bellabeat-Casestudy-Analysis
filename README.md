# Bellabeat_casestudy
A case study analysis demonstrating the 6 steps of the data analysis process : Ask, Prepare, Process, Analyze, Share and Act, as taught in the Google data analytics course.

## Introduction
I'm a junior data analyst on Bellabeat's marketing analysis team, focusing on the Bellabeat app, a high-tech health product for women. My task is to analyze smart device fitness data to understand customer usage patterns. The insights I uncover will inform Bellabeat's marketing strategy, and I'll present my findings and recommendations to the executive team.

## Project Overview
The objective of this data analysis project is to uncover trends and usage patterns among smart device users and understand their relevance to Bellabeat customers. The analysis follows the six-step data analysis process: Ask, Prepare, Process, Analyze, Share, and Act, as instructed in the Google Data Analytics course.

## Data Sources 
The dataset used is FitBit Fitness Tracker Data. It was made available by Mobius on Kaggle under CC0: Public Domain Creative Common License. This dataset can be found [here](https://www.kaggle.com/arashnic/fitbit)


## STEP 1: ASK
Bellabeat is a leading high-tech manufacturer specialising in health-focused products for women, utilising advanced technology to gather comprehensive data on activity, sleep, stress, and reproductive health. 

My goal is to conduct an in-depth analysis of data from FitBit smart device users, extracting valuable insights and identifying trends, patterns, and correlations within various health-related metrics and how they relate to bellabeat customers. This analysis aims to pinpoint growth opportunities and provide the Marketing department with recommendations and strategies for effective marketing initiatives.


My primary stakeholders are; 
Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer and;
Sando Mur: Mathematician and Bellabeat’s cofounder


## STEP 2: PREPARE
About the dataset:The dataset used is FitBit Fitness Tracker Data. It was made available by Mobius on Kaggle under CC0: Public Domain Creative Common License. 
This dataset encompasses information from 33 Fitbit users who willingly shared personal tracker data, providing minute-level details on physical activity, heart rate, and sleep monitoring, offering a comprehensive snapshot of users' behaviors. Comprising eighteen downloadable files in .csv format, this analysis focuses on 7 out of the provided 18 csv files.

The dataset is open-source, with the owner dedicating their work to the public and relinquishing all rights globally under copyright law. The data is organized in a long format, with each subject's data spanning multiple rows. Each user is assigned a unique ID, and data is recorded by day and time.

It's important to note that this dataset may have inherent biases due to its small sample size of 33 users, making it uncertain whether this sample is truly representative of the entire population. Collected in 2016, the data is considered outdated, and it's plausible that users' habits and patterns have evolved since then.


## STEP 3: PROCESS
I will be using R to clean, process, analyze and visualize my data. 
#### Cleaning the data
After logging into Rstudio, I installed and loaded the following packages that I needed to process and clean the data using ‘install.packages()’.
#### Installing the Required Packages.
```{r}
install.packages("tidyverse")
install.packages("janitor")
install.packages("lubridate")
install.packages("skimr")
install.packages("dplyr")
```
#### Loading the installed packages.
```{r}
library(tidyverse)
library(janitor)
library(lubridate)
library(skimr)
library(dplyr)
```
After installing and loading the packages, I imported the following datasets into Rstudio using ‘read_csv()’;
#### Importing the Datasets.
I imported the datasets into Rstudio using the 'read_csv()' function. 
```{r}
daily_activity <- read.csv("Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv", header = TRUE)
daily_steps <- read.csv("Fitabase Data 4.12.16-5.12.16/dailySteps_merged.csv", header = TRUE)
daily_sleep <- read.csv("Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv", header = TRUE)
hourly_steps <- read.csv("Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv", header = TRUE)
hourly_calories <- read.csv("Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv", header = TRUE)
hourly_intensities <- read.csv("Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv", header = TRUE)
weight <- read.csv("Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv", header = TRUE)
```
#### Viewing the datasets.
I used the 'head()' function to look at  the different datasets to see the columns and the first 6 rows of each.
```{r}
head(daily_activity)
head(daily_sleep)
head(daily_steps)
head(hourly_calories)
head(hourly_intensities)
head(hourly_steps)
head(weight)
```
#### View the summary of the datasets.
I used the 'str()'function to display the summary and the structure of the datasets i imported.
```{r}
str(daily_activity)
str(daily_sleep)
str(daily_steps)
str(hourly_calories)
str(hourly_intensities)
str(hourly_steps)
str(weight)
```
#### Finding the number of unique participants for each dataset. 
I used the 'n_distinct()' function to find the number of unique participants in each dataset. 
```{r}
n_distinct(daily_activity$Id)
n_distinct(daily_sleep$Id)
n_distinct(daily_steps$Id)
n_distinct(hourly_calories$Id)
n_distinct(hourly_intensities$Id)
n_distinct(hourly_steps$Id)
n_distinct(weight$Id)
```
All the datasets had 33 unique participants except the ‘daily_sleep’ and ‘weight’ dataset which had 24 and 8 participants respectively.  I made the decision to exclude the ‘weight’ dataset because its sample size of 8 unique participants was too small to draw conclusions and make recommendations and I retained the rest of the datasets for my analysis. 
#### Checking for duplicates. 
 I used the ‘duplicated()’ function to check for duplicates in my dataset. 
```{r}
sum(duplicated(daily_activity))
sum(duplicated(daily_sleep))
sum(duplicated(daily_steps))
sum(duplicated(hourly_calories))
sum(duplicated(hourly_intensities))
sum(duplicated(hourly_steps))
```
#### Cleaning duplicates in the 'daily_sleep' dataset.
I discovered that it was only the ‘daily_sleep' dataset that had 3 duplicate values. I removed the duplicate values using the following code;
```{r}
daily_sleep <- unique(daily_sleep)
sum(duplicated(daily_sleep))
```
#### Cleaning the column names. 
I cleaned the column names by removing all capital letters using the clean_names() and rename_with() functions. 
```{r} 
clean_names(daily_activity)
daily_activity <- rename_with(daily_activity, tolower)
clean_names(daily_sleep)
daily_sleep <- rename_with(daily_sleep, tolower)
clean_names(daily_steps)
daily_steps <- rename_with(daily_steps, tolower)
clean_names(hourly_calories)
hourly_calories <- rename_with(hourly_calories, tolower)
clean_names(hourly_intensities)
hourly_intensities <- rename_with(hourly_intensities, tolower)
clean_names(hourly_steps)
hourly_steps <- rename_with(hourly_steps, tolower)
```
#### Cleaning and converting the date formats. 
I cleaned the date formats in the datasets to make them consistent. In the  ‘daily_activity’ and ‘daily_sleep’ datasets, I changed the date format and in ‘hourly_calories’,’hourly_intensities’ and ‘hourly_steps’ datasets, i converted the date from date-string to date-time format.
```{r}
daily_activity <- daily_activity %>%
  rename(date = activitydate) %>%
  mutate(date = as_date(date, format = "%m/%d/%Y"))
daily_sleep <- daily_sleep %>%
  rename(date = sleepday) %>%
  mutate(date = as_date(date, format ="%m/%d/%Y %I:%M:%S %p", tz = Sys.timezone()))
hourly_calories <- hourly_calories %>%
  rename(date_time = activityhour) %>%
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
hourly_intensities <- hourly_intensities %>%
  rename(date_time = activityhour) %>%
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
hourly_steps<- hourly_steps %>%
  rename(date_time = activityhour) %>%
  mutate(date_time = as.POSIXct(date_time, format ="%m/%d/%Y %I:%M:%S %p" , tz=Sys.timezone()))
```
#### Merging three datasets hourly_intensities, hourly_steps and hourly_calories. 
I decided to merge the three datasets containing time-related information into a single dataset using the “id” and “date_time” columns using the 'merge()' function. 
```{r}
hourly_activity <- merge(hourly_intensities,hourly_calories,by=c("id","date_time"))
hourly_activity <- merge(hourly_activity,hourly_steps,by=c("id","date_time"))
View(hourly_activity)
```

## STEP 4 & 5: ANALYZE AND SHARE
## Viewing the summary of the 'daily_activity' dataset
I viewed the summary of the ‘daily_activity’ dataset using; 
```{r}
daily_activity %>%  
  select(totalsteps,
         totaldistance,
         sedentaryminutes, calories) %>%
  summary()
```
The summary yielded the following insights:
- The mean total daily steps amount to 7,638, slightly exceeding the recommended daily goal of 7,500 steps. 
- Sedentary minutes on an average is 991(~17 hours).
- The majority of participants fall into the category of light users. 
- On average, participants sleep for 419 minutes (~7 hours).
- The average calorie burn rate is 97 calories per hour.

### Visualization 1: Comparing total steps taken with calories burned.
I plotted the line graph using the following code;
```{r}
ggplot(data = daily_activity, aes(x = totalsteps, y = calories)) + 
  geom_smooth(color = "black")+ labs(title ="Steps vs. Calories")
```





