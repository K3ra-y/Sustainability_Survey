Milestone 3
================
Heather VT., Kera Y., Marcelle C., Wilson D.
April 13th, 2019

## Introduction

The current analysis seeks to explore the relationship between `self-rated sustainability importance` and `recycling frequency`. Additionally, the analysis also aims to determine whether this relationship is confounded by an `age of the individual` and `background (environmentally-conscious family)`, and analyze whether a video about personal sustainability impacts and changes influences one's opinions on the importance of sustainability. With this purpose, we published a [survey](https://ubc.ca1.qualtrics.com/jfe/form/SV_4SJCJH59wUakrEF) using [UBC-hosted version of Qualtrics](https://ubc.ca1.qualtrics.com/) to collect responses to be used in this analysis.

## Methods

### Survey study design

We asked four questions, with one bonus question if participants decided to watch the video. Study participation consent was obtainened by listing the expectations of their involvement, purpose of the study and noting that participation is optional. All data will be stored by UBC qualtrics and no identifying information will be publically released. We designed the questions to allow us to answer our research question: How does one's opinion of the importance of being sustainable affect their recycling habits? We planned to test whether our data meets the assumptions of ordinary least squares linear regression, and ordinal regression, and then choose the model that our data does not violate the assumptions of. We added an additional question to assess if their opinion changed after watching a 2-minute [video](https://www.youtube.com/watch?v=Zsc8G0NnMTs) about personal habits that can change your climate impact.

Q1: How important is being environmentally sustainable to you on a scale from 1-10? Q2: How often do you generally recycle? Q3: What is your age group? Q4: Did you grow up in an environmentally-conscious family? Q5: After watching [this video](https://www.youtube.com/watch?v=Zsc8G0NnMTs) about recycling, how important is sustainability to you on a self-ranked scale from 1-10 scale?

Null hypothesis: There is no relationship between self-rated opinion of the importance of environmental sustainability, and the frequency of one's recycling behaviours.

Alternate Hypothesis: There is a relationship between self-rated opinion of the importance of environmental sustainability and the frequency of one's recycling habits.

Second analysis quesion: Does watchinga video significantly change one's opinions of the importance of being sustainable? Null hypothesis: Watching this video does not change one's self-rated importance of being sustainable. Alternate hypothesis: Watching this video doe change one's self-ratedd importance of being sustianable.

We plan to analyze this additional question using a paired t-test.

### Data collection methods

Data was collected through an online survey from participants who agreed to give consent to share their data to be used to address our survey objective. Participants rated themselves in terms of how important environmental sustainability is to them (main predictor). We also collected data on frequency of recycling habits, age of the participants and whether or not the participant was brought up in environmentally consious family. Our assumption is that the main response is frequency of recycling, and age and background are possible confounding factors. Data was then downloaded and stored on a private github repository, where it was cleaned and de-identified before publishing to the [public repository](https://github.com/UBC-MDS/Sustainability_Survey/tree/master/data).

### Analysis methods

After some exploratory data analysis done in [Milestone 2](https://github.com/UBC-MDS/Sustainability_Survey/blob/master/Milestone2/Milestone2_EDA.md) we noticed that the ages were not evenly distributed (Figure 1). Therefore, we decided to regroup the ages from 30-39 into one age group.

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
    ##   self_rating_after recycling_freq_nf
    ## 1                NA                 4
    ## 2                 6                 4
    ## 3                 9                 4
    ## 4                 9                 5
    ## 5                 3                 4
    ## 6                 9                 4

``` r
tidy_data <- tidy_data %>%
  mutate(age = if_else((age == "30-34" | age == "35-39"), "30-39", as.character(age)))
```

![](https://raw.githubusercontent.com/UBC-MDS/Sustainability_Survey/master/Milestone2/Milestone2_EDA_files/imgs/age%20distribution-1.png)

> Figure 1: Age distribution of respondents

``` r
ggplot(tidy_data, aes(age)) +
  geom_histogram(stat = "Count", fill = "#ff9999") +
  labs(x = "Age Group",
       title = "Distribution of Ages of Survey Respondents") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))
```

![](images_report/age%20distribution-1.png)

> Figure 2: Redistribution of age categories.

We first want to analyze whether our relationship has confounders.

We then want to see if our data meets the assumptions of linear and/or ordinal regression.

#### Important assumptions of the study:

-   1.  There are only two possible confounding variables: age, background.

-   1.  The strength of the (X,Y) association within each strata defined by the confounders is the same.

-   1.  If we choose to do linear regression, there is a simple linear relationshing in how Y varies accross the strata. In order to do linear regression, we assume that all variables are multivariate normal. This must be tested by checking the residual and qqnorm plots. Linear regression also assumes that there is little or no multicollinearity in the data.

-   1.  If we choose to do ordinal regression, the data must fit the proportional odds assumption that all categories are equally spaced.

First, we check to see if all variables are multivariate normal. See Figure 3.

``` r
lm_test <- lm(recycling_freq %>% as.numeric() ~ self_rating_before + background + age, data = tidy_data)

lm_data <- tidy_data %>%
  mutate(recycling_freq_num = as.numeric(recycling_freq),
         lm_fitted = lm_test %>% fitted(),
         lm_resid = lm_test %>% resid())

resid_plot <- lm_data %>%
  ggplot(aes(x= lm_fitted, y = lm_resid)) +
  geom_point() +
  labs(x="yhat", y="residual",title="Residual Plot") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))

qq_norm <- lm_data %>%
  ggplot(aes(sample= lm_resid)) +
  stat_qq() +
  labs(title="QQ Normality Plot") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))

grid.arrange(resid_plot,qq_norm,nrow=1)
```

![](images_report/unnamed-chunk-1-1.png) &gt; Figure 3: Residual and QQ-norm plot of residuals under linear model assumptions

Since these plots show that the y variable is not normally distributed for each x, we will not be able to do linear regression. This is unfortunate because linear regression is easy to interpret. However, ordinal regression will allow more precision for each change in category of Y, and will allow us to capture the inherent order of the recycling categories. Thus, ordinal linear regression is performed to analyze the relationship between `self-rated sustainability importance` and `recycling frequency`, as well as `age of the individual` and `background (environmentally-conscious family)`.

Assumptions of the Ordinal Linear regression are; (1) one predictor in the model (`recycling frequency`), (2) Each category has its own regression model, (3) No category has 0 count. After fitting the additive model, we also explored multiplicative model to understand whether there are any interaction effects.

Results
-------

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

> The simple model with just X and Y has a significant F-statistic,
> meaning we can reject the null hypothesis that there is no effect of X
> on Y.

> In order to understand if there are any confounding variables though, we need to take a closer look and compare the simple full model with the interaction model that include confounding variables.

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
    ## -2.7171 -0.2977  0.1394  0.3962  0.8692
    ##
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)         3.108770   0.376207   8.263 1.38e-11 ***
    ## self_rating_before  0.166487   0.042164   3.949 0.000204 ***
    ## backgroundNo       -0.143422   0.159799  -0.898 0.372915    
    ## age25-29            0.001856   0.200379   0.009 0.992640    
    ## age30-39            0.153181   0.216479   0.708 0.481843    
    ## age40+              0.086924   0.226483   0.384 0.702441    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.6156 on 62 degrees of freedom
    ## Multiple R-squared:  0.2722, Adjusted R-squared:  0.2135
    ## F-statistic: 4.638 on 5 and 62 DF,  p-value: 0.001166

> Including all predictive variables in the model shows that on average,
> for each unit increase in X, there is a 0.176 increase in y. This
> means that for every increase in self-importance of sustainability
> rating there is a 0.17 increase in frequency of recycling. The p-value
> is 0.000103, which is significant with an alpha of 0.05. Therefore, we
> can reject the null hypothesis and say that there is a positive
> relationship between self-rated importance of being sustianable and
> one’s frequency of recycling.

We compiled the self-rated sustainability importance, recycling frequency, age and background information from 68 survey respondents. A general idea of the relationship between self-rated sustainability importance and recycling frequency can be shown by the following boxplots (Figure 4).

``` r
# Visualization of relationship between recycling freq. and self rating before watching the video
tidy_data %>%
  ggplot(aes(x = recycling_freq, y = self_rating_before)) +
  geom_boxplot(size = .75) +
  geom_jitter(alpha = .1, height = .2) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, vjust = 1)) +
  coord_flip() +
  theme_bw() +
  labs(y = "Rating before watching the video", x = "Recycling Frequency") +
  ggtitle("Sustainability Importance vs. Recycling Freq.") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))
```

![](images_report/visualization-1.png) *Figure 4: Sustainability importance vs. Recycling Frequency*

Looking at the boxplots if figure 2, it is easy to notice that people who recycle more often consider sustainability more important (having a higher self-rated mean, and narrower range) than those who rarely recycle, which suggests that a person's opinion about sustainability importance influences the recycling frequency.

Thus, fitting an ordinal regression model considering self-rated importance of sustainability before watching the video, individuals' age and background as predictors:

``` r
# Fit a OLR model without interaction
olr_model <- polr(recycling_freq ~ self_rating_before + background + age, data = tidy_data)

tidy_data <- tidy_data %>%
  cbind(., olr_model_prob = predict(olr_model, ., type = "probs")) %>% #estimated probability
  cbind(., olr_model_decision = predict(olr_model))                   #estimated result according to the probabilities
```

Analyzing the goodness of fit, looking for AIC and likelihood, we
obtained:

``` r
gf <- table(tidy_data$recycling_freq, tidy_data$olr_model_decision)
accuracy <- (gf[1] + gf[6] + gf[11] + gf[16]) / 68 # 68 observations

print(paste("AIC = ", AIC(olr_model)))
```

    ## [1] "AIC =  115.911780418842"

``` r
print(paste("Log likelihood = ", logLik(olr_model)))
```

    ## [1] "Log likelihood =  -49.9558902094209"

``` r
print(paste("Overall accuracy = ", accuracy)) # The larger the better
```

    ## [1] "Overall accuracy =  0.661764705882353"

The values look nice, so now we can get the coefficients:

``` r
# Get the coefficients
summary(olr_model)
```

    ##
    ## Re-fitting to get Hessian

    ## Call:
    ## polr(formula = recycling_freq ~ self_rating_before + background +
    ##     age, data = tidy_data)
    ##
    ## Coefficients:
    ##                       Value Std. Error t value
    ## self_rating_before  0.61882     0.1661  3.7254
    ## backgroundNo       -0.51116     0.5683 -0.8995
    ## age25-29            0.09463     0.6829  0.1386
    ## age30-39            0.52693     0.7355  0.7164
    ## age40+              0.93777     0.8340  1.1244
    ##
    ## Intercepts:
    ##                   Value   Std. Error t value
    ## Rarely|Sometimes   0.2980  1.3518     0.2204
    ## Sometimes|Usually  1.1814  1.2742     0.9272
    ## Usually|Always     5.0900  1.4830     3.4322
    ##
    ## Residual Deviance: 99.91178
    ## AIC: 115.9118

``` r
exp(coef(olr_model))
```

    ## self_rating_before       backgroundNo           age25-29
    ##          1.8567319          0.5997996          1.0992467
    ##           age30-39             age40+
    ##          1.6937258          2.5542798

Interpreting the coefficients:

-   Example of slope interpretation: For `self_rating_before`, one unit increase in `self_rating_before`, there is a ~0.67 increase in the expected value of recycling frequency (in log odds scale), given that all of the other variables in the model are constant. We can also calculate the 95% confidence interval given the estimate's standard error. Another way to interpret this estimate is that we can be 95% confident that on average, a one-unit increase in one's self-rating means that there is somewhere between a 1.4- 2.7 increased odds of recycling more frequently. A positive value indicates a positive increase in the odds of the next response category as the predictor increases by one unit.

-   Example of estimate interpretation: ~5.49 is the expected log odds of recycle "usually" versus "always" when all the predictors are 0.
-   If a person rates themselves one unit higher, the odds of increase in recycling from a lower frequency to a higher frequency is multiplied by 1.96.

``` r
upper_CI = exp(0.6730300 + 1.96*0.1722552)
lower_CI = exp(0.6730300 - 1.96*0.1722552)
print(paste("95% CI for non-interaction model= ", lower_CI, upper_CI))
```

    ## [1] "95% CI for non-interaction model=  1.39851338977876 2.74738676330879"

Multiplicative effect of the confounders are explored below:

``` r
# Fit a OLR model with interaction
olr_model_interaction <- polr(recycling_freq ~ self_rating_before + background * age, data = tidy_data)

tidy_data <- tidy_data %>%
  cbind(., olr_model_prob2 = predict(olr_model_interaction, .,
                                    type = "probs")) %>% #estimated probability
  cbind(., olr_model_decision2 = predict(olr_model_interaction))                   #estimated result according to the probabilities
```

``` r
gf2 <- table(tidy_data$recycling_freq, tidy_data$olr_model_decision2)
accuracy <- (gf2[1] + gf2[6] + gf2[11] + gf2[16]) / 68 # 68 observations

print(paste("AIC for OLR model with interaction = ", AIC(olr_model_interaction)))
```

    ## [1] "AIC for OLR model with interaction =  113.819560665681"

``` r
print(paste("Log likelihood of OLR model with interaction = ", logLik(olr_model_interaction)))
```

    ## [1] "Log likelihood of OLR model with interaction =  -45.9097803328404"

``` r
print(paste("Overall accuracy of OLR model with interaction = ", accuracy)) # The larger the better
```

    ## [1] "Overall accuracy of OLR model with interaction =  0.779411764705882"

To compare the two models we can do a likelihood ratio test:

``` r
anova(olr_model, olr_model_interaction)
```

    ## Likelihood ratio tests of ordinal regression models
    ##
    ## Response: recycling_freq
    ##                                   Model Resid. df Resid. Dev   Test    Df
    ## 1 self_rating_before + background + age        60   99.91178             
    ## 2 self_rating_before + background * age        57   91.81956 1 vs 2     3
    ##   LR stat.    Pr(Chi)
    ## 1                    
    ## 2  8.09222 0.04414376

This ANOVA test shows that we did not observe a significant difference in the model when interaction between background and age was added to the model. Thus, we will use only the simple model without interaction to explain our findings.

``` r
#summary(olr_model_interaction)
# Get the coefficients
# exp(coef((olr_model_interaction)))
```

Discussion
----------

We found that self-rated opinion of the importance of environmental sustanability is a significant predictor of recyling frequency behaviours. In our study we did not find age or family values to be significant confounders. Interestingly, in the literature it appears that income is a significant predictor of recycling frequency, which we did not include in our survey (Schultz et al. 1995).

### Discussion of results

> The main assumption of ordinal regression is that each predicted category is evenly spaced. We are assuming that this is true, as it is plausible that the difference between each recycling category is equally different to people taking our survey. Of course, it is possible to validate this assumption as we can not read people's minds to make sure that they understand the differences between each category and perceive them as evenly spaced.

### Discussion of survey/study design

> To make this study as causal as possible we designed our questions to have a reasonable number of categories for ordinal regression analysis, and included questions that would allow us to reasonably accurately assess whether there were any confounding variables that might be affecting our hypothesized relationship. This study designed allowed us to easily make conclusions about each category of recycling frequency.

> There are other difficulties with self-reported survey data, such as reporting bias; We are aware that there is most-likely a positive inflation of respondent's recycling habits due to reporting bias. There may also be respondent bias, as we surveyed mostly our classmates and professors, and a few other university students. Nonetheless, we will assume that the vast majority of people who didn't respond would have responded in the same way as those who did. However, it should be noted that these conclusions may only generalize well to a similar demographic of people, such as those studying STEM subjects in University. In future studies, it would be interesting to include education and area of study as possible confounders. Another form of bias that we tried to avoid is researcher bias, which can happen when we let our own biases affect how we form the survey questions. This could affect the purity of the study. To avoid this, we asked a few classmates if the survey questions made sense and appeared un-biased, to gain some outside perspective.

Bonus analysis: Not enough x observations?
------------------------------------------

``` r
tidy_data_ttest <- tidy_data %>%
  filter(watch %in% 'Have watched')


#t.test(tidy_data$t.test$self_rating_before, tidy_data_ttest$self_rating_after, paired=TRUE)
```

### References

1)  Schultz, P. W., Oskamp, S., & Mainieri, T. (1995). Who recycles and
    when? A review of personal and situational factors. Journal of
    Environmental Psychology, 15(2), 105-121.
    <http://dx.doi.org/10.1016/0272-4944(95)90019-5>.

### NOTES for reference

> 3-5 page
    report

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
