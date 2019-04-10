Milestone 2
================
Wilson D., Marcelle C., Kera Y., Heather VT.
4/5/2019

This survey aimed to answer the question *“Does a person’s frequency of
recycling influence her/his opinion and attitudes towards the importance
of sustainability?”*. The dataset columns description are as follows:

  - `self_rating_before`: How important is being environmentally
    sustainable to you on a scale from 1-10?
  - `recycling_freq`: How often do you generally recycle?
  - `age`: What is your age group?
  - `background`: Did you grow up in an environmentally-conscious
    family?
  - `watch`: Will you watch the video?
  - `self_rating_after`: After watching this video about recycling, how
    important is sustainability to you on a self-ranked scale from 1-10
    scale?

### Load Data

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

> Analyzing the summary table, we noticed:
>
> **self\_rating\_before: How important is being environmentally
> sustainable to you on a scale from 1-10?**: the average of the
> responses before watching the video was 7.8, with median 8.0.
>
> **recycling\_freq: How often do you generally recycle?**: Most
> participants have a good recycling habit: 29 (48.3%) answered
> “Usually” and 27 (45%) answered “Always”. Only 4 participants
> (6.7%) replied that they recycle “Sometimes” or “Rarely”.
>
> **age: What is your age group?**: A total of 36 participants (60%) are
> from the younger groups, being 20-29 years old. We had a considerable
> number of people with 40+, but only 3 respondents (5%) are 35-39 years
> old.
>
> **background: Did you grow up in an environmentally-conscious
> family?**: 28 respondents (46.7%) replied that they grew up in an
> environmentally-conscious family.
>
> **watch: Will you watch the video?**: Only 38 respondents (63%)
> confirmed that they watched the video.
>
> **self\_rating\_after: After watching this video about recycling, how
> important is sustainability to you on a self-ranked scale from 1-10
> scale?**: The self rank about sustainability importance after watching
> the video had a slightly higher mean (8.1 vs. 7.8) and median (9
> vs. 8) than the answers from the first question. However, some people
> who didn’t watch the video answered the last question, since we had 15
> respondents who said on watch that they’d skip the video, and only 9
> participants answered NA to self\_rating\_after.

### Exploratory Data Analysis

In order to explore our survey results and have a better understanding
of how the variables impact the responses, we performed the following
analysis:

``` r
tidy_data %>%
  ggplot() +
  geom_bar(aes(x = recycling_freq, fill = background)) +
  facet_grid(~background) +
  theme_bw() +
  labs(x = "How often do you recycle? ",
       title = "Recycling Freq. vs. Environmentally-Conscious Family") +
  scale_fill_discrete(name = "Environmentally-Conscious Family?") +
  theme(plot.title = element_text(size = 12, face = "bold", hjust = 0.5),
        axis.text.x = element_text(size = 9, angle = 45, hjust = 1),
        axis.title = element_text(size = 10))
```

![](Milestone2_EDA_files/imgs/plot%20family%20vs.%20frequency-1.png)<!-- -->

> As we can see from the above plot, most of our respondent have a good
> recycling habit whether or not they live in a
> environmentally-consicous family. However, if one respondent did not
> grow up in an environmentally-conscious family then he/she may not
> recycle at all.

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

![](Milestone2_EDA_files/imgs/age%20distribution-1.png)<!-- -->

``` r
qplot(tidy_data$age, tidy_data$self_rating_before, geom="boxplot") +
  ylim(0, 10) +
  theme_bw() +
  labs(x = "Age Group",
       y = "Rating before watching the video") +
  ggtitle("Sustainability Importance vs. Age Group") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))
```

![](Milestone2_EDA_files/imgs/plot%20age%20group%20vs.%20sustainability%20importance%20before%20watching%20the%20video-1.png)<!-- -->

> Analyzing the boxplots above is easy to see that older people (`35-39`
> and `40+` groups) consider sustainability more important than younger
> groups, having a higher mean (~9 in a scale from 1-10) and narrower
> range. Additionaly, the other 3 groups (`20-24`, `25-29` and `30-34`)
> have one outlier each, where at least one respondent of each of these
> groups evaluated sustainability with a considerable lower importance
> in comparison with the other participants from their respective group.
> However, it’s important to take into account that we have fewer
> responses from the second oldest age group, which can distort the
> analysis. Further analysis with confidence intervals is warranted.

``` r
p1 <- tidy_data %>%
  filter(watch %in% 'Have watched') %>%
  ggplot(aes(self_rating_before)) +
  geom_bar(fill = "cyan3", bins = 10) +
  scale_x_continuous(breaks = c(1:10),limits=c(1, 11)) +
  ylim(0, 16) +
  labs(title = "Rating before watching the video", x = "Ratings") +
  theme_bw() +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 13),
        axis.title = element_text(size = 11))

p2 <- tidy_data %>%
  filter(watch %in% 'Have watched') %>%
  ggplot(aes(self_rating_after)) +
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

![](Milestone2_EDA_files/imgs/grouped%20bar-1.png)<!-- -->

> These histograms show that there was a slight shift in ratings towards
> more positive ratings after watching the video. Further analysis with
> confidence intervals will
follow.

``` r
# self rank before and after watching the video for recycling frequency, for respondents who watched the video
tidy_data %>% filter(watch %in% 'Have watched') %>%
  ggplot(aes(x = self_rating_before, y = self_rating_after, color = recycling_freq), alpha = 0.5) +
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

![](Milestone2_EDA_files/imgs/plot%20self%20rank%20before%20and%20after%20watching%20the%20video%20for%20recycling%20frequency%20and%20age%20group-1.png)<!-- -->

``` r
# self rank before and after watching the video for age group, for respondents who watched the video
tidy_data %>% filter(watch %in% 'Have watched') %>%
  ggplot(aes(x = self_rating_before, y = self_rating_after, color = age)) +
  geom_point(alpha = 0.5) +
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

``` r
data_adj <- tidy_data %>%
  mutate(delta = Q6_1 - Q1_1) %>%
  mutate(Q1_adj = (Q1_1 %/% 2 + Q1_1 %% 2), Q6_adj = Q6_1 %/% 2 + Q6_1 %% 2, delta_adj = Q6_adj - Q1_adj)

(data_adj %>%
  filter(Q5 %in% 'Have watched') %>%
    arrange(delta))
```

    ## # A tibble: 38 x 10
    ##     Q1_1 Q2      Q3    Q4    Q5         Q6_1 delta Q1_adj Q6_adj delta_adj
    ##    <dbl> <fct>   <fct> <fct> <fct>     <dbl> <dbl>  <dbl>  <dbl>     <dbl>
    ##  1     9 Always  30-34 No    Have wat…     7    -2      5      4        -1
    ##  2     9 Always  30-34 No    Have wat…     9     0      5      5         0
    ##  3     3 Usually 20-24 No    Have wat…     3     0      2      2         0
    ##  4     9 Usually 25-29 No    Have wat…     9     0      5      5         0
    ##  5     9 Always  30-34 No    Have wat…     9     0      5      5         0
    ##  6     9 Usually 35-39 Yes   Have wat…     9     0      5      5         0
    ##  7     4 Usually 25-29 No    Have wat…     4     0      2      2         0
    ##  8     6 Usually 30-34 No    Have wat…     6     0      3      3         0
    ##  9    10 Always  20-24 Yes   Have wat…    10     0      5      5         0
    ## 10    10 Always  25-29 Yes   Have wat…    10     0      5      5         0
    ## # ... with 28 more rows

``` r
# Filter the respondents who confirmed that watched the video and ranked theirselves
# with a lower score (considering the original responses, Q_1 and Q6_1) after that
score_decr <- data_adj %>%  
  filter(Q5 %in% 'Have watched', delta < 0)

score_decr
```

    ## # A tibble: 1 x 10
    ##    Q1_1 Q2     Q3    Q4    Q5            Q6_1 delta Q1_adj Q6_adj delta_adj
    ##   <dbl> <fct>  <fct> <fct> <fct>        <dbl> <dbl>  <dbl>  <dbl>     <dbl>
    ## 1     9 Always 30-34 No    Have watched     7    -2      5      4        -1

``` r
# Filter the respondents who confirmed that watched the video and ranked theirselves
# with a higher score (considering the adjusted responses, Q_adj and Q6_adj) after that
score_incr <- data_adj %>%  
  filter(Q5 %in% 'Have watched', delta_adj > 0)

score_incr
```

    ## # A tibble: 7 x 10
    ##    Q1_1 Q2      Q3    Q4    Q5          Q6_1 delta Q1_adj Q6_adj delta_adj
    ##   <dbl> <fct>   <fct> <fct> <fct>      <dbl> <dbl>  <dbl>  <dbl>     <dbl>
    ## 1     7 Usually 20-24 No    Have watc…     9     2      4      5         1
    ## 2     5 Usually 25-29 Yes   Have watc…     8     3      3      4         1
    ## 3     8 Always  30-34 No    Have watc…     9     1      4      5         1
    ## 4     8 Usually 30-34 Yes   Have watc…     9     1      4      5         1
    ## 5     8 Usually 40+   No    Have watc…    10     2      4      5         1
    ## 6     8 Always  20-24 Yes   Have watc…     9     1      4      5         1
    ## 7     8 Usually 20-24 Yes   Have watc…     9     1      4      5         1

``` r
qplot(tidy_data$Q2, tidy_data$Q1_1, geom = "boxplot") +
  ylim(0, 10) +
  theme_bw() +
  labs(x = "Recycling Frequency",
       y = "Rating before watching the video") +
  ggtitle("Sustainability Importance vs. Recycling Freq.") +
  theme(plot.title = element_text(size = 14, face = "bold", hjust = 0.5),
        axis.text = element_text(size = 12),
        axis.title = element_text(size = 13))
```

![](Milestone2_EDA_files/imgs/plot%20recycling%20fre.%20vs.%20sustainability%20importance%20before%20watching%20the%20video-1.png)

> Analyzing the boxplots above is easy to notice that people who recycle more often consider sustainability more important than the ones who rarely reclycle, which suggests that the recycling frequency seems to influence a person's opinion about sustainability importance.
