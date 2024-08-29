#  II.2. SQL GROUP BY: Diving Deeper

Here are two ways to take your query to the next level using **GROUP BY**. This valuable SQL aggregator allows numeric values to be calculated and organized within categorical dimensions. **GROUP** **BY** will always follow the **FROM** statement. When grouping data, you will need to focus the requested output information to be aggregated to the same level that you are “grouping”.


- The startup has received some investment capital, so they are able to launch their product into a Region, rather than a few States. Write a query that shows the Region with the greatest number of households with pets. Add columns with summed dog and cat populations, and assign column names to your calculated fields using the **AS field_name** option shown below:

       SELECT states.region, SUM(states.number_of_pet_households_in_1000) AS Total_Pet_Households,
       SUM(states.dog_population_in_1000) AS Dog_Total,
       SUM(states.cat_population) AS Cat_Total,
       (SUM(states.dog_population_in_1000) + SUM(states.cat_population)) AS Total_Companion_Pets
       FROM states
       GROUP BY states.region
       ORDER BY SUM(states.number_of_pet_households_in_1000) DESC;


![](https://d2mxuefqeaa7sj.cloudfront.net/s_B1A66F7274B7AF6872B989BAACEC7E575795488BB4EB5975F6281C527EDF1962_1515387298167_file.png)


Based on this analysis, the **Central Region** offers the greatest market opportunity based on volume of pet households and companion pet population.


- Finally, let’s build one more query that considers percent of household with pet grouped by region. As you build this query, let’s add a condition on the **GROUP BY** aggregation, using the **HAVING** command. Only return regions that have a pet household percent of 55% or greater. Your query looks like:

      SELECT states.region, AVG(states.percentage_of_households_with_pets)
      FROM states
      GROUP BY states.region
      HAVING AVG(states.percentage_of_households_with_pets) >55
      ORDER BY states.region DESC;

These results place the **Western Region** at the top of the list, showing that 59.95% of households have pets in this territory.

You’re now prepared to speak to the marketing team about options that the data suggests. From here, the team will need additional data sources for analysis of distribution costs for the top regions, cost of acquisition data, and per capita average pet expenditures to broaden the view.


