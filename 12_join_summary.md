# Summary Notes:

- `INNER JOIN` is the most common joining type, and is the default assumed, if you only include the command word `JOIN` in your query.
- `ON` is the phrase to specify the matching unique identifier in both tables.
- If the matching key columns in both tables have exactly matching names `USING(column_name)` may be used instead of the `ON` command structure.
- When combining tables, the column or field names must include a way for SQL to correctly identify their source table. This is accomplished by providing the full table name or assigning an **alias**, as was demonstrated in the Query Question #2.
- Appropriate use of `WHERE` clause filters can enable queries to narrow the focus of the inquiry and search rapidly through very large datasets quickly containing millions of records.
- As shown in the final query, there are many tools for manipulating text fields and including specific parts in your reports as you answer stakeholder’s questions. Some of these essential tools are `SUBSTRING`, `POSITION`, `COALESCE` and `CASE` statements, which are all coming in the next sections.
