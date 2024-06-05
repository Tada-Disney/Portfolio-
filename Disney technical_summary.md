# A/B Test Report: Food and Drink Banner

## Purpose
The purpose of this project is to conduct an A/B test to evaluate the impact of a banner that showcase key products in the food and drink category on the GloBox website. The banner is designed to increase the awareness and revenue of this product category, which has grown significantly in the last few months. The banner will be displayed at the top of the website for the test group, while the control group will see the website as usual. The project will measure the difference in conversion rate and average amount spent between the two group. 

## Hypotheses
H0: p1 = p2
Ha: p1 != p2
H0: uA - uB = 0
Ha: uA - UB != 0

## Methodology
### Test Design
- **Population:** The test group saw the banner at the top of the website, while the control group saw the website as usual, without the banner.
- **Duration:** The test period lasted from 2023-01-25 to 2023-02-06.
- **Success Metrics:** We measured the conversion rate and the average amount spent by each group during the test period.

## Results
### Data Analysis
- **Pre-Processing Steps:** I wrote the following SQL queries. The COALESCE function is used to fill in the null values in the spent and country columns. A CASE statement is used to categorise customers into two groups and stored the column conversion
The first query is for obtaining the first analysis dataset
```sql
SELECT DISTINCT users.id, COALESCE(users.country, 'unknown') AS country, 
COALESCE(users.gender, 'O') AS gender,
groups.group,
COALESCE(groups.device,'Unknown') AS device,
CASE
WHEN activity.spent > 0 THEN 'Yes' ELSE 'No' END AS conversion,
ROUND(COALESCE(activity.spent,0),2) AS spent
FROM users
INNER JOIN groups ON users.id = groups.uid
LEFT JOIN activity ON groups.uid = activity.uid;
```

The second SQL query was used to acquire the dataset for the purpose of examining the novelty effect in user activity
```sql
SELECT groups.group, groups.join_dt, 
CASE WHEN activity.spent > 1 THEN 1 ELSE 0 END AS conversion
from groups
LEFT JOIN activity ON groups.uid = activity.uid;
```

- **Statistical Tests Used:** Proportions_ztest()for two samples, T**wo sample mean test statistic, 95% Confidence interval for the difference between means,
95% Confidence interval for difference in proportions

- **Results Overview:** 
- This report presents the results of an A/B test conducted to evaluate the impact of a banner that showcases key products in the food and drink category on the GloBox website. The banner was designed to increase the awareness and revenue of this category, which has grown significantly in the last few months. The research question was whether the banner would increase the conversion rate and the average amount spent by the visitors who saw it compared to those who did not.The test group consisted of 24,600 visitors who were randomly selected and shown the banner at the top of the website, while the control group consisted of another 24,343 visitors who saw the website as usual. The test ran for 12days.Proportions_ztest()for two samples with a significance level of 0.05 was used to compared the difference in proportions between the two groups. A two-sample t-test with a significance level of 0.05 was used to compare the means of the two groups.The report provides the descriptive statistics, the p-values, the conclusions, and the 95% confidence intervals for the difference in conversion rate and average amount spent between the test and control groups.
 
## Findings
- We performed a two-proportion z-test to compare the proportions of group A and group B. The z-test statistic was -3.86 and the p-value was 0.0001, which is less than the significance level of 0.05.
- 95% Confidence interval for difference in proportions between the two group is 0.0035 and 0.0107
-We perfomed a two-sample mean test statistic for difference in the average amount spent between group A and Group B.The resulting P-value is 0.944 which is more than the significance level of 0.05
- 95% confidence interval for the difference in the average amount spent between the treatment and control group is -0.44 and 0.047

## Interpretation
- **Outcome of the ZTest(s):** The resulting p-value(0.0001) is less than the significance level(0.05). Therefore, we rejected the null hypothesis that the proportions are equal. 
- **Confidence Level:** We are 95% confident that the true difference in conversion rate between group A and group B in the population is between 0.0035 and 0.0107 percentage points
- **Outcome of the t-test:** The resulting P-value for difference in average spent is 0.944 which is more than significance level of 0.05. Therefore we fail to reject the null hypothesis that there is no difference between the average amount spent between the two groups.  
- **Confidence Level:** We are 95% confident that the true mean lies between -0.44 and 0.47.

## Conclusions
- **Key Takeaways:**
- We concluded that there is a statistically significant difference between the proportions(conversion rate) of group A and group B.
- There is not enough evidence to conclude that there is a significant difference in spending between the two groups
- We did not see any sign of novelty effect in our experiment, as the participants’ performance did not change significantly after the introduction of the new feature. 

- **Limitations/Considerations:** 
- **Sample Size and Generalizability:**
- The sample mean calculator suggested an impractical sample size (103,440,214) based on the provided standard deviation.
- Our actual sample size is 48,943, significantly smaller.
- As a result, our findings may have limited generalizability to the broader population.
- Interpret results cautiously, considering the potential impact of the sample size on validity and reliability.

- **Statistical Power and Required Sample Size:**
- Achieving adequate statistical power is crucial for detecting meaningful effects.
- An acceptable power level is typically set at 80% or higher.
- To achieve this, we would need a larger sample size than our current one.
- Balancing practical constraints with the desired power level is essential.

- **Effect Size:**
- The difference between the sample means (0.009999999999999787 units) was minimal.
- Larger effect sizes enhance the test’s ability to detect significant differences.

In conclusion, while our study provides valuable insights, researchers should interpret the results cautiously, considering the impact of sample size and other limitations. Future investigations could explore alternative power analyses and effect sizes to refine our understanding of the topic

 
## Recommendations
- **Next Steps:** We didn’t see enough improvement in our success metrics to be confident in releasing the feature in its current state. However, perhaps there were some promising results that show we could possibly make changes to the banner experience and get better improvement next time. Maybe we could do some further data analysis to understand this better, or we need a larger sample size to make a confident recommendation. Therefore, we recommend the stakeholder to:
- Provide us with more budget and resources to collect and analyse more data and feedback from the customers.
- We request your approval to extend the deadline and scope of our project to allow us to run the experiment for a longer time and test different variations of the banner ad feature. We believe this will help us improve the banner ad performance and achieve our success metrics.

- **Further Analysis:** We could adjust the confidence level and re-run the analysis to see if it yields different result. We could also consider exploring other factors or variables that maybe influencing the average spent.

