# Guided Project:Analyzing CIA Factbook Data Using SQLite and R
## Introduction
In this project, I analyze the data from the CIA World Factbook -- a collection of statistics on the history, people, government, economy, geography, communications, transportation, military and transnational issues of all the countries around the globe -- using SQLite and R. In particular, I construct histograms and run summary statistics on a small subset of "population" variables to illustrate basic features of the sample data. 

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
The RSQLite package embeds the "SQLite" database engine in R while the DBI package enables communication between R and the specified database.
```
conn <- dbConnect(SQLite(),"factbook.db")
```
In the next few lines of code, I perform a query to extract the first five observations from the *facts* table in the *factbook* database.
```
query_1 <- "SELECT* from facts limit 5"
result_1 <- dbGetQuery(conn,query_1)
```
The *facts* table contains geographical information for each country in the world. Let's go ahead and find the minimum and maximum values of the *population* and *population_growth* variables. 

```
query_2 <- "select MIN(population), MAX(population), MIN(population_growth), MAX(population_growth) from facts"
result_2 <- dbGetQuery(conn, query_2)
```
Text
```
query_3 <- "Select* from facts where population = (Select MIN(population) from facts)"
result_3 <- dbGetQuery(conn,query_3)

query_4 <- "Select* from facts where population = (Select MAX(population) from facts)"
result_4 <- dbGetQuery(conn,query_4)
```
## Reference
[Access the data here](https://github.com/factbook/factbook.sql/releases)
