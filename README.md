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

**Metric choice:**

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

**Baseline:** How the metrics behave before the change.

|Item|	Description|	Estimator|
| ----- | ------ | -------| 
|Number of cookies|	Unique cookies to view course overview page per day:|	40000|
|Number of clicks	| Unique cookies to click "Start free trial" per day:|3200|
|Number of enrollments|Enrollments per day:|660|
|CTP|	Click-through-probability on "Start free trial":	|0.08|
|Gross Conversion|Probability of enrolling, given click:|	0.20625|
|Retention|Probability of payment, given enroll:|	0.53|
|Net Conversion|Probability of payment, given click| 0.109313|

**Measuring Variability:**

Estimate the standard deviation analytically for each metric selected as an evaluation metric.

