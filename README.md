# Sustainability Survey

## Team

| Name   | Github.com |
| :------: | :---: |
| Marcelle Chiriboga Carvalho | [`@mchiriboga`](https://github.com/mchiriboga) |
| Weishun Deng | [`@xiaoweideng`](https://github.com/xiaoweideng) |
| Heather Van Tassel | [`@heathervant`](https://github.com/heathervant) |
| Kera Yucel | [`@K3ra-y`](https://github.com/K3ra-y) |

# Aim of the Survey

***Question of Interest :*** Does a person’s frequency of recycling influence her/his opinion and attitudes towards the importance of sustainability?

## Survey will consist of following questions;

* [Response]How sustainable are you from 1-10 scale? *Continuous*

  - We will set a scaler and participants will slide and rank themselves based on self perception.


* [Main Covariate] How often do you recycle? *Ordinal*

  - This is the main predictor towards our target question.
  - Options: `Never`, `Sometimes`, `Often`, `Always`


* [Confounder]Your age group *Ordinal*

  - This will be binned to ensure the privacy of the participants.
  - Options: `20-25`, `25-30`, `30-35`, `35+`


* [Confounder]Did you grow up in environmentally conscious family? *Categorical*

  - `Yes`/`No`


* **Bonus Question** [Confounder] After watching this video about recycling, how sustainable are you from 1-10 scale? *Continuous*

  - Again, this will be a scaler with 1-10 scale.


 > Questions haven't been used: Your education major, how often they recycle, how often do you think about eating less meat, flying vs driving vs biking/walking, how do you commute to school, did your parents recycle/grow up in a environmentally conscious family, have you studied the environment or taken any sustainability courses.

 ## Other question we aim to answer

 Additionally, Does the video about recycling *change* in opinion and attitudes towards the importance of sustainability?

 # Plan for analyzing the survey results

 **Null hypothesis**: A person’s frequency of recycling does not influence her/his opinion and attitudes towards the importance of sustainability.



 **Alternative hypothesis**: A person’s frequency of recycling influences her/his opinion and attitudes towards the importance of sustainability.

 Response variable will be set as a scale and this will allow us to have a continuous response variable. We plan to perform *exploratory data analysis* to summarize the main quality of the response variable and predictors. Then we will use *linear regression* to identify which  predictors (frequency of recycling, age and family habits) has an effect on the response variable.


 If time allows and we get enough and reliable results from the bonus question, then we will perform a pairwise two sample t test between the self-rank before watching the video and the self-rank after watching the video.

 Analysis will be carried out in R statistical software.

# Aspects of the UBC Office of Research Ethics document on Using Online Surveys that are relevant to our survey
