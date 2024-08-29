# I.2. dwSQL Using `CASE`: Diving Deeper

Here is another way to take your query skills to the next level:

•	The management team needs to know how many sales agents are in each of the performance categories. By simply wrapping another query as an outer loop to your previous query you can quickly answer this question. This method allows you to ask more than one question at a time. SQL begins with inner loop logic and executes this first, and then feeds that information to the outer loop logic. This is referred to a SQL Subquery. Your resulting table and successful query for this request follow:

**Sales Achievement Groupings Calculations with subquery        
```
SELECT Achievement_Grouping, COUNT(*) AS Agent_Count
FROM 
(
    SELECT  sales_pipeline.sales_agent AS Agent,
        CASE
        WHEN (SUM(sales_pipeline.close_value)/COUNT(sales_pipeline.close_date))>2000 
        THEN 'Incentive Qualifier'
        WHEN (SUM(sales_pipeline.close_value)/COUNT(sales_pipeline.close_date))>1000 THEN 'Performance Baseline'
        ELSE 'Profitability Review'
        END AS Achievement_Grouping
    FROM sales_pipeline 
    GROUP BY Agent
) AS TEMP
GROUP BY Achievement_Grouping
ORDER BY Achievement_Grouping;
```

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=03-CASE-SUMMARY.md)