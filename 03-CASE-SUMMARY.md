# I.3. Summary Notes:              
•	`CASE` statement is a flexible tool that allows you to conditionally create and assign categories or compute values based on information stored in the current table.         
•	Using the familiar Boolean logic of `IF`/`THEN`/`ELSE`, SQL will evaluate the conditions set in a `CASE` statement until it finds a true condition, at which point it exits.         
•	Each `CASE` statement must contain at least one pairing of `WHEN` and `THEN` filtering clauses.             
•	An `ELSE` clause may be used last in the filtering series for assigning values that fall outside the boundaries of the `WHEN` filter(s).            
•	The `CASE` statement concludes with an `END` command, typically followed by an assigned alias for the newly created column of information.                  
•	`CASE` statements are flexible tools for assigning categorical data or calculating a range of values.                 
•	Adding a **second logical loop** to your query, in the form of an **outer loop query** to receive data from the inner loop query, is simple method to create queries that answer compound questions. This is called a _Subquery_, which we will discuss in more detail in upcoming lessons.

# [continue](https://data.world/classrooms/guide-to-data-analysis-with-sql-part-2/workspace/file?filename=04-JOIN-INTRO.md)