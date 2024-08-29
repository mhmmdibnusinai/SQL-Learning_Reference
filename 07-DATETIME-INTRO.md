![know-it-all.png](https://view.dwcontent.com/file_view/bgadoci/data-world-marketing-assets/know-it-all.png?auth=eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJwcm9kLXVzZXItY2xpZW50Om5yaXBwbmVyIiwiaXNzIjoiYWdlbnQ6bnJpcHBuZXI6OjQ0NTA1YTI5LTQ3OTQtNGEzMS1iMjk2LTk5YzNjOTc5MzgwMCIsImlhdCI6MTUxNjk4NTg2OSwicm9sZSI6WyJ1c2VyIiwiZW1wbG95ZWUiLCJ1c2VyX2FwaV9yZWFkIiwidXNlcl9hcGlfd3JpdGUiLCJ1c2VyX2FwaV9hZG1pbiJdLCJnZW5lcmFsLXB1cnBvc2UiOmZhbHNlLCJ1cmwiOiI2NTUyMTZmMjg0MGU4NTkzZjU0NjMxZDYyODNkYzAzYWNhZmRhMWZhIn0.Mu-p4Ae6JlLQLd1jg_Pum45W34g33xNZAjCe9-FvIV1qMp28oEwt-4DmLotquXpdDYhDxCvts_Hv10EbjVNSaA)
# III.1. Datetime Data       
**dwSQL: Functions for Dates (`DATE_DIFF`, `DATE_ADD`, `FILTER`)**          
**CRM (Customer Relationship Management)**        

------------------

**Introduction**          
SQL functions provide a wide variety of tools for drawing conclusions and insights from stored information. In the following scenarios, three specific functions will be discussed and demonstrated: `DATE_DIFF`, `DATE_ADD` and `FILTER`.
 
* `DATE_DIFF`: calculates a numeric difference between two dates based on the unit of increment selected. The syntax is:  `DATE_DIFF` (date_column1, date_column2, “date part”)
 
* `DATE_ADD`: returns a new date, calculated by adding a specified amount of time specified in the clause, formatted as: `DATE_ADD`(date_column, added amount, “date part”)

In both cases, the date part can be any of the following: **“year”**, **“decade”**, **“century”**, **“quarter”**, **“month”**, **“week”**, **“day”**, **“hour”**, **“minute”**, **“second”**, or **“millisecond”**.

`FILTER`, as shown in the previous lesson, this function allows for a specialized `WHEN` criteria to be set against a single column within the `SELECT` column list.  

**Scenario**          
The sales management team wants to use last year’s sales activity information to adjust their forecasting and budgeting plans for the upcoming year. They have requested various items from you regarding sales and marketing activities. 

1.	The top grossing product from the last year is the **GTXPro**. The sales management team needs more specifics about the number of days required to close a sale for this product. They want to see the ranges for days to close, including minimum days, maximum days and averages. You will be able to use the SQL function `DATE_DIFF` to calculate their request. Your successful query will look like the following:

**Observing time required to "Win" a sale for top grossing product (GTXPro)**         
``` 
SELECT sales_pipeline.sales_agent,
        COUNT(sales_pipeline.opportunity_id) as qty_closed,
        MIN(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as min_close_days,
        MAX(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as max_close_days,
        AVG(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as avg_close_days,
        AVG(sales_pipeline.close_value) as avg_deal_value 
FROM sales_pipeline
WHERE sales_pipeline.product LIKE 'GTXPro' AND sales_pipeline.deal_stage = 'Won'
GROUP BY sales_pipeline.sales_agent
ORDER BY qty_closed DESC, avg_deal_value DESC;
```

Before you present these results to the management team, switch the sorting order of the output between the highest sales close values and quantity of closed deals. Contrasting the sales agents details from the two different patterns show an outlier of a single sales agent who has vastly outsold the others in terms of the number of sales, but whose average closing sales value is $700-1,000 lower than several other team members. Highlighting the variety in the sales activities will enable the management team to make forward looking decisions and explore trade offs being made between volume and discounting.
 
2.	Sales management would like also to have the information describing the days required to close sales by products. Use the `DATE_DIFF` function to quickly create this analysis showing the range of days required to close a sale by product, as shown here: 

**Time required to **"Win"** the sale by Product**               
```
SELECT   sales_pipeline.product, 
        MIN(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as min_close_length,
        MAX(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as max_close_length,
        AVG(DATE_DIFF(sales_pipeline.created_on, sales_pipeline.close_date, "day")) 
as avg_close_length
FROM sales_pipeline
WHERE sales_pipeline.deal_stage = 'Won'
GROUP BY sales_pipeline.product
ORDER BY sales_pipeline.product;
```

Although there are a few very long max times to close that are reflected in the output of the query, the average amount of days required to close a sale averages about 50 days for all products.
 
3.	The sales management team would like to review the sales campaigns that are taking longer than the normal average amount for each product. The team selects 55 to be the upper limit for normal average time for sales. Create a query that shows the sales that are in process and are taking longer than the average amount of time. You will need to include `DATE_DIFF` and `DATE_ADD` into your successful query. Additionally, exclude sales campaigns that are in progress by testing for **NULL** in the **close_date** column. Your successful query will contain:

**Completed sales campaigns, won or lost, whose resolution took longer than 55 days**     
``` 
SELECT sales_pipeline.account AS customer_acct, 
    sales_pipeline.sales_agent,
    sales_pipeline.product,
    sales_pipeline.deal_stage,
    DATE_DIFF(created_on, sales_pipeline.close_date,"day") AS cycle_days,
    sales_pipeline.created_on,
    sales_pipeline.close_date
FROM sales_pipeline 
WHERE DATE_ADD(sales_pipeline.created_on, 55, "day") < close_date 
    AND close_date IS NOT NULL
ORDER BY deal_stage, cycle_days DESC;
```

For the team to become aware of the reasons why a portion of the completed campaigns took significantly longer than average will prove valuable for improving efficiencies and avoiding lost sales with significant investment of sales time and effort.

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=08-DATETIME-DIVING-DEEPER.md)