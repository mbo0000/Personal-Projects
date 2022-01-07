# Stack Overflow
## Background
Stack Overflow is an online platform for asking and finding technical questions or answers. It is a community-based space for developers to seek answers and help others on a variety of topics. Since its inception, Stack Overflow has been a preferred platform by the majority of the developers and technologists communities. In this project, I would like to understand its current performance through users growth vs users engagement. 

## Project Goal
By identifying some of the platform’s KPIs and metrics, we can better assess its past and current performance. More importantly, this will give insight into the retention rate among its existing/new users and possibly influence marketing strategy/campaign if needed be.

## Target KPIs
Platform’s current performance through defined metrics.
- DAU/MAU ratio for user engagement.
- Population growth

Users trends and behaviors
- What are the most popular topics by month? 

## Scope
- Using publicly available data through BigQuery Public.
- Metrics are confined to public users only. We will exclude enterprises and private organizations as they are not the target audience we are going after nor the data is available. 
- We will be excluding product technical performance or business metrics as they are not within the goal of the project nor readily available.
- Data in 2021 and late 2008 are incomplete or missing. Insight on these observations is limited.
- Measuring __overall__ platform's performance with at-a-glance metrics only and not deep-dive analysis.

## Tools
- Standard SQL in the BigQuery environment.
    - All of our data is hosted on the BigQuery platform.
    - SQL to handle the size of the dataset.
- Python in Jupyter Notebook.
    - Leveraging Python’s libraries(Pandas, Numpy, Matplotlib, etc…) as an all-in-one hub for all project processes. These include data cleaning, processing, visualization, and analysis. 
    
## Content
### Users Growth
To calculate the users growth, I would like to understand what counts as growth. My definition is new users sign up(created an account) with the platform. We can make a few assumptions with the user table before proceeding:
1. id field is uniquely generated, i.e unique to each user account.
2. Since we can not determine if 2 ids belong to the same user with available data, we can assume each observation is a unique user. 
3. Each observation must have a creation_date. 

To our result easier to digest, the sum of new users signup will be grouped by the year of creation_date field. 

Since the launch in late 2008, on average, the platform gained ~1,109,925 of new users annually. The current overall population is 15,534,618 users. We can see the overall population growth is trending positively year after year, with a big uptick in 2020. 

![image](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/user_signup.png)

However, the population growth alone does not adequately provide information on the platform's performance. For that, we have to look to the DAU/MAU ratio or product "stickiness". DAU is the Daily Active Users(DAU) and MAU is the Monthly Active Users(MAU) metrics.

### DAU/MAU ratio
Product "stickiness" is defined by how often users engage with a product - [geckoboard](https://www.geckoboard.com/best-practice/kpi-examples/dau-mau-ratio/). This metric determines how much is the community engages with the platform. It is also a useful metric when comparing the platform's popularity to competitors'. To be consider as active, a user must participated on the platform by post a question, make a comment or anwer a question on a post. 

With that in mind, the 3 tables of interest are __posts_questions__, __comments__, __posts_answers__. Again before proceeding to process the data, we will need to establish a few assumptions:
1. id field in each table is unique to a post type or comment(though, this is not a fair assumption since I manually checked since I am paranoid sometimes 😂).
2. Each post or comment must have a creation_date(again, ✔️-d).  

__Formula__: 
- DAU = the sum of all users created a post or comment. 
- MAU = the sum of the DAU grouped by the month and year. 
- Ratio = DAU/MAU for each date. 

The estimated population mean of the DAU/MAU ratio is calculated by the sum of (all observed DAU/MAU)/size of observation. The result is equal to 8.74% with a 95% confidence interval of ~8.64% and ~8.83%. This is a low ratio compared to the industry standard of 20%. 

Let plot all data points for better visualization of the DAU/MAU ratio trend. 

![image](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/dau_mau_ratio.png)

We can easily see the negative trend of DAU/MAU ratio overall. Notice, there are a few outliers of 100% ratio and mid to high 30s. Those data points did not impact the final result or have a large enough difference to be significant. However, it is interesting to see the context behind these outliers. 

__Note__: Aggregated data does not give us the whole picture of users retention or engagement. Maybe some months, the ratio is higher or lower due to colleges starting/closing. Or a certain period of a day, the platform might have a higher ratio compared to the rest of the day because of office hours. In addition, Cohort analysis can give a greater depth into the users' retention rate. With granularity levels of daily, weekly, monthly, or quarterly periods, we can have a clearer picture of the actual retention rate among new users joined. That is out of scope for this at-a-glance project. But they are great topics for an indept Stack Overflow case study.  

### Top Trending Topics/Tags by Question Posts
To know what topic/s users are interested in, understanding topics asked by the majority of the community is a great place to start. This task required splitting the tag field in the __posts_questions__ table and counting the total unique tag from all questions asked.

Build upon the previous rules, we can conclude each observation must have a non-null tag's value; each tag is separated by a single | character between them; each observation has a unique tag in a string of tags. After all observations are split, they will be grouped and counted.

Below are the top 10 topics asked by the community with Javascript, Java, and Python leading the chart respectively. 

![](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/top_10_topics.png)

Javascript is a programming language that shines on both client-side and server-side for web development. With the top 10 included Javascript, html, php and css, the assumption is many of the users on the platform are web developers. Would be an interesting case to test. Unfortunately, that is also out of scope.  

### Problem and recommendations

__Problem:__ Stack Overflow has a positive users growth rate averaging ~7.68% annually. However, its average "stickiness" factor is hovering at ~8.74% overall. This falls short of the industry standard of 20%. 

__Recommendation__: While Stack Overflow has a ranking or leaderboard, reputation, badges, and among others achievement systems, it can benefit from the additional incentive for users to be more active on the platform. In turn, it will improve the DAU/MAU ratio for a better product value as a whole. 
- One method, widely adopted, is to have a weekly or montly automated emails to low active and inactive users with question posts without an answer or without a community accepted answer. Question posts can be selected, randomly or methodically based on the top trending topics of the month. Below is an example of what are the top 3 topics for each month in 2021. 
    - ![image](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/top_3_2021.png) 

## Appendix - A
__Data Source__ 
[BigQuery public dataset for Stack Overflow](https://console.cloud.google.com/marketplace/product/stack-exchange/stack-overflow).
