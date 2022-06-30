Data Analyst Capstone
================

By: Diwan Paskins

Date: June 29th, 2022

**Case Study: How Does a Bike-Share Navigate Speedy Succes?**

### **Introduction:**

The purpose of this document is to consolidate 12 months (June 2021-May
2022) of Cyclistic’s trip data into a single data frame to help answer
the key question: “How do annual members and casual riders use
Cyclistic’s bikes differently?” The data has been made available to the
public by Motivate International Inc. and can be found at
[https://divvytripdata.s3.amazonaws.com/index.html](https://divvy-tripdata.s3.amazonaws.com/index.html).
I will be using the 6 phases of the Data Analysis process; **Ask**,
**Prepare**, **Process**, **Analyze** and **Act**.

### **Scenario:**

I am a junior data analyst working in the marketing analyst team at
Cyclistic, a bike-share company in Chicago. The director of marketing
believes the company’s future success depends on maximizing the number
of annual memberships. Therefore, your team wants to understand how
casual riders and annual members use Cyclistic bikes differently. From
these insights, your team will design a new marketing strategy to
convert casual riders into annual members. But first, Cyclistic
executives must approve your recommendations, so they must be backed up
with compelling data insights and professional data visualizations.

## **ASK PHASE:**

### Identify business task and stakeholders

The director of marketing and my manager has set a clear goal to design
marketing strategies aimed at converting casual riders into annual
members. I am assigned with creating insights to answer the following
question: *“How do annual members and casual riders use Cyclistic bikes
differently?”*

## **PREPARE PHASE:**

### **About the data** :

Data sourced from <https://divvy-tripdata.s3.amazonaws.com/index.html>
made available to the public by Motivate International Inc. under this
license: <https://ride.divvybikes.com/data-license-agreement>.

In order protect the privacy of customers, any personally identifiable
information has been omitted, meaning I won’t be able to connect pass
purchases to credit card numbers to determine if casual riders live in
the Cyclistic service area or if they have purchased multiple single
passes. The data sets will also have a different name because Cyclistic
is a fictional company.

The data set I will be working with includes 12 CSV files with a
combined size of over 1GB, so I will be using RStudio to complete my
analysis since it is much more capable of handling large pools of data.

### Download data and store it appropriately

#### Install packages I will be using to help with analysis:

``` r
options(repos = list(CRAN="http://cran.studio.com/")) #creates directory to install packages for knitting 
install.packages("tidyverse") #helps wrangle data
```

    ## Installing package into 'C:/Users/diwan/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## Warning: unable to access index for repository http://cran.studio.com/src/contrib:
    ##   cannot open URL 'http://cran.studio.com/src/contrib/PACKAGES'

    ## Warning: package 'tidyverse' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

    ## Warning: unable to access index for repository http://cran.studio.com/bin/windows/contrib/4.1:
    ##   cannot open URL 'http://cran.studio.com/bin/windows/contrib/4.1/PACKAGES'

``` r
install.packages("lubridate") #date functions
```

    ## Installing package into 'C:/Users/diwan/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## Warning: unable to access index for repository http://cran.studio.com/src/contrib:
    ##   cannot open URL 'http://cran.studio.com/src/contrib/PACKAGES'

    ## Warning: package 'lubridate' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

    ## Warning: unable to access index for repository http://cran.studio.com/bin/windows/contrib/4.1:
    ##   cannot open URL 'http://cran.studio.com/bin/windows/contrib/4.1/PACKAGES'

``` r
install.packages("ggplot2") #visualization
```

    ## Installing package into 'C:/Users/diwan/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## Warning: unable to access index for repository http://cran.studio.com/src/contrib:
    ##   cannot open URL 'http://cran.studio.com/src/contrib/PACKAGES'

    ## Warning: package 'ggplot2' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

    ## Warning: unable to access index for repository http://cran.studio.com/bin/windows/contrib/4.1:
    ##   cannot open URL 'http://cran.studio.com/bin/windows/contrib/4.1/PACKAGES'

``` r
install.packages("dplyr") #data manipulation
```

    ## Installing package into 'C:/Users/diwan/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## Warning: unable to access index for repository http://cran.studio.com/src/contrib:
    ##   cannot open URL 'http://cran.studio.com/src/contrib/PACKAGES'

    ## Warning: package 'dplyr' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

    ## Warning: unable to access index for repository http://cran.studio.com/bin/windows/contrib/4.1:
    ##   cannot open URL 'http://cran.studio.com/bin/windows/contrib/4.1/PACKAGES'

#### Load packages into R workspace:

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.1.3

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.6     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.9
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.1.1     v forcats 0.5.1

    ## Warning: package 'ggplot2' was built under R version 4.1.3

    ## Warning: package 'dplyr' was built under R version 4.1.3

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## Warning: package 'lubridate' was built under R version 4.1.3

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(ggplot2)
library(dplyr)
library(readr) #import/read CSV files
```

#### Download CSV files:

``` r
#Load CSV files; previous 12 months of Cyclistic trip data

X202106_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202106-divvy-tripdata/202106_divvy_tripdata.csv")
```

    ## Rows: 729595 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202107_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202107-divvy-tripdata/202107_divvy_tripdata.csv")
```

    ## Rows: 822410 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202108_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202108-divvy-tripdata/202108_divvy_tripdata.csv")
```

    ## Rows: 804352 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202109_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202109-divvy-tripdata/202109_divvy_tripdata.csv")
```

    ## Rows: 756147 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202110_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202110-divvy-tripdata/202110_divvy_tripdata.csv")
```

    ## Rows: 631226 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202111_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202111-divvy-tripdata/202111_divvy_tripdata.csv")
```

    ## Rows: 359978 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202112_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202112-divvy-tripdata/202112_divvy_tripdata.csv")
```

    ## Rows: 247540 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202201_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202201-divvy-tripdata/202201_divvy_tripdata.csv")
```

    ## Rows: 103770 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202202_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202202-divvy-tripdata/202202_divvy_tripdata.csv")
```

    ## Rows: 115609 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202203_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202203-divvy-tripdata/202203_divvy_tripdata.csv")
```

    ## Rows: 284042 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202204_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202204-divvy-tripdata (1)/202204_divvy_tripdata.csv")
```

    ## Rows: 371249 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
X202205_divvy_tripdata <- read_csv("C:/Users/diwan/OneDrive/Desktop/Bike_Share_Capstone/202205-divvy-tripdata/202205_divvy_tripdata.csv")
```

    ## Rows: 634858 Columns: 13
    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

### Identify how the data is organized

``` r
#Compare column names for consistency before I join data sets

colnames(X202106_divvy_tripdata)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
##colnames(X202107_divvy_tripdata)
##colnames(X202108_divvy_tripdata)
##colnames(X202109_divvy_tripdata)
##colnames(X202110_divvy_tripdata)
##colnames(X202111_divvy_tripdata)
##colnames(X202112_divvy_tripdata)
##colnames(X202201_divvy_tripdata)
##colnames(X202202_divvy_tripdata)
##colnames(X202203_divvy_tripdata)
##colnames(X202204_divvy_tripdata)
##colnames(X202205_divvy_tripdata)
```

``` r
#Inspect the structure of the data sets and look for incongruencies

str(X202106_divvy_tripdata)
```

    ## spec_tbl_df [729,595 x 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:729595] "99FEC93BA843FB20" "06048DCFC8520CAF" "9598066F68045DF2" "B03C0FE48C412214" ...
    ##  $ rideable_type     : chr [1:729595] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:729595], format: "2021-06-13 14:31:28" "2021-06-04 11:18:02" ...
    ##  $ ended_at          : POSIXct[1:729595], format: "2021-06-13 14:34:11" "2021-06-04 11:24:19" ...
    ##  $ start_station_name: chr [1:729595] NA NA NA NA ...
    ##  $ start_station_id  : chr [1:729595] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:729595] NA NA NA NA ...
    ##  $ end_station_id    : chr [1:729595] NA NA NA NA ...
    ##  $ start_lat         : num [1:729595] 41.8 41.8 41.8 41.8 41.8 ...
    ##  $ start_lng         : num [1:729595] -87.6 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num [1:729595] 41.8 41.8 41.8 41.8 41.8 ...
    ##  $ end_lng           : num [1:729595] -87.6 -87.6 -87.6 -87.6 -87.6 ...
    ##  $ member_casual     : chr [1:729595] "member" "member" "member" "member" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   ride_id = col_character(),
    ##   ..   rideable_type = col_character(),
    ##   ..   started_at = col_datetime(format = ""),
    ##   ..   ended_at = col_datetime(format = ""),
    ##   ..   start_station_name = col_character(),
    ##   ..   start_station_id = col_character(),
    ##   ..   end_station_name = col_character(),
    ##   ..   end_station_id = col_character(),
    ##   ..   start_lat = col_double(),
    ##   ..   start_lng = col_double(),
    ##   ..   end_lat = col_double(),
    ##   ..   end_lng = col_double(),
    ##   ..   member_casual = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
##str(X202107_divvy_tripdata)
##str(X202108_divvy_tripdata)
##str(X202109_divvy_tripdata)
##str(X202110_divvy_tripdata)
##str(X202111_divvy_tripdata)
##str(X202112_divvy_tripdata)
##str(X202201_divvy_tripdata)
##str(X202202_divvy_tripdata)
##str(X202203_divvy_tripdata)
##str(X202204_divvy_tripdata)
##str(X202205_divvy_tripdata)
```

#### Combine all 12 data sets to form a single data frame:

``` r
##Stack individual month's data frames into one big data frame

all_trips<- bind_rows(X202106_divvy_tripdata, X202107_divvy_tripdata, X202108_divvy_tripdata, X202109_divvy_tripdata, X202110_divvy_tripdata, X202111_divvy_tripdata, X202112_divvy_tripdata, X202201_divvy_tripdata, X202202_divvy_tripdata, X202203_divvy_tripdata, X202204_divvy_tripdata, X202205_divvy_tripdata)
```

### Sort and filter data:

``` r
## Remove lat, long fields as this data was dropped beginning in 2020

all_trips<- all_trips %>%
  select(-c(start_lat, end_lat, start_lng, end_lng))
```

## PROCESS PHASE

## Clean and prepare data for analysis:

``` r
#Inspect new table that has been created
##colnames(all_trips) #list of column names
##nrow(all_trips) #how many rows are in data frame?
##dim(all_trips) #Dimensions of the data frame?
##head(all_trips) #See the first six rows of the data frame
##tail(all_trips) #See the last six rows of the data frame
str(all_trips) #See list of columns and data types (numeric, character, etc.)
```

    ## tibble [5,860,776 x 9] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:5860776] "99FEC93BA843FB20" "06048DCFC8520CAF" "9598066F68045DF2" "B03C0FE48C412214" ...
    ##  $ rideable_type     : chr [1:5860776] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:5860776], format: "2021-06-13 14:31:28" "2021-06-04 11:18:02" ...
    ##  $ ended_at          : POSIXct[1:5860776], format: "2021-06-13 14:34:11" "2021-06-04 11:24:19" ...
    ##  $ start_station_name: chr [1:5860776] NA NA NA NA ...
    ##  $ start_station_id  : chr [1:5860776] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:5860776] NA NA NA NA ...
    ##  $ end_station_id    : chr [1:5860776] NA NA NA NA ...
    ##  $ member_casual     : chr [1:5860776] "member" "member" "member" "member" ...

``` r
##summary(all_trips) #Statistical summary of data. Mainly for numerics
glimpse(all_trips) #to get a quick view of data frame
```

    ## Rows: 5,860,776
    ## Columns: 9
    ## $ ride_id            <chr> "99FEC93BA843FB20", "06048DCFC8520CAF", "9598066F68~
    ## $ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", ~
    ## $ started_at         <dttm> 2021-06-13 14:31:28, 2021-06-04 11:18:02, 2021-06-~
    ## $ ended_at           <dttm> 2021-06-13 14:34:11, 2021-06-04 11:24:19, 2021-06-~
    ## $ start_station_name <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,~
    ## $ start_station_id   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,~
    ## $ end_station_name   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, "Michigan Ave &~
    ## $ end_station_id     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, "13042", NA, NA~
    ## $ member_casual      <chr> "member", "member", "member", "member", "member", "~

### Identified a few problems that need to be fixed:

1 I want to add some more possibilities to aggregate the data since the
data can only be aggregated at the ride-level. So were going to add some
additional columns, such as day, month, year.

2 Also want to add a calculated field for ride_length to the data frame.

3 There are some rides where ride_length shows up negative, including
hundreds of rides where Divvy took bikes out of circulation for Quality
Control reasons. Going to want to delete those rides.

### (1)Add columns that list the date, month, day and year of each ride.

``` r
#The default format for as.date() is yyyy-mm-dd
all_trips$date <- as.Date(all_trips$started_at)

all_trips$month <- format(as.Date(all_trips$date),
                          "%m")
all_trips$day <- format(as.Date(all_trips$date),
                        "%d")
all_trips$year <- format(as.Date(all_trips$date),
                         "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date),
                                "%A")
```

### (2) Add a ride_length calculation to all_trips, in seconds.

``` r
all_trips$ride_length <- difftime(all_trips$ended_at,all_trips$started_at)

#Inspect structure of column
str(all_trips)
```

    ## tibble [5,860,776 x 15] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:5860776] "99FEC93BA843FB20" "06048DCFC8520CAF" "9598066F68045DF2" "B03C0FE48C412214" ...
    ##  $ rideable_type     : chr [1:5860776] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:5860776], format: "2021-06-13 14:31:28" "2021-06-04 11:18:02" ...
    ##  $ ended_at          : POSIXct[1:5860776], format: "2021-06-13 14:34:11" "2021-06-04 11:24:19" ...
    ##  $ start_station_name: chr [1:5860776] NA NA NA NA ...
    ##  $ start_station_id  : chr [1:5860776] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:5860776] NA NA NA NA ...
    ##  $ end_station_id    : chr [1:5860776] NA NA NA NA ...
    ##  $ member_casual     : chr [1:5860776] "member" "member" "member" "member" ...
    ##  $ date              : Date[1:5860776], format: "2021-06-13" "2021-06-04" ...
    ##  $ month             : chr [1:5860776] "06" "06" "06" "06" ...
    ##  $ day               : chr [1:5860776] "13" "04" "04" "03" ...
    ##  $ year              : chr [1:5860776] "2021" "2021" "2021" "2021" ...
    ##  $ day_of_week       : chr [1:5860776] "Sunday" "Friday" "Friday" "Thursday" ...
    ##  $ ride_length       : 'difftime' num [1:5860776] 163 377 359 1550 ...
    ##   ..- attr(*, "units")= chr "secs"

### (3)There are some rides where ride_length shows up negative, including hundreds of rides where Divvy took bikes out of circulation for Quality Control reasons. Going to want to delete those rides.

``` r
#Noticed some of the ended_at times are before the started_at times resulting in negative ride_length so were going to check entries where ride_length is 0 or negative.
nrow(subset(all_trips, ride_length <= 0))
```

    ## [1] 646

``` r
##646 entries where ride_length <= 0, we will be removing these
```

### Create new version of data frame to represent this data:

``` r
#Will be removing ride lengths that are equal to or less than zero and station_name HQQR where bikes were taken out for quality control
all_trips_v2 <- all_trips[!( all_trips$start_station_name == "HQQR"| all_trips$ride_length<1),]
```

## ANALYZE PHASE

### Conduct descriptive analysis:

``` r
##Descriptive analysis on ride_length (all figures in seconds)
mean(all_trips_v2$ride_length) #straight average (total ride length/rides)
```

    ## Time difference of NA secs

``` r
median(all_trips_v2$ride_length) #midpoint number in the ascending array of ride lengths
```

    ## Time difference of NA secs

``` r
#max(all_trips_v2$ride_length) #longest ride
#min(all_trips_v2$ride_length) #shortest ride
```

### Compare members and casual users

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_length
    ## 1                     casual           1954.1719 secs
    ## 2                     member            789.9261 secs

``` r
#aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = median)
#aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = max)
#aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = min)
```

### See the average ride time by each day for members vs casual users

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
    ## 1                      casual                   Friday           1845.4728 secs
    ## 2                      member                   Friday            772.5645 secs
    ## 3                      casual                   Monday           1960.0605 secs
    ## 4                      member                   Monday            766.4229 secs
    ## 5                      casual                 Saturday           2124.8475 secs
    ## 6                      member                 Saturday            888.5175 secs
    ## 7                      casual                   Sunday           2245.0838 secs
    ## 8                      member                   Sunday            900.4007 secs
    ## 9                      casual                 Thursday           1778.6946 secs
    ## 10                     member                 Thursday            753.1489 secs
    ## 11                     casual                  Tuesday           1680.4336 secs
    ## 12                     member                  Tuesday            742.1712 secs
    ## 13                     casual                Wednesday           1709.6426 secs
    ## 14                     member                Wednesday            743.3577 secs

### Noticed the days of the week are out of order

``` r
#Want them to be in order starting with Sunday as the first day of the week
day_of_week <- ordered(all_trips_v2$day_of_week, 
                                    levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```

### Now to check the changes

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
    ## 1                      casual                   Friday           1845.4728 secs
    ## 2                      member                   Friday            772.5645 secs
    ## 3                      casual                   Monday           1960.0605 secs
    ## 4                      member                   Monday            766.4229 secs
    ## 5                      casual                 Saturday           2124.8475 secs
    ## 6                      member                 Saturday            888.5175 secs
    ## 7                      casual                   Sunday           2245.0838 secs
    ## 8                      member                   Sunday            900.4007 secs
    ## 9                      casual                 Thursday           1778.6946 secs
    ## 10                     member                 Thursday            753.1489 secs
    ## 11                     casual                  Tuesday           1680.4336 secs
    ## 12                     member                  Tuesday            742.1712 secs
    ## 13                     casual                Wednesday           1709.6426 secs
    ## 14                     member                Wednesday            743.3577 secs

### Analyze ridership data by type and weekday

``` r
all_trips_v2 %>%
  mutate(weekday=wday(started_at, label=TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday)
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 15 x 4
    ## # Groups:   member_casual [3]
    ##    member_casual weekday number_of_rides average_duration
    ##    <chr>         <ord>             <int> <drtn>          
    ##  1 casual        Sun              414079 2245.0838 secs  
    ##  2 casual        Mon              257478 1960.0605 secs  
    ##  3 casual        Tue              241449 1680.4336 secs  
    ##  4 casual        Wed              240805 1709.6426 secs  
    ##  5 casual        Thu              261244 1778.6946 secs  
    ##  6 casual        Fri              306880 1845.4728 secs  
    ##  7 casual        Sat              481621 2124.8475 secs  
    ##  8 member        Sun              336724  900.4007 secs  
    ##  9 member        Mon              400624  766.4229 secs  
    ## 10 member        Tue              453431  742.1712 secs  
    ## 11 member        Wed              443050  743.3577 secs  
    ## 12 member        Thu              431161  753.1489 secs  
    ## 13 member        Fri              391558  772.5645 secs  
    ## 14 member        Sat              376881  888.5175 secs  
    ## 15 <NA>          <NA>             823145        NA secs

### Noticed “NA” in the member_casual column, so needed to write in a code to remove NA values

``` r
all_trips_v2 <- all_trips_v2 %>% filter(!is.na(member_casual))
```

### Summary of my analysis:

### *“How do annual members and casual riders use Cyclistic bikes differently?”*

When comparing casual riders vs riders with a member ship (“member”), I
noticed that casual riders, on average, rode almost 2x longer than
riders with a member ship. Then, when comparing average ride time by
each day for members vs casual users, I also found that casual users not
only ride longer than members on any given day, but also record their
highest ride times on the weekends. Finally, when comparing the number
of rides taken by casual users vs members, I noticed that casual members
also take more rides than members on the weekends compared to the
weekday.

## SHARE PHASE

### Visualize the number of rides by rider type

``` r
#Sometimes my y-axis shows up in scientfic notation as a default, so to prevent this from happening for readability
options(scipen = 100)

#Visualization
all_trips_v2 %>%
  mutate(weekday=wday(started_at, label=TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>% 
  ggplot(aes(x= weekday, y= number_of_rides, fill= member_casual)) +
  geom_col(position = "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the
    ## `.groups` argument.

![](githubrmd_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

From this chart we will see that on weekdays, bike riders with a
membership go out on more bike rides compared to the amount of rides
taken by casual members on weekends.

### Visualization for average duration

``` r
#visualization
all_trips_v2 %>%
  mutate(weekday=wday(started_at, label=TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x= weekday, y= average_duration, fill= member_casual)) + 
  geom_col(position= "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the
    ## `.groups` argument.
    ## Don't know how to automatically pick scale for object of type difftime.
    ## Defaulting to continuous.

![](githubrmd_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

From this chart we will see that casual bike riders spend more time out
on the bikes compared to riders with a membership.

### Average ride time each day for members vs casual users

``` r
counts<- aggregate(all_trips_v2$ride_length ~
                   all_trips_v2$member_casual+
                     all_trips_v2$day_of_week, FUN= mean)
#Export as CSV file
write.csv(counts, file = "avg_ride_length.csv")
```

## ACT

### TOP 3 RECOMMENDATIONS BASED ON MY ANALYSIS

1 Re-asses membership incentives to increase usage of bike share
services by annual members.

2 Offer attractive promotions for new and returning members to increase
the number of casual user subscribers and incentives for using the bike
share services on weekdays.

3 We also could offer weekly challenges offering incentives for riders
who exceed a certain number of rides or duration geared towards to
getting a higher rider turnout on weekdays.
