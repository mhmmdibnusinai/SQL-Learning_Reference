# III.2. dwSQL Working with Dates: Diving Deeper

Here is an example of putting the functions together for more complete analysis:

•	The sales executives ask you to build onto the previous query and create a summary table of the tallies for sales campaigns, won or lost, whose resolution took longer than 55 days. Include a column totaling the number of long campaigns. Your resulting table and successful query for this request follow:           

**Number of sales campaigns, won or lost, whose resolution took longer than 55 days**        
``` 
SELECT 
    COUNT(DATE_DIFF(created_on, close_date,"day")) FILTER (WHERE deal_stage='Won')
AS accts_long_close,
    COUNT(DATE_DIFF(created_on, close_date,"day")) FILTER (WHERE deal_stage='Lost')
 AS leads_long_loss,
    COUNT(created_on) as total_long_campaigns
FROM sales_pipeline 
WHERE DATE_ADD(created_on, 55, "day") < close_date AND close_date IS NOT NULL;
```

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=09-DATETIME-SUMMARY.md)