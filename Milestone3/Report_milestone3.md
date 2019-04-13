Milestone3\_report
================

Wilson D., Marcelle C., Kera Y., Heather VT. 4/12/2019

Introduction and 0uestion
-------------------------

This survey aimed to answer the question *"Does one's attitudes towards the importance of sustainability affect how often they recycle?"*. The dataset columns description are as follows:

-   `self_rating_before`: How important is being environmentally sustainable to you on a scale from 1-10?
-   `recycling_freq`: How often do you generally recycle?
    -   answer: Always, Usually, sometimes, rarely, never
-   `age`: What is your age group?
    -   answers: &lt;19, 20-24, 25-29, 30-34, 35-39, 40+
-   `background`: Did you grow up in an environmentally-conscious family? - answers: Yes, No
-   `watch`: Will you watch the video? -answers: Yes, No
-   `self_rating_after`: After watching this video about recycling, how important is being environmentally sustainable to you on a scale from 1-10?

Methods
-------

### Survey study design

### Data collection methods

> Online survey with Qualtrics UBC

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
class(tidy_data$recycling_freq)
```

    ## [1] "factor"

``` r
tidy_data <- tidy_data %>%
  mutate(recycling_freq_nf = if_else(recycling_freq =="Always", 
                                     5, if_else(recycling_freq == "Usually",
                                     4, if_else(recycling_freq == "Sometimes",
                                     3, if_else(recycling_freq == "Rarely",
                                     2, if_else(recycling_freq == "Never",
                                     1, 0)))))) 


tidy_data          
```

    ##    self_rating_before recycling_freq   age background        watch
    ## 1                   7        Usually 20-24        Yes         <NA>
    ## 2                   1        Usually 30-34         No         <NA>
    ## 3                   7        Usually 20-24         No Have watched
    ## 4                   9         Always 30-34         No Have watched
    ## 5                   3        Usually 20-24         No Have watched
    ## 6                   9        Usually 25-29         No Have watched
    ## 7                   9         Always 30-34         No Have watched
    ## 8                   7      Sometimes 20-24        Yes    Will pass
    ## 9                   1         Rarely 25-29         No    Will pass
    ## 10                  8        Usually 20-24         No    Will pass
    ## 11                  7        Usually 20-24        Yes Have watched
    ## 12                  9        Usually 25-29         No    Will pass
    ## 13                  9        Usually 35-39        Yes    Will pass
    ## 14                  7        Usually 20-24         No         <NA>
    ## 15                  7         Always 25-29        Yes Have watched
    ## 16                  7        Usually 25-29        Yes Have watched
    ## 17                  9        Usually 35-39        Yes Have watched
    ## 18                  4        Usually 25-29         No Have watched
    ## 19                  5        Usually 25-29        Yes Have watched
    ## 20                  6      Sometimes 25-29         No    Will pass
    ## 21                  6        Usually 30-34         No Have watched
    ## 22                 10         Always 20-24        Yes Have watched
    ## 23                  8         Always 30-34         No Have watched
    ## 24                  8        Usually 30-34        Yes Have watched
    ## 25                  8        Usually   40+         No Have watched
    ## 26                 10         Always 25-29        Yes Have watched
    ## 27                  9        Usually 20-24        Yes    Will pass
    ## 28                 10         Rarely   40+         No Have watched
    ## 29                  6        Usually 25-29         No    Will pass
    ## 30                  9        Usually 20-24         No         <NA>
    ## 31                  8         Always 25-29         No    Will pass
    ## 32                  7        Usually 25-29         No    Will pass
    ## 33                 10         Always 25-29        Yes    Will pass
    ## 34                  7         Always 20-24         No Have watched
    ## 35                  9         Always   40+         No Have watched
    ## 36                  5        Usually 25-29        Yes Have watched
    ## 37                  9        Usually 30-34         No Have watched
    ## 38                  6        Usually 25-29         No    Will pass
    ## 39                  8         Always 20-24        Yes Have watched
    ## 40                  9         Always 30-34         No Have watched
    ## 41                 10         Always 35-39         No Have watched
    ## 42                  8         Always 25-29        Yes         <NA>
    ## 43                  9         Always 20-24        Yes Have watched
    ## 44                 10         Always 20-24        Yes    Will pass
    ## 45                  8         Always 20-24        Yes Have watched
    ## 46                  7         Always 30-34        Yes Have watched
    ## 47                  6        Usually 20-24        Yes    Will pass
    ## 48                  8         Always   40+         No Have watched
    ## 49                  9         Always   40+         No Have watched
    ## 50                  8         Always 30-34        Yes Have watched
    ## 51                  8        Usually 20-24        Yes Have watched
    ## 52                  9         Always 25-29        Yes Have watched
    ## 53                  7        Usually 20-24         No         <NA>
    ## 54                  8         Always   40+         No Have watched
    ## 55                  9        Usually 30-34        Yes    Will pass
    ## 56                 10         Always   40+        Yes         <NA>
    ## 57                 10         Always   40+        Yes Have watched
    ## 58                 10         Always   40+        Yes Have watched
    ## 59                 10         Always 20-24         No Have watched
    ## 60                  9        Usually   40+         No Have watched
    ## 61                 10         Always 25-29         No Have watched
    ## 62                  9         Always 20-24        Yes Have watched
    ## 63                  9         Always   40+         No Have watched
    ## 64                 10         Always   40+         No Have watched
    ## 65                  9         Always   40+         No Have watched
    ## 66                 10         Always   40+        Yes Have watched
    ## 67                  7        Usually   40+         No Have watched
    ## 68                  8        Usually 25-29         No Have watched
    ##    self_rating_after recycling_freq_nf
    ## 1                 NA                 4
    ## 2                  6                 4
    ## 3                  9                 4
    ## 4                  9                 5
    ## 5                  3                 4
    ## 6                  9                 4
    ## 7                  9                 5
    ## 8                 NA                 3
    ## 9                  1                 2
    ## 10                NA                 4
    ## 11                 8                 4
    ## 12                 9                 4
    ## 13                NA                 4
    ## 14                NA                 4
    ## 15                 8                 5
    ## 16                 8                 4
    ## 17                 9                 4
    ## 18                 4                 4
    ## 19                 8                 4
    ## 20                 6                 3
    ## 21                 6                 4
    ## 22                10                 5
    ## 23                 9                 5
    ## 24                 9                 4
    ## 25                10                 4
    ## 26                10                 5
    ## 27                 5                 4
    ## 28                10                 2
    ## 29                 7                 4
    ## 30                10                 4
    ## 31                NA                 5
    ## 32                 7                 4
    ## 33                10                 5
    ## 34                 7                 5
    ## 35                 9                 5
    ## 36                 5                 4
    ## 37                 9                 4
    ## 38                 5                 4
    ## 39                 9                 5
    ## 40                 7                 5
    ## 41                10                 5
    ## 42                 8                 5
    ## 43                 9                 5
    ## 44                NA                 5
    ## 45                 8                 5
    ## 46                 7                 5
    ## 47                NA                 4
    ## 48                 8                 5
    ## 49                10                 5
    ## 50                 8                 5
    ## 51                 9                 4
    ## 52                 9                 5
    ## 53                 8                 4
    ## 54                 8                 5
    ## 55                NA                 4
    ## 56                10                 5
    ## 57                10                 5
    ## 58                10                 5
    ## 59                10                 5
    ## 60                 9                 4
    ## 61                 5                 5
    ## 62                 9                 5
    ## 63                10                 5
    ## 64                10                 5
    ## 65                 9                 5
    ## 66                 8                 5
    ## 67                 9                 4
    ## 68                 8                 4

``` r
simple_model= lm(data=tidy_data, recycling_freq_nf ~ self_rating_before)
age_confounder= lm(data=tidy_data, recycling_freq_nf ~ self_rating_before + age)
background_confounder= lm(data=tidy_data, recycling_freq_nf ~ self_rating_before + background)
interaction_effects = lm(data=tidy_data, recycling_freq_nf ~ self_rating_before + background*age)

summary(simple_model)
```

    ## 
    ## Call:
    ## lm(formula = recycling_freq_nf ~ self_rating_before, data = tidy_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.7704 -0.4128  0.2296  0.4084  0.8385 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)         2.98277    0.30500   9.779 1.83e-14 ***
    ## self_rating_before  0.17876    0.03743   4.776 1.03e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6029 on 66 degrees of freedom
    ## Multiple R-squared:  0.2569, Adjusted R-squared:  0.2456 
    ## F-statistic: 22.81 on 1 and 66 DF,  p-value: 1.033e-05

> The simple model with just X and Y has a significant F-statistic, meaning we can reject the null hypothesis that there is no effect of X on Y.

> In order to understand if there are confounding variable though, we need to take a closer look and compare the simple linear model with the linear models that include confounding variables.

``` r
anova(simple_model, age_confounder)
```

    ## Analysis of Variance Table
    ## 
    ## Model 1: recycling_freq_nf ~ self_rating_before
    ## Model 2: recycling_freq_nf ~ self_rating_before + age
    ##   Res.Df    RSS Df Sum of Sq      F Pr(>F)
    ## 1     66 23.988                           
    ## 2     62 23.122  4   0.86641 0.5808 0.6776

> Age does not seem to be a significant factor in predicting recycling frequency. p-value&gt;0.05

``` r
#summary(background_confounder)
anova(simple_model, background_confounder)
```

    ## Analysis of Variance Table
    ## 
    ## Model 1: recycling_freq_nf ~ self_rating_before
    ## Model 2: recycling_freq_nf ~ self_rating_before + background
    ##   Res.Df    RSS Df Sum of Sq      F Pr(>F)
    ## 1     66 23.988                           
    ## 2     65 23.740  1   0.24781 0.6785 0.4131

> Background does not seem to be a significant factor in predicting recycling frequency.

``` r
summary(interaction_effects)
```

    ## 
    ## Call:
    ## lm(formula = recycling_freq_nf ~ self_rating_before + background * 
    ##     age, data = tidy_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.6688 -0.2381  0.0000  0.4487  0.8766 
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)            3.04157    0.38662   7.867 1.14e-10 ***
    ## self_rating_before     0.16838    0.04224   3.986 0.000193 ***
    ## backgroundNo          -0.01232    0.27866  -0.044 0.964892    
    ## age25-29               0.29954    0.27690   1.082 0.283919    
    ## age30-34               0.11140    0.34913   0.319 0.750840    
    ## age35-39              -0.55698    0.46310  -1.203 0.234056    
    ## age40+                 0.27464    0.35755   0.768 0.445587    
    ## backgroundNo:age25-29 -0.55243    0.39379  -1.403 0.166085    
    ## backgroundNo:age30-34  0.20402    0.46885   0.435 0.665102    
    ## backgroundNo:age35-39  0.84394    0.79435   1.062 0.292523    
    ## backgroundNo:age40+   -0.31883    0.44832  -0.711 0.479872    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6046 on 57 degrees of freedom
    ## Multiple R-squared:  0.3545, Adjusted R-squared:  0.2413 
    ## F-statistic: 3.131 on 10 and 57 DF,  p-value: 0.002997

> There are no interaction effects, so we can just look at the single variables as potential confounders... (better explanation needed)

``` r
all_variables= lm(data=tidy_data, recycling_freq_nf ~ self_rating_before + background + age)

summary(all_variables)
```

    ## 
    ## Call:
    ## lm(formula = recycling_freq_nf ~ self_rating_before + background + 
    ##     age, data = tidy_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.7215 -0.2918  0.1179  0.4111  0.8872 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)         3.04086    0.37623   8.083 3.17e-11 ***
    ## self_rating_before  0.17607    0.04237   4.156 0.000103 ***
    ## backgroundNo       -0.16052    0.15897  -1.010 0.316589    
    ## age25-29            0.01157    0.19888   0.058 0.953785    
    ## age30-34            0.27821    0.23211   1.199 0.235306    
    ## age35-39           -0.29734    0.38331  -0.776 0.440912    
    ## age40+              0.08049    0.22470   0.358 0.721438    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6106 on 61 degrees of freedom
    ## Multiple R-squared:  0.2955, Adjusted R-squared:  0.2262 
    ## F-statistic: 4.264 on 6 and 61 DF,  p-value: 0.001192

> including all predictive variables in the model shows that on average, for each unit increase in X, there is a 0.176 increase in y. This means that for every increase in self-importance of sustainability rating there is a 0.17 increase in frequency of recycling. The p-value is 0.000103, which is significant with an alpha of 0.05. Therefore, we can reject the null hypothesis and say that there is a positive relationship between self-rated importance of being sustianable and one's frequency of recycling.

#### Important assumptions:

-   1.  There are only two possible confounding variables: age, background. In the literature it appears that income is a significant predictor of recycling frequency, which we did not include in our survey. (Schultz et al. 1995)

-   1.  The strength of the (X,Y) association within each strata defined by the confounders is the same.

-   1.  There is a simple linear relationshing in how Y varies accross the strata.

Discussion
----------

### Discussion of results

### Discussion of survey/study design

### References

1.  Schultz, P. W., Oskamp, S., & Mainieri, T. (1995). Who recycles and when? A review of personal and situational factors. Journal of Environmental Psychology, 15(2), 105-121. <http://dx.doi.org/10.1016/0272-4944(95)90019-5>.

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
