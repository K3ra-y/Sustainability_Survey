# Milestone 1 - Proposal

### Aim of the Survey

***Question of Interest :*** Does a person’s frequency of recycling influence her/his opinion and attitudes towards the importance of sustainability? We are interested in this question because sometimes our actions do not line up with our beliefs. This survey will attempt to understand whether a person's actions are actually predictive of their beliefs, by looking at how frequently they recycle compared to how important they believe ecological sustainability to be.

### Questions to be Analyzed

* [Response - Continuous] How important is being environmentally sustainable to you on a scale from 1-10?

  - We will set a slider and participants will slide and rank themselves based on their self-perception of how important environmental sustainability is to them.


* [Main Covariate - Ordinal] How often do you generally recycle? This question pertains to all of the recyclable products that you have used in the past year (on average).

  - This is the main predictor towards our target question.
  - Options: `Never`, `Rarely`, `Sometimes`, `Often`, `Always`


* [Potential Confounder - Ordinal] What is your age group?

  - This will be binned to ensure the privacy of the participants.
  - Options: `<20`, `20-25`, `25-30`, `30-35`, `>35`


* [Potential Confounder - Categorical] Did you grow up in an environmentally-conscious family?

  - `Yes`/`No`


* **Bonus Question** [Confounder - Continuous] After watching [this video](https://www.youtube.com/watch?v=Zsc8G0NnMTs) about recycling, how important is sustainability to you on a self-ranked scale from 1-10 scale?

  - Again, this will be a scaler with 1-10 scale.

 > Questions haven't been used: Gender, Education Major, how often do they think about eating less meat, how do they commute to school, have they studied the environment or taken any sustainability courses.

 ### Other question we aim to answer

 Additionally, we would like to know if the video about recycling *changes* their opinion on the importance of sustainability and if so which predictors plays role in this change.

 ### Plan for Analyzing the Survey Results

 **Null hypothesis**: A person's opinion and attitudes towards the importance of sustainability is not affected by frequency of recycling, age, and whether they grow up in environmentally conscious family or not.

 **Alternative hypothesis**: A person's opinion and attitudes towards the importance of sustainability is affected by frequency of recycling, age or whether they grow up in environmentally conscious family or not.

 Response variable will be set as a scale and this will allow us to have a continuous response variable. We plan to perform *exploratory data analysis* to summarize the main quality of the response variable and predictors. Then we will use *linear regression* to identify which  predictors (frequency of recycling, age and family habits) has an effect on the response variable.

 If time allows and we get enough and reliable results from the bonus question, then we will perform a paired sample t-test between the self-rank before watching the video and the self-rank after watching the video. We will ensure to not violate the assumptions of paired sample t-test via setting the scaler as continuous variable, making the observations independent from each other. Additionally, we will confirm the response variable is normally distributed and outliers are removed.

 The analysis will be carried out in R statistical software.

 ### Awareness of biases and attempts to minimize them
 > Although these are self-reported measures, we can assume that respondents are being as honest as possible. We are aware that there will probably be a positive inflation about their recycling habits due to reporting bias. We also expect to observe respondent bias, as we are going to be surveying mostly our classmates, nonetheless we will assume that the vast majority of people who didn’t respond would have responded in the same way as those who did. Another form of bias that we hope to avoid is researcher bias, which can happen when we let our own biases affect how we form the survey questions. This could affect the purity of the study. To avoid this, we will ask to course instructor, lab instructor or a few classmates if the survey questions make sense or appear biased to them to get outsider perspective.


### Relevant Aspects of the [UBC Office of Research Ethics Document on Using Online Surveys](https://ethics.research.ubc.ca/sites/ore.ubc.ca/files/documents/Online_Survey-GN.pdf)

> Under FIPPA, “personal information” is defined as “recorded information about an identifiable individual.” This includes directly identifying information (e.g. SIN number, student number, etc.) as well as indirectly identifying information (e.g. postal code, date of birth) that alone or in combination with other information might reasonably be expected to identify an individual. (...)
>
> If you are not collecting any personal information in your survey, then you are free to use whatever online survey tool you prefer as long as the survey is completely anonymous.

This survey will ask the participants about sustainability self assessment, recycling frequency, age group and background. We will not collect any personal info, such as student id or email address, and the questions were formulated in a way to protect privacy of the participants. Thus, even if the information collected is combined with other data, it will not be possible to identify a respondent. For this reason, since we will not ask personal questions, any online survey tool can be used in this analysis as long as the survey is anonymous.

We are planning to use [SurveyMonkey](www.surveymonkey.com) as the tool to carry out the survey which facilitates [anonymous responses](https://help.surveymonkey.com/articles/en_US/kb/How-do-I-make-surveys-anonymous).

### Adhering to the [Cross-cultural survey guidelines](http://ccsg.isr.umich.edu/index.php/chapters/ethical-considerations-in-surveys-chapter#seven)
We aim to follow the following guidelines:
- 1.1.11 Employ appropriate tools and methods of analysis.

- 1.1.12 Make interpretations of research results that are consistent with the data.

- 1.1.13 Be clear and honest about how much confidence can be placed in the conclusions drawn from the data.

- 1.1.14 Report research findings, even if they are not in line with the researcher’s hypothesis.
