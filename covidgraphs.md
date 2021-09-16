R Notebook
================

# Working with COVID-19 Data in R

## Liam Osler

Clone the global COVID-19 Data from the github repository:

``` bash
git clone https://github.com/owid/covid-19-data.git
```

Load the required R Libraries:

``` r
library(ggplot2)
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(plotly)
```

    ## Warning: package 'plotly' was built under R version 4.1.1

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

Locate the Canadian Vaccination Data:

``` r
list.files("../covid-19-data-master/public/data/vaccinations/country_data/")[35]
```

    ## [1] NA

``` r
list.files("../../canadapopulation/")
```

    ## [1] "canadapopulation.csv" "canpop.csv"           "canpop.csv.xlsx"

``` r
canadaPopulation <- read.csv("../../covid-19-data-master/public/data/vaccinations/country_data/Canada.csv")
```

Read in the csv file:

``` r
canadaVaccination <- read.csv("../../covid-19-data-master/public/data/vaccinations/country_data/Canada.csv")
```

Convert the data to a data frame:

``` r
canadaVaccination$date <- as.Date(canadaVaccination$date)
```

The date column will now be in a “date” format:

``` r
head(canadaVaccination)
```

    ##         date total_vaccinations people_fully_vaccinated people_vaccinated
    ## 1 2020-12-14                  5                       0                 5
    ## 2 2020-12-15                723                       0               723
    ## 3 2020-12-16               3023                       0              3023
    ## 4 2020-12-17               7202                       0              7202
    ## 5 2020-12-18              11174                       0             11174
    ## 6 2020-12-19              11894                       0             11894
    ##   location                                        source_url
    ## 1   Canada https://covid19tracker.ca/vaccinationtracker.html
    ## 2   Canada https://covid19tracker.ca/vaccinationtracker.html
    ## 3   Canada https://covid19tracker.ca/vaccinationtracker.html
    ## 4   Canada https://covid19tracker.ca/vaccinationtracker.html
    ## 5   Canada https://covid19tracker.ca/vaccinationtracker.html
    ## 6   Canada https://covid19tracker.ca/vaccinationtracker.html
    ##                                        vaccine total_boosters
    ## 1 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA
    ## 2 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA
    ## 3 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA
    ## 4 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA
    ## 5 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA
    ## 6 Moderna, Oxford/AstraZeneca, Pfizer/BioNTech             NA

``` r
canadaVaccination$date[1]
```

    ## [1] "2020-12-14"

Plot the total vaccinations:

``` r
library(plotly)
canadaVaccination <- read.csv("../../covid-19-data-master/public/data/vaccinations/country_data/Canada.csv")
canadaVaccination$date <- as.Date(canadaVaccination$date)

hline <- function(y = 0, color = "black") {
  list(
    type = "line",
    x0 = 0,
    x1 = 1,
    xref = "paper",
    y0 = y,
    y1 = y,
    line = list(color = color)
  )
}

plot_ly(canadaVaccination, x = ~date, y = ~total_vaccinations, type = 'scatter', mode = 'lines', name = 'Total Doses')%>%
  layout(title = 'Canada: Vaccinations', plot_bgcolor = "#e5ecf6", yaxis = list(title = 'Total Vaccinations'), xaxis = list(title = 'Date'))%>% 
  add_trace(y = ~people_vaccinated, name = 'People Vaccinated \n(1st or 2nd Dose)', mode = 'lines')%>% 
  add_trace(y = ~people_fully_vaccinated, name = 'People Fully Vaccinated \n(1st and 2nd Dose)', mode = 'lines')%>%
  layout(shapes = hline(38131104), plot_bgcolor = "#e5ecf6")%>%
  add_text(showlegend = FALSE, x = c("2021-08-01"), y = c(36131104),
           text = c("Population (38.1M)"))
```
