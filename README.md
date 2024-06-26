**Udacity A/B testing final project**

**Background:** Udacity offers a free online A/B testing course. The course provides a comprehensive introduction to the principles and practices of A/B testing. This course demonstrated learners the knowledge and skills to design, implement, and analyze A/B tests in real-word scenarios. This page presents a walkthrough of the final project results.

**What is A/B testing?** A/B testing also known as split testing, is an experiment to compare two or more versions of a service to determine which one performs better.

**Why A/B testing?** A/B testing can help to make data-driven decision and optimize the service performance.

**How to conduct A/B testing?** A/B testing involves serval key steps: experiment design, metric choice, estimating baseline values of valuation metrics, experiment sizing, data analyzation, results and recommendation.

**Current stage:**

Udacity homepage overview presents two options:

1. Start free trial. If student clicks ‘start free trial’, they will be asked to enter their credit card information, then get enrolled in a free trial of the paid version of the course. After 14 days, the student will automatically be charged unless they cancel first.

2. Access course materials. If student clicks ‘access course materials’, they will be able to view the course content but receive no coaching or certificate, and they cannot get feedback after submitting final project.

**Future stage:**

If the student selected ‘start free trial’, they were prompt to specify the amount of time they could dedicate to the course each week. If the student indicated 5 or more hours per week, the checkout process is as usual. If they indicated less than 5 hours per week, a message would appear informing them that Udacity courses require more time commitment for successful completion. They would then be encouraged to access the course materials for free instead. The student had the choice to proceed with the free trial or opt to access the course materials for free. A screenshot demonstrating the updated page is provided.

**Hypothesis:**
By setting clearer expectations for student upfront, it would reduce the number of frustrated students who abandoned the free trial due to time constraints, while maintaining the number of students who continued past the free trial and completed the course. If the hypothesis proven true, this could enhance the overall student experience and optimize coaches capacity to support students who likely to complete the course.

**Unit of diversion:**
Unit of diversion is how we define what an individual subject is in the experiment. In this experiment, the unit of diversion is a cookie. However, once a student enrolls in the free trial, they are tracked using their user ID. Each user ID can only enroll in the free trial once. If users do not enroll, their user ID is not included in the experiment data, even if they were logged in when they visited the course overview page.

**Cookies uniqueness:** 
Whenever the term "unique cookies" is mentioned, their uniqueness is assessed on a daily basis. This means that if the same cookie visits on different days, it will be counted as separate instances. User IDs are inherently unique as the website prohibits duplicate enrollments under the same user ID.

**Metric choice**

**Invariant metric:** The metrics should not change across your experiment and control. Invariant metric serve as ‘sanity checks’ to ensure the validity of our experiment, both in terms of how we introduced a change to a portion of the population and how we gathered data. There are two types of invariant metrics. Population sizing metrics, and any other metrics you don’t expect to change.

|         Metric Name      |         Metric Formula |    d<sub>min</sub>  |             Notation | Metric Type | 
| ------------------------ | ---------------------- | --------------------| ---------------------|-------------| 
|Number of Cookies in Course Overview Page|# unique daily cookies on page|3000 cookies|*C*<sub>cookies</sub>|Population sizing metric|
|Number of Cookies click on 'Start free trial' Button|# unique daily cookies who clicked|240 clicks|*C*<sub>clicks</sub>| Population sizing metric|
|Free Trial button Click-Through-Probability|*C*<sub>clicks</sub> / *C*<sub>cookies</sub>|0.01|CTP|Population sizing metric|

**Evaluation metric:** The metrics where we anticipate observing a change, and they are aligned with the business objectives we seek to accomplish. For each metric, we establish a dmin value, which is the practical significance boundary for each metric. They are given as absolute changes and they represent the minimum change that is practically meaningful to the business.

|         Metric Name      |         Metric Formula |    d<sub>min</sub>  |             Notation | Metric Type | 
| ------------------------ | ---------------------- | --------------------| ---------------------|-------------| 
|Gross Conversion|*C*<sub>enrolled</sub> / *C*<sub>clicked</sub>|0.01|*Conversion*<sub>Gross</sub>|Performance metric|
|Retention|*C*<sub>paid</sub> / *C*<sub>enrolled</sub>|0.01|*Retention*|Performance metric|
|Net Conversion|*C*<sub>paid</sub> / *C*<sub>clicked</sub>|0.0075|*Conversion*<sub>Net</sub>|Performance metric|

**Estimating baseline values:**

**Baseline:** How the metrics behave before the change (pageview = 5000).

|Item|	Description|	Estimator|
| ----- | ------ | -------| 
|Number of cookies|	Unique cookies to view course overview page per day:|	40000|
|Number of clicks	| Unique cookies to click "Start free trial" per day:|3200|
|Number of enrollments|Enrollments per day:|660|
|CTP|	Click-through-probability on "Start free trial":	|0.08|
|Gross Conversion|Probability of enrolling, given click:|	0.20625|
|Retention|Probability of payment, given enroll:|	0.53|
|Net Conversion|Probability of payment, given click| 0.109313|

**Measuring Variability**

Estimate the standard deviation analytically for each metric selected as an evaluation metric.

Below is a scaled-downed version of metrics:

|Item|	Estimator|
| ------ | -------| 
|Baseline CTP|	0.08|
|Baseline clicks|$$3200* {\frac{5000}{40000}}=400$$|
|Baseline cookies|5000|
|Baseline enrollments|$$660 * {\frac{5000}{40000}}=82.5$$|
|Baseline gross conversion|0.20625|
|Baseline Retention|	0.53|
|Baseline net conversion|0.109313|

**Analytical estimate of standard deviation:**

We assume probabilities are binomially distributed and apply the below formula:

$$S.D.=\sqrt{\frac{\hat{p}*(1-\hat{p})}{n}}$$


|Evaluation Metric|Standard Deviation|
|----------|---------|
| Gross Conversion  | 0.0202 |
| Retention         | 0.0549 |
| Net Conversion    | 0.0156 |

- Gross Conversion: Number of users to enroll in a free trial divided by the number of cookies clicking the free trial.
- Retention: Number of paying users (remain enrolled past 14 free days) divided by the number of total enrolled users.
- Net Conversion: Number of paying users divided by the number of cookies that clicked the free trial button.

Variability tends to be lower and closer to analytical estimate when unit of analysis (denominator of the metric) equals to unit of diversion. The two units are the same for Gross Conversion and Net Conversion but not for Retention. Need to calculate empirically if decide to use Retention.

**Sizing**

**Number of Samples given Power:**

Given $\alpha$ =0.05 (significance level) and $\beta$ = 0.2 (power), we want to estimate how many total pageviews (cookies who viewed the course overview page) we need in the experiment. This amount will be divided into two groups: control and experiment. We can calculate by using the below formula:

$$ n = \frac{{(Z_{1-\alpha/2} \cdot sd_1 + Z_{1-\beta} \cdot sd_2)^2}}{{d^2}} $$

$$ SD_1 = \sqrt{p(1-p) + p(1-p)} $$

$$ SD_2 = \sqrt{p(1-p) + (p + d) \left( 1 - \left( 1 - (p + d) \right) \right)} $$

- n: minimum sample size required for each group.
​
- $Z_{1-\alpha/2}$: the critical value for the desired level of statistical significance.
​
- $Z_{1-\beta}$: the critical value for the desired statistical power.

- SD1: the standard deviation for the control group.

- SD2: the standard deviation for the treatment group.

- p: the probability of success (e.g., conversion rate).

- d: the minimum detectable effect size.

| Metric                       | Gross conversion       | Retention| Net conversion|
|-----------------------------|------------------------|-----------|---------------|
| Baseline Conversion Rate    | 0.20625                |0.53|0.109312|
| d_min                       | 0.01                   |0.01|0.0075|
| Alpha                       | 0.05                   |0.05|0.05|
| Beta                        | 0.2                    |0.2|0.2|
| Power (1-beta)              | 0.8                    |0.8|0.8|
| Sample Size (enrollments/group) | 25,835             |78,230|54,826|
| Total sample Size | 51,670             |39,115|27,413|
| Number of groups            | 2 (treatment and control)| 2 (treatment and control)| 2 (treatment and control)|
| Clicks / Pageview           |3200/40000 = 0.08 |660/40000 = 0.0165|3200/40000 = 0.08|
| Pageviews (cookies)         | 645,875                |4,741,212|685,325|

**Duration vs. Exposure:**

The maximum sample size represents the effective size, which should be assessed regarding the efficiency of duration and exposure. It's essential to consider how long it will take to obtain this number of samples for the experiment.
- Retention has 4.7 million pageviews. Given we have 40,000 page views per day, it would take us 119 days to get all the samples. We will drop this metric because including it will bias our results.
- Net conversion has the highest sample size which is 685,325. This means an 17-day experiment with 100% diversion and 22-day experiment with 80% diversion.

**Analysis** 

**Sanity Checks:** 

Check whether the invariant metrics are equivalent between the two groups. If the invariant metric is a simple count that should be randomly split between the 2 groups, use a binomial test. Otherwise, construct a confidence interval for a difference in proportions.

It is expected that approximately half (50%) of the total pageviews in control group and experiment group. A binomial random variable will be the number of successes it is expected to get out of N experiments. When N gets larger, we can apply central limit theorem by approximate the binomial distribution to a normal distribution with a mean of p and a standard deviation $\sqrt{\frac {p*(1-p)}{n}}$

Difference between counts: 

$$\hat{P}= \frac{X}{N}$$

$$X \sim N(\mu, \sqrt{\frac {p*(1-p)}{n}})$$

$$M.E. = Z_{1-\alpha/2}* \sqrt{\frac {p*(1-p)}{n}}$$

$$C.I. = [\hat{p} - M.E., \hat{p} + M.E. ]$$

| Metric | Control | Experiment| p|$\hat{p}$| Confidence interval| Result|
|-------------------------------|---------|------------------------|-------------------------|---|---|--|
| Number of Cookies| 345543  |344660|0.5|0.5006|0.4988 to 0.5012| Pass
| Number of Clicks on Free Trial Button| 28378  |28325|0.5|0.5005|0.4959 to 0.5042| Pass

Differences between probabilities: The proportion of clicks given a pageview (observed CTP) is about the same in both groups since this should not change due to the experiment. It is expected to see no difference (CTP<sub>exp</sub>−CTP<sub>cont</sub>) = 0

$$\hat{P}<sub>pool</sub> = \frac{X_{exp}+X_{cont}}{N_{exp}+N_{cont}}$$

$$S.D._{pool} = \sqrt{\hat{P}<sub>pool</sub>*(1-\hat{P}<sub>pool</sub>) * ({\frac{1}{n_1}+ \frac{1}{n_2}})}$$

$$\hat{d} = \frac{X_{exp}}{N_{exp}} - \frac{X_{cont}}{N_{cont}}$$

| Metric | Total clicks | Total pageviews| p|$\hat{p}$| $\hat{d}$|M.E.|Confidence interval| Result|
|-------------------------------|---------|------------|------------|-----------|---------------|---|---|--|
| Free Trial button Click-Through-Probability|56703|690203|0.0821|0.0822|0.0001|0.0013|-0.0013 to 0.0013| Pass

**Check for Practical and Statistical Significance**

A metric is statistically significant if the confidence interval does not include 0 (that is, you can be confident there was a change), and it is practically significant if the confidence interval does not include the practical significance boundary (that is, you can be confident there is a change that matters to the business.)

| Metric | dmin | Observed Difference | CI Lower Bound | CI Upper Bound | Result |
|------|--------------|--------------|--------------|--------------|------|
| Gross Conversion | 0.01 | -0.0205 | -0.0292 | -0.012 | Both satistically and practically significant |
| Net Conversion | 0.0075 | -0.0049 | -0.0116 | 0.0018 | Not statistically or practically significant |

- The negative change of 2.06% means the Gross Conversion rate of the experiment group (the one exposed to the change, i.e. asked how many hours they can devote to studying) has decreased and this change was significant. This means less people enrolled in the Free Trial after due to the pop-up.

- A change of less than a 0.5% is a very small decrease which is not statistically significant, and not practically significant.

- There is only one practical and statistical significant metric so no Bonferroni correcton needed.

**Sign Tests**

The sign test is a statistical method to test for consistent differences between pairs of observations. It is to calculate how likely the experiment side had a higher measurement than the control side that occurred if there was no difference between the two sides. Downside is it does not estimate the size of the effect. The upside is it is easy to do and it is applicable under a lot of circumstances. We could use the non-parametric sign tests to compare the results to what we got from parametric hypothesis test. We use binomial distributs with p = 0.5 and number of total days to estimate the probability of x days being a success (higher metric value in experiment). 

$$p(successes) = \frac{n!}{k!(n-k)!} p^{x}(1-p)^{n-x}$$

| Metric | p-value for sign test | Statistically Significant @ $\alpha$ = 0.05 |
|------|--------------|--------------|
| Gross Conversion | 0.0026 | Yes |
| Net Conversion | 0.6774 | No |

**Recommendation**

The aim of this study was to assess if filtering students based on their study time commitment would enhance the student experience and enable coaches to better support those likely to complete the course, while retaining a satisfactory number of students past the free trial. Although a statistically and practically signficant decrease in Gross Conversion was observed, indicating fewer enrollments, there were no significant differences in Net Conversion, implying no increase in the number of students staying for the required 14 days to initiate payment. Given these findings, I recommend not to launch and suggest exploring alternative experiments instead.

**Follow-Up Experiment: How to Reduce Early Cancellations**

We can break down the issue by first define 'cancel early'. It means student who enrolled but cancelled before the end of 14 day trial period and therefore before paying. Then we want to understand the reason why students cancel early. It could be due to course content not in line with students' expectations. To prevent cancelling early, there are two timepoint for intervention: 1. before clicking free-trial 2. after clicking free-trial but before payment.

In the above experiment, a pop-up window asking students time commitment is to filter out students who likely to become frustrated later in the course. This filter did not focus on other reasons student may cancel early. A student may cancel early if they do not have the pre-requisite skill set and knowledge. That is, their committed time may not be enough if they don't come in with the pre-requisite skill set.

Appending a checklist of prerequisite skills in the popup related to time commitment could provide valuable insights. This experiment would utilize the infrastructure and data pipeline of the original experiment and would be structured similarly, including the unit of diversion. The only variance would be the content of the form. If a student's response satisfies both the time and prerequisite criteria (as indicated by a radiobox checklist), they would be guided to enroll in the free trial; otherwise, they would be encouraged to access the free version. This experiment requires minimal resources and may enhance the effectiveness of the pre-enrollment filter. A successful experiment would involve a significant decrease in Gross Conversion combined with a significant increase in Net Conversion.

For post-enrollment but pre-payment, multiple approaches can be used and deployed together with pre-enrollment intervention.
An ideal approach would be one which minimizes the use of additional coaching resources to best meet the original intent of the intervention. One effective method could involve implementing peer coaching or guidance through team collaboration. By forming teams of students who can support each other, discuss coursework, share frustrations, and hold each other accountable, students may be more inclined to persevere through challenges and remain engaged in the long term. The experiment would operate as follows.

Setup: After enrollment, students will be assigned randomly to either a control group, where they will not be grouped into a team, or an experimental group, where they will be.

Null Hypothesis: Engagement in a team will not significantly increase the number of students enrolled beyond the 14 day free trial period.

Unit of Diversion: The unit of diversion will be user-id as the change happens after a student creates an account and enrolls in a course.

Invariant Metrics: The invariant metric will be user-id as equal distribution between experiment and control is expected as a property of the setup.

Evaluation Metrics: The evaluation metric willl be Retention. The change is successful when there is a statistically and practically significant increase in Retention.

If there is a statistically and practically significant improvement in Retention, and assuming it has an acceptable impact on overall Udacity resources (since establishing and maintaining teams will require resources), the experiment will be launched.
