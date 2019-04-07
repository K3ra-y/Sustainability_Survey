Milestone 2
================
Wilson D., Marcelle C., Kera Y., Heather VT.
4/5/2019

This survey aimed to answer the question *"Does a person’s frequency of recycling influence her/his opinion and attitudes towards the importance of sustainability?"*. The dataset columns description are as follows:

-   `Q1_1`: How important is being environmentally sustainable to you on a scale from 1-10?
-   `Q2`: How often do you generally recycle?
-   `Q3`: What is your age group?
-   `Q4`: Did you grow up in an environmentally-conscious family?
-   `Q5`: Will you watch the video?
-   `Q6_1`: After watching this video about recycling, how important is sustainability to you on a self-ranked scale from 1-10 scale?

### Load Data

``` r
# tidy data and assign correct data types
tidy_data <- read_csv("../data/Sustainable Survey_April 4, 2019_18.11.csv", col_types = cols(.default = col_character()))[3:62,18:23]

tidy_data <- tidy_data %>% 
  mutate(Q1_1 = as.numeric(Q1_1),
         Q2 = as.factor(Q2),
         Q3 = as.factor(Q3),
         Q4 = as.factor(Q4),
         Q5 = as.factor(Q5),
         Q6_1 = as.numeric(Q6_1))

# factor relevel
tidy_data$Q2 <- tidy_data$Q2 %>% fct_relevel("Rarely","Sometimes","Usually","Always")
tidy_data$Q4 <- tidy_data$Q4 %>% fct_relevel("Yes","No")
tidy_data$Q5 <- tidy_data$Q5 %>% fct_relevel("Have watched","Will pass")
```

``` r
# Creating summary table
summary(tidy_data)
```

    ##       Q1_1                Q2         Q3       Q4                Q5    
    ##  Min.   : 1.000   Rarely   : 2   20-24:19   Yes:28   Have watched:38  
    ##  1st Qu.: 7.000   Sometimes: 2   25-29:17   No :32   Will pass   :15  
    ##  Median : 8.000   Usually  :29   30-34:11            NA's        : 7  
    ##  Mean   : 7.767   Always   :27   35-39: 3                             
    ##  3rd Qu.: 9.000                  40+  :10                             
    ##  Max.   :10.000                                                       
    ##                                                                       
    ##       Q6_1       
    ##  Min.   : 1.000  
    ##  1st Qu.: 7.000  
    ##  Median : 9.000  
    ##  Mean   : 8.059  
    ##  3rd Qu.: 9.000  
    ##  Max.   :10.000  
    ##  NA's   :9

> Analyzing the summary table, we noticed:
>
> **Q1\_1: How important is being environmentally sustainable to you on a scale from 1-10?**: the average of the responses before watching the video was 7.8, with median 8.0.
>
> **Q2: How often do you generally recycle?**: Most participants have a good recycling habit: 29 (48.3%) answered "Usually" and 27 (45%) answered "Always". Only 4 participants (6.7%) replied that they recycle "Sometimes" or "Rarely".
>
> **Q3: What is your age group?**: A total of 36 participants (60%) are from the younger groups, being 20-29 years old. We had a considerable number of people with 40+, but only 3 respondents (5%) are 35-39 years old.
>
> **Q4: Did you grow up in an environmentally-conscious family?**: 28 respondents (46.7%) replied that they grew up in an environmentally-conscious family.
>
> **Q5: Will you watch the video?**: Only 38 respondents (63%) confirmed that they watched the video.
>
> **Q6\_1: After watching this video about recycling, how important is sustainability to you on a self-ranked scale from 1-10 scale?**: The self rank about sustainability importance after watching the video had a slightly higher mean (8.1 vs. 7.8) and median (9 vs. 8) than the answers from the first question. However, some people who didn't watch the video answered the last question, since we had 15 respondents who said on Q5 that they'd skip the video, and only 9 participants answered NA to Q6\_1.

### Exploratory Data Analysis

In order to explore our survey results and have a better understanding of how the variables impact the responses, we performed the following analysis:

``` r
tidy_data %>% 
  ggplot() +
  geom_bar(aes(x = Q2, fill = Q4)) +
  facet_grid(~Q4) +
  theme_bw() +
  labs(x = "How often do you recycle? ",
       title = "Recycling Freq. vs. Environmentally-Conscious Family") +
  scale_fill_discrete(name = "Environmentally-Conscious Family?") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text.x = element_text(size = 9, angle = 45, hjust = 1),
        axis.title = element_text(size = 10))
```

![](Milestone2_EDA_files/imgs/plot%20family%20vs.%20frequency-1.png)

> As we can see from the above plot, most of our respondent have a good recycling habit whether or not they live in a environmentally-consicous family. However, if one respondent did not grow up in an environmentally-conscious family then he/she may not recycle at all.

``` r
ggplot(tidy_data, aes(Q3)) +
  geom_histogram(stat = "Count", fill = "#ff9999") +
  labs(x = "Age Group",
       title = "Distribution of Ages of Survey Respondents") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 13))
```

![](Milestone2_EDA_files/imgs/age%20distribution-1.png)

``` r
qplot(tidy_data$Q3, tidy_data$Q1_1, geom="boxplot") +
  ylim(0, 10) +
  theme_bw() +
  labs(x = "Age Group",
       y = "Rating before watching the video") +
  ggtitle("Sustainability Importance vs. Age Group") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 13))
```

![](Milestone2_EDA_files/imgs/plot%20age%20group%20vs.%20sustainability%20importance%20before%20watching%20the%20video-1.png)

> Analyzing the boxplots above is easy to see that older people (`35-39` and `40+` groups) consider sustainability more important than younger groups, having a higher mean (~9 in a scale from 1-10) and narrower range. Additionaly, the other 3 groups (`20-24`, `25-29` and `30-34`) have one outlier each, where at least one respondent of each of these groups evaluated sustainability with a considerable lower importance in comparison with the other participants from their respective group. However, it's important to take into account that we have fewer responses from the second oldest age group, which can distort the analysis. Further analysis with confidence intervals is warranted.

``` r
p1 <- tidy_data %>%
  ggplot(aes(Q1_1)) +
  geom_bar(fill = "cyan3", bins = 10) +
  scale_x_continuous(breaks = c(1:10)) +
  ylim(0, 16) +
  labs(title = "Rating before watching the video", x = "Ratings") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 11))

p2 <- tidy_data %>%
  filter(Q5 %in% 'Have watched') %>%
  ggplot(aes(Q6_1)) +
  geom_bar(fill = "pink3" ,bins = 10) +
  scale_x_continuous(breaks = c(1:10),limits=c(1, 11)) +
  ylim(0, 16) +
  labs(title = "Rating after watching the video", y = "", x = "Ratings") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 14),
        axis.title = element_text(size = 11))

grid.arrange(p1, p2, nrow = 1)
```

![](Milestone2_EDA_files/imgs/grouped%20bar-1.png)

> These histograms show that there was a slight shift in ratings towards more positive ratings after watching the video. Further analysis with confidence intervals will follow.

``` r
# self rank before and after watching the video for recycling frequency, for respondents who watched the video
tidy_data %>% filter(Q5 %in% 'Have watched') %>% 
  ggplot(aes(x = Q1_1, y = Q6_1, color = Q2), alpha = 0.5) +
  geom_point() +
  labs(x = "Rating before watching the video",
       y = "Rating after watching the video",
       colour ="Recycling Frequency",
       title = "Sustainability Ratings Analyzed by Recycling Frequency") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 13)) +
  geom_abline(slope = 1, intercept = 0, linetype = 3)
```

![](Milestone2_EDA_files/imgs/plot%20self%20rank%20before%20and%20after%20watching%20the%20video%20for%20recycling%20frequency%20and%20age%20group-1.png)

``` r
# self rank before and after watching the video for age group, for respondents who watched the video
tidy_data %>% filter(Q5 %in% 'Have watched') %>% 
  ggplot(aes(x = Q1_1, y = Q6_1, color = Q3)) +
  geom_point() +
  labs(x = "Rating before watching the video",
       y = "Rating after watching the video",
       colour ="Age Group",
       title = "Sustainability Ratings Analyzed by Age Group") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 13)) +
  geom_abline(slope = 1, intercept = 0, linetype = 3)
```

![](Milestone2_EDA_files/imgs/plot%20self%20rank%20before%20and%20after%20watching%20the%20video%20for%20recycling%20frequency%20and%20age%20group-2.png)

> Looking only at the respondents who reported having watched the video, most of them changed their opinion on how sustainable they think they are in the positive direction, as evidenced by the higher number of points above the diagonal line. There was only one respondents who scored less after watching the video. Respondents who scored more after the watching the video are the ones who usually recycle. Respondents scores do not appear to be affected by age group. There may be other underlying confounding factors in which we did not capture in our data collection.
