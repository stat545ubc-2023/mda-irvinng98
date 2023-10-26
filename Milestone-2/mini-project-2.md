Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.2.3

    ## Warning: package 'tibble' was built under R version 4.2.3

    ## Warning: package 'tidyr' was built under R version 4.2.2

    ## Warning: package 'readr' was built under R version 4.2.2

    ## Warning: package 'purrr' was built under R version 4.2.2

    ## Warning: package 'dplyr' was built under R version 4.2.3

    ## Warning: package 'stringr' was built under R version 4.2.2

    ## Warning: package 'lubridate' was built under R version 4.2.2

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  Is there a flow trend for each year (ie. how does the flow change
    throughout a year)?
2.  Is there a trend for the amount of missing values in the variable
    SYM?
3.  Are there years or months where flow is more volatile?
4.  How do the maximum and minimum flow rates (sub-setting by the
    extreme variable) change throughout the years/months?
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

``` r
data <- flow_sample
df1 <- as.data.frame(data)

# For Question 1: I Chose to do task 2 and 8

# Determine the observation counts of the month variable using group_by to group unique values and summarize to calculate the observations of each values. This will tell us whether some months may be more or less accurate due to sample size differences
result1 <- data %>%
  group_by(month) %>%
  summarise(Count = n())
print(result1)
```

    ## # A tibble: 11 × 2
    ##    month Count
    ##    <dbl> <int>
    ##  1     1    15
    ##  2     2    28
    ##  3     3    37
    ##  4     4     8
    ##  5     5    16
    ##  6     6    79
    ##  7     7    13
    ##  8     8     1
    ##  9    11     5
    ## 10    12    14
    ## 11    NA     2

``` r
# Make two geom layers (scatter plot and linear regression line). This would give us a rough idea of what the flow trend is like over the years. Here I set the scatter plot size to be blue and a size of 3 whereas for the linear regression line, I use linear modeling with a red dashed line.
plot1 <- ggplot(data, aes(x = year, y = flow)) +
  geom_point(color = "blue", size = 3) +
  geom_smooth(method = "lm", color = "red", linetype = "dashed")
print(plot1)
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 2 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 2 rows containing missing values (`geom_point()`).

![](https://github.com/stat545ubc-2023/mda-irvinng98/blob/main/figures/unnamed-chunk-2-1.png)<!-- -->

``` r
# For Question 2: I Chose to do task 2 and 8
# Group the data by the year column and calculate the number of NA values in the sym column. Then summarize the sum of the NA values.
result2 <- data %>%
  group_by(year) %>%
  summarise(NA_count = sum(is.na(sym)))
print(result2)
```

    ## # A tibble: 109 × 2
    ##     year NA_count
    ##    <dbl>    <int>
    ##  1  1909        2
    ##  2  1910        2
    ##  3  1911        2
    ##  4  1912        2
    ##  5  1913        1
    ##  6  1914        2
    ##  7  1915        2
    ##  8  1916        1
    ##  9  1917        1
    ## 10  1918        1
    ## # ℹ 99 more rows

``` r
# Plot the results of result2 (ie. the amount of NA values in sym column for each year). I chose to use geom_point and geom_line as they allow better visualization of where there is a decrease or increase in the amount of sym NA values since there are so many years. Here I chose a solid blue line for the line graph and for the scatter plot, I chose red dots with a size of 4 and a filled circle shape.
plot2 <- ggplot(result2, aes(x = year)) +
  geom_line(aes(y = NA_count), color = "blue", linetype = "solid", size = 1.5) +
  geom_point(aes(y = NA_count), color = "red", shape = 16, size = 4) 
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
print(plot2)
```

![](https://github.com/stat545ubc-2023/mda-irvinng98/blob/main/figures/unnamed-chunk-2-2.png)<!-- -->

``` r
# For Question 3: I chose to do task 1 and 8
# Here I calculate the summary stats for the flow variable across the years. First, I group by years (using group_by) and then I  calculate the different summary statistics (ie. range, mean, median, SD, and IQR) all using the flow variable since that's what we're interested in.
result3 <- data %>%
  group_by(year) %>%
  summarise(
    Range = max(flow, na.rm = TRUE) - min(flow, na.rm = TRUE),
    Mean = mean(flow, na.rm = TRUE),
    Median = median(flow, na.rm = TRUE),
    SD = sd(flow, na.rm = TRUE),
    IQR = IQR(flow, na.rm = TRUE)
  )
print(result3)
```

    ## # A tibble: 109 × 6
    ##     year Range  Mean Median    SD   IQR
    ##    <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1  1909    0  314    314     NA    0  
    ##  2  1910    0  230    230     NA    0  
    ##  3  1911  258. 135.   135.   183. 129. 
    ##  4  1912  168.  89.9   89.9  119.  84.1
    ##  5  1913  226. 119.   119.   160. 113. 
    ##  6  1914  207. 111.   111.   146. 103. 
    ##  7  1915  229. 121.   121.   162. 115. 
    ##  8  1916  302. 158.   158.   214. 151. 
    ##  9  1917  168.  90.0   90.0  119.  84.0
    ## 10  1918  339. 176.   176.   240. 169. 
    ## # ℹ 99 more rows

``` r
# For plotting the volatility of the flow over the years, I chose to use a bar plot and error bars to look at the range and SD respectively. Specifically for the error bars, I chose to show the standard deviations using the formula mean +/- SD in red.

plot3 <- ggplot(result3, aes(x = year)) +
  geom_bar(aes(y = Range), stat = "identity", fill = "skyblue", alpha = 0.7) +  # First geom layer: Bar plot for Range
  geom_errorbar(aes(ymin = Mean - SD, ymax = Mean + SD), width = 0.2, color = "red", size = 1.5) +  # Second geom layer: Error bars for Standard Deviation
  labs(y = "Flow", title = "Volatility of Flow Across Years")
print(plot3)
```

![](https://github.com/stat545ubc-2023/mda-irvinng98/blob/main/figures/unnamed-chunk-2-3.png)<!-- -->

``` r
# For Question 4: I chose to do task 2 and 8
# Create a new df where the max_flow column contains the max flow rate for each year and the min flow rate is the minimum flow rate of each year. Once again, I grouped by year.
result4 <- data %>%
  group_by(year) %>%
  summarise(
    max_flow = max(flow, na.rm = TRUE),
    min_flow = min(flow, na.rm = TRUE)
  )
print(result4)
```

    ## # A tibble: 109 × 3
    ##     year max_flow min_flow
    ##    <dbl>    <dbl>    <dbl>
    ##  1  1909      314   314   
    ##  2  1910      230   230   
    ##  3  1911      264     5.75
    ##  4  1912      174     5.8 
    ##  5  1913      232     6.12
    ##  6  1914      214     7.16
    ##  7  1915      236     6.94
    ##  8  1916      309     6.97
    ##  9  1917      174     6.06
    ## 10  1918      345     6.03
    ## # ℹ 99 more rows

``` r
# Here I created two line plots visualizing the max and min flow rate from result4. 
plot4 <- ggplot(result4, aes(x = year)) +
  geom_line(aes(y = max_flow), color = "blue", linetype = "solid", size = 1) +
  geom_line(aes(y = min_flow), color = "red", linetype = "solid", size = 1) +
  labs(y = "Flow Rate", title = "Maximum and Minimum Flow Rates Over the Years") +
  theme_minimal()
print(plot4)
```

![](https://github.com/stat545ubc-2023/mda-irvinng98/blob/main/figures/unnamed-chunk-2-4.png)<!-- -->

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

Given the operations completed above, we now have a better understanding
of the overall trends of the sample data. By examining the flow trends
thorughout the year, patterns and seasonal changes in flow rates are
identified. Assessing trends in the missing values over time also
highlights potential data collection issues and periods of incomplete
information. Moreover, analyzing flow volatitiy across diffrent years
and months offers crucial information for downstream analyses (eg.
questions 1,3, and 4). I think my research questions doesn’t necessarily
need to be refined.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

I would argue that my data is tidy since the data fits the format of
“tidy” data (ie. each row is an observation, each column is a variable
and each cell is a value). In particular, each row represents the flow
data from a station at a given timepoint. I would argue that even the NA
cells in the sym column because NA are technically values which
represent missing data.

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
# Before (Making it untidy)
untidy_data <- as.data.frame(t(data))
untidy_data
```

    ##                   V1      V2      V3      V4      V5      V6      V7      V8
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1909    1910    1911    1912    1913    1914    1915    1916
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              7       6       6       8       6       6       6       6
    ## day                7      12      14      25      11      18      27      20
    ## flow          314.00  230.00  264.00  174.00  232.00  214.00  236.00  309.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                   V9     V10     V11     V12     V13     V14     V15     V16
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1917    1918    1919    1920    1921    1922    1923    1924
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       6       6       7       6       6       6       7
    ## day               17      15      22       3       9       5      14       5
    ## flow          174.00  345.00  185.00  248.00  171.00  209.00  377.00  167.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V17     V18     V19     V20     V21     V22     V23     V24
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1925    1926    1927    1928    1929    1930    1931    1932
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              5       7       6       5       6       6       6       6
    ## day               23       7      27      28       2       8      18       3
    ## flow          212.00  130.00  221.00  289.00  215.00  263.00  187.00  279.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V25     V26     V27     V28     V29     V30     V31     V32
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1933    1934    1935    1936    1937    1938    1939    1940
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       5       6       6       6       6       5       5
    ## day               17      31      16       1      17      23      29      26
    ## flow          311.00  269.00  197.00  219.00  148.00  220.00  178.00  204.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V33     V34     V35     V36     V37     V38     V39     V40
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1941    1942    1943    1944    1945    1946    1947    1948
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       7       7       6       6       5       6       6
    ## day               25       5      10      12      22      29      11       9
    ## flow          126.00  166.00  209.00  158.00  126.00  204.00  167.00  292.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V41     V42     V43     V44     V45     V46     V47     V48
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1949    1950    1951    1952    1953    1954    1955    1956
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       6       6       6       6       7       6       6
    ## day                7      21      16      30      14       8      24       5
    ## flow          121.00  286.00  243.00  155.00  260.00  286.00  246.00  257.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V49     V50     V51     V52     V53     V54     V55     V56
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1957    1958    1959    1960    1961    1962    1963    1964
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              5       5       6       7       6       6       6       6
    ## day               20      26      22       1       7      26      19      14
    ## flow          152.00  191.00  212.00  182.00  266.00  184.00  183.00  246.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V57     V58     V59     V60     V61     V62     V63     V64
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1965    1966    1967    1968    1969    1970    1971    1972
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       6       6       6       6       6       6       6
    ## day               19       1      22      27       6       5       7      12
    ## flow          289.00  225.00  275.00  191.00  203.00  155.00  199.00  311.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V65     V66     V67     V68     V69     V70     V71     V72
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1973    1974    1975    1976    1977    1978    1979    1980
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       6       7       7       6       6       6       5
    ## day               24      25      14       1       9       6      13      22
    ## flow          215.00  317.00  144.00  191.00  207.00  189.00  150.00  190.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V73     V74     V75     V76     V77     V78     V79     V80
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1981    1982    1983    1984    1985    1986    1987    1988
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              5       6       5       6       5       6       6       6
    ## day               27      22      31      30      25       2      17       8
    ## flow          227.00  192.00  155.00  220.00  160.00  313.00  146.00  244.00
    ## sym             <NA>    <NA>    <NA>       E    <NA>    <NA>       A    <NA>
    ##                  V81     V82     V83     V84     V85     V86     V87     V88
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1989    1990    1991    1992    1993    1994    1995    1996
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       6       7       6       6       6       6       6
    ## day               16      25       4      13       2      27       7       9
    ## flow          208.00  239.00  242.00  131.00  128.00  147.00  263.00  231.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>       A    <NA>    <NA>
    ##                  V89     V90     V91     V92     V93     V94     V95     V96
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1997    1998    1999    2000    2001    2002    2003    2004
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       5       6       7       5       6       6       6
    ## day                6      28      19       2      29      29      11      12
    ## flow          213.00  133.00  205.00  153.00  165.00  226.00  164.00  162.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                  V97     V98     V99    V100    V101    V102    V103    V104
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            2005    2006    2007    2008    2009    2010    2011    2012
    ## extreme_type maximum maximum maximum maximum maximum maximum maximum maximum
    ## month              6       5       6       7       6       6       6       6
    ## day               23      21       7       1      17      30      23       7
    ## flow          167.00  168.00  298.00  163.00  143.00  138.00  179.00  332.00
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                 V105    V106    V107    V108    V109    V110    V111    V112
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            2013    2014    2015    2016    2018    1909    1910    1911
    ## extreme_type maximum maximum maximum maximum maximum minimum minimum minimum
    ## month              6       6       6       6       5    <NA>    <NA>       2
    ## day               21      26       9       8      27    <NA>    <NA>      27
    ## flow          466.00  178.00  151.00  107.00  199.00    <NA>    <NA>    5.75
    ## sym             <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>    <NA>
    ##                 V113    V114    V115    V116    V117    V118    V119    V120
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1912    1913    1914    1915    1916    1917    1918    1919
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              3       3      11       1       3       2       2       2
    ## day               14      18      17      27       2      23      20      28
    ## flow            5.80    6.12    7.16    6.94    6.97    6.06    6.03    4.56
    ## sym             <NA>       B    <NA>    <NA>       B       B       B       B
    ##                 V121    V122    V123    V124    V125    V126    V127    V128
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1920    1921    1922    1923    1924    1925    1926    1927
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              4       3       3      12       4      12       4       2
    ## day                3      11      27      20      20      31       5      17
    ## flow            5.69    6.37    6.57    6.43    6.26    5.44    5.52    6.48
    ## sym                B       B       B       E    <NA>       B       B       B
    ##                 V129    V130    V131    V132    V133    V134    V135    V136
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1928    1929    1930    1931    1932    1933    1934    1935
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month             12       4      12       3       1      12       2      12
    ## day               29       8      29      26       5      26      26      19
    ## flow            6.09    4.90    6.00    4.70    3.62    5.27    4.08    5.95
    ## sym                B       B       B       B       B       B       B       B
    ##                 V137    V138    V139    V140    V141    V142    V143    V144
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1936    1937    1938    1939    1940    1941    1942    1943
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              2       2       2       2       3      11       2       2
    ## day               16      14      15      10      12      23      17      28
    ## flow            5.64    5.15    6.63    5.49    6.43    6.14    6.37    5.04
    ## sym                B       B       B       B       B    <NA>       B       B
    ##                 V145    V146    V147    V148    V149    V150    V151    V152
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1944    1945    1946    1947    1948    1949    1950    1951
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              3       4       3       3       1      12       3       3
    ## day               13      11      14       4      27      11      31       5
    ## flow            5.55    6.99    7.70    7.31    8.41    6.09    5.04    4.25
    ## sym                B    <NA>       B       B       B       B    <NA>       B
    ##                 V153    V154    V155    V156    V157    V158    V159    V160
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1952    1953    1954    1955    1956    1957    1958    1959
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              4       2       3       2      12       3       3       1
    ## day                1       9      28      11       6       5       1       3
    ## flow            6.06    6.00    6.43    6.74    6.09    5.86    6.14    6.68
    ## sym             <NA>       B       B       B       B       B       B       B
    ##                 V161    V162    V163    V164    V165    V166    V167    V168
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1960    1961    1962    1963    1964    1965    1966    1967
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              2       1       2       2       3       3       3       3
    ## day               27      19       5       2      23      18       3      11
    ## flow            7.25    7.59    5.44    6.43    7.31    7.87    7.93    7.53
    ## sym                B       B       B       B       B       B       B       B
    ##                 V169    V170    V171    V172    V173    V174    V175    V176
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1968    1969    1970    1971    1972    1973    1974    1975
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              4       1      11       3       3       1       3       3
    ## day               22      31      23      22       1       4      22       8
    ## flow            7.53    7.50    5.21    5.41    7.16    6.71    6.09    5.66
    ## sym             <NA>       B       B       B       B       B       B       B
    ##                 V177    V178    V179    V180    V181    V182    V183    V184
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1976    1977    1978    1979    1980    1981    1982    1983
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              3      11       1       2       3       2       1      12
    ## day               23      21      30       9       4      10      16       2
    ## flow            5.95    5.83    5.75    5.40    4.14    7.24    6.15    5.83
    ## sym                B       B       B       B       B       B       B    <NA>
    ##                 V185    V186    V187    V188    V189    V190    V191    V192
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1984    1985    1986    1987    1988    1989    1990    1991
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              3       3       2      11       1       3       3       3
    ## day               14       3      18      30       5      30      23       2
    ## flow            5.91    6.04    5.55    6.19    5.11    6.40    8.12    6.39
    ## sym                B       B       B       B       B       B       B       B
    ##                 V193    V194    V195    V196    V197    V198    V199    V200
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            1992    1993    1994    1995    1996    1997    1998    1999
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month             12       2       2       2       2       2      12       1
    ## day               28      15      25      11      26      28      20      24
    ## flow            6.88    5.64    7.35    4.85    6.51    6.73    6.25    6.68
    ## sym                B       B       B       B       B       B       B       B
    ##                 V201    V202    V203    V204    V205    V206    V207    V208
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            2000    2001    2002    2003    2004    2005    2006    2007
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month             12       3       3       2       1       3       2       2
    ## day               11      15      17      23      26      24      18      14
    ## flow            7.88    6.27    5.58    5.64    7.80    8.44    7.83    7.98
    ## sym                B       B       B       B       B       B       B       B
    ##                 V209    V210    V211    V212    V213    V214    V215    V216
    ## station_id   05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001 05BB001
    ## year            2008    2009    2010    2011    2012    2013    2014    2015
    ## extreme_type minimum minimum minimum minimum minimum minimum minimum minimum
    ## month              3      12       4       1      12       1       1       3
    ## day               31       7       5      30      25       2      28       4
    ## flow            7.00    6.30    6.40    7.13    6.12    5.31    6.09    6.59
    ## sym                B       B       B       B       B       B       B       B
    ##                 V217    V218
    ## station_id   05BB001 05BB001
    ## year            2016    2018
    ## extreme_type minimum minimum
    ## month              3       3
    ## day               28      31
    ## flow            7.70    6.77
    ## sym                B       B

``` r
# After (Making it back to tidy)
tidy_data <- as.data.frame(t(untidy_data),row.names = FALSE)
tidy_data
```

    ##     station_id year extreme_type month  day   flow  sym
    ## 1      05BB001 1909      maximum     7    7 314.00 <NA>
    ## 2      05BB001 1910      maximum     6   12 230.00 <NA>
    ## 3      05BB001 1911      maximum     6   14 264.00 <NA>
    ## 4      05BB001 1912      maximum     8   25 174.00 <NA>
    ## 5      05BB001 1913      maximum     6   11 232.00 <NA>
    ## 6      05BB001 1914      maximum     6   18 214.00 <NA>
    ## 7      05BB001 1915      maximum     6   27 236.00 <NA>
    ## 8      05BB001 1916      maximum     6   20 309.00 <NA>
    ## 9      05BB001 1917      maximum     6   17 174.00 <NA>
    ## 10     05BB001 1918      maximum     6   15 345.00 <NA>
    ## 11     05BB001 1919      maximum     6   22 185.00 <NA>
    ## 12     05BB001 1920      maximum     7    3 248.00 <NA>
    ## 13     05BB001 1921      maximum     6    9 171.00 <NA>
    ## 14     05BB001 1922      maximum     6    5 209.00 <NA>
    ## 15     05BB001 1923      maximum     6   14 377.00 <NA>
    ## 16     05BB001 1924      maximum     7    5 167.00 <NA>
    ## 17     05BB001 1925      maximum     5   23 212.00 <NA>
    ## 18     05BB001 1926      maximum     7    7 130.00 <NA>
    ## 19     05BB001 1927      maximum     6   27 221.00 <NA>
    ## 20     05BB001 1928      maximum     5   28 289.00 <NA>
    ## 21     05BB001 1929      maximum     6    2 215.00 <NA>
    ## 22     05BB001 1930      maximum     6    8 263.00 <NA>
    ## 23     05BB001 1931      maximum     6   18 187.00 <NA>
    ## 24     05BB001 1932      maximum     6    3 279.00 <NA>
    ## 25     05BB001 1933      maximum     6   17 311.00 <NA>
    ## 26     05BB001 1934      maximum     5   31 269.00 <NA>
    ## 27     05BB001 1935      maximum     6   16 197.00 <NA>
    ## 28     05BB001 1936      maximum     6    1 219.00 <NA>
    ## 29     05BB001 1937      maximum     6   17 148.00 <NA>
    ## 30     05BB001 1938      maximum     6   23 220.00 <NA>
    ## 31     05BB001 1939      maximum     5   29 178.00 <NA>
    ## 32     05BB001 1940      maximum     5   26 204.00 <NA>
    ## 33     05BB001 1941      maximum     6   25 126.00 <NA>
    ## 34     05BB001 1942      maximum     7    5 166.00 <NA>
    ## 35     05BB001 1943      maximum     7   10 209.00 <NA>
    ## 36     05BB001 1944      maximum     6   12 158.00 <NA>
    ## 37     05BB001 1945      maximum     6   22 126.00 <NA>
    ## 38     05BB001 1946      maximum     5   29 204.00 <NA>
    ## 39     05BB001 1947      maximum     6   11 167.00 <NA>
    ## 40     05BB001 1948      maximum     6    9 292.00 <NA>
    ## 41     05BB001 1949      maximum     6    7 121.00 <NA>
    ## 42     05BB001 1950      maximum     6   21 286.00 <NA>
    ## 43     05BB001 1951      maximum     6   16 243.00 <NA>
    ## 44     05BB001 1952      maximum     6   30 155.00 <NA>
    ## 45     05BB001 1953      maximum     6   14 260.00 <NA>
    ## 46     05BB001 1954      maximum     7    8 286.00 <NA>
    ## 47     05BB001 1955      maximum     6   24 246.00 <NA>
    ## 48     05BB001 1956      maximum     6    5 257.00 <NA>
    ## 49     05BB001 1957      maximum     5   20 152.00 <NA>
    ## 50     05BB001 1958      maximum     5   26 191.00 <NA>
    ## 51     05BB001 1959      maximum     6   22 212.00 <NA>
    ## 52     05BB001 1960      maximum     7    1 182.00 <NA>
    ## 53     05BB001 1961      maximum     6    7 266.00 <NA>
    ## 54     05BB001 1962      maximum     6   26 184.00 <NA>
    ## 55     05BB001 1963      maximum     6   19 183.00 <NA>
    ## 56     05BB001 1964      maximum     6   14 246.00 <NA>
    ## 57     05BB001 1965      maximum     6   19 289.00 <NA>
    ## 58     05BB001 1966      maximum     6    1 225.00 <NA>
    ## 59     05BB001 1967      maximum     6   22 275.00 <NA>
    ## 60     05BB001 1968      maximum     6   27 191.00 <NA>
    ## 61     05BB001 1969      maximum     6    6 203.00 <NA>
    ## 62     05BB001 1970      maximum     6    5 155.00 <NA>
    ## 63     05BB001 1971      maximum     6    7 199.00 <NA>
    ## 64     05BB001 1972      maximum     6   12 311.00 <NA>
    ## 65     05BB001 1973      maximum     6   24 215.00 <NA>
    ## 66     05BB001 1974      maximum     6   25 317.00 <NA>
    ## 67     05BB001 1975      maximum     7   14 144.00 <NA>
    ## 68     05BB001 1976      maximum     7    1 191.00 <NA>
    ## 69     05BB001 1977      maximum     6    9 207.00 <NA>
    ## 70     05BB001 1978      maximum     6    6 189.00 <NA>
    ## 71     05BB001 1979      maximum     6   13 150.00 <NA>
    ## 72     05BB001 1980      maximum     5   22 190.00 <NA>
    ## 73     05BB001 1981      maximum     5   27 227.00 <NA>
    ## 74     05BB001 1982      maximum     6   22 192.00 <NA>
    ## 75     05BB001 1983      maximum     5   31 155.00 <NA>
    ## 76     05BB001 1984      maximum     6   30 220.00    E
    ## 77     05BB001 1985      maximum     5   25 160.00 <NA>
    ## 78     05BB001 1986      maximum     6    2 313.00 <NA>
    ## 79     05BB001 1987      maximum     6   17 146.00    A
    ## 80     05BB001 1988      maximum     6    8 244.00 <NA>
    ## 81     05BB001 1989      maximum     6   16 208.00 <NA>
    ## 82     05BB001 1990      maximum     6   25 239.00 <NA>
    ## 83     05BB001 1991      maximum     7    4 242.00 <NA>
    ## 84     05BB001 1992      maximum     6   13 131.00 <NA>
    ## 85     05BB001 1993      maximum     6    2 128.00 <NA>
    ## 86     05BB001 1994      maximum     6   27 147.00    A
    ## 87     05BB001 1995      maximum     6    7 263.00 <NA>
    ## 88     05BB001 1996      maximum     6    9 231.00 <NA>
    ## 89     05BB001 1997      maximum     6    6 213.00 <NA>
    ## 90     05BB001 1998      maximum     5   28 133.00 <NA>
    ## 91     05BB001 1999      maximum     6   19 205.00 <NA>
    ## 92     05BB001 2000      maximum     7    2 153.00 <NA>
    ## 93     05BB001 2001      maximum     5   29 165.00 <NA>
    ## 94     05BB001 2002      maximum     6   29 226.00 <NA>
    ## 95     05BB001 2003      maximum     6   11 164.00 <NA>
    ## 96     05BB001 2004      maximum     6   12 162.00 <NA>
    ## 97     05BB001 2005      maximum     6   23 167.00 <NA>
    ## 98     05BB001 2006      maximum     5   21 168.00 <NA>
    ## 99     05BB001 2007      maximum     6    7 298.00 <NA>
    ## 100    05BB001 2008      maximum     7    1 163.00 <NA>
    ## 101    05BB001 2009      maximum     6   17 143.00 <NA>
    ## 102    05BB001 2010      maximum     6   30 138.00 <NA>
    ## 103    05BB001 2011      maximum     6   23 179.00 <NA>
    ## 104    05BB001 2012      maximum     6    7 332.00 <NA>
    ## 105    05BB001 2013      maximum     6   21 466.00 <NA>
    ## 106    05BB001 2014      maximum     6   26 178.00 <NA>
    ## 107    05BB001 2015      maximum     6    9 151.00 <NA>
    ## 108    05BB001 2016      maximum     6    8 107.00 <NA>
    ## 109    05BB001 2018      maximum     5   27 199.00 <NA>
    ## 110    05BB001 1909      minimum  <NA> <NA>   <NA> <NA>
    ## 111    05BB001 1910      minimum  <NA> <NA>   <NA> <NA>
    ## 112    05BB001 1911      minimum     2   27   5.75 <NA>
    ## 113    05BB001 1912      minimum     3   14   5.80 <NA>
    ## 114    05BB001 1913      minimum     3   18   6.12    B
    ## 115    05BB001 1914      minimum    11   17   7.16 <NA>
    ## 116    05BB001 1915      minimum     1   27   6.94 <NA>
    ## 117    05BB001 1916      minimum     3    2   6.97    B
    ## 118    05BB001 1917      minimum     2   23   6.06    B
    ## 119    05BB001 1918      minimum     2   20   6.03    B
    ## 120    05BB001 1919      minimum     2   28   4.56    B
    ## 121    05BB001 1920      minimum     4    3   5.69    B
    ## 122    05BB001 1921      minimum     3   11   6.37    B
    ## 123    05BB001 1922      minimum     3   27   6.57    B
    ## 124    05BB001 1923      minimum    12   20   6.43    E
    ## 125    05BB001 1924      minimum     4   20   6.26 <NA>
    ## 126    05BB001 1925      minimum    12   31   5.44    B
    ## 127    05BB001 1926      minimum     4    5   5.52    B
    ## 128    05BB001 1927      minimum     2   17   6.48    B
    ## 129    05BB001 1928      minimum    12   29   6.09    B
    ## 130    05BB001 1929      minimum     4    8   4.90    B
    ## 131    05BB001 1930      minimum    12   29   6.00    B
    ## 132    05BB001 1931      minimum     3   26   4.70    B
    ## 133    05BB001 1932      minimum     1    5   3.62    B
    ## 134    05BB001 1933      minimum    12   26   5.27    B
    ## 135    05BB001 1934      minimum     2   26   4.08    B
    ## 136    05BB001 1935      minimum    12   19   5.95    B
    ## 137    05BB001 1936      minimum     2   16   5.64    B
    ## 138    05BB001 1937      minimum     2   14   5.15    B
    ## 139    05BB001 1938      minimum     2   15   6.63    B
    ## 140    05BB001 1939      minimum     2   10   5.49    B
    ## 141    05BB001 1940      minimum     3   12   6.43    B
    ## 142    05BB001 1941      minimum    11   23   6.14 <NA>
    ## 143    05BB001 1942      minimum     2   17   6.37    B
    ## 144    05BB001 1943      minimum     2   28   5.04    B
    ## 145    05BB001 1944      minimum     3   13   5.55    B
    ## 146    05BB001 1945      minimum     4   11   6.99 <NA>
    ## 147    05BB001 1946      minimum     3   14   7.70    B
    ## 148    05BB001 1947      minimum     3    4   7.31    B
    ## 149    05BB001 1948      minimum     1   27   8.41    B
    ## 150    05BB001 1949      minimum    12   11   6.09    B
    ## 151    05BB001 1950      minimum     3   31   5.04 <NA>
    ## 152    05BB001 1951      minimum     3    5   4.25    B
    ## 153    05BB001 1952      minimum     4    1   6.06 <NA>
    ## 154    05BB001 1953      minimum     2    9   6.00    B
    ## 155    05BB001 1954      minimum     3   28   6.43    B
    ## 156    05BB001 1955      minimum     2   11   6.74    B
    ## 157    05BB001 1956      minimum    12    6   6.09    B
    ## 158    05BB001 1957      minimum     3    5   5.86    B
    ## 159    05BB001 1958      minimum     3    1   6.14    B
    ## 160    05BB001 1959      minimum     1    3   6.68    B
    ## 161    05BB001 1960      minimum     2   27   7.25    B
    ## 162    05BB001 1961      minimum     1   19   7.59    B
    ## 163    05BB001 1962      minimum     2    5   5.44    B
    ## 164    05BB001 1963      minimum     2    2   6.43    B
    ## 165    05BB001 1964      minimum     3   23   7.31    B
    ## 166    05BB001 1965      minimum     3   18   7.87    B
    ## 167    05BB001 1966      minimum     3    3   7.93    B
    ## 168    05BB001 1967      minimum     3   11   7.53    B
    ## 169    05BB001 1968      minimum     4   22   7.53 <NA>
    ## 170    05BB001 1969      minimum     1   31   7.50    B
    ## 171    05BB001 1970      minimum    11   23   5.21    B
    ## 172    05BB001 1971      minimum     3   22   5.41    B
    ## 173    05BB001 1972      minimum     3    1   7.16    B
    ## 174    05BB001 1973      minimum     1    4   6.71    B
    ## 175    05BB001 1974      minimum     3   22   6.09    B
    ## 176    05BB001 1975      minimum     3    8   5.66    B
    ## 177    05BB001 1976      minimum     3   23   5.95    B
    ## 178    05BB001 1977      minimum    11   21   5.83    B
    ## 179    05BB001 1978      minimum     1   30   5.75    B
    ## 180    05BB001 1979      minimum     2    9   5.40    B
    ## 181    05BB001 1980      minimum     3    4   4.14    B
    ## 182    05BB001 1981      minimum     2   10   7.24    B
    ## 183    05BB001 1982      minimum     1   16   6.15    B
    ## 184    05BB001 1983      minimum    12    2   5.83 <NA>
    ## 185    05BB001 1984      minimum     3   14   5.91    B
    ## 186    05BB001 1985      minimum     3    3   6.04    B
    ## 187    05BB001 1986      minimum     2   18   5.55    B
    ## 188    05BB001 1987      minimum    11   30   6.19    B
    ## 189    05BB001 1988      minimum     1    5   5.11    B
    ## 190    05BB001 1989      minimum     3   30   6.40    B
    ## 191    05BB001 1990      minimum     3   23   8.12    B
    ## 192    05BB001 1991      minimum     3    2   6.39    B
    ## 193    05BB001 1992      minimum    12   28   6.88    B
    ## 194    05BB001 1993      minimum     2   15   5.64    B
    ## 195    05BB001 1994      minimum     2   25   7.35    B
    ## 196    05BB001 1995      minimum     2   11   4.85    B
    ## 197    05BB001 1996      minimum     2   26   6.51    B
    ## 198    05BB001 1997      minimum     2   28   6.73    B
    ## 199    05BB001 1998      minimum    12   20   6.25    B
    ## 200    05BB001 1999      minimum     1   24   6.68    B
    ## 201    05BB001 2000      minimum    12   11   7.88    B
    ## 202    05BB001 2001      minimum     3   15   6.27    B
    ## 203    05BB001 2002      minimum     3   17   5.58    B
    ## 204    05BB001 2003      minimum     2   23   5.64    B
    ## 205    05BB001 2004      minimum     1   26   7.80    B
    ## 206    05BB001 2005      minimum     3   24   8.44    B
    ## 207    05BB001 2006      minimum     2   18   7.83    B
    ## 208    05BB001 2007      minimum     2   14   7.98    B
    ## 209    05BB001 2008      minimum     3   31   7.00    B
    ## 210    05BB001 2009      minimum    12    7   6.30    B
    ## 211    05BB001 2010      minimum     4    5   6.40    B
    ## 212    05BB001 2011      minimum     1   30   7.13    B
    ## 213    05BB001 2012      minimum    12   25   6.12    B
    ## 214    05BB001 2013      minimum     1    2   5.31    B
    ## 215    05BB001 2014      minimum     1   28   6.09    B
    ## 216    05BB001 2015      minimum     3    4   6.59    B
    ## 217    05BB001 2016      minimum     3   28   7.70    B
    ## 218    05BB001 2018      minimum     3   31   6.77    B

``` r
# Given the definition of tidy data, by transposing our data, our data would inherently be untidy. Here, each row is a variable and each column is an observation.
```

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

Here are the 2 research questions that I want to pursue 1. Is there a
flow trend for each year (ie. how does the flow change throughout a
year)? 2. Are there years or months where flow is more volatile?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

I still think the flow trends are a very interesting point to address
and would like to continue exploring the temporal impact on it, but also
the temporal impact on the flow’s volatility.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

``` r
# Here I filter out the rows where flow has NA values and then remove the station_id and sym columns (using the select function) as they are not useful for my analysis. I then use rename to make the column names better (ie. capitalize the first letter) and finally I grouped by year.
updated_data <- data %>%
  filter(!is.na(flow)) %>%  # Remove rows with NA values in the flow column
  select(-station_id, -sym) %>%  # Remove station_id and sym columns
  rename(Year = year, Month = month, Day = day, FlowRate = flow) %>%  # Rename columns
  group_by(Year)

updated_data
```

    ## # A tibble: 216 × 5
    ## # Groups:   Year [109]
    ##     Year extreme_type Month   Day FlowRate
    ##    <dbl> <chr>        <dbl> <dbl>    <dbl>
    ##  1  1909 maximum          7     7      314
    ##  2  1910 maximum          6    12      230
    ##  3  1911 maximum          6    14      264
    ##  4  1912 maximum          8    25      174
    ##  5  1913 maximum          6    11      232
    ##  6  1914 maximum          6    18      214
    ##  7  1915 maximum          6    27      236
    ##  8  1916 maximum          6    20      309
    ##  9  1917 maximum          6    17      174
    ## 10  1918 maximum          6    15      345
    ## # ℹ 206 more rows

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Is there a flow trend for each year (ie. how does
the flow change throughout a year)?

**Variable of interest**: Flow Rate

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
# Use the linear regression model function lm() to fit linear regression model to look at the relationship between flow rate and year
flow_model <- lm(FlowRate ~ Year, data = updated_data)
summary(flow_model)
```

    ## 
    ## Call:
    ## lm(formula = FlowRate ~ Year, data = updated_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -117.03 -102.97    8.45   92.11  367.80 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) 583.1516   479.7697   1.215    0.226
    ## Year         -0.2409     0.2443  -0.986    0.325
    ## 
    ## Residual standard error: 112 on 214 degrees of freedom
    ## Multiple R-squared:  0.004523,   Adjusted R-squared:  -0.0001287 
    ## F-statistic: 0.9723 on 1 and 214 DF,  p-value: 0.3252

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
# Here i am making a tibble which would have the info from the regression model (eg. p values, std error etc.). I am using this to assess the relationship between time (year) and the flow rate.

library(broom)
```

    ## Warning: package 'broom' was built under R version 4.2.3

``` r
# Extract regression coefficients and statistics (with tidy() from broom) then look at the model summary
flow_model_summary <- tidy(flow_model)
flow_model_summary
```

    ## # A tibble: 2 × 5
    ##   term        estimate std.error statistic p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 (Intercept)  583.      480.        1.22    0.226
    ## 2 Year          -0.241     0.244    -0.986   0.325

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
write.csv(result1, file = here::here("output", "result1.csv"), row.names = FALSE)
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(flow_model, file = here::here("output", "flow_model.rds"))
```

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
