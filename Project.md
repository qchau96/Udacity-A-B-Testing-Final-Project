# A/B Testing Project - Udacity Free Trial Screener
## Table of Contents
1. [Overview](#overview)
2. [Metric Choice](#metric-choice)
   - [Invariant Metrics](#invariant-metrics)
   - [Evaluation Metrics](#evaluation-metrics)
3. [Measuring Standard Deviation](#measuring-standard-deviation)
   - [Gross conversion](#gross-conversion)
   - [Net conversion](#net-conversion)
4. [Sizing](#sizing)
   - [Choosing Number of Samples given Power](#choosing-number-of-samples-given-power)
   - [Choosing Duration vs. Exposure](#choosing-duration-vs-exposure)
5. [Analysis](#analysis)
   - [Sanity Checks](#sanity-checks)
   - [Effect Size Tests](#effect-size-tests)
   - [Run Sign Tests](#run-sign-tests)

## 1. Overview

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback. In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.


The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.


The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## 2. Metric Choice

Below is the Conversion Funnel base on the information provided

### a. Invariant Metrics

**Number of cookies** Number of unique cookies to view the course overview page. (dmin=3000)

**Number of clicks** Number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)

**Click-through-probability** Number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)

Since these metrics measure user activities before the experiment starts, they must be not variant due to applying this change. Therefore, these metrics above should be invariant metrics for this experiment.

### b. Evaluation Metrics

**Gross conversion** Number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)

**Retention** Number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)

**Net conversion** Number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

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




