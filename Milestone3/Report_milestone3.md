Milestone 3
================
Heather VT., Kera Y., Marcelle C., Wilson D.
4/12/2019

Introduction
------------

The current analysis seeks to explore the relationship between `self-rated sustainability importance` and `recycling frequency`. Additionally, the analysis also aims to determine whether this relationship is confounded by an `individual’s age` and `background (environmentally-conscious family)`, and analyze if a video about recycling changes individuals’ opinions on the importance of sustainability. With this purpose, we published a [survey](https://ubc.ca1.qualtrics.com/jfe/form/SV_4SJCJH59wUakrEF) using [UBC-hosted version of Qualtrics](https://ubc.ca1.qualtrics.com/) to collect responses to be used in this analysis.

Methods
-------

### Survey study design

### Data collection methods

> Online survey with Qualtrics UBC
>
> Wanted to

### Analysis methods

Results
-------

``` r
# load data
tidy_data <- read.csv("../data/tidy_data.csv")

# factor relevel
tidy_data$recycling_freq <- tidy_data$recycling_freq %>% fct_relevel("Rarely","Sometimes","Usually","Always")
tidy_data$background <- tidy_data$background %>% fct_relevel("Yes","No")
tidy_data$watch <- tidy_data$watch %>% fct_relevel("Have watched","Will pass")
```

``` r
# Creating summary table
summary(tidy_data)
```

    ##  self_rating_before   recycling_freq    age     background
    ##  Min.   : 1.000     Rarely   : 2     20-24:20   Yes:30    
    ##  1st Qu.: 7.000     Sometimes: 2     25-29:19   No :38    
    ##  Median : 8.000     Usually  :31     30-34:11             
    ##  Mean   : 7.912     Always   :33     35-39: 3             
    ##  3rd Qu.: 9.000                      40+  :15             
    ##  Max.   :10.000                                           
    ##                                                           
    ##           watch    self_rating_after
    ##  Have watched:46   Min.   : 1.000   
    ##  Will pass   :15   1st Qu.: 7.500   
    ##  NA's        : 7   Median : 9.000   
    ##                    Mean   : 8.119   
    ##                    3rd Qu.: 9.000   
    ##                    Max.   :10.000   
    ##                    NA's   :9

``` r
head(tidy_data)
```

    ##   self_rating_before recycling_freq   age background        watch
    ## 1                  7        Usually 20-24        Yes         <NA>
    ## 2                  1        Usually 30-34         No         <NA>
    ## 3                  7        Usually 20-24         No Have watched
    ## 4                  9         Always 30-34         No Have watched
    ## 5                  3        Usually 20-24         No Have watched
    ## 6                  9        Usually 25-29         No Have watched
    ##   self_rating_after
    ## 1                NA
    ## 2                 6
    ## 3                 9
    ## 4                 9
    ## 5                 3
    ## 6                 9

``` r
#simple_model= lm(data=tidy_data, recycling_freq ~ self_rating_before)
#age_confounder= lm(data=tidy_data, recycling_freq ~ self_rating_before + age)
#background_confounder= lm(data=tidy_data, recycling_freq ~ self_rating_before + background)
#interaction_effects = lm(data=tidy_data, recycling_freq ~ self_rating_before + background*age)

#summary(simple_model)
```

Discussion
----------

### Discussion of results

### Discussion of survey/study design

### NOTES for reference

> 3-5 page report

    > Your target audience is other Data Scientists who are not familiar with your project.
    Clearly introduce the survey topic and question you were interested in answering.
    Link to your study's data and code in the methods section of your report.
    Include effective visualizations and/or tables that help communicate your findings.
    Your discussion should have 2 key focuses:
        Discussing the results and findings of your survey and analysis of the survey data.
        Discussing your survey/study design, specifically:
        what did you do well to make this study as causal as possible?
        what was not done well and how did that effect your studies conclusions?
        what would you do differently next time to improve your survey/study design and why?
