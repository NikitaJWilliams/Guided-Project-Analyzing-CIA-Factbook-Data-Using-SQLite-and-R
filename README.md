# Guided Project:Analyzing CIA Factbook Data Using SQLite and R
## Introduction
In this project, I analyze the data from the CIA World Factbook -- a collection of statistics on the history, people, government, economy, geography, communications, transportation, military and transnational issues of all the countries around the globe -- using SQLite and R. In particular, I construct histograms and run summary statistics on selected population variables to illustrate basic features of the sample demographics data. 

| Variable | Description |
| --- | --- | 
| name | The name of the country |
| area | The total land and sea area of the country |
| population | The country's population |
| population_growth | The country's population growth as a percentage |
| birth_rate | The country's birth rate, or the number of births a year per 1,000 people |
| death_rate | The country's death rate, or the number of death a year per 1,000 people |
| area | The country's total area (both land and water) |
| area_land | The country's land area in square kilometers|
| area_water | The country's waterarea in square kilometers |


## Code/Analysis
This is a list of packages that I will be using to conduct my data analysis. 
```
library(RSQLite)
library(DBI)
library(tidyverse)
```
The name of the database file that I will be working with is "factbook.db". I access "factbook.db" with the help of SQLite. 
```
conn <- dbConnect(SQLite(),"factbook.db")
```
## Reference
[Access the data here](https://github.com/factbook/factbook.sql/releases)
