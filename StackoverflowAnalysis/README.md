# Stack Overflow
## Background
Stack Overflow is an online platform for asking and finding technical questions or answers. It is a community-based space for developers to seek answers and help others on a variety of topics. Since its inception, Stack Overflow has been a preferred platform by the majority of the developers and technologists communities. To promote growth within the platform, we will be using available data and gain additional insight into its current performance. 

## Project Goal
By identifying some of the platform’s KPIs and metrics, we can better assess its past and current performance. More importantly, this will give insight into the retention rate among its existing/new users and possibly influence marketing strategy/campaign.

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
Since the launch in late 2008, on average, the platform gained ~1,109,925 of new users annually. The current overall population is 15,534,618 users. We can see the overall population growth is trending positively year after year, with a big uptick in 2020. 

![image](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/user_signup.png)

However, the population growth alone does not adequately provide information on the platform's performance. For that, we have to look to the DAU/MAU ratio or product "stickiness". DAU is the Daily Active Users(DAU) and MAU is the Monthly Active Users(MAU) metrics.

### DAU/MAU ratio
Product "stickiness" is defined by how often users engage with a product - [geckoboard](https://www.geckoboard.com/best-practice/kpi-examples/dau-mau-ratio/). This metric determines how much is the community engages with the platform. It is also a useful metric when comparing the platform's popularity to competitors'. 

The estimated population mean of the DAU/MAU ratio is calculated by the sum of (all observed DAU/MAU)/size of observation. The result is equal to 8.74% with a 95% confidence interval of ~8.64% and ~8.83%. This is a low ratio compared to the industry standard of 20%. Let plot all data points for better visualization of the DAU/MAU ratio trend. 

![image](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/dau_mau_ratio.png)

We can easily see the negative trend of DAU/MAU ratio overall. Notice, there are a few outliers of 100% ratio and mid to high 30s. Those data points did not impact the final result or have a large enough difference to be significant. However, it is interesting to see the context behind these outliers. 

Cohort analysis can give a greater depth into the users' retention rate. But that is out of scope for this project. 

### Top Trending Topics/Tags by Question Posts
To know what topic/s users are interested in, understanding topic/s asked by the community is a great place to start. This task required splitting the tag field in the __posts_questions__ table and counting the total unique tag from questions asked. 

Below are the top 10 topics asked by the community with Javascript, Java, and Python leading the chart respectively. 

![](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/top_10_topics.png)

Javascript is a programming language that shines on both client-side and server-side for web development. With the top 10 included Javascript, html, php and css, the assumption is many of the users on the platform are web developers. Would be an interesting hypothesis to test on a different project. 

### Problem and recommendations

__Problem:__ Stack Overflow has a positive users growth rate averaging ~7.68% annually. However, its average "stickiness" factor is hovering at ~8.74% overall. This falls short of the industry standard of 20%. 

__Recommendation__: While Stack Overflow has a ranking or leaderboard, reputation, badges, and among others achievement systems, it can benefit from the additional incentive for users to be more active on the platform. In turn, it will improve the DAU/MAU ratio for a better product value as a whole. 
- One of the incentives that come to mind is a monthly/quarter/annual tournament that encourages users to meet a challenge. Challenges can be # of questions answered and accepted by the community. With the top most popular topics data, the challenge can target those specific areas or be even broader. Differentiating from the current systems, challenges can be hosted by other companies and orgs that seek out top talents or other cash incentive rewards. Challenges can change based on different top trending topics. Below are the top 3 most popular topics in 2021. 
    - ![](https://github.com/mbo0000/Portfolio/blob/main/StackoverflowAnalysis/charts/top_3_2021.png) 

## Appendix - A
__Data Source__ 
[BigQuery public dataset for Stack Overflow](https://console.cloud.google.com/marketplace/product/stack-exchange/stack-overflow).
