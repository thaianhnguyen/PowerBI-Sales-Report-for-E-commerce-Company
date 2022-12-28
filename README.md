# Sales Report for E-commerce Company
The report has been published on Power BI service at here: [**_SALE REPORT_**](https://app.powerbi.com/view?r=eyJrIjoiNDhmZWUxNzAtZTljNi00MDVhLWFmZTYtMTc4MTgzMDNhNTY3IiwidCI6ImFmMWYzNzUzLTM5MjUtNGU2Zi05NDliLTk3YzAwNzMyMDgwMyIsImMiOjEwfQ%3D%3D&fbclid=IwAR19SyBorqdDhtuXZaKvqBwRLDbzsqN-1SsMNP7veJgnAn43vM0rNTJC4YQ)
---

Table of contents
=================

<!--ts-->
* [Business Scenario and Dataset](#Business-scenario-dataset)
* [Data Cleaning](#Data-cleaning)
* [Data Modeling](#Data-modeling)
* [Visualization](#Visualization)
   * [Overview Page](#overview-page)
   * [Sales by Product/Region](#Product-region)
   * [Customer Analysis](#Customer-analysis)
* [Insights](#Insights)
<!--te-->
<a name="Business-scenario-dataset"/> </br>
## Business scenario & Dataset
---

Supposing I am tasked with making FY2018 annual sales report for the Sales manager in my e-commerce company. He/she wants to know about the sales performance for the last year and also some insights about customers based on their buying behaviors and product reviews.

Dataset was collected from Kaggle: [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

About the company:
Olist connects small business owners from all over Brazil to channels, who sell their products through the Olist Store and ship them directly to the customers using Olist logistics partners. Once the customer receives the product, or the estimated delivery date is due, the customer gets a satisfaction survey by email where he can give a note for the purchase experience and write down some comments.

<a name="Data-cleaning"/> </br>
## Data cleaning  

I imported directly from SQL servers only tables that are relevant to the business requirements. I then checked the data type, data profiling to make changes to ensure accordance to the metadata description.

Below are some notable changes:</br>
- Merge Customer_unique_ID into Order_items (which will be our fact tables) to remove unnecessary Customer_ID column, simplifying the data schema afterwards.</br>
- Remove duplicates in Geography Table, leaving only one latitude and longtitude for each State. Also create a new State_ID column to merge into Customer columns to remove unnecessary Zip Code column.</br>
- There are quite a few null values in Delivered_date, but only 7 records for "Delivered" orders - which we will focus on this report. Due to such a small amount, I dropped them.

<a name="Data-modeling"/> </br>
## Data modeling</br>
I created a Date table with the fiscal year starting from October.</br>
For this dataset, I chose to build a Snowflake data schema with the Order_items table as the fact table since it contains the most record, with its primary key being the composition of three columns. 
Below are the data model:</br>
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_1.jpg)

<a name="Visualization"/> </br>
## Visualization:

The report has 4 pages.

<a name="Overview-page"/> </br>
### Overview page:
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_2.jpg)</br>
The page provides the user with a big picture of sales performance via some important KPIs over the last year. Generally speaking, it seemed the company saw great growth compared to the previous year (except for the last quarter, September specifically). 
However, it lacked insightful information in terms of regional and product-wise sales. That is why I created the second page:

<a name="Product-region"/> </br>
### Sales by Product/Sale
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_3.jpg)</br>
For this page, I hope that the users can gain insights about sales for a specific region or for a specific product category, or both. That is why I created two 2 simultaneous slicers and also edit the interaction between certain visuals to be "Filter". For example, the user want to now the sales and top 10 product categories sold in Bahia:</br>
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_4.jpg)</br>
Or how much Art-labeled products was selling in Rio de Janeiro:</br>
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_5.jpg)</br>
Lastly, I address the sales manager's requirement regarding customer insight in the third page:

<a name="Customer-analysis"/> </br>
### Customer Analysis:
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_8.jpg)</br>
For this page, I want to show how many customers are returning for a second purchase, how positive their reviews are and how on-time our delivery is. I also included the segmentation of customers based on RFM analysis that I conducted with SQL [**here**](https://github.com/thaianhnguyen/RFM-analysis-with-SQL/blob/main/rfm%20read%20me.md).

_The highlight of this page, firstly, is the cohort analysis of returning customer_. This gives us insights of how many new customers are returning for the subsequent months. For example, we can see that for January 2018, there are 6838 new customers, only 23 of who return in the February and 25 of who return in March and so on.</br>
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_9.jpg)</br>

_The second highlight is that the manager can decide the period, based on which retention rate are calculated_. For example, retention rate with a period of 6 months would mean a returning customer is one that has made repeated purchase for the last 6 months. 

The last page is a decompostion tree visual, which should deepen insights about what total sales are made of, across dimensional variables, which are State, Product Category and Date here. </br>
![alt text](https://github.com/thaianhnguyen/Sales-Report-for-E-commerce-Company/blob/main/images%20BI/Screenshot_7.jpg)</br>

<a name="Insights"/> </br>
## Insights
- Sales this year has seen an great increase in all aspects, compared to last year. This is a good sign.
- Top selling product categories are beauty products and watches gifts while the top 3 states are Sao Paulo, Rio de Janeiro and Minas Gerais, the first of which triples the other two.
- However, the retention rate is alarmingly low, compared to the expected ratio of 15-30% in e-commerce industry. And it seems it is not the sellers, but __the logistics__, to be blamed:
   - The reason might not be the product quality as the CSAT ratio is greater than 75% (even for churn customer segment). 
   - The on time delivery rate is not good (below 95%).
   - The average delivery days increases and on time delivery rate decreases for the "likely to churn" and "churn" segment. </br> 
Therefore, it might be useful conducting further analysis into the correlation between late delivery and retention rate.
- The "middle" segment is witnessing drastic decreases in sales for the past 6 months. It's worth giving more attention to these customers.

This is the end of my blog. I hope with my report, the sales manager can draw useful insights that are actionable. 



Thank you for reading my blog.
