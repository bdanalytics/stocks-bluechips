# HW1-Stocks-BlueChips: <Predicted attribute> <regression/classification>
bdanalytics  

**  **    
**Date: (Wed) Mar 11, 2015**    

# Introduction:  

Data: 
Source: 
    Training:   https://courses.edx.org/c4x/MITx/15.071x_2/asset/IBMStock.csv
                https://courses.edx.org/c4x/MITx/15.071x_2/asset/GEStock.csv
                https://courses.edx.org/c4x/MITx/15.071x_2/asset/ProcterGambleStock.csv
                https://courses.edx.org/c4x/MITx/15.071x_2/asset/CocaColaStock.csv
                https://courses.edx.org/c4x/MITx/15.071x_2/asset/BoeingStock.csv
    New:        <prdct_url>  
Time period: 



# Synopsis:

Based on analysis utilizing <> techniques, <conclusion heading>:  

### ![](<filename>.png)

## Potential next steps include:

# Analysis: 

```r
rm(list=ls())
set.seed(12345)
options(stringsAsFactors=FALSE)
source("~/Dropbox/datascience/R/mydsutils.R")
source("~/Dropbox/datascience/R/myplot.R")
source("~/Dropbox/datascience/R/mypetrinet.R")
# Gather all package requirements here
suppressPackageStartupMessages(require(plyr))
suppressPackageStartupMessages(require(reshape2))

#require(sos); findFn("pinv", maxPages=2, sortby="MaxScore")

# Analysis specific global variables
glb_separate_predict_dataset <- FALSE

script_df <- data.frame(chunk_label="import_data", chunk_step_major=1, chunk_step_minor=0)
print(script_df)
```

```
##   chunk_label chunk_step_major chunk_step_minor
## 1 import_data                1                0
```

## Step `1`: import data

```r
entity_IBM_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/IBMStock.csv", 
    comment="entity_df", print_diagn=TRUE)
```

```
## [1] "Reading file ./data/IBMStock.csv..."
## [1] "dimensions of data in ./data/IBMStock.csv: 480 rows x 2 cols"
##     Date StockPrice
## 1 1/1/70   360.3190
## 2 2/1/70   346.7237
## 3 3/1/70   327.3457
## 4 4/1/70   319.8527
## 5 5/1/70   270.3752
## 6 6/1/70   267.2050
##        Date StockPrice
## 80   8/1/76  274.75727
## 218  2/1/88  112.02300
## 347 11/1/98  156.85850
## 364  4/1/00  114.24579
## 420 12/1/04   96.87591
## 423  3/1/05   91.30773
##        Date StockPrice
## 475  7/1/09   109.4368
## 476  8/1/09   118.4310
## 477  9/1/09   119.0557
## 478 10/1/09   122.2395
## 479 11/1/09   125.2735
## 480 12/1/09   128.8964
## 'data.frame':	480 obs. of  2 variables:
##  $ Date      : chr  "1/1/70" "2/1/70" "3/1/70" "4/1/70" ...
##  $ StockPrice: num  360 347 327 320 270 ...
##  - attr(*, "comment")= chr "entity_df"
## NULL
```

```r
entity_IBM_df$Symbol <- "IBM"

entity_GE_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/GEStock.csv", 
    comment="entity_df", print_diagn=TRUE)
```

```
## [1] "Reading file ./data/GEStock.csv..."
## [1] "dimensions of data in ./data/GEStock.csv: 480 rows x 2 cols"
##     Date StockPrice
## 1 1/1/70   74.25333
## 2 2/1/70   69.97684
## 3 3/1/70   72.15857
## 4 4/1/70   74.25273
## 5 5/1/70   66.66524
## 6 6/1/70   67.59318
##        Date StockPrice
## 17   5/1/71  120.59800
## 73   1/1/76   51.77619
## 157  1/1/83   95.75429
## 244  4/1/90   65.00550
## 348 12/1/98   93.94955
## 473  5/1/09   13.47350
##        Date StockPrice
## 475  7/1/09   11.76182
## 476  8/1/09   14.02333
## 477  9/1/09   15.59190
## 478 10/1/09   15.79773
## 479 11/1/09   15.50800
## 480 12/1/09   15.75455
## 'data.frame':	480 obs. of  2 variables:
##  $ Date      : chr  "1/1/70" "2/1/70" "3/1/70" "4/1/70" ...
##  $ StockPrice: num  74.3 70 72.2 74.3 66.7 ...
##  - attr(*, "comment")= chr "entity_df"
## NULL
```

```r
entity_GE_df$Symbol <- "GE"

entity_PG_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/ProcterGambleStock.csv", 
    comment="entity_df", print_diagn=TRUE)
```

```
## [1] "Reading file ./data/ProcterGambleStock.csv..."
## [1] "dimensions of data in ./data/ProcterGambleStock.csv: 480 rows x 2 cols"
##     Date StockPrice
## 1 1/1/70  111.87429
## 2 2/1/70  111.45368
## 3 3/1/70  108.45143
## 4 4/1/70  106.28864
## 5 5/1/70   73.33286
## 6 6/1/70   48.31864
##        Date StockPrice
## 1    1/1/70  111.87429
## 185  5/1/85   52.26955
## 187  7/1/85   57.66182
## 192 12/1/85   69.15238
## 221  5/1/88   74.32619
## 354  6/1/99   89.36227
##        Date StockPrice
## 475  7/1/09   54.04545
## 476  8/1/09   53.09810
## 477  9/1/09   55.76476
## 478 10/1/09   57.51818
## 479 11/1/09   61.29700
## 480 12/1/09   62.05273
## 'data.frame':	480 obs. of  2 variables:
##  $ Date      : chr  "1/1/70" "2/1/70" "3/1/70" "4/1/70" ...
##  $ StockPrice: num  111.9 111.5 108.5 106.3 73.3 ...
##  - attr(*, "comment")= chr "entity_df"
## NULL
```

```r
entity_PG_df$Symbol <- "PG"

entity_KO_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/CocaColaStock.csv", 
    comment="entity_df", print_diagn=TRUE)
```

```
## [1] "Reading file ./data/CocaColaStock.csv..."
## [1] "dimensions of data in ./data/CocaColaStock.csv: 480 rows x 2 cols"
##     Date StockPrice
## 1 1/1/70   83.36810
## 2 2/1/70   81.59105
## 3 3/1/70   81.33810
## 4 4/1/70   76.80591
## 5 5/1/70   69.27857
## 6 6/1/70   72.01545
##        Date StockPrice
## 86   2/1/77   75.93842
## 156 12/1/82   51.01318
## 217  1/1/88   38.07700
## 337  1/1/98   65.20550
## 456 12/1/07   62.88850
## 460  4/1/08   60.37909
##        Date StockPrice
## 475  7/1/09   49.36045
## 476  8/1/09   49.15095
## 477  9/1/09   51.58857
## 478 10/1/09   54.09000
## 479 11/1/09   55.90800
## 480 12/1/09   57.79091
## 'data.frame':	480 obs. of  2 variables:
##  $ Date      : chr  "1/1/70" "2/1/70" "3/1/70" "4/1/70" ...
##  $ StockPrice: num  83.4 81.6 81.3 76.8 69.3 ...
##  - attr(*, "comment")= chr "entity_df"
## NULL
```

```r
entity_KO_df$Symbol <- "KO"

entity_BA_df <- myimport_data(
    url="https://courses.edx.org/c4x/MITx/15.071x_2/asset/BoeingStock.csv", 
    comment="entity_df", print_diagn=TRUE)
```

```
## [1] "Reading file ./data/BoeingStock.csv..."
## [1] "dimensions of data in ./data/BoeingStock.csv: 480 rows x 2 cols"
##     Date StockPrice
## 1 1/1/70   27.85381
## 2 2/1/70   22.38105
## 3 3/1/70   23.10524
## 4 4/1/70   21.57136
## 5 5/1/70   18.93286
## 6 6/1/70   15.44318
##        Date StockPrice
## 108 12/1/78   71.56000
## 187  7/1/85   47.21864
## 231  3/1/89   65.81091
## 260  8/1/91   47.13864
## 310 10/1/95   66.25364
## 334 10/1/97   51.65043
##        Date StockPrice
## 475  7/1/09   41.48273
## 476  8/1/09   45.99429
## 477  9/1/09   51.36286
## 478 10/1/09   51.15909
## 479 11/1/09   50.69650
## 480 12/1/09   55.02864
## 'data.frame':	480 obs. of  2 variables:
##  $ Date      : chr  "1/1/70" "2/1/70" "3/1/70" "4/1/70" ...
##  $ StockPrice: num  27.9 22.4 23.1 21.6 18.9 ...
##  - attr(*, "comment")= chr "entity_df"
## NULL
```

```r
entity_BA_df$Symbol <- "BA"

entity_df <- rbind(entity_IBM_df, entity_GE_df, entity_PG_df, entity_KO_df, entity_BA_df)

if (glb_separate_predict_dataset) {
    predct_df <- myimport_data(
        url="<prdct_url>", 
        comment="predct_df", print_diagn=TRUE)
} else {
    predct_df <- entity_df[sample(1:nrow(entity_df), nrow(entity_df) / 1000),]
    comment(predct_df) <- "predct_df"
    myprint_df(predct_df)
    str(predct_df)
}         
```

```
##        Date StockPrice Symbol
## 1904 8/1/08   54.17381     KO
## 15   3/1/71  351.90783    IBM
## 'data.frame':	2 obs. of  3 variables:
##  $ Date      : chr  "8/1/08" "3/1/71"
##  $ StockPrice: num  54.2 351.9
##  $ Symbol    : chr  "KO" "IBM"
##  - attr(*, "comment")= chr "predct_df"
```

```r
script_df <- rbind(script_df, 
                   data.frame(chunk_label="inspect_data", 
                              chunk_step_major=max(script_df$chunk_step_major)+1, 
                              chunk_step_minor=1))
print(script_df)
```

```
##    chunk_label chunk_step_major chunk_step_minor
## 1  import_data                1                0
## 2 inspect_data                2                1
```

### Step `2`.`1`: inspect data

```r
#print(str(entity_df))
#View(entity_df)

# List info gathered for various columns
# <col_name>:   <description>; <notes>

# Create new features that help diagnostics
#   Convert factors to dummy variables
#   Potential Enhancements:
#       One code chunk to cycle thru entity_df & predct_df ?
#           Use with / within ?
#           for (df in c(entity_df, predct_df)) cycles thru column names
#           for (df in list(entity_df, predct_df)) does not change the actual dataframes
#
#       Build splines   require(splines); bsBasis <- bs(training$age, df=3)

entity_df <- mutate(entity_df, 
    Symbol_fctr=as.factor(Symbol),
    
    Date.my=as.Date(strptime(Date, "%m/%d/%y")),
#     Year=year(Date.my),
    Month=months(Date.my),
#     Weekday=weekdays(Date.my)
    
                    )
# 
# predct_df <- mutate(predct_df, 
#                     )

print(summary(entity_df))
```

```
##      Date             StockPrice         Symbol          Symbol_fctr
##  Length:2400        Min.   :  9.294   Length:2400        BA :480    
##  Class :character   1st Qu.: 46.894   Class :character   GE :480    
##  Mode  :character   Median : 63.248   Mode  :character   IBM:480    
##                     Mean   : 77.601                      KO :480    
##                     3rd Qu.: 89.499                      PG :480    
##                     Max.   :438.902                                 
##     Date.my              Month          
##  Min.   :1970-01-01   Length:2400       
##  1st Qu.:1979-12-24   Class :character  
##  Median :1989-12-16   Mode  :character  
##  Mean   :1989-12-15                     
##  3rd Qu.:1999-12-08                     
##  Max.   :2009-12-01
```

```r
print(summary(predct_df))
```

```
##      Date             StockPrice        Symbol         
##  Length:2           Min.   : 54.17   Length:2          
##  Class :character   1st Qu.:128.61   Class :character  
##  Mode  :character   Median :203.04   Mode  :character  
##                     Mean   :203.04                     
##                     3rd Qu.:277.47                     
##                     Max.   :351.91
```

```r
#pairs(subset(entity_df, select=-c(col_symbol)))

#   Histogram of predictor in entity_df & predct_df
# Check for predct_df & entity_df features range mismatches

# Other diagnostics:
# print(subset(entity_df, <col1_name> == max(entity_df$<col1_name>, na.rm=TRUE) & 
#                         <col2_name> <= mean(entity_df$<col1_name>, na.rm=TRUE)))

# print(table(entity_df$<col_name>))
# print(which.min(table(entity_df$<col_name>)))
# print(which.max(table(entity_df$<col_name>)))
# print(which.max(table(entity_df$<col1_name>, entity_df$<col2_name>)[, 2]))
# print(table(entity_df$<col1_name>, entity_df$<col2_name>))
# print(xtabs(~ <col1_name>, entity_df))
# print(xtabs(~ <col1_name> + <col2_name>, entity_df))
# print(xtab_entity_df <- mycreate_xtab(entity_df, c("<col1_name>", "<col2_name>")))
# print(xtab_entity_df <- mutate(xtab_entity_df, 
#             <col3_name>=(<col1_name> * 1.0) / (<col1_name> + <col2_name>))) 

print(sort(tapply(entity_df$StockPrice, entity_df$Symbol_fctr, mean, na.rm=TRUE)))
```

```
##        BA        GE        KO        PG       IBM 
##  46.59293  59.30350  60.02973  77.70452 144.37503
```

```r
print(sort(tapply(entity_df$StockPrice, entity_df$Symbol_fctr, min, na.rm=TRUE)))
```

```
##        GE        BA        KO       IBM        PG 
##  9.293636 12.736364 30.057143 43.395000 46.884545
```

```r
print(sort(tapply(entity_df$StockPrice, entity_df$Symbol_fctr, max, na.rm=TRUE)))
```

```
##       BA       KO       PG       GE      IBM 
## 107.2800 146.5843 149.6200 156.8437 438.9016
```

```r
print(sort(tapply(entity_df$StockPrice, entity_df$Symbol_fctr, median, na.rm=TRUE)))
```

```
##        BA        KO        GE        PG       IBM 
##  44.88340  51.43699  55.81205  78.33608 112.11460
```

```r
print(sort(tapply(entity_df$StockPrice, entity_df$Symbol_fctr, sd, na.rm=TRUE)))
```

```
##       PG       BA       GE       KO      IBM 
## 18.19414 19.89184 23.99255 25.16629 87.82208
```

```r
myprint_df(entity_IBM_df <- subset(entity_df, Symbol == "IBM"))
```

```
##     Date StockPrice Symbol Symbol_fctr    Date.my    Month
## 1 1/1/70   360.3190    IBM         IBM 1970-01-01  January
## 2 2/1/70   346.7237    IBM         IBM 1970-02-01 February
## 3 3/1/70   327.3457    IBM         IBM 1970-03-01    March
## 4 4/1/70   319.8527    IBM         IBM 1970-04-01    April
## 5 5/1/70   270.3752    IBM         IBM 1970-05-01      May
## 6 6/1/70   267.2050    IBM         IBM 1970-06-01     June
##        Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 91   7/1/77  266.63316    IBM         IBM 1977-07-01      July
## 173  5/1/84  111.17909    IBM         IBM 1984-05-01       May
## 177  9/1/84  124.35263    IBM         IBM 1984-09-01 September
## 327  3/1/97  141.67750    IBM         IBM 1997-03-01     March
## 414  6/1/04   89.42000    IBM         IBM 2004-06-01      June
## 430 10/1/05   82.06333    IBM         IBM 2005-10-01   October
##        Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 475  7/1/09   109.4368    IBM         IBM 2009-07-01      July
## 476  8/1/09   118.4310    IBM         IBM 2009-08-01    August
## 477  9/1/09   119.0557    IBM         IBM 2009-09-01 September
## 478 10/1/09   122.2395    IBM         IBM 2009-10-01   October
## 479 11/1/09   125.2735    IBM         IBM 2009-11-01  November
## 480 12/1/09   128.8964    IBM         IBM 2009-12-01  December
```

```r
#myprint_df(entity_IBM_df <- mutate(entity_IBM_df, Month=months(Date.my)))
print(month_mean_entity_IBM_arr <- 
          sort(tapply(entity_IBM_df$StockPrice, entity_IBM_df$Month, mean, na.rm=TRUE)))
```

```
##   October  November      July September      June    August  December 
##  137.3466  138.0187  139.0670  139.0885  139.0907  140.1455  140.7593 
##   January       May     April     March  February 
##  150.2384  151.5022  152.1168  152.4327  152.6940
```

```r
print(month_mean_entity_IBM_arr > mean(entity_IBM_df$StockPrice, na.rm=TRUE))
```

```
##   October  November      July September      June    August  December 
##     FALSE     FALSE     FALSE     FALSE     FALSE     FALSE     FALSE 
##   January       May     April     March  February 
##      TRUE      TRUE      TRUE      TRUE      TRUE
```

```r
myprint_df(entity_GE_df <- subset(entity_df, Symbol == "GE"))
```

```
##       Date StockPrice Symbol Symbol_fctr    Date.my    Month
## 481 1/1/70   74.25333     GE          GE 1970-01-01  January
## 482 2/1/70   69.97684     GE          GE 1970-02-01 February
## 483 3/1/70   72.15857     GE          GE 1970-03-01    March
## 484 4/1/70   74.25273     GE          GE 1970-04-01    April
## 485 5/1/70   66.66524     GE          GE 1970-05-01      May
## 486 6/1/70   67.59318     GE          GE 1970-06-01     June
##        Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 545  5/1/75   46.53857     GE          GE 1975-05-01       May
## 685  1/1/87   94.15762     GE          GE 1987-01-01   January
## 777  9/1/94   49.66286     GE          GE 1994-09-01 September
## 848  8/1/00   55.92304     GE          GE 2000-08-01    August
## 854  2/1/01   47.05789     GE          GE 2001-02-01  February
## 922 10/1/06   35.68091     GE          GE 2006-10-01   October
##        Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 955  7/1/09   11.76182     GE          GE 2009-07-01      July
## 956  8/1/09   14.02333     GE          GE 2009-08-01    August
## 957  9/1/09   15.59190     GE          GE 2009-09-01 September
## 958 10/1/09   15.79773     GE          GE 2009-10-01   October
## 959 11/1/09   15.50800     GE          GE 2009-11-01  November
## 960 12/1/09   15.75455     GE          GE 2009-12-01  December
```

```r
myprint_df(entity_KO_df <- subset(entity_df, Symbol == "KO"))
```

```
##        Date StockPrice Symbol Symbol_fctr    Date.my    Month
## 1441 1/1/70   83.36810     KO          KO 1970-01-01  January
## 1442 2/1/70   81.59105     KO          KO 1970-02-01 February
## 1443 3/1/70   81.33810     KO          KO 1970-03-01    March
## 1444 4/1/70   76.80591     KO          KO 1970-04-01    April
## 1445 5/1/70   69.27857     KO          KO 1970-05-01      May
## 1446 6/1/70   72.01545     KO          KO 1970-06-01     June
##         Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 1461  9/1/71  107.64190     KO          KO 1971-09-01 September
## 1467  3/1/72  127.53909     KO          KO 1972-03-01     March
## 1469  5/1/72  129.13182     KO          KO 1972-05-01       May
## 1565  5/1/80   34.16286     KO          KO 1980-05-01       May
## 1594 10/1/82   44.04333     KO          KO 1982-10-01   October
## 1738 10/1/94   49.82952     KO          KO 1994-10-01   October
##         Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 1915  7/1/09   49.36045     KO          KO 2009-07-01      July
## 1916  8/1/09   49.15095     KO          KO 2009-08-01    August
## 1917  9/1/09   51.58857     KO          KO 2009-09-01 September
## 1918 10/1/09   54.09000     KO          KO 2009-10-01   October
## 1919 11/1/09   55.90800     KO          KO 2009-11-01  November
## 1920 12/1/09   57.79091     KO          KO 2009-12-01  December
```

```r
print(month_mean_entity_GE_arr <- 
          sort(tapply(entity_GE_df$StockPrice, entity_GE_df$Month, mean, na.rm=TRUE)))
```

```
##   October September      June    August      July  November  December 
##  56.23897  56.23913  56.46844  56.50315  56.73349  57.28879  59.10217 
##       May   January  February     March     April 
##  60.87135  62.04511  62.52080  63.15055  64.48009
```

```r
print(month_mean_entity_KO_arr <- 
          sort(tapply(entity_KO_df$StockPrice, entity_KO_df$Month, mean, na.rm=TRUE)))
```

```
## September   October    August      July  November  December   January 
##  57.60024  57.93887  58.88014  58.98346  59.10268  59.73223  60.36849 
##  February      June       May     March     April 
##  60.73475  60.81208  61.44358  62.07135  62.68888
```

```r
myprint_df(entity_PG_df <- subset(entity_df, Symbol == "PG"))
```

```
##       Date StockPrice Symbol Symbol_fctr    Date.my    Month
## 961 1/1/70  111.87429     PG          PG 1970-01-01  January
## 962 2/1/70  111.45368     PG          PG 1970-02-01 February
## 963 3/1/70  108.45143     PG          PG 1970-03-01    March
## 964 4/1/70  106.28864     PG          PG 1970-04-01    April
## 965 5/1/70   73.33286     PG          PG 1970-05-01      May
## 966 6/1/70   48.31864     PG          PG 1970-06-01     June
##         Date StockPrice Symbol Symbol_fctr    Date.my   Month
## 1062  6/1/78   86.42409     PG          PG 1978-06-01    June
## 1111  7/1/82   85.15190     PG          PG 1982-07-01    July
## 1198 10/1/89  127.28455     PG          PG 1989-10-01 October
## 1309  1/1/99   87.47842     PG          PG 1999-01-01 January
## 1357  1/1/03   85.82238     PG          PG 2003-01-01 January
## 1423  7/1/08   63.73227     PG          PG 2008-07-01    July
##         Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 1435  7/1/09   54.04545     PG          PG 2009-07-01      July
## 1436  8/1/09   53.09810     PG          PG 2009-08-01    August
## 1437  9/1/09   55.76476     PG          PG 2009-09-01 September
## 1438 10/1/09   57.51818     PG          PG 2009-10-01   October
## 1439 11/1/09   61.29700     PG          PG 2009-11-01  November
## 1440 12/1/09   62.05273     PG          PG 2009-12-01  December
```

```r
myprint_df(entity_BA_df <- subset(entity_df, Symbol == "BA"))
```

```
##        Date StockPrice Symbol Symbol_fctr    Date.my    Month
## 1921 1/1/70   27.85381     BA          BA 1970-01-01  January
## 1922 2/1/70   22.38105     BA          BA 1970-02-01 February
## 1923 3/1/70   23.10524     BA          BA 1970-03-01    March
## 1924 4/1/70   21.57136     BA          BA 1970-04-01    April
## 1925 5/1/70   18.93286     BA          BA 1970-05-01      May
## 1926 6/1/70   15.44318     BA          BA 1970-06-01     June
##        Date StockPrice Symbol Symbol_fctr    Date.my Month
## 1959 3/1/73   21.43545     BA          BA 1973-03-01 March
## 2033 5/1/79   39.85955     BA          BA 1979-05-01   May
## 2043 3/1/80   57.23476     BA          BA 1980-03-01 March
## 2129 5/1/87   44.60150     BA          BA 1987-05-01   May
## 2271 3/1/99   34.77348     BA          BA 1999-03-01 March
## 2297 5/1/01   65.42727     BA          BA 2001-05-01   May
##         Date StockPrice Symbol Symbol_fctr    Date.my     Month
## 2395  7/1/09   41.48273     BA          BA 2009-07-01      July
## 2396  8/1/09   45.99429     BA          BA 2009-08-01    August
## 2397  9/1/09   51.36286     BA          BA 2009-09-01 September
## 2398 10/1/09   51.15909     BA          BA 2009-10-01   October
## 2399 11/1/09   50.69650     BA          BA 2009-11-01  November
## 2400 12/1/09   55.02864     BA          BA 2009-12-01  December
```

```r
for (sym in unique(entity_df$Symbol)) {
    entity_sym_df <- subset(entity_df, Symbol == sym)
    print(sprintf("sym=%s", sym))
    print(tapply(entity_sym_df$StockPrice, entity_sym_df$Month, mean, na.rm=TRUE))
}    
```

```
## [1] "sym=IBM"
##     April    August  December  February   January      July      June 
##  152.1168  140.1455  140.7593  152.6940  150.2384  139.0670  139.0907 
##     March       May  November   October September 
##  152.4327  151.5022  138.0187  137.3466  139.0885 
## [1] "sym=GE"
##     April    August  December  February   January      July      June 
##  64.48009  56.50315  59.10217  62.52080  62.04511  56.73349  56.46844 
##     March       May  November   October September 
##  63.15055  60.87135  57.28879  56.23897  56.23913 
## [1] "sym=PG"
##     April    August  December  February   January      July      June 
##  77.68671  76.82266  78.29661  79.02575  79.61798  76.64556  77.39275 
##     March       May  November   October September 
##  77.34761  77.85958  78.45610  76.67903  76.62385 
## [1] "sym=KO"
##     April    August  December  February   January      July      June 
##  62.68888  58.88014  59.73223  60.73475  60.36849  58.98346  60.81208 
##     March       May  November   October September 
##  62.07135  61.44358  59.10268  57.93887  57.60024 
## [1] "sym=BA"
##     April    August  December  February   January      July      June 
##  47.04686  46.86311  46.17315  46.89223  46.51097  46.55360  47.38525 
##     March       May  November   October September 
##  46.88208  48.13716  45.14990  45.21603  46.30485
```

```r
# Other plots:
# print(myplot_histogram(entity_df, "<col1_name>"))
# print(myplot_box(df=entity_df, ycol_names="<col1_name>"))
# print(myplot_box(df=entity_df, ycol_names="<col1_name>", xcol_name="<col2_name>"))
print(myplot_line(subset(entity_df, Symbol == "KO"), "Date.my", "StockPrice"))
```

![](Stocks_BlueChips_files/figure-html/inspect_data_1-1.png) 

```r
symbol_cast_entity_df <- dcast(entity_df, Date.my ~ Symbol, value.var="StockPrice")
myprint_df(symbol_cast_entity_df)
```

```
##      Date.my       BA       GE      IBM       KO        PG
## 1 1970-01-01 27.85381 74.25333 360.3190 83.36810 111.87429
## 2 1970-02-01 22.38105 69.97684 346.7237 81.59105 111.45368
## 3 1970-03-01 23.10524 72.15857 327.3457 81.33810 108.45143
## 4 1970-04-01 21.57136 74.25273 319.8527 76.80591 106.28864
## 5 1970-05-01 18.93286 66.66524 270.3752 69.27857  73.33286
## 6 1970-06-01 15.44318 67.59318 267.2050 72.01545  48.31864
##        Date.my       BA        GE      IBM       KO       PG
## 71  1975-11-01 24.93263  48.33211 219.6200 86.01789 92.19579
## 105 1978-09-01 68.32950  53.38400 290.2820 44.65900 88.77100
## 363 2000-03-01 35.66565 141.70261 111.4091 47.53565 62.88174
## 452 2007-08-01 99.26000  38.56739 111.9713 54.02304 64.35478
## 469 2009-01-01 42.99100  14.47200  87.5965 43.91900 58.87150
## 474 2009-06-01 47.96864  12.83273 106.8009 48.64591 51.90045
##        Date.my       BA       GE      IBM       KO       PG
## 475 2009-07-01 41.48273 11.76182 109.4368 49.36045 54.04545
## 476 2009-08-01 45.99429 14.02333 118.4310 49.15095 53.09810
## 477 2009-09-01 51.36286 15.59190 119.0557 51.58857 55.76476
## 478 2009-10-01 51.15909 15.79773 122.2395 54.09000 57.51818
## 479 2009-11-01 50.69650 15.50800 125.2735 55.90800 61.29700
## 480 2009-12-01 55.02864 15.75455 128.8964 57.79091 62.05273
```

```r
# print(myplot_line(entity_df, "Date.my", tail(names(symbol_cast_entity_df), -1)))
print(myplot_line(entity_df, "Date.my", "StockPrice", facet_row_colnames="Symbol"))
```

![](Stocks_BlueChips_files/figure-html/inspect_data_1-2.png) 

```r
print(myplot_line(subset(entity_df, Symbol %in% c("KO", "PG")), 
                  "Date.my", "StockPrice", facet_row_colnames="Symbol") + 
    geom_vline(xintercept=as.numeric(as.Date("2003-03-01"))) +
    geom_vline(xintercept=as.numeric(as.Date("1983-01-01")))        
        )
```

![](Stocks_BlueChips_files/figure-html/inspect_data_1-3.png) 

```r
print(myplot_line(subset(entity_df, Date.my >= as.Date("1995-01-01") & 
                                    Date.my <= as.Date("2005-12-31")), 
                  "Date.my", "StockPrice", facet_row_colnames="Symbol") +
    geom_vline(xintercept=as.numeric(as.Date("2000-03-01"))) +
    geom_vline(xintercept=as.numeric(as.Date("1997-09-01"))) +
    geom_vline(xintercept=as.numeric(as.Date("1997-11-30"))) +
    geom_vline(xintercept=as.numeric(as.Date("2004-01-01"))) +
    geom_vline(xintercept=as.numeric(as.Date("2005-12-31")))         
        )
```

![](Stocks_BlueChips_files/figure-html/inspect_data_1-4.png) 

```r
# print(myplot_scatter(entity_df, "<col1_name>", "<col2_name>"))

script_df <- rbind(script_df, 
                   data.frame(chunk_label="extract_features", 
                              chunk_step_major=max(script_df$chunk_step_major)+1, 
                              chunk_step_minor=0))
print(script_df)
```

```
##        chunk_label chunk_step_major chunk_step_minor
## 1      import_data                1                0
## 2     inspect_data                2                1
## 3 extract_features                3                0
```

## Step `3`: extract features

```r
# script_df <- rbind(script_df, 
#                    data.frame(chunk_label="extract_features", 
#                               chunk_step_major=max(script_df$chunk_step_major)+1, 
#                               chunk_step_minor=0))
print(script_df)
```

```
##        chunk_label chunk_step_major chunk_step_minor
## 1      import_data                1                0
## 2     inspect_data                2                1
## 3 extract_features                3                0
```

Null Hypothesis ($\sf{H_{0}}$): mpg is not impacted by am_fctr.  
The variance by am_fctr appears to be independent. 

```r
# print(t.test(subset(cars_df, am_fctr == "automatic")$mpg, 
#              subset(cars_df, am_fctr == "manual")$mpg, 
#              var.equal=FALSE)$conf)
```
We reject the null hypothesis i.e. we have evidence to conclude that am_fctr impacts mpg (95% confidence). Manual transmission is better for miles per gallon versus automatic transmission.

## remove nearZeroVar features (not much variance)
#require(reshape)
#var_features_df <- melt(summaryBy(. ~ factor(0), data=entity_df[, features_lst], 
#                             FUN=var, keep.names=TRUE), 
#                             variable_name=c("feature"))
#names(var_features_df)[2] <- "var"
#print(var_features_df[order(var_features_df$var), ])
# summaryBy ignores factors whereas nearZeroVar inspects factors

# k_fold <- 5
# entity_df[order(entity_df$classe, 
#                   entity_df$user_name, 
#                   entity_df$my.rnorm),"my.cv_ix"] <- 
#     rep(1:k_fold, length.out=nrow(entity_df))
# summaryBy(X ~ my.cv_ix, data=entity_df, FUN=length)
# tapply(entity_df$X, list(entity_df$classe, entity_df$user_name, 
#                            entity_df$my.cv_ix), length)

#require(DAAG)
#entity_df$classe.proper <- as.numeric(entity_df$classe == "A")
#rnorm.glm <- glm(classe.proper ~ rnorm, family=binomial, data=entity_df)
#cv.binary(rnorm.glm, nfolds=k_fold, print.details=TRUE)
#result <- cv.lm(df=entity_df, form.lm=formula(classe ~ rnorm), 
#                    m=k_fold, seed=12345, printit=TRUE)

#plot(mdl_1$finalModel, uniform=TRUE, main="base")
#text(mdl_1$finalModel, use.n=TRUE, all=TRUE, cex=0.8)



```
## R version 3.1.2 (2014-10-31)
## Platform: x86_64-apple-darwin13.4.0 (64-bit)
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] reshape2_1.4.1  plyr_1.8.1      doBy_4.5-13     survival_2.38-1
## [5] ggplot2_1.0.0  
## 
## loaded via a namespace (and not attached):
##  [1] codetools_0.2-10 colorspace_1.2-5 digest_0.6.8     evaluate_0.5.5  
##  [5] formatR_1.0      grid_3.1.2       gtable_0.1.2     htmltools_0.2.6 
##  [9] knitr_1.9        labeling_0.3     lattice_0.20-30  MASS_7.3-39     
## [13] Matrix_1.1-5     munsell_0.4.2    proto_0.3-10     Rcpp_0.11.4     
## [17] rmarkdown_0.5.1  scales_0.2.4     splines_3.1.2    stringr_0.6.2   
## [21] tcltk_3.1.2      tools_3.1.2      yaml_2.1.13
```
