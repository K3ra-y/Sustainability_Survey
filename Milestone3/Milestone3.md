Milestone 3
================
Heather VT., Kera Y., Marcelle C., Wilson D.
April 13th, 2019

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
#summary(tidy_data)
```

``` r
#simple_model = lm(data=tidy_data, recycling_freq ~ self_rating_before)
#age_confounder = lm(data=tidy_data, recycling_freq ~ self_rating_before + age)
#background_confounder = lm(data=tidy_data, recycling_freq ~ self_rating_before + background)
#interaction_effects = lm(data=tidy_data, recycling_freq ~ self_rating_before + background*age)

#summary(simple_model)
```

We compiled the self-rated sustainability importance, recycling frequency, age and background information from 68 survey entries. Thus, to have a general idea of the relationship between self-rated sustainability importance and recycling frequency, we can analyze the following boxplots.

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

![](images_report/visualization-1.png)

Analyzing the boxplots above is easy to notice that people who recycle more often consider sustainability more important (having a higher self rating mean, and narrower range) than the ones who rarely reclycle, which suggests that a person's opinion about sustainability importance influences the recycling frequency.

Thus, fitting an ordinal regression model considering self-rated sustainability importance before watching the video, individuals' age and background as predictors:

``` r
# Fit a OLR model without interaction
olr_model <- polr(recycling_freq ~ self_rating_before + background + age, data = tidy_data)

tidy_data <- tidy_data %>%
  cbind(., olr_model_prob = predict(olr_model, ., type = "probs")) %>% #estimated probability
  cbind(., olr_model_decision = predict(olr_model))                   #estimated result according to the probabilities
```

Analyzing the goodness of fit, looking for AIC and likelihood, we obtained:

``` r
gf <- table(tidy_data$recycling_freq, tidy_data$olr_model_decision)
accuracy <- (gf[1] + gf[6] + gf[11] + gf[16]) / 68 # 68 observations

print(paste("AIC = ", AIC(olr_model))) 
```

    ## [1] "AIC =  115.158392854266"

``` r
print(paste("Log likelihood = ", logLik(olr_model))) 
```

    ## [1] "Log likelihood =  -48.5791964271329"

``` r
print(paste("Overall accuracy = ", accuracy)) # The larger the better
```

    ## [1] "Overall accuracy =  0.75"

The values look nice, so now we can get the coefficients:

``` r
# Get the coefficients
coef(summary(olr_model))
```

    ## 
    ## Re-fitting to get Hessian

    ##                         Value Std. Error    t value
    ## self_rating_before  0.6730300  0.1722552  3.9071686
    ## backgroundNo       -0.6121531  0.5838547 -1.0484681
    ## age25-29            0.1367529  0.6936227  0.1971575
    ## age30-34            1.0566521  0.8263358  1.2787200
    ## age35-39           -1.1883473  1.2716763 -0.9344732
    ## age40+              0.9466290  0.8470890  1.1175083
    ## Rarely|Sometimes    0.5565850  1.3782016  0.4038488
    ## Sometimes|Usually   1.4665515  1.2980113  1.1298449
    ## Usually|Always      5.4871804  1.5288542  3.5890802

Interpreting the coefficients:

-   Example of slope interpretation: For `self_rating_before`, one unit increase in `self_rating_before`, there is a ~0.67 increase in the expected value of recycling frequency (in log odds scale), given that all of the other variables in the model are constant. A positive slope indicates a tendency for the response level to increase as the predictor increases.

-   Example of intercept interpretation: ~5.49 is the expected log odds of recycle "usually" versus "always" when all the predictors are 0.

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