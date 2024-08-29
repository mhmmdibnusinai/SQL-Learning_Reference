# II.1. Merging Data          
**SQL: JOINs (INNER, LEFT, RIGHT) - Combining Information**           
**CRM (Customer Relationship Management)**

-----------------

**Introduction**            
Being able to combine information from separate source tables is a powerful tool for successful analysis in SQL. When combining columns from different tables, the first step is to identify which column can serve as a unique identifier between the tables, often labeled as a **“key”**. Next, select the information needed to be included in your query output and consider which columns may be excluded. From these decisions, the type of `JOIN` needed to accomplish your desired output will be clear: `INNER`, `LEFT`, `RIGHT` or one of the others available in SQL. 

![](https://i.imgur.com/vAaqzqs.png)

You’ve already previewed the `INNER JOIN` in an earlier lesson. An `INNER JOIN` brings to the query output the rows from each table that match exactly based on the shared unique identifier. The key or identifier is typically a column containing an account number, product code or transaction ID. Rows without a corresponding match are excluded from the output. Sometimes, an `INNER JOIN` is visually represented by the overlapping center sections of a Venn diagram.

A `LEFT` or `RIGHT JOIN` determines that the designated table (`LEFT` or `RIGHT`) is fully included in the resulting output, even if there are no matching components in the table being joined in. Additional information is added to the output from the other table based on matching components. Columns without matching data are defined as **NULL**, which literally mean no data.

In queries referencing more than one data source, columns need to be referenced with their source designation. This can be done by adding the table name as a prefix, or by aliasing, which will be described later in this lesson.  

The required `JOIN` framework for combining tables begins with this: 

```
SELECT <output column list>
FROM primary_table1 
LEFT JOIN secondary_table2 
ON table1.connecting_column = table2.connecting_column
```
-----------------

**Scenario**         
Regional sales management has several analysis questions regarding accounts and activity in the sales pipeline. They have requested your assistance to create queries to extract the insights from their customer relationship management system. 

1.	Before creating any sales analysis for senior management, you will need to run a series of queries to  verify the integrity of the reported data. Verify that all completed sales have been documented correctly, including a final negotiated price for the sale. Create a query with a compounded `WHERE` clause, filtering for **“Won”** deals that also have **NULL** or zero-dollar  values in the close_value column sorted by the closing date. Control the order of operation for the logical operators with parenthesis. Your successful query will look like this:

**Testing for missing close sales values**      
```
SELECT sales_pipeline.sales_agent, 
    sales_pipeline.opportunity_id,
    sales_pipeline.deal_stage,
    sales_pipeline.close_date,
    sales_pipeline.close_value
FROM sales_pipeline
WHERE sales_pipeline.deal_stage ='Won' 
  AND (sales_pipeline.close_value IS NULL 
  OR sales_pipeline.close_value <1)
ORDER BY sales_pipeline.close_date;
```

Your query has revealed one sales agent who is missing credit for 19 sales. Correcting this data issue will prevent under reported sales for the agent and company, as well as, be a factor for the accounting department which receives information from the CRM system.

2.	Next, the management team would like a current profile for each active account reflecting their internal revenues, number of employees, how many opportunities are in progress with that account, and how many sales closed with that account last year with the associated close value. Your successful query will `JOIN` the sales_pipeline and accounts tables. It is recommended that you use the `FILTER` function in the `SELECT` statement to create the requested counts. The query will look like the following:

**Active accounts profiles including revenue, # employees, deal counts and last year's sales**      
```
SELECT accounts.account, 
    accounts.revenue AS company_revenue,
    accounts.employees AS number_employed,
    count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='In_Progress') as active_deals,
    count(sales_pipeline.opportunity_id) 
FILTER (WHERE sales_pipeline.deal_stage='Won') as closed_deals,
    sum(sales_pipeline.close_value) AS last_year_sales
FROM sales_pipeline RIGHT JOIN accounts
GROUP BY accounts.account
ORDER BY last_year_sales DESC;
```

Your analysis creates 97 rows of active account profiles, allowing the sales management to have a clear snapshot on these valuable accounts and open opportunities.

3.	 Now the team wants to focus in on the opportunities designated as “in progress” and calculate the potential sales that those opportunities represent, organized by sales agent. In creating this analysis, you will need to join the products table into the sales_pipeline table to bring in the MRSP sales_price for each product to estimate the potential revenue.

**Joining sales_pipeline with products to calculate # deals in progress and potential revenue**        
```
SELECT  
    sales_pipeline.sales_agent,
    count(sales_pipeline.opportunity_id) AS in_progress_deals,
    sum(products.sales_price) AS potential_revenue
FROM sales_pipeline LEFT JOIN products USING (product)
WHERE sales_pipeline.deal_stage = 'In_Progress'
GROUP BY sales_pipeline.sales_agent
ORDER BY potential_revenue DESC;
```
Your executive team gratefully uses this information for adjusting overall sales guidance and measuring agents’ performance against their individual goals.

