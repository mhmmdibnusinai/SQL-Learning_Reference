![Imgur](https://i.imgur.com/AMOHamt.png)

# II.1. Filtering and Grouping Data

**dwSQL: Filtering and Grouping Data**           
**Population and Ownership of Dogs and Cats in America**

------------

_Population and ownership by household of dogs and cats broken down by state via American Veterinary Medical Association, with added regional designations._

_Derivative analysis from dataset contributed by data.world collaborator @datanerd_

---------------

**How to use AND, OR, NOT, IN, & BETWEEN**

Building on the basics of selecting desired data, add criteria to further refine the information retrieved for your report using conditional operators. These include command words such as `AND`, `OR`, `NOT`, `IN` and `BETWEEN`. They are added to the SQL query within the components of a `WHERE` clause, and are placed after the `SELECT` and `FROM` portions of the query. This structure will look like:

      SELECT <desired data here>
      FROM <table name here>
      WHERE <filtering criteria>

-------------------

**Background**

A colleague is launching a startup that has developed revolutionary nutritional supplement for companion animals. You’ve offered to create analysis to quantify the largest market opportunity based on the demographic of households with dog or cat pets. In your research, you’ve discovered that a data.world collaborator (@datanerd) has already saved a project with details of cat and dog ownership from the American Veterinary Medical Association.

To begin a new SQL query, click “New query” from the queries pane on the left side of the workspace. Choose “SQL query” from the drop-down menu that is presented. A new window opens, providing you the opportunity to enter your commands. The available data elements are listed in the Dataset schema pane on the right edge, with symbols describing their data type. Next to the “1.” in the newly opened window, begin your query.


1. Start to understand the scope of your dataset by looking at the total number of households and total number of pet households by state (location). Add the `ORDER BY` statement in descending order to place the largest number of pet households at the beginning of the list. You can **LIMIT** the output to the top ten to cause your report to be more concise. Your query will contain the following:


       SELECT location, states.number_of_households_in_1000,
              states.number_of_pet_households_in_1000
       FROM states
       ORDER BY number_of_pet_households_in_1000 DESC
       LIMIT 10;


Notice that the top five pet households (by largest count) are also the largest states by volume of households. It will be important to continue your analysis to find insights that provide additional depth into the areas that will have potential for a good return for the investment.


2. Add insight by looking for the states with an average of at least 2 dogs per household **AND** 2 cats per household. Order the report alphabetically by state. Your query will contain the following:


       SELECT location, mean_number_of_dogs_per_household,
              states.mean_number_of_cats
       FROM states
       WHERE states.mean_number_of_dogs_per_household >= 2 AND
             states.mean_number_of_cats >= 2
       ORDER BY states.location;

Based on this analysis, you can say that Arkansas, New Mexico and Oklahoma have the largest average cat and dog percentages. These may be the best territories into which to offer a new product for companion animals, when considering density of target population.


3. If the nutritional product is focused for cats, you will need to reshape your query to find out the states that are the best targets for this demographic. Let’s run this query with another SQL conditional command tool `BETWEEN`. Let’s discover the states that have higher averages of cats, specifically, between 2.2 and 4 per household. Order the results of the query by the actual cat population in descending order (largest ones on top of list). Your query look like this:


       SELECT location, states.mean_number_of_cats, states.cat_population
       FROM states
       WHERE states.mean_number_of_cats BETWEEN 2.2 AND 4
       ORDER BY states.cat_population DESC;

This analysis would direct the marketing effort at Texas, Ohio, Indiana and Tennessee. (Challenge Question: What state dropped off the list by using 2.2 as the lower limit?)    

# [**Continue**](https://data.world/classrooms/guide-to-data-analysis-with-sql-and-datadotworld/workspace/file?filename=05_filter_and_group_2.md)