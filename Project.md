# A/B Testing Project - Udacity Free Trial Screener
## Table of Contents
1. [Overview](#overview)
2. [Metric Choice](#metric-choice)
   - [Invariant Metrics](#invariant-metrics)
   - [Evaluation Metrics](#evaluation-metrics)
3. [Measuring Standard Deviation](#measuring-standard-deviation)
   - [Gross Conversion](#gross-conversion)
   - [Net Conversion](#net-conversion)
   - [Retention Rate](#retention-rate)
4. [Sizing](#sizing)
   - [Number of Samples given Power](#number-of-samples-given-power)
   - [Duration vs. Exposure](#duration-vs-exposure)
5. [Analysis](#analysis)
   - [Sanity Checks](#sanity-checks)
   - [Effect Size Tests](#effect-size-tests)
   - [Sign Tests](#sign-tests)
6. [Recommendation](#recommendation)  

## 1. Overview

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback. In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.


The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.


The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## 2. Metric Choice

Below is the Conversion Funnel base on the information provided

### a. Invariant Metrics

- **Number of cookies** Number of unique cookies to view the course overview page. (dmin=3000)

- **Number of clicks** Number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)

- **Click-through-probability** Number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)

Since these metrics measure user activities before the experiment starts, they must be not variant due to applying this change. Therefore, these metrics above should be invariant metrics for this experiment.

### b. Evaluation Metrics

- **Gross conversion** Number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)

- **Retention** Number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)

- **Net conversion** Number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

Because these metrics measure user activities after the experiment starts, they should be use as evaluation metrics.

Since the purpose of the experiment is to reduce the number of frustrated students who left the free trial because they didn't have enough time, we expect that the **gross conversion** will ***decrease*** because there will be fewer students enrolling the courses, **retention rate** will ***increase*** because students are more likely to stay committed in the courses, and **net conversion** will ***not significantly change***. 
 
The reason that the **number of user-id** is ***NOT chosen*** as an invariant metric is that it can change when the experiment happens. It is also not a good evaluation metric because it can’t prove that it reduces the number of frustrating users.

## 3. Measuring Standard Deviation

For each evaluation metric, estimate its standard deviation analytically, given a sample size of 5,000 cookies visiting the course overview page and the information below

<table>
<tr>
<td>Unique cookies to view course overview page per day:</td>
<td>40000</td>
</tr>
<tr>
<td>Unique cookies to click "Start free trial" per day:</td>
<td>3200</td>
</tr>
<tr>
<td>Enrollments per day:</td>
<td>660</td>
</tr>
<tr>
<td>Click-through-probability on "Start free trial":</td>
<td>0.08</td>
</tr>
<tr>
<td>Probability of enrolling, given click:</td>
<td>0.20625</td>
</tr>
<tr>
<td>Probability of payment, given enroll:</td>
<td>0.53</td>
</tr>
<tr>
<td>Probability of payment, given click</td>
<td>0.1093125</td>
</tr>
</table>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{Estimate&space;Standard&space;Deviation}=\sqrt{\frac{\hat{P}(1-\hat{P})}{N}}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;\text{Estimate&space;Standard&space;Deviation}=\sqrt{\frac{\hat{P}(1-\hat{P})}{N}}" title="\text{Estimate Standard Deviation}=\sqrt{\frac{\hat{P}(1-\hat{P})}{N}}" /></a>

### Gross Conversion

Gross Conversion =  Number of Enrolls/ Number of Clicks

N = Number of Clicks Sample = (Click Baseline/Cookie Baseline)*Cookie Sample = (3,200/40,000)*5,000 = 400 

P = Probability of enrolling, given click = 0.20625

**SD of Gross Conversion** = sqrt(0.20625(1 - 0.20625)/400) = 0.02023060414

### Net Conversion

Net Conversion = Number of Payments/ Number of Clicks

N = Number of Clicks Sample = (Click Baseline/Cookie Baseline)*Cookie Sample = (3,200/40,000)*5,000 = 400 

P = Probability of payment, given click = 0.1093125

**SD of Net Conversion** = sqrt(0.1093125(1 - 0.1093125)/ 400 = 0.0156

### Retention Rate

Retention = Number of Payments/Number of Enrolls

N = Number of Enroll Sample= (Enroll Baseline/Cookie Baseline)*Cookie Sample =(660/40,000)*5,000 = 82.5

P = Probability of payment, given enroll = 0.53

**SD of Retention** = sqrt(0.53(1 - 0.53)/82.5) =  0.05494901218

## 4. Sizing

### a. Number of Samples vs. Power

Since Retention Rate = Gross Conversion * Net Conversion, I will only use Gross and Net Conversion as my evaluation metrics. During my analysis phase, I choose to use Bonferroni correction because there are only two evaluation metrics and therefore, the method won’t be too conservative. 

- Bonferroni method: alpha individual = alpha overall/number of metrics = 0.05/2 = 0.025

Since the online calculator does not have option to choose alpha = 0.025, I calculate the size for alpha = 0.02 and alpha = 0.03 and take the average. 
https://www.evanmiller.org/ab-testing/sample-size.html
#### Gross Conversion

- Dmin = 0.01
- Baseline Conversion Rate = 0.20625
- Sample Size of Clicks (alpha = 0.025) = (33,014 + 29,844)/2 = 31,429

#### Net Conversion
- Dmin = 0.0075
- Baseline conversion rate = 0.1093125
- Sample Size of Clicks (alpha = 0.025) = (35,016 + 31,660)/2 = 33,338  

#### Page View needed 

- Page View Needed = (31,429 + 33,338)* (3,200/40,000) = 809,588

### b. Duration vs. Exposure
- Assuming there were no other experiments running, and the change is not risky, I chose fraction = 1. Meaning, 100% of Udacity's traffic will be diverted to this experiment

- Length of experiment = Page Views Needed/ Page View per Day = 809,588/40,000 = 21 days

- Since I chose to cover 100% of the traffic for this experiment, the risk is we do not have enough time to study the learning effect.

## 5. Analysis

### a. Sanity Checks
We want to check if invariant metrics are evenly spread in control group and experimental group so we need to check if the true proportion falls into confidence interval with 0.5 probability

***SD (binomial distribution) = sqrt[P(1-P)/(Ncont+Nexp)]***

#### Number of Cookies
- P = 0.5
- Total cookies in control: 345,543
- Total cookies in experiment: 344,660
- Fraction of Cookies = Total cookies in control/(Total cookies in control + Total cookies in experiment)= 345,543/(345,543 + 344,660) = 0.5006396669
- SD = sqrt(0.5(1-0.5)/(345,543 + 344,660)) = 0.0006018407403
- Margin of error(95% confidence level) = 1.96 * SD = 0.0012
- 95% Confidence interval = [0.5-0.0012, 0.5+0.0012] = [0.4988, 0.5012]

Since the fraction = 0.5006396669, which is in the range confidence interval, it passed sanity check!

#### Number of Clicks
- P = 0.5
- Total clicks in control: 28,378
- Total clicks in experiment: 28,325
- Fraction of Clicks = Total clicks in control/(Total clicks in control + Total clicks in experiment)= 28,378/(28,378 + 28,325) = 0.5004673474
- SD = sqrt(0.5(1-0.5)/ (28,378 + 28,325) = 0.00209974708
- Margin of error (95% confidence level) = 1.96 * SD = 0.0041
- 95% Confidence interval = [0.5 - 0.0041, 0.5 + 0.0041] = [0.4959, 0.5041]

Since the fraction = 0.5004673474, which is in the range confidence interval, it passed sanity check!

#### Click-Through Probability
- d = Xexp/Nexp - Xcont/Ncont = 0.08218244067 - 0.08212581357 = 0.000057
- P pool = (Xexp + Xcont)/(Nexp + Ncont) = 0.0821540909
- SD pool = sqrt[P pool * (1 - P pool) * (1/Nexp + 1/Ncont)] = 0.0006610608156
- Margin of error (95% confidence level) =  1.96 * SD pool = 0.0013
- Confidence interval =[-0.0013, 0.0013]

Since d = 0.000057, which is in the range confidence interval, it passed sanity check!

### b. Effect Size Tests

#### Gross Conversion
- d = Xexp/Nexp - Xcont/Ncont = 3,785/17,293 - 3,423/17,260 = -0.02056
- P pool = (Xexp + Xcont)/(Nexp + Ncont) = 0.2086
- SD pool = sqrt[P pool * (1 - P pool) * (1/Nexp + 1/Ncont)] = 0.0044
- Margin of error = 2.24 * SD pool = 0.0098 _(because I am using bonferroni correction: alpha=0.025 and Z-Score=2.24)_
- Confidence Interval = [d - Margin of Error, d + Margin of Error] = [-0.0303, -0.0108]

Since upper bound < 0, it is statistically significant. Upper bound also < d min = -0.01, it is practically significant.

#### Net Conversion
- d = Xexp/Nexp - Xcont/Ncont = 1,945/17,293 - 2,033/17,260 = -0.0049
- P pool = (Xexp + Xcont)/(Nexp + Ncont) = 0.1151
- SD pool = sqrt[P pool * (1 - P pool) * (1/Nexp + 1/Ncont)] = 0.0034
- Margin of error = 2.24 * SD pool = 0.0077 _(because I am using bonferroni correction: alpha=0.025 and Z-Score=2.24)_
- Confidence Interval = [d - Margin of Error, d + Margin of Error] = [-0.0126, 0.0028]

Since 0 is in the confidence interval, it is not statistically significant. Also, d min = -0.0075 is in the confidence interval, it is not practically significant.

### c. Sign Tests
https://www.graphpad.com/quickcalcs/binomial1.cfm
#### Gross Conversion
- Total Days: 23
- Days with higher gross conversion: 4
- two-tail P value = 0.0026

Since P value < 0.05, it is statistically significant

#### Net Conversion
- Total Days: 23
- Days with higher gross conversion: 10
- two-tail P value= 0.6776

Since P value < 0.05, it is not statistically significant

## 6. Recommendation

I would recommend **launching** this change in the Udacity website!

The analysis above shows that adding the question after students click “START FREE TRIAL” button can significantly decrease Gross Conversion rate both statistically and practically. It means there are more students who won't try the free trial if they know they cannot spend enough time for the courses. This will help Udacity improves the students experience on the website! The experiment also does not affect the Net Conversion rate both statistically and practically, meaning it will not only affect the conversion of users in overall process but also help students have a better learning experience on Udacity!




















