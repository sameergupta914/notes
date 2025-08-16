# SQL Interview & OA Master Notes (Cheat Sheet)

- Relational model: tables (relations), rows (tuples), columns (attributes).

- Keys:

  - PK (Primary Key): unique + not null.

  - UK (Unique Key): unique, can be null (vendor rules differ).

  - FK (Foreign Key): references PK/UK; maintains referential integrity.

- Integrity constraints: NOT NULL, CHECK, DEFAULT, FK actions (CASCADE/RESTRICT/SET NULL/SET DEFAULT).

- Normalization:

  1NF: atomic columns (no arrays/lists).

  2NF: 1NF + no partial dependency on a composite key.

  3NF: 2NF + no transitive dependency (non-key → non-key).

  BCNF: every determinant is a candidate key.

  - Normalize for integrity; denormalize for read performance (with care).

- SQL Families
  DDL: CREATE | ALTER | DROP | TRUNCATE

  DML: INSERT | UPDATE | DELETE | MERGE

  DQL: SELECT

  DCL/TCL: GRANT/REVOKE and COMMIT/ROLLBACK/SAVEPOINT/SET TRANSACTION

- Execution Order Mental Model
  FROM → JOIN → WHERE → GROUP BY → HAVING → WINDOW → SELECT → DISTINCT → ORDER BY → LIMIT/OFFSET

- WHERE vs HAVING: WHERE filters rows before aggregation; HAVING filters groups after aggregation.

# Subqueries & CTEs — when, why, and faster patterns

- Scalar, derived, correlated
  Scalar subquery (single value):

  - eg: Compare an order against customer's avg order amount
  
      SELECT o.order_id, o.amount,
            (SELECT AVG(amount) FROM Orders oo
              WHERE oo.customer_id = o.customer_id) AS avg_for_customer
      FROM Orders o;
      Derived table (subquery in FROM):

      SELECT c.customer_id, x.first_date, x.last_date
      FROM Customers c
      JOIN (
        SELECT customer_id,
              MIN(order_date) AS first_date,
              MAX(order_date) AS last_date
        FROM Orders
        GROUP BY customer_id
      ) x ON x.customer_id = c.customer_id;


  Correlated subquery (runs per row):


  -- Latest order per customer (correlated)
  SELECT c.customer_id,
        (SELECT MAX(order_date)
          FROM Orders o
          WHERE o.customer_id = c.customer_id) AS last_order
  FROM Customers c;
  Performance hint: Many correlated subqueries can be rewritten as JOIN + GROUP BY for set-based execution (often faster).

# Recursive CTE

- A CTE (“common table expression”) is a temporary result set named in a WITH clause.
- A recursive CTE is a CTE that refers to itself so it can repeatedly build rows—perfect for hierarchies, paths, and sequences.

- It has two parts:
  - Anchor (base) member – seeds the first rows
  - Recursive member – reads the CTE and adds the “next” rows
- They’re combined with UNION ALL and stop when the recursive SELECT returns no new rows (or when a configured depth limit is hit).

- MySQL rules (key points)

  - Use WITH RECURSIVE name AS ( anchor UNION ALL recursive ).
  - Both SELECTs must return the same columns and compatible types (use CAST if needed).
  - Default depth limit is 1000; change with SET SESSION cte_max_recursion_depth = 5000;.
- Eg:

    - Generate a simple sequence (1…10)
      - WITH RECURSIVE seq(n) AS (
          SELECT 1            -- anchor
          UNION ALL
          SELECT n + 1        -- recursive step
          FROM seq
          WHERE n < 10        -- termination condition
        )
        SELECT n FROM seq;


        Idea: Start at 1, keep adding 1 while n < 10.

    - Build a date series (from A to B)
       - WITH RECURSIVE d(dt) AS (
            SELECT DATE('2025-08-01')       -- start date
            UNION ALL
            SELECT dt + INTERVAL 1 DAY
            FROM d
            WHERE dt < '2025-08-10'         -- stop condition
          )
          SELECT dt FROM d;


    Handy for left-joining against missing dates in reports.

    - Traverse a hierarchy (employees → reports)

         - Assume employees(id, manager_id, name) where the CEO’s manager_id is NULL.

          WITH RECURSIVE org AS (
            -- Anchor: start at the CEO (or any root)
            SELECT 
              id,
              manager_id,
              name,
              0         AS lvl,
              CAST(id AS CHAR(200)) AS path_ids   -- track path to guard against cycles
            FROM employees
            WHERE manager_id IS NULL

            UNION ALL

            -- Recursive: get direct reports of previous level
            SELECT 
              e.id,
              e.manager_id,
              e.name,
              o.lvl + 1 AS lvl,
              CONCAT(o.path_ids, ',', e.id) AS path_ids
            FROM employees e
            JOIN org o ON e.manager_id = o.id
            -- optional cycle guard (useful if data can be dirty):
            WHERE FIND_IN_SET(e.id, o.path_ids) = 0
          )
          SELECT id, manager_id, name, lvl
          FROM org
          ORDER BY lvl, id;   -- breadth-ish order


# Aggregation & GROUP BY — correctness, advanced grouping

- WHERE vs HAVING

  -- Only orders from 2025, then group by customer
  SELECT customer_id, SUM(amount)
  FROM Orders
  WHERE order_date >= '2025-01-01'           -- filters rows BEFORE grouping
  GROUP BY customer_id
  HAVING SUM(amount) > 1000;                 -- filters groups AFTER aggregation

- Conditional aggregation (very interview-friendly)

  SELECT c.city,
        SUM(CASE WHEN o.amount >= 100 THEN 1 ELSE 0 END) AS big_orders,
        SUM(o.amount) AS total_amount
  FROM Customers c
  LEFT JOIN Orders o ON o.customer_id = c.customer_id
  GROUP BY c.city;

- DISTINCT in aggregates

  SELECT customer_id,
        COUNT(DISTINCT order_date) AS active_days
  FROM Orders
  GROUP BY customer_id;

- Groupwise maximum (get the row with max per group)
  Two ways:

  A) JOIN on max:

  -- Last order row per customer
  WITH last_dates AS (
    SELECT customer_id, MAX(order_date) AS last_dt
    FROM Orders
    GROUP BY customer_id
  )
  SELECT o.*
  FROM Orders o
  JOIN last_dates d
    ON d.customer_id = o.customer_id
  AND d.last_dt = o.order_date;

  B) Window function + filter (simpler):


  SELECT *
  FROM (
    SELECT o.*,
          ROW_NUMBER() OVER (PARTITION BY customer_id
                              ORDER BY order_date DESC, order_id DESC) AS rn
    FROM Orders o
  ) x
  WHERE rn = 1;

- GROUPING SETS / ROLLUP / CUBE (multi-level totals)

  -- City, customer totals, and grand total in one pass (Postgres/SQL Server/Oracle)
  SELECT c.city, o.customer_id, SUM(o.amount) AS total_amount,
        GROUPING(c.city)    AS g_city,
        GROUPING(o.customer_id) AS g_cust
  FROM Customers c
  JOIN Orders o ON o.customer_id = c.customer_id
  GROUP BY ROLLUP (c.city, o.customer_id);
Interpret rows with GROUPING() flags to identify subtotal/grand total rows.

- Functional dependency & ONLY_FULL_GROUP_BY
  Some engines (MySQL with ONLY_FULL_GROUP_BY off) allow selecting non-grouped columns if “functionally dependent.” For interviews, stick to portable: every non-aggregated selected column must appear in GROUP BY.


# Window Functions — frames, ordering, and real use-cases

- 7.1 Mental model
PARTITION BY defines groups (keeps all rows).

ORDER BY defines sequence inside group.

Frame defines how many rows are included in the calculation relative to current row.

Default frames differ by DB. To avoid surprises, be explicit.

- 7.2 Running total, moving average

-- Running total by customer, ordered by date
SELECT o.customer_id, o.order_date, o.amount,
       SUM(o.amount) OVER (
         PARTITION BY o.customer_id
         ORDER BY o.order_date
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM Orders o;

-- 7-day moving average (requires dense daily rows per customer or a date spine)
SELECT customer_id, order_date, amount,
       AVG(amount) OVER (
         PARTITION BY customer_id
         ORDER BY order_date
         ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
       ) AS ma_7
FROM Orders;
ROWS vs RANGE:

ROWS counts row positions (deterministic).

RANGE groups by value range; with duplicates it may expand unexpectedly. Prefer ROWS for precise rolling windows.

- 7.3 Ranking and ties

SELECT customer_id, amount,
       ROW_NUMBER()  OVER (PARTITION BY customer_id ORDER BY amount DESC) AS rn,
       RANK()        OVER (PARTITION BY customer_id ORDER BY amount DESC) AS rnk,
       DENSE_RANK()  OVER (PARTITION BY customer_id ORDER BY amount DESC) AS drnk
FROM Orders;
ROW_NUMBER: 1,2,3,4 (no ties).

RANK: 1,1,3 (skips after ties).

DENSE_RANK: 1,1,2 (no gaps).

Top-N per group:


SELECT *
FROM (
  SELECT o.*,
         ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS rn
  FROM Orders o
) x
WHERE rn <= 3;
- 7.4 LAG/LEAD (deltas, churn)

SELECT customer_id, order_date, amount,
       LAG(amount)  OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_amt,
       amount - LAG(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS delta
FROM Orders;
- 7.5 Percentiles and distribution

-- Continuous percentile (Postgres/SQL Server)
SELECT
  PERCENTILE_CONT(0.9) WITHIN GROUP (ORDER BY amount) OVER (PARTITION BY customer_id) AS p90
FROM Orders;
- 7.6 Conditional windows (filter inside window)
Most engines don’t support COUNT(DISTINCT ...) OVER easily. Common tricks:

-- Count of distinct order_date per customer using "change detection" trick
SELECT customer_id, order_date,
       SUM(is_new_day) OVER (PARTITION BY customer_id ORDER BY order_date
                             ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS distinct_days_so_far
FROM (
  SELECT o.*,
         CASE WHEN order_date
                <> LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date)
              THEN 1 ELSE 0 END AS is_new_day
  FROM Orders o
) t;
- 7.7 Frame pitfalls to know
Without explicit frame, some DBs default to RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW (not the same as ROWS).

ORDER BY with duplicates + RANGE ⇒ can include all peers with the same ordering value. Use ROWS to avoid surprises.

# NULLs & Three-Valued Logic
Comparisons with NULL → UNKNOWN (not true/false). Use IS NULL / IS NOT NULL.

COUNT(*) counts rows; COUNT(col) ignores NULLs.

COALESCE(a,b,…) returns first non-null.

Pitfall: NOT IN (subquery) returns empty set if subquery returns any NULL. Prefer NOT EXISTS.

# Set Operations
UNION (distinct), UNION ALL (keeps duplicates), INTERSECT, EXCEPT/MINUS.

Columns count & types must align.

# Indexing (high-leverage topic)
B-tree (default): good for equality and range.

Hash (Postgres hash): equality only.

Bitmap (DW/Oracle): low-cardinality, read-mostly.

GiST/GIN (Postgres): full-text, arrays, JSON, geo.

Clustered (SQL Server/InnoDB PK): physical order matches index key.

Composite index order matters: put most selective/most commonly filtered first (but consider sort/order patterns!).

Covering index: includes all needed columns for a query (data fetched from index only).

SARGable predicates: avoid functions on indexed columns in WHERE; rewrite to keep column “as is”.


-- Bad (non-sargable)
WHERE DATE(order_ts) = '2025-08-01'
-- Good
WHERE order_ts >= '2025-08-01' AND order_ts < '2025-08-02'
Partial/Filtered indexes: index a subset of rows (Postgres/SQL Server).

# Query Performance & Plans
Know join algorithms: Nested Loops, Hash Join, Merge Join.

Read plans: scan type (seq/index), join order, estimated rows, filter predicates, costs.

Stats matter: outdated stats → bad plans. Keep autovacuum/auto-stats on or update stats.

Use EXPLAIN (ANALYZE) (Postgres), EXPLAIN (MySQL), SET STATISTICS IO/TIME (SQL Server).

# Transactions, Isolation & Concurrency
ACID: Atomicity, Consistency, Isolation, Durability.

Isolation levels (lowest → highest):

Read Uncommitted → Read Committed → Repeatable Read → Serializable.

Anomalies: dirty read, non-repeatable read, phantom read.

MVCC (Postgres/MySQL InnoDB): snapshot reads, writers don’t block readers (mostly).

Deadlocks: cyclic waits; keep transactions short, consistent ordering, proper indexes; handle 1205/40P01 by retry.

# Modifications & Upserts
INSERT: single/multi-row; INSERT…SELECT.

UPSERT:

Postgres: INSERT ... ON CONFLICT (key) DO UPDATE ... RETURNING *;

MySQL: INSERT ... ON DUPLICATE KEY UPDATE ...;

SQL Server/Oracle: MERGE (use carefully).

UPDATE with JOIN:


-- Postgres
UPDATE t
SET col = s.new_val
FROM staging s
WHERE s.id = t.id;
DELETE using CTE (and keep one per group):


WITH d AS (
  SELECT id,
         ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at DESC) AS rn
  FROM users
)
DELETE FROM users
USING d
WHERE users.id = d.id AND d.rn > 1;
# Views, Materialized Views, Functions, Triggers
Views: saved SELECT; always reads base tables.

Materialized Views: persisted results; refresh needed; great for heavy aggregates.

Stored procedures/functions: procedural logic; beware portability.

Triggers: BEFORE/AFTER INSERT/UPDATE/DELETE; use judiciously (hidden work, can hamper bulk loads).

# Security & Safety
Roles, privileges, GRANT/REVOKE, least privilege.

SQL injection: always parameterize queries; never string-concatenate user input.

Row-Level Security (Postgres), Always Encrypted (SQL Server), masked columns.

# Dates/Times & Time Zones
Use proper types: DATE, TIMESTAMP [WITH TIME ZONE].

Store UTC; convert at edges.

Range filters for days: [inclusive, exclusive) pattern (see sargability example above).

Extract/trunc functions: DATE_TRUNC('month', ts) (PG), DATE_FORMAT (MySQL), DATENAME/DATEPART (T-SQL).

# Strings, Patterns, Full-Text
Case-insensitive search: Postgres ILIKE, MySQL collations, SQL Server COLLATE.

Wildcards: % (many), _ (single). Leading % prevents index use.

Regex: PG ~, SQL Server LIKE (limited), MySQL REGEXP.

Full-text: to_tsvector/to_tsquery (PG), MATCH…AGAINST (MySQL InnoDB FTS), SQL Server FTS.

# JSON / Semi-Structured
Postgres JSONB: ->, ->>, @>, GIN index on jsonb_path_ops.

MySQL JSON: JSON_EXTRACT, ->>, virtual/generated columns + index.

SQL Server: JSON_VALUE, OPENJSON.

-- Postgres: index a JSONB property
CREATE INDEX idx_user_meta_city ON users USING GIN ((meta->>'city'));
# Partitions & Large Tables
Partitioning: range/list/hash; prunes scans; helps maintenance.

Beware global vs local indexes (Oracle) and constraint routing (PG).

Use bulk operations (COPY/bcp/BULK INSERT) for large loads.

# Common Interview Patterns (with templates)

- A) Nth highest salary

-- Dense ranks (keep ties)
SELECT salary
FROM (
  SELECT salary,
         DENSE_RANK() OVER (ORDER BY salary DESC) AS r
  FROM emp
) t
WHERE r = :n;
- B) Top N per group (e.g., top 3 products per category)

SELECT *
FROM (
  SELECT p.*,
         ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) AS rn
  FROM products p
) x
WHERE rn <= 3;
C) Running total & moving average

SELECT d,
       amt,
       SUM(amt) OVER (ORDER BY d
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS run_total,
       AVG(amt) OVER (ORDER BY d
         ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS ma_7
FROM t;
D) Gaps & Islands (consecutive sequences)

-- Number islands of consecutive days per id
WITH x AS (
  SELECT id, d,
         d - INTERVAL '1 day' * ROW_NUMBER() OVER (PARTITION BY id ORDER BY d) AS grp
  FROM t
)
SELECT id, MIN(d) AS start_d, MAX(d) AS end_d
FROM x
GROUP BY id, grp;
E) Relational division (find customers who bought all products in a set)

SELECT c.customer_id
FROM purchases p
JOIN customers c ON c.customer_id = p.customer_id
WHERE p.product_id IN (101,102,103)
GROUP BY c.customer_id
HAVING COUNT(DISTINCT p.product_id) = 3;
F) De-duplicate keep latest

DELETE FROM users u
USING (
  SELECT id, ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at DESC) rn
  FROM users
) x
WHERE u.id = x.id AND x.rn > 1;
G) Pagination (stable)

-- Stable keyset pagination (better than OFFSET for large pages)
SELECT *
FROM posts
WHERE (created_at, id) < (:last_created_at, :last_id)
ORDER BY created_at DESC, id DESC
LIMIT 20;
21) Vendor Syntax Quickies
Limit/Top:

Postgres/MySQL: LIMIT n OFFSET m

SQL Server: OFFSET m ROWS FETCH NEXT n ROWS ONLY or SELECT TOP n ...

UPSERT: PG ON CONFLICT, MySQL ON DUPLICATE KEY UPDATE, SQL Server MERGE.

Boolean: PG boolean; MySQL uses TINYINT(1); SQL Server BIT.

Identifiers: quoted vs backticked; case sensitivity differs.

Default window frame: varies—be explicit.

# Best Practices (Interview Soundbites)
Write sargable predicates; avoid functions on indexed columns.

Prefer EXISTS/NOT EXISTS over IN/NOT IN with potential NULLs.

Use the right index: composite with correct column order; add include columns (SQL Server) / covering indexes.

Keep transactions small and consistent; handle deadlock retries.

Validate data types, collations, and time zones early.

Use parameterized queries to prevent SQL injection.

Add constraints to enforce business rules; don’t rely solely on app code.

For analytics, prefer window functions over correlated subqueries.

# OA/Interview Tips
Start with a clear query shape (FROM/JOIN → WHERE → GROUP → WINDOW → SELECT…).

Check NULL behavior and edge cases (empty groups, ties).

Keep solutions portable, but mention vendor-specific optimizations if asked.

If performance is discussed, talk indexes, plans, sargability, and cardinality.

For long problems, build with CTEs step-by-step (easier to debug).

Show you know trade-offs: normalization vs denormalization, MVCC vs locks, heap vs clustered.