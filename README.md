# Guided Project:Analyzing CIA Factbook Data Using SQLite and R
## Introduction
In this project, I analyze the data from the CIA World Factbook -- a collection of statistics on the history, people, government, economy, geography, communications, transportation, military and transnational issues of all the countries around the globe -- using SQLite and R. In particular, I construct histograms and run summary statistics on a small subset of "population" variables to illustrate basic features of the sample data. 

| Variable | Description |
| :---: | :---: | 
| name | The name of the country |
| area | The total land and sea area of the country |
| population | The country's population |
| population_growth | The country's population growth as a percentage |
| birth_rate | The country's birth rate, or the number of births a year per 1,000 people |
| death_rate | The country's death rate, or the number of death a year per 1,000 people |
| area | The country's total area (both land and water) |
| area_land | The country's land area in square kilometers|
| area_water | The country's waterarea in square kilometers |


## Code/Data Analysis
This is a list of packages that I will be using to conduct my data analysis. 
```
library(RSQLite)
library(DBI)
library(tidyverse)
library(gridExtra)
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
The *facts* table records primary geographical information about each country in the world. Let's go ahead and find the minimum and maximum values in each of the *population* and *population_growth* columns. 

```
query_2 <- "select MIN(population), MAX(population), MIN(population_growth), MAX(population_growth) from facts"
result_2 <- dbGetQuery(conn, query_2)
```
When we take a glance at the output, there are two values which come across as fabricated: MAX(population) and MIN(population). The *factbook* database reports the smallest national population to be 0 and the largest national population to be around 7.2 billion. Common knowledge tells us that none of the countries on the planet can possibly meet such a criteria. 

We can operate a query to trace the source of the problem. 

```
query_3 <- "Select* from facts where population = (Select MIN(population) from facts)"
result_3 <- dbGetQuery(conn,query_3)

query_4 <- "Select* from facts where population = (Select MAX(population) from facts)"
result_4 <- dbGetQuery(conn,query_4)
```
It turns out that the CIA World Factbook includes Antarctica and Earth in its observations. Antarctica and Earth are outliers and in this case, we can exclude them as to not skew the distribution of the data. In addition to removing those observations, I am also interested in extracting the following variables: *population*, *population_growth*, *birth_rate* and *death_rate*, so that I can examine the distribution of their values later on. I execute the two commands together in a single query to save me time and space.

```
query_5 <- "select population,population_growth,birth_rate,death_rate from facts where population!= (Select MIN(population) from facts) and population!= (Select MAX(population) from facts)"
result_5 <- dbGetQuery(conn, query_5)
```
The next step is to construct a histogram for each of the variables in order to investigate the distribution of their values. 
```
create_histogram <- function (x,y)  {
      ggplot(data = result_5) + aes_string(x = x) + theme(panel.background = element_rect(fill = "white")) + geom_histogram()
 }
 demographics <- c("population","population_growth","birth_rate", "death_rate")
 histograms <- map(demographics,create_histogram)
 do.call("grid.arrange",c(histograms,ncol = 2))
```
### Population Histograms
![](https://i.ibb.co/3vbQtnX/Rplot.png)


The upper left histogram shows that the population ll
```
query_6 <- "Select name country_or_dependent_territory from facts ORDER BY cast(population as float)/cast(area_land as float) DESC LIMIT 10"
result_6 <- dbGetQuery(conn, query_6)
```
| Rank by Population Density | Country (or dependent territory) |
| :---: | :---: |
| 1 | Macau |
| 2 | Monaco |
| 3 | Singapore |
| 4 | Hong Kong |
| 5 | Gaza Strip |
| 6 | Gibraltar |
| 7 | Bahrain |
| 8 | Maldives |
| 9 | Malta |
| 10 | Bermuda |

## Reference
[Access the data here](https://github.com/factbook/factbook.sql/releases)
