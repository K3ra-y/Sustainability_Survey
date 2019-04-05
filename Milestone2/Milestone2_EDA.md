Milestone2\_EDA
================
Wilson D
4/5/2019

This dataset is aimed to answer the question “Does a person’s frequency
of recycling influence her/his opinion and attitudes towards the
importance of sustainability?”. The description of columns as follows:

  - `Q1_1`: How important is being environmentally sustainable to you on
    a scale from 1-10?
  - `Q2`: How often do you generally recycle?
  - `Q3`: What is your age group?
  - `Q4`: Did you grow up in an environmentally-conscious family?
  - `Q5`: Will you watch the video?
  - `Q6_1`: After watching this video about recycling, how important is
    sustainability to you on a self-ranked scale from 1-10 scale?

# Load Data

``` r
#tidy data and give correct data type
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

> ***Need to be done*** Briefly discuss about the summary table.

# EDA

``` r
tidy_data %>% 
  ggplot() +
  geom_bar(aes(x = Q2, fill = Q4)) +
  facet_grid(~Q4) +
  theme_bw() +
  labs(x = "How often do you recycle? ",
       title = "Frequency of recycling vs. Did you grow up in an environmentally-conscious family?") +
  scale_fill_discrete(name = "environmentally-conscious family?") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](Milestone2_EDA_files/figure-gfm/plot%20family%20vs.%20frequency-1.png)<!-- -->

> As we can see from the above plot, most of our respondent have a good
> recycling habit whether or not they live in a
> environmentally-consicous family. However, if one respondent did not
> grow up in an environmentally-conscious family then he/she may do not
> recycle at all.
