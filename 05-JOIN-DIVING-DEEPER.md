# II.2. dwSQL `JOIN`s: Diving Deeper

**Consider the following to take your query skills to the next level:**

•	The VP of Sales has requested analysis by sales agent for the quantity of deals recorded in each of the three internally defined deal stages: successfully **“won”** sale, a deal **“in progress”** or a **“lost”** opportunity. Add `FILTER` criteria to your `SELECT` statement enables you to collect information on a conditional basis, expanding the possibilities of data you can create and collect.  Your successful query will look like the following:        

**Sales Agent list with their Qty of Deals in each stage, win/loss ratio (`JOIN` and `FILTER`)**       

```
SELECT  
    sales_teams.sales_agent,
    count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='Won') AS closed_sales,
    count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='In_Progress') AS in_progress,
    count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='Lost') AS lost_leads,
(count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage ='Won') / count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='Lost')) AS won_lost_ratio,
    sum(sales_pipeline.close_value) AS total_sales_rev
FROM sales_teams LEFT JOIN sales_pipeline USING (sales_agent)
GROUP BY sales_teams.sales_agent
ORDER BY closed_sales DESC;
```
With this, senior management is pleased to observe that this team of sales agents have successfully converted more opportunities to completed sales than losses, as reflected by the calculated ratio.

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=06-JOIN-SUMMARY.md)