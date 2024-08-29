# II.3. Summary Notes:        
•	`INNER JOIN` brings to the query output the rows from each table that match exactly based on the shared unique identifier, typically referred to as a **KEY**.          
•	A `JOIN` created without a specific designation for joining type will default to an `INNER JOIN`.          
•	Each column must be referenced with a prefix of its source table name.         
•	Missing or unmatched data is defined as **NULL**, which literally mean no data. Queries built to test for **NULL** can be very useful to find data consistency issues or surface other types of business issues, as we will explore later.          
•	Tables may have an alias assigned to them within the `FROM` statement. Use of aliases can reduce potential for typos in many SQL environments. However, data.world’s workspace pneumatically provides table and column names for auto-complete, making this less of an issue.         
•	`FILTER` function allows a `WHERE` criteria to be set with an aggregating function inside the `SELECT` statement creating a new variable with conditional aggregation. More examples of this function will be described in the next lesson.

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=07-DATETIME-INTRO.md)