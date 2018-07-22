Getting and cleaning data
================

### Week 1

##### 1 The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

<https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv>

and load the data into R. The code book, describing the variable names is here:

<https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf>

How many properties are worth $1,000,000 or more?

-   164

-   25

-   2076

-   53 correct

``` r
library(data.table)
housing_data <- data.table::fread("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv")

# VAL attribute says how much property is worth, .N is the number of rows
```

``` r
# VAL == 24 means more than $1,000,000
sum(housing_data$VAL==24,na.rm = TRUE)
```

    ## [1] 53

##### 2 Use the data you loaded from Question 1. Consider the variable FES in the code book. Which of the "tidy data" principles does this variable violate?

-   Tidy data has one variable per column. correct

-   Each tidy data table contains information about only one type of observation.

-   Numeric values in tidy data can not represent categories.

-   Each variable in a tidy data set has been transformed to be interpretable.

##### 3 Download the Excel spreadsheet on Natural Gas Aquisition Program here:

<https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx>

Read rows 18-23 and columns 7-15 into R and assign the result to a variable called: dat

 What is the value of:

``` r
#sum(dat$Zip*dat$Ext,na.rm=T)
```

 (original data source: <http://catalog.data.gov/dataset/natural-gas-acquisition-program>)

-   154339

-   36534720 correct

-   33544718

-   NA

``` r
library(readxl)
data <- read_excel("G:/Dataset/coursera/oildata.xlsx",col_names = F)
dat <- data[18:23,7:15]
names(dat) <- dat[1,]
dat<- dat[-1,]
head(dat)
```

    ## # A tibble: 5 x 9
    ##   Zip   CuCurrent PaCurrent PoCurrent Contact      Ext   Fax   email Stat~
    ##   <chr> <chr>     <chr>     <chr>     <chr>        <chr> <chr> <chr> <chr>
    ## 1 74136 0         1         0         918-491-6998 0     918-~ <NA>  1    
    ## 2 30329 1         0         0         404-321-5711 <NA>  <NA>  <NA>  1    
    ## 3 74136 1         0         0         918-523-2516 0     918-~ <NA>  1    
    ## 4 80203 0         1         0         303-864-1919 0     <NA>  <NA>  1    
    ## 5 80120 1         0         0         345-098-8890 456   <NA>  <NA>  1

``` r
dat$Zip<- as.numeric(dat$Zip)
dat$Ext<- as.numeric(dat$Ext)
```

``` r
sum(dat$Zip*dat$Ext,na.rm=T)
```

    ## [1] 36534720

##### 4 Read the XML data on Baltimore restaurants from here:

<https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml>

How many restaurants have zipcode 21231?

-   100

-   181

-   130

-   127 correct

``` r
library(tidyverse)
library(xml2)
data <- read_xml("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml")

# Point locations
point <- data %>% xml_find_all("//zipcode")%>% xml_text()%>% as.numeric()
```

``` r
head(point)
```

    ## [1] 21206 21231 21224 21211 21223 21218

``` r
table(point == 21231)
```

    ## 
    ## FALSE  TRUE 
    ##  1200   127

##### 5 The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

<https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv>

using the fread() command load the data into an R object

``` r
#DT
```

 The following are ways to calculate the average value of the variable

``` r
#pwgtp15
```

 broken down by sex. Using the data.table package, which will deliver the fastest user time?

``` r
library(data.table)
DT <- fread("G:/Dataset/coursera/idahohouse.csv")
```

``` r
library(microbenchmark)

bmark= microbenchmark(
  v3 = sapply(split(DT$pwgtp15,DT$SEX),mean),
  v6 = DT[,mean(pwgtp15),by=SEX],
  v7 = tapply(DT$pwgtp15,DT$SEX,mean),
  v8 = mean(DT$pwgtp15,by=DT$SEX),
  times=100
)
bmark
```

    ## Unit: microseconds
    ##  expr     min       lq      mean   median       uq      max neval
    ##    v3 410.121 423.1650 549.98145 437.9185 462.9365 4250.459   100
    ##    v6 607.698 650.4630 826.83647 697.7195 772.3450 6672.267   100
    ##    v7 450.749 479.4015 591.71636 501.4250 545.2600 2443.191   100
    ##    v8  32.074  35.9230  41.67983  37.6340  42.7660  218.959   100

``` r
#tapply(DT$pwgtp15,DT$SEX,mean)
```

``` r
#mean(DT$pwgtp15,by=DT$SEX)
```

``` r
#mean(DT[DT$SEX==1,]$pwgtp15); mean(DT[DT$SEX==2,]$pwgtp15)
```

``` r
#rowMeans(DT)[DT$SEX==1]; rowMeans(DT)[DT$SEX==2]
```

``` r
#sapply(split(DT$pwgtp15,DT$SEX),mean)
```

``` r
#DT[,mean(pwgtp15),by=SEX]  correct
```
