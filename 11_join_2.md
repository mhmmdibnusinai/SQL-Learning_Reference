# IV.2. SQL `JOIN`: Diving Deeper

Here are other ways to take your query questions to the next level:

**Background**      
The management team has asked to look at revenue from convenience stores within the counties spending the largest amount per person. They want to consider an approach which might result in product sales with higher margins based on convenient access, and not compete head to head with the low margin, high volume big box retailers that presently sell a tremendous volume of product into the territory. Joining the 2016 sales table with the reference file showing which stores are convenience locations will provide the insight requested. Your query will look like this:


       SELECT sales_2016.county, sales_2016.store_number, sales_2016.store_name,
       COUNT(sales_2016.sale_dollars) AS qty_sold,
       ROUND(AVG(sales_2016.sale_dollars),2) AS avg_sale_price,
       SUM(sales_2016.sale_dollars) AS total_sold
       FROM sales_2016 INNER JOIN stores_convenience
       ON sales_2016.store_number = stores_convenience.store
       WHERE stores_convenience.store IS NOT NULL
       AND sales_2016.county IN('Johnson','Dickinson', 'Polk')
       GROUP BY sales_2016.county, sales_2016.store_number, sales_2016.store_name
       ORDER BY sales_2016.county, total_sold DESC;
  
_This analysis reports the convenience stores that are in the three counties with the greatest amount of spending per person. We’ve grouped the convenience stores by their county and placed them in descending order so that their sales totals are ranked greatest to least._


Finally, you’ve discovered that new stores, soon to open, are already listed in the Stores table, while they do not have any transactions listed in the Sales_2016 table. You want to provide this list of new stores to the investment team, ordered by zip code. You’ll need to extract the zip code from the concatenated store address column from the Stores tables. Test for the stores that are listed in the Stores table as ‘I’ (inactive), but have no sales credited to their store number in the sales transaction table. This method will reveal the stores which are “coming soon”. Some text manipulation SQL functions are required to extract the information that has been requested. Your query may look like:


       SELECT CASE
       WHEN SUBSTRING(store_address,(POSITION('IA' in stores.store_address)+3),1) <'1'
       THEN 'no zip'
       ELSE SUBSTRING(store_address,(POSITION('IA' in stores.store_address)+3),5)
       END AS zipcode,
              stores.name, stores.store AS store_id, store_status,
              SUBSTRING(store_address,1,(POSITION('IA' in stores.store_address)+1)) AS st_address,
              COALESCE(sales.total, 'no sales data') AS sales_totals
       FROM stores LEFT JOIN sales USING (store)
       WHERE store_status = 'A' AND sales.total IS NULL
       ORDER BY zip_code;
  
_From this query, you observe that there are presently 104 stores being prepared for new openings. This query report provides valuable information for the investment team’s deliberation as they seek to understand the soon coming competition in the territory._

# [**Continue**](https://data.world/classrooms/guide-to-data-analysis-with-sql-and-datadotworld/workspace/file?filename=12_join_summary.md)