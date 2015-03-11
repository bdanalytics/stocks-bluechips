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
    
    Date.my=as.Date(strptime(Date, "%m/%d/%y"))
#     Year=year(Date.my),
#     Month=months(Date.my),
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
##     Date.my          
##  Min.   :1970-01-01  
##  1st Qu.:1979-12-24  
##  Median :1989-12-16  
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
# Other plots:
# print(myplot_histogram(entity_df, "<col1_name>"))
# print(myplot_box(df=entity_df, ycol_names="<col1_name>"))
# print(myplot_box(df=entity_df, ycol_names="<col1_name>", xcol_name="<col2_name>"))
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
## [1] plyr_1.8.1      doBy_4.5-13     survival_2.38-1 ggplot2_1.0.0  
## 
## loaded via a namespace (and not attached):
##  [1] codetools_0.2-10 colorspace_1.2-5 digest_0.6.8     evaluate_0.5.5  
##  [5] formatR_1.0      grid_3.1.2       gtable_0.1.2     htmltools_0.2.6 
##  [9] knitr_1.9        lattice_0.20-30  MASS_7.3-39      Matrix_1.1-5    
## [13] munsell_0.4.2    proto_0.3-10     Rcpp_0.11.4      reshape2_1.4.1  
## [17] rmarkdown_0.5.1  scales_0.2.4     splines_3.1.2    stringr_0.6.2   
## [21] tcltk_3.1.2      tools_3.1.2      yaml_2.1.13
```
