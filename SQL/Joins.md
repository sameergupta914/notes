# JOINS

- In SQL (Structured Query Language), a join is a way to combine data from two or more database tables based on a related column between them.
- Joins are used when we want to query information that is distributed across multiple tables in a database, and the information we need is not contained in a single table. By joining tables together, we can create a virtual table that contains all of the information we need for our query.

# Cross Join

- In SQL, a cross join (also known as a Cartesian product) is a type of join that returns the Cartesian product of the two tables being joined. In other words, it returns all possible combinations of rows from the two tables.
- Cross joins are not commonly used in practice, but they can be useful in certain scenarios, such as generating test data or exploring all possible combinations of items in a product catalogue. However, it's important to be cautious when using cross joins with large tables, as they can generate a very large result set, which can be resource-intensive and slow to process.
- Syntax:

            SELECT a.column1, b.column2
            FROM tableA a
            CROSS JOIN tableB b;


# Inner Join

- In SQL, an inner join is a type of join operation that combines data from two or more tables based on a  specified condition. The inner join returns only the rows from both tables that satisfy the specified condition, i.e., the matching rows.
- When you perform an inner join on two tables, the result set will only contain rows where there is a match between the joining columns in both tables. If there is no match, then the row will not be included in the result set.

- syntax:

            SELECT a.column1, b.column2
            FROM tableA a
            INNER JOIN tableB b
            ON a.common_column = b.common_column;


# Left Join

- A left join, also known as a left outer join, is a type of SQL join operation that returns all the rows from the left table (also known as the "first" table) and matching rows from the right table (also known as the "second" table). If there are no matching rows in the right table, the result will contain NULL values in the columns that come from the right table.
- In other words, a left join combines the rows from both tables based on a common column, but it also includes all the rows from the left table, even if there are no matches in the right table. This is useful when you want to include all the records from the first table, but only some records from the second table.
- Syntax:

            SELECT a.column1, b.column2
            FROM tableA a
            LEFT JOIN tableB b
            ON a.common_column = b.common_column;


# Right Join

- A right join, also known as a right outer join, is a type of join operation in SQL that returns all the rows from the right table and matching rows from the left table. If there are no matches in the left table, the result will still contain all the rows from the right table, with NULL values for the columns from the left table.
- Syntax:

            SELECT a.column1, b.column2
            FROM tableA a
            RIGHT JOIN tableB b
            ON a.common_column = b.common_column;



# Full Outer Join

- A full outer join, sometimes called a full join, is a type of join operation in SQL that returns all matching rows from both the left and right tables, as well as any non-matching rows from either table. In other words, a full outer join returns all the rows from both tables and matches rows with common values in the specified columns, and fills in NULL values for columns where there is no match.
- Syntax:

            SELECT a.column1, b.column2
            FROM tableA a
            LEFT JOIN tableB b ON a.common_column = b.common_column
            UNION
            SELECT a.column1, b.column2
            FROM tableA a
            RIGHT JOIN tableB b ON a.common_column = b.common_column;

# Sql Set Operations

- UNION: The UNION operator is used to combine the results of two or more SELECT statements into a single result set. The UNION operator removes duplicate rows between the various SELECT statements.

- UNION ALL: The UNION ALL operator is similar to the UNION operator, but it does not remove duplicate rows from the result set.

- INTERSECT: The INTERSECT operator returns only the rows that appear in both result sets of two SELECT statements.

- EXCEPT: The EXCEPT or MINUS operator returns only the distinct rows that appear in the first result set but not in the second result set of two SELECT statements.

# Self Join

- A self join is a type of join in which a table is joined with itself. This means that the table is treated as two separate tables, with each row in the table being compared to every other row in the same table.
- Self joins are used when you want to compare the values of two different rows within the same table. For example, you might use a self join to compare the salaries of two employees who work in the same department, or to find all pairs of customers who have the same billing address.
-Syntax:

            SELECT a.column1, b.column2
            FROM employees a
            JOIN employees b
            ON a.manager_id = b.employee_id;

- Natural join: syntax:
            SELECT *
            FROM tableA
            NATURAL JOIN tableB;

- Inner vs Outer joins:

    Inner JOIN → keep only matching rows.

            SELECT c.customer_id, c.name, o.order_id, o.amount
            FROM Customers c
            JOIN Orders o
            ON o.customer_id = c.customer_id;

- Left JOIN → keep all left rows; fill NULL for missing right.

  -customers with their last order amount (if any)

            SELECT c.customer_id, c.name, o.amount
            FROM Customers c
            LEFT JOIN Orders o
                ON o.customer_id = c.customer_id
            AND o.order_date = (SELECT MAX(order_date)
                                FROM Orders
                                WHERE customer_id = c.customer_id);

- Right JOIN is symmetric
    Full OUTER JOIN keeps non-matching from both sides.

    -Count orders per customer:

            SELECT c.customer_id,
                COUNT(o.order_id) AS order_cnt
            FROM Customers c
            LEFT JOIN Orders o
            ON o.customer_id = c.customer_id
            GROUP BY c.customer_id;

    -Be careful: COUNT(*) counts rows even if order_id is NULL
    -Use COUNT(o.order_id) to count only matched orders.

- CROSS join (Cartesian)
  
        SELECT c.name, d.date
        FROM Customers c
        CROSS JOIN (SELECT DATE '2025-08-01' AS date
                    UNION ALL SELECT DATE '2025-08-02') d;

    - Use for generating combinations/calendars—but know it multiplies row counts.

- SELF join
    
    -Employees(manager_id) example: get employee with their manager's name

            SELECT e.name  AS employee,
                m.name  AS manager
            FROM Employees e
            LEFT JOIN Employees m
            ON m.id = e.manager_id;

- SEMI / ANTI joins (EXISTS / NOT EXISTS)
    - Semi-join: keep customers who have at least one order.

            SELECT c.*
            FROM Customers c
            WHERE EXISTS (
            SELECT 1 FROM Orders o
            WHERE o.customer_id = c.customer_id
            );

- Anti-join: customers with no orders (safe with NULLs):

            SELECT c.*
            FROM Customers c
            WHERE NOT EXISTS (
            SELECT 1 FROM Orders o
            WHERE o.customer_id = c.customer_id
            );

    Interview trap: WHERE customer_id NOT IN (SELECT customer_id FROM Orders) fails if the subquery yields NULL. Prefer NOT EXISTS.

- ON vs WHERE with OUTER joins
    Filters that go in ON vs WHERE change results:

    -- Keep all customers; only sum completed orders (status='completed')
    SELECT c.customer_id, SUM(o.amount) AS amt
    FROM Customers c
    LEFT JOIN Orders o
      ON o.customer_id = c.customer_id
    AND o.status = 'completed'        -- filter in ON retains customers with zero completed orders
    GROUP BY c.customer_id;

    -- If you put AND o.status='completed' in WHERE, it turns LEFT into INNER (drops customers with 0 completed)
