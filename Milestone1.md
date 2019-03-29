# Milestone 1

### Aim of the Survey

We are looking at which factors influence a person's opinion and attitudes towards the importance of sustainability.

### Questions to be Analyzed

* How sustainable are you?

  - Scaler from 1 to 10, where the respondents should slide and rank themselves based on self perception.

* What is your age group?

  - Age bins to ensure participants' privacy.

* What is your Education Major?

  - Behavioral and Social Sciences, Business, Engineering, Health and Medical Sciences, Humanities, Life Sciences, Physical Sciences, Others.

* Did you grow up in an environmentally conscious family?

  - Yes/No

* Do you use a reusable mug?
  -  Yes/No


 > how often they recycle, how often do you think about eating less meat, flying vs driving vs biking/walking, how do you commute to school, did your parents recycle/grow up in a environmentally conscious family, have you studied the environment or taken any sustainability courses.

 ### Other question we aim to answer

 Additionally, we would like to know which factors may drive the *change* in opinion and attitudes towards the importance of sustainability.

 ### Plan for Analyzing the Survey Results

 Response variable will be set as a scale and this will allow us to have a continuous response variable. Then we will use *linear regression* to identify which  predictors (age, education major, background, habits) has an effect on the response variable.

 Analysis will be carried out in R statistical software.

### Relevant Aspects of the [UBC Office of Research Ethics Document on Using Online Surveys](https://ethics.research.ubc.ca/sites/ore.ubc.ca/files/documents/Online_Survey-GN.pdf)

> Under FIPPA, “personal information” is defined as “recorded information about an identifiable individual.” This includes directly identifying information (e.g. SIN number, student number, etc.) as well as indirectly identifying information (e.g. postal code, date of birth) that alone or in combination with other information might reasonably be expected to identify an individual. (...)
>
> If you are not collecting any personal information in your survey, then you are free to use whatever online survey tool you prefer as long as the survey is completely anonymous.

This survey will ask the participants about sustainability self assessment, age group, education major and background. We won't collect any personal info, such as student id or email address, and the questions were formulated in a way to protect participant's privacy. Thus, even if the information collected is combined with other data, it won't be possible to identify a respondent. For this reason, since we will not ask personal questions, any online survey tool can be used in this analysis as long as the survey is anonymous.

We are planning to use [SurveyMonkey](www.surveymonkey.com) as the tool to carry out the survey which facilitates [anonymous responses](https://help.surveymonkey.com/articles/en_US/kb/How-do-I-make-surveys-anonymous).
