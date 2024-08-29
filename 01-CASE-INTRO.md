# I.1. Conditional Operations Using `CASE`        

**dwSQL: `CASE` - Creating New Information**        
**CRM (Customer Relationship Management)**        
  
--------------

**Introduction**      
When analyzing information, we may sometimes need to create additional information for the resulting output. The `CASE` statement is a flexible tool that allows you to conditionally assign categories or compute values, depending on the data contained in a table. Consider the `CASE` statement as adding a new and separate column of information, which may be categorical,  calculations (e.g. percent of totals or value ranges), or separated from other output columns with a comma. It commonly follows the `SELECT` command and contains at least one pairing of `WHEN` and `THEN` qualifiers. The `CASE` statement concludes with an `END` command. You may include an `ELSE` clause to assign values that don’t match your `WHEN` filter(s). Note that `CASE` statements may be used multiple times in an output list. The basic structure for creating additional information using a `CASE` statement is:

```
CASE 
WHEN <condition clause> THEN <value set>
ELSE <set value otherwise>
END AS new_column_name;
```

**Scenario**         
You’ve been asked to participate in a sales and product management teams’ analysis efforts. They have several questions for you regarding understanding the mix of products being sold. 

1.	Let’s begin by calculating the percent of products sold within each of the three main categories in the current product offering: **GTK**s, **GTX**s and **MRG**s. We will create a Boolean flag within a `CASE` statement to calculate the portion of total items sold for each category. You will need to multiply the averages by 100 to see the resulting percentages. Your successful query will look like the following:

```
SELECT
    	(AVG(CASE			
		 WHEN sales_pipeline.product LIKE 'GTX%' THEN 1
		 ELSE 0			
	   END)*100) AS GTX_percent,
	
	(AVG(CASE			
		 WHEN sales_pipeline.product LIKE 'MGR%' THEN 1
		 ELSE 0			
	   END)*100) AS MGR_percent,

	(AVG(CASE			
		 WHEN sales_pipeline.product LIKE 'GTK%' THEN 1
		 ELSE 0			
	   END)*100) AS GTK_percent
FROM sales_pipeline
WHERE sales_pipeline.deal_stage='Won';
```
 
As you review the results with the management team, it is clear that the **GTK** product line only represents a tiny portion of completed sales, even though it is a large revenue contributor when it is sold.  The team is interested in other perspectives the data might have to offer.
 
2.	The management team now wants a report that reflects the internal sales’ departments classification for accounts: **Enterprise**, **Commercial** and **SMB**. Use the combined criteria below to create these categories based on the number of employees and the company’s annual revenue. Write your query using a `CASE` statement to classify each account category by name and total value of sales closed to date. You will need to `JOIN` the **sales_pipeline table** and aggregate the total sales for each account. Your query will contain the following:

```
SELECT accounts.account,
    CASE WHEN accounts.employees>5000 
           AND accounts.revenue>3000 
           THEN 'Enterprise'
         WHEN accounts.employees>500 
           AND accounts.revenue>1000 
           THEN 'Commercial'
         ELSE 'SMB'
         END AS Account_Classification,
         SUM(sales_pipeline.close_value) as Total_Sales
FROM accounts JOIN sales_pipeline USING (account)
GROUP BY accounts.account
ORDER BY Total_Sales DESC, Account_Classification;
```

Upon reviewing the query result, an interesting insight is that the third-highest rank in sales value is a Small Medium Business “SMB” account, despite its relatively smaller size.
 
3.	The team is preparing for the annual sales conference and the recognition of significant sales performance. At the beginning of the year, the executive team announced profitability goals and set an incentive award level for sales reps who achieve a ratio of at least $2000 in revenue per closed sale. This is determined by the **close value** of each sale on a particular **close date**.   A base level of $1000 revenue per sale was set as a minimum acceptable level, with ratios below this being flagged for review for improvement. Create a query to identify the agents who have qualified for this incentive award, along with supporting aggregate numbers for their annual sales performance. Your query will look like this:

```
SELECT  sales_pipeline.sales_agent AS Agent,
    COUNT(sales_pipeline.close_date) AS Qty_Sold,
    SUM(sales_pipeline.close_value) AS Sales_amt,
    (SUM(sales_pipeline.close_value)/COUNT(sales_pipeline.close_date))
      AS Rev_Per_Sale,       
    CASE            
    WHEN (SUM(sales_pipeline.close_value)/COUNT(sales_pipeline.close_date))>2000 
    THEN 'Incentive Qualifier'          
    WHEN (SUM(sales_pipeline.close_value)/COUNT(sales_pipeline.close_date))>1000 
    THEN 'Performance Baseline'
    ELSE 'Profitability Review'
    END AS Achievement_Grouping
FROM sales_pipeline 
GROUP BY Agent 
ORDER BY Rev_Per_Sale DESC;
```

Based on this analysis, there are 13 sales agents that have exceeded the incentive targets for the last year and therefore qualify for the bonus incentive award. Fortunately, all but two members of the sales organization exceeded the baseline “on target” profitability goals.
