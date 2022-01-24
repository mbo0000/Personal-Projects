# Stack Overflow
## Background
This project is a hypothetical case study for a marketing campaign in an effort to increase user engagement of Stack Overflow. 

## Business Task:
To increase user engagement, the team at Stack Overflow launched a Re-engagement email campaign. The Senior Leadership Team and Marketing team are requesting a weekly report on the performance of the campaign. Tasks given to the Analytic team are to define the key metrics to measure the campaign success and the method to deliver the metrics. 


## Key Stakeholders
- SLT
- Marketing

## Project Goals:
- Identify key metrics to measure campaign progress and method of delivering weekly results.
- Provide insight or recommendations if any. 

## Scope
- Using publicly available data through BigQuery Public dataset service.
- Metrics are confined to public users only. 
- Data is confined to September of 2019 and before the present time frame in this case study. The time is before the pandemic shutdown of 2020 and 2021. 2019 provides more stable and reliable data to analyze. 

## Understanding DAU and Users Demographic
Daily Active Users(DAU) is defined by users' activities of logging in to the site, creating a comment or a post. Before digging into the metrics asked above, I would like to see the current DAU metric for the past 30 days. This will provide insight into users' trends and behavior. 

```
-- dau past 30 days

SELECT 
    active_date,
    COUNT(DISTINCT owner_user_id) AS dau,
FROM `portfolio-331917.stored_views.active_users`
WHERE 
    active_date BETWEEN DATE '2019-09-01' AND DATE '2019-09-30' 
GROUP BY 1
ORDER BY 1 DESC 
```

As shown in the graph, the majority of users are active during the weekday and drop off during the weekend consistently. DAU peaks out on Tue-Thur and begins to sharply decline on Sat-Sun. My assumption on this trend would be that our demographic are likely to be college students and working professionals. DAU is also slightly trending positively week after week. 
![image]()

Furthermore, cohorts analysis can provide valuable information on our churn rate with with DAU metrics:

```
-- cohort analysis
WITH cohort AS
(
    SELECT 
        active_date,
        owner_user_id,
        CASE 
            WHEN DATE_DIFF(DATE '2019-09-30', DATE(creation_date), DAY) BETWEEN 0 AND 7 THEN "1 WEEK"
            WHEN DATE_DIFF(DATE '2019-09-30', DATE(creation_date), DAY) BETWEEN 8 AND 14 THEN "2 WEEKS"
            WHEN DATE_DIFF(DATE '2019-09-30', DATE(creation_date), DAY) BETWEEN 15 AND 21 THEN "3 WEEKS"
            WHEN DATE_DIFF(DATE '2019-09-30', DATE(creation_date), DAY) BETWEEN 22 AND 28 THEN "4 WEEKS"
            WHEN DATE_DIFF(DATE '2019-09-30', DATE(creation_date), DAY) > 28 THEN "5+ WEEKS"
        END AS week_join
    FROM `portfolio-331917.stored_views.active_users` AS a
        JOIN `bigquery-public-data.stackoverflow.users` AS u 
        ON a.owner_user_id = u.id
    WHERE 
        active_date BETWEEN DATE '2019-09-01' AND  DATE '2019-09-30'
        AND active_date > DATE(creation_date)
    ORDER BY 1
)
 
SELECT active_date, week_join, COUNT(DISTINCT owner_user_id) AS dau FROM cohort GROUP BY 1,2 ORDER BY 1,2 DESC
```

The churn rate for newly signed up users is consistently negative. There is a high drop-off rate for each cohort. The majority of our DAU tend to be more tenure with 5+ weeks who are most likely regular users. This also helps us narrow down our target demographic when selecting a re-engagement group. 
![image]()

Zoom into users who signed up 4 weeks or less. 

![image]()

## Identifying Metrics

For simplicity, let's assume the product team has existing data from the campaign with an email_sent table. Possible schema for the table:

| field             | type     |
| ----------------- | -------- |
| id                | INTEGER  |
| creation_date     | DATETIME |
|post_id            | INTEGER  |
|user_id            | INTEGER  |
|email_view         | INTEGER  |
|click_url          | INTEGER  |
|last_access_date   | DATETIME |

Revisiting the task from above, SLT and Marketing would like to know whether the email re-engagement campaign is successful. Two of the fastest metrics to determine the campaign performance are email view rate and click rate. View rate will give us a better idea of the portion of our recipients who are likely to open the email. This can help even narrow down our target demographic even further. Click rate is the percentage of recipients who opened the email and clicked on the email URL content.
- view_rate: those received email and viewed / total recipients.
- click_rate: out of those open the email, how many click on the content link. 

Since the data for this portion of the project does not exist, I created a staging dataset to help demonstrate. Data consisted of 30 records in a Google Sheet and uploaded to BigQuery.  DISCLAIMER: The data [here]() are not actual existing data. They are made up and purely for illustrative purposes. 
 
Below is a sample query for retrieving the metrics. 

```
SELECT 
    ROUND((SELECT COUNT(id) FROM `portfolio-331917.reengagement_campaign.email_sent` WHERE email_view = 1)/
        COUNT(id) * 100, 2) AS view_rate,
    ROUND((SELECT COUNT(id) FROM `portfolio-331917.reengagement_campaign.email_sent` WHERE click_url = 1)/
        (SELECT COUNT(id) FROM `portfolio-331917.reengagement_campaign.email_sent` WHERE email_view = 1) * 100, 2) AS click_rate
FROM `portfolio-331917.reengagement_campaign.email_sent`
```

Sample Output from our dummy data:

| view_rate | click_rate |
| --------- | ---------- |
| 60%       | 55.56%     |

## Finding:
If the initial assumption is correct, our target demographics for the email campaign are most likely college students and working professionals. This is a hypothesis based on the data on the DAU trend. To verify this, maybe we can dig into text analysis of users' data table. DAU during the summer months will be another indicator if the hypothesis is correct. It consistently peaked out from Wednesday to Friday and dipped on Sat to Sunday. The time frame of when to send out email is during these 3 peak days to increase email view chance. 
 
The churn rate among new users is high where the majority of users become inactive after 1 week of signing up. For newly signup users, after 1 week of becoming inactive, they should be added to the target group of email recipients. 
 
With the sample model above, we can define the target thresholds to determine if the campaign is successful on a weekly-by-week basis or overall. 

## Appendix - A:
__Data Source__ 
- BigQuery public dataset for Stackoverflow.

__Tools and Technology__:
- Standard SQL in the BigQuery environment.
- Python in Jupyter Notebook.
