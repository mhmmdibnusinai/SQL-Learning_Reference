# I.2. SQL `SELECT`: Diving Deeper

Here are two ways to take your query to the next level.

- First, let’s add some basic descriptive statistics to understand the normal ranges of the annual park visitors, using minimum, average and maximum functions. Your query will look like:


      SELECT MIN(bigbend_annual_park_recreation_visitation_1904_2015.visitor_count), 
             AVG(bigbend_annual_park_recreation_visitation_1904_2015.visitor_count), 
             MAX(bigbend_annual_park_recreation_visitation_1904_2015.visitor_count) 
      FROM bigbend_annual_park_recreation_visitation_1904_2015;


- Next let’s incorporate a filter SQL command, `WHERE`. Restrict the results to only contain years when at least 300,000 visitors came to the park, and have SQL place the results in descending (DESC) order presenting largest attendance years first. Your query looks like:


      SELECT bigbend_annual_park_recreation_visitation_1904_2015.year,
             bigbend_annual_park_recreation_visitation_1904_2015.visitor_count
      FROM bigbend_annual_park_recreation_visitation_1904_2015
      WHERE bigbend_annual_park_recreation_visitation_1904_2015.visitor_count >= 300000
      ORDER BY bigbend_annual_park_recreation_visitation_1904_2015.year DESC;

These results will contain 21 rows, beginning with the largest attendance value of 398,583 that occurred in 2005. We will look at expanding the use of filtering commands in the next lesson.
