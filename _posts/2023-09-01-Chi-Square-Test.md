---
layout: post
title: Assessing Campaign Performance Using Chi-Square Test For Independence
toc: true
image: "/posts/ab-testing-title-img.png"
tags: [AB Testing, Hypothesis Testing, Chi-Square, Python]
---

## Project Overview
### Project Purpose
The purpose of this project is to demonstrate understanding of hypothesis testing using the Chi-Square Test for Independence to assess performance of two mailers promoting a new service.<br><br>

## GitHub Repository Link
The source code and documentations for this project can be found at my [GitHub repository](https://github.com/Wint-Thandar/python-projects/tree/main/chi_square_test).<br><br>

### Context
Earlier this year, a grocery retailer client ran a campaign promoting a new "Delivery Club" - a $100/year membership offering free grocery delivery instead of the normal $10 fee.<br><br>

The campaign randomly split customers into three groups:<br>

- Group 1 received a low-quality, low-cost mailer.<br>
- Group 2 received a high-quality, high-cost mailer.<br>
- Group 3 received no mailer (control).<br><br>

The client observed higher signup rates for groups receiving mailers versus the control group. Now they want to determine if there is a significant difference between the two mailer types, to make informed decisions for future campaigns aimed at optimizing return on investment.<br><br>

### Actions
The **Chi-Square Test for Independence** compares *rates* between groups, calculating a test statistic based on observed and expected frequencies. It was chosen over a **Z-Test for Proportions** because:<br>

* The resulting test statistic for both tests will be the same.<br>
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders.<br>
* The Chi-Square Test can extend out to more than 2 groups - meaning the client can have one consistent approach to measuring signficance.<br>

The data was extracted from the *campaign_data* table, isolating Group 1 and Group 2, excluding the control group. <br><br>

***Hypotheses*** and `0.05` ***acceptance criteria*** were defined as:<br>

**Null Hypothesis:** No relationship between mailer type and signup rate. They are independent.<br>
**Alternate Hypothesis:** Relationship exists between mailer type and signup rate. They are not independent.<br>
**Acceptance Criteria:** `0.05`<br>

The data was aggregated into a 2x2 matrix of *`signup_flag`* by *`mailer_type`* and analyzed using the Chi-Square Test to calculate the test statistic, p-value, degrees of freedom, and expected values.<br><br>

### Results & Discussion
Observed signup rates were:<br>

* Mailer 1 (Low Cost): **`32.8%`** signup rate<br>
* Mailer 2 (High Cost): **`37.8%`** signup rate<br><br>

However, the Chi-Square Test gave:<br>

* Chi-Square Statistic: **`1.94`**<br>
* p-value: **`0.16`**<br><br>

With a `0.05` acceptance criteria, the critical value is **`3.84`**.<br><br>

Since the p-value of `0.16` exceeds `0.05`, we retain the null hypothesis that no statistically significant relationship exists between mailer type and signup rate.<br><br>

Although Mailer 2 had a higher observed rate, the difference is not statistically significant based on this test. Making campaign decisions based solely on observed rates could lead to overspending without improving results.<br><br>

Without running this Hypothesis Test, the client may have concluded that they should always look to go with higher cost mailers - and from what we've seen in this test, that may not be a great decision.  It would result in them spending more, but not *necessarily* gaining any significant extra revenue as a result.<br><br>

Additional A/B testing over time may provide more insight into differences between the mailers. For now, we cannot conclude a significant performance difference exists.<br><br>

___

## Concept Overview
#### A/B Testing
A/B Testing randomly splits users into groups A and B, each receiving a different experience, to measure and compare responses. This guides data-driven business decisions. Applications range from testing ads, email subject lines, or mailing coupons versus a control group. Amazon constantly A/B tests website features to stay competitive. Reportedly, Netflix A/B tests images for the same movie to pull in more viewers.<br><br>

#### Hypothesis Testing
Hypothesis testing assesses the plausibility of an assumed viewpoint based on sample data - determining if a perspective on data is likely true. There are many different scenarios Hypothesis Tests can be applied to, and they all have slightly different techniques and formulas - however they all have some shared, fundamental steps & logic that underpin how they work.<br>

**Null Hypothesis**
The initial assumption that a result is purely by chance or no relationship exists between groups. The aim of the Hypothesis Test is to look for evidence to support or reject the Null Hypothesis. If the Null Hypothesis is rejected, that would mean we’d be supporting the Alternate Hypothesis.<br><br>

**Alternate Hypothesis**
The Alternate Hypothesis is essentially the opposite viewpoint to the Null Hypothesis - that the result is *not* by chance, or that there *is* a relationship between two outcomes or groups.<br><br>

**Acceptance Criteria**
The p-value threshold chosen prior to the test for rejecting/supporting the null hypothesis, typically `0.05`. Lower thresholds require higher confidence in non-random outcomes.<br><br>

**Types of Hypothesis Test**
There are many different types of Hypothesis Tests, each of which is appropriate for use in differing scenarios - depending on a) the type of data that you’re looking to test and b) the question that you’re asking of that data. Here, comparing signup *rates* between groups, the Chi-Square Test for Independence was selected.<br><br>

#### Chi-Square Test For Independence
This test assumes observed categorical frequencies will match expected frequencies across groups. <br><br>

The *assumption* represents the null hypothesis of no difference between groups. The test calculates a statistic that is compared to the acceptance criteria to reject or support this assumption based on the actual observed data.<br><br>

The *observed frequencies* come directly from the data itself.<br><br>

The *expected frequencies* are calculated from the data as a whole.<br><br>

___

## Data Overview & Preparation
The *campaign_data* table contained customer group assignments and Delivery Club signup flags. <br><br>

The data was imported and filtered to only include Group 1 (Mailer 1) and Group 2 (Mailer 2), excluding the control group.<br><br>

The python code below:<br>

* Load in the Python libraries we require for importing the data and performing the chi-square test (using scipy)<br>
* Import the required data from the *`campaign_data`* table<br>
* Exclude customers in the control group, giving us a dataset with Mailer 1 & Mailer 2 customers only<br>

```python
# install the required python libraries
import pandas as pd
from scipy.stats import chi2_contingency, chi2

# import campaign data
campaign_data = pd.read_excel("grocery_database.xlsx", sheet_name = "campaign_data")

# remove customers who were in the control group
campaign_data = campaign_data.loc[campaign_data["mailer_type"] != "Control"]
```
<br>
A sample of this data (the first 10 rows) can be seen below:
<br>

| **customer_id** | **campaign_name** | **mailer_type** | **signup_flag** |
|---|---|---|---|
| 74 | delivery_club | Mailer1 | 1 |
| 524 | delivery_club | Mailer1 | 1 |
| 607 | delivery_club | Mailer2 | 1 |
| 343 | delivery_club | Mailer1 | 0 |
| 322 | delivery_club | Mailer2 | 1 |
| 115 | delivery_club | Mailer2 | 0 |
| 1 | delivery_club | Mailer2 | 1 |
| 120 | delivery_club | Mailer1 | 1 |
| 52 | delivery_club | Mailer1 | 1 |
| 405 | delivery_club | Mailer1 | 0 |
| 435 | delivery_club | Mailer2 | 0 |

<br>

In the `DataFrame` we have:<br>

* `customer_id`
* `campaign_name`
* `mailer_type` (either Mailer1 or Mailer2)
* `signup_flag` (either 1 or 0)

___

## Applying Chi-Square Test For Independence
#### Set Hypotheses & Acceptance Criteria For Test
In the code below, Hypotheses and `0.05` acceptance criteria are specified:<br>

```python
# specify hypotheses & acceptance criteria for test
null_hypothesis = "No relationship between mailer type and signup rate. They are independent."
alternate_hypothesis = "Relationship exists between mailer type and signup rate. They are not independent." 
acceptance_criteria = 0.05
```

<br>

#### Calculate Observed Frequencies & Expected Frequencies
As mentioned in the section above, the *observed frequencies* are the actual rates per group in the data itself. The *expected frequencies* are the *expected* rate calculated based on *all* of the data combined.<br><br>

The python codes below:<br>

* Summarises the dataset to a 2x2 matrix for *`signup_flag`* by *`mailer_type`*<br>
* Based on this, calculates the:<br>
    * Chi-Square Statistic<br>
    * p-value<br>
    * Degrees of Freedom<br>
    * Expected Values<br>
* Prints out the Chi-Square Statistic & p-value from the test<br>
* Calculates the Critical Value based on Acceptance Criteria & the Degrees Of Freedom<br>
* Prints out the Critical Value<br>

```python
# aggregate data to get observed values
observed_values = pd.crosstab(campaign_data["mailer_type"], campaign_data["signup_flag"]).values

# run the chi-square test
chi2_statistic, p_value, dof, expected_values = chi2_contingency(observed_values, correction = False)

# print chi-square statistic
print(chi2_statistic)
>> 1.94

# print p-value
print(p_value)
>> 0.16

# find the critical value of the test
critical_value = chi2.ppf(1 - acceptance_criteria, dof)

# print critical value
print(critical_value)
>> 3.84
```
<br>
Based on the observed values, the signup rates are:<br>

* Mailer 1 (Low Cost): **32.8%** signup rate<br>
* Mailer 2 (High Cost): **37.8%** signup rate<br>

From this, we can see that the higher cost mailer does lead to a higher signup rate.  The results from our Chi-Square Test will provide us more information about how confident we can be that this difference is robust, or if it might have occured by chance.<br><br>

We have a Chi-Square Statistic of **`1.94`** and a p-value of **`0.16`**.  The critical value for our specified Acceptance Criteria of `0.05` is **`3.84`**.<br><br>

***Note*:** The Chi-Square Test is run with the *`correction`* parameter set to *`False`*. This denotes the application of the **Yates Correction** when your Degrees of Freedom are equal to 1.  In this situation, the correction helps to avoid overestimating statistical significance.<br><br>

___

<br>

## Analysing The Results
With a p-value of **`0.16`** exceeding the `0.05` acceptance criteria, we _retain_ the Null Hypothesis and conclude that there is no significant difference between the signup rates of Mailer 1 and Mailer 2.<br><br>

The Chi-Square statistic of **`1.94`** also fell below the `3.84` critical value, leading to the same conclusion.<br><br>

To make this script more dynamic, we can create code to automatically interpret the results and explain the outcome:<br>

```python
# print the results (based on p-value)
if p_value <= acceptance_criteria:
    print(f"As our p-value of {p_value} is lower than our acceptance_criteria of {acceptance_criteria} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our p-value of {p_value} is higher than our acceptance_criteria of {acceptance_criteria} - we retain the null hypothesis, and conclude that: {null_hypothesis}")

>> As our p-value of 0.16351 is higher than our acceptance_criteria of 0.05 - we retain the null hypothesis, and conclude that: "No relationship between mailer type and signup rate. They are independent."


# print the results (based on chi-square statistic)
if chi2_statistic >= critical_value:
    print(f"As our chi-square statistic of {chi2_statistic} is higher than our critical value of {critical_value} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our chi-square statistic of {chi2_statistic} is lower than our critical value of {critical_value} - we retain the null hypothesis, and conclude that: {null_hypothesis}")
    
>> As our chi-square statistic of 1.9414 is lower than our critical value of 3.841458820694124 - we retain the null hypothesis, and conclude that: "No relationship between mailer type and signup rate. They are independent."
```
<br>
As we can see from the outputs of these print statements, although Mailer 2 had a higher observed rate, we do not have evidence of a statistically significant difference between the mailers based on this test.<br><br>

___
<br>

## Discussion
The client may have concluded higher cost mailers perform better, but our results found the difference was not statistically significant. Making decisions based solely on observed rates could lead to overspending without improving results.<br><br>

Additional A/B testing over time may reveal more conclusive insights into mailer performance differences. For now, we cannot confirm a significant difference exists.<br><br><br><br>
