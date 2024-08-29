![Imgur](https://i.imgur.com/QVOl2oj.png)

# I. Selecting Data

**SQL: Selecting Data**      

**Exploring Big Bend National Park**

---------------

When accessing data using SQL, Structured Query Language, the two foundational parts of the command sequence are `SELECT` and `FROM`. Using `SELECT`, you choose the information you want to include in your report. `FROM` identifies the source table or file name from which to retrieve or calculate that information. This structure will look like:


      SELECT <desired data here>
      FROM <table name here>

Let’s say you are interested in seeing how many people visited Big Bend National Park over the years. You’ve discovered that a data.world collaborator has already shared a public domain table containing visitor information spanning the years from 1904 to 2015. We have linked to that dataset: “BIGBEND_Annual_Park_Recreation_Visitation_1904_2015.csv”.

To begin a new SQL query, click “New query” from the queries pane on the left side of the workspace. Choose “SQL query” from the drop-down menu that is presented. A new window opens, providing you the opportunity to enter your commands. The available data elements are listed in the Dataset schema pane on the right edge, with symbols describing their data type. Next to the “1.” in the newly opened window, begin your query by identifying the desired data.

Enter `SELECT` and the data desired. At this point, you have three general options:


1. Use an asterisk (*) to cause all the columns in the table to be listed in the results.  


         SELECT *
         FROM bigbend_annual_park_recreation_visitation_1904_2015;     


2. Choose specific columns from the table, for example, selecting the year and the visitor_count for that year.  Begin typing the name of the column (or data element) and observe the dropdown box offering you options with the full name correctly in place. Observe that the full naming convention is table_name.data_name. You may list them on one line, or use multiple lines for clarity. Separate multiple names with commas.


       SELECT bigbend_annual_park_recreation_visitation_1904_2015.year,
              bigbend_annual_park_recreation_visitation_1904_2015.visitor_count
       FROM bigbend_annual_park_recreation_visitation_1904_2015;


3. You can also use the SELECT command to create an aggregate, like an average or sum. As an example, let’s sum all the counted visitors and see if that number truly equals the value stored in the total_visitors column. In this case, you will type SELECT SUM(visitor_count), total_visitors.


       SELECT SUM(bigbend_annual_park_recreation_visitation_1904_2015.visitor_count), 
              bigbend_annual_park_recreation_visitation_1904_2015.total_visitors
       FROM bigbend_annual_park_recreation_visitation_1904_2015;

Your query confirms that the number recorded as total_visitors is indeed the summation of all the visitors spanning the years recorded in the table.


![](https://d2mxuefqeaa7sj.cloudfront.net/s_9D70CE64993CA107336D82D79DBE5A168C30D894EE07BF8C411D1E86BD767105_1515386046170_file.png)



