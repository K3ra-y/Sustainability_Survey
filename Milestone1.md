# Milestone 1 - Proposal

### Aim of the Survey

***Question of Interest :*** Does a person’s frequency of recycling influence her/his opinion and attitudes towards the importance of sustainability?

### Questions to be Analyzed

* [Response - Continuous] How sustainable are you from 1-10 scale?

  - We will set a scaler and participants will slide and rank themselves based on self perception.


* [Main Covariate - Ordinal] How often do you recycle?

  - This is the main predictor towards our target question.
  - Options: `Never`, `Sometimes`, `Often`, `Always`


* [Confounder - Ordinal] What is your age group?

  - This will be binned to ensure the privacy of the participants.
  - Options: `<20`, `20-25`, `25-30`, `30-35`, `>35`


* [Confounder - Categorical] Did you grow up in environmentally conscious family?

  - `Yes`/`No`


* **Bonus Question** [Confounder] After watching this video about recycling, how sustainable are you from 1-10 scale? *Continuous*

  - Again, this will be a scaler with 1-10 scale.

 > Questions haven't been used: Education Major, how often they recycle, how often do they think about eating less meat, flying vs driving vs biking/walking, how do they commute to school, did their parents recycle/grow up in a environmentally conscious family, have they studied the environment or taken any sustainability courses.

 ### Other question we aim to answer

 Additionally, we would like to know if the video about recycling *changes* the opinion towards the importance of sustainability.

 ### Plan for Analyzing the Survey Results

 **Null hypothesis**: A person’s frequency of recycling does not influence her/his opinion and attitudes towards the importance of sustainability.

 **Alternative hypothesis**: A person’s frequency of recycling influences her/his opinion and attitudes towards the importance of sustainability.

 Response variable will be set as a scale and this will allow us to have a continuous response variable. We plan to perform *exploratory data analysis* to summarize the main quality of the response variable and predictors. Then we will use *linear regression* to identify which  predictors (frequency of recycling, age and family habits) has an effect on the response variable.

 If time allows and we get enough and reliable results from the bonus question, then we will perform a pairwise two sample t test between the self-rank before watching the video and the self-rank after watching the video.

 The analysis will be carried out in R statistical software.

### Relevant Aspects of the [UBC Office of Research Ethics Document on Using Online Surveys](https://ethics.research.ubc.ca/sites/ore.ubc.ca/files/documents/Online_Survey-GN.pdf)

> Under FIPPA, “personal information” is defined as “recorded information about an identifiable individual.” This includes directly identifying information (e.g. SIN number, student number, etc.) as well as indirectly identifying information (e.g. postal code, date of birth) that alone or in combination with other information might reasonably be expected to identify an individual. (...)
>
> If you are not collecting any personal information in your survey, then you are free to use whatever online survey tool you prefer as long as the survey is completely anonymous.

This survey will ask the participants about sustainability self assessment, recycling frequency, age group and background. We won't collect any personal info, such as student id or email address, and the questions were formulated in a way to protect participant's privacy. Thus, even if the information collected is combined with other data, it won't be possible to identify a respondent. For this reason, since we will not ask personal questions, any online survey tool can be used in this analysis as long as the survey is anonymous.

We are planning to use [SurveyMonkey](www.surveymonkey.com) as the tool to carry out the survey which facilitates [anonymous responses](https://help.surveymonkey.com/articles/en_US/kb/How-do-I-make-surveys-anonymous).
