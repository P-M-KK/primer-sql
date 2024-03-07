# Reference

Though there is a SQL standard, it is not easy to read, and every vendor deviates enough that their references are more useful:

- [(Google) BigQuery](https://cloud.google.com/bigquery/docs)
- [(IBM) DB2](https://www.ibm.com/docs/en/i/7.2?topic=reference-sql) (versioned)
- [MariaDB](https://mariadb.com/kb/en/sql-statements-structure/)
- [MySQL](https://dev.mysql.com/doc/refman/8.0/en/) (versioned)
- [Oracle](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/) (versioned)
- [PostgreSQL](https://www.postgresql.org/docs/current/sql.html)
- [(AWS) Redshift](https://docs.aws.amazon.com/redshift/latest/dg/c_SQL_commands.html)
- [(Microsoft) SQL Server / Azure SQL / Azure Synapse](https://learn.microsoft.com/en-us/sql/t-sql/language-reference)
- [SQLite](https://www.sqlite.org/lang.html)

---

# [Intro](https://en.wikipedia.org/wiki/SQL)

SQL (Structured Query Language) is a set-based & declarative programming language used for databases, even many "NoSQL" databases.

Though SQL is divided into DDL (Definition), DQL (Query), DML (Manipulation), & DCL (Control), this page focuses on DQL.

Many vendors have extensions to SQL, such as T-SQL & PL/SQL, that provide procedural programming constructs.

---

# Syntax & Logic

Though queries are written in this order:
```
  WITH sub-queries
SELECT columns
  FROM table/sub-query
       [join op] table/sub-query
       [pivot op] ...
 WHERE condition
 GROUP BY [group op] columns
HAVING condition
WINDOW windows
[set op] sub-query
 ORDER BY columns
 LIMIT n
OFFSET n
```

They are processed in this order:
```
  WITH    (lazily evaluate sub-queries for later clauses)
  FROM    (for each row in table/sub-query)
       [join op] (combine columns from multiple tables/sub-queries)
       [pivot op] (flip rows & columns from table/sub-query)
 WHERE    (if row meets condition)
 GROUP BY (aggregate rows by columns)
          [group op] (aggregate groups)
HAVING    (if group/aggregate meets condition)
WINDOW    (lazily evaluate analytic windows for later clauses)
SELECT    (return columns from row/group)
[set op]  (combine rows/groups from multiple sub-queries)
 ORDER BY (sort rows/groups)
OFFSET    (skip first X rows/groups)
 LIMIT    (stop after X rows/groups)
```

---

# [Join Operators](https://en.wikipedia.org/wiki/Join_(SQL))

Join Operators combine columns from 2 tables/queries based on user-defined conditions.

The relationship between the tables/queries (one-to-many, many-to-many) & the join conditions (any comparison operator except `=`) can result in extra rows.

Joining a table/query with itself is possible.

`RIGHT` versions exist for all `LEFT` joins.

Though many vendors still support deprecated `,`/`(+)`/`*=` join syntax, they should be avoided.

- ![Venn And](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Venn0001.svg/50px-Venn0001.svg.png) `(INNER) JOIN`
- ![Venn Or](https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Venn0111.svg/50px-Venn0111.svg.png) `FULL (OUTER) JOIN`
- ![Venn P](https://upload.wikimedia.org/wikipedia/commons/thumb/1/10/Venn0101.svg/50px-Venn0101.svg.png) `LEFT (OUTER) JOIN`
  - Can access columns from the RIGHT table
- ![Venn And](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Venn0001.svg/50px-Venn0001.svg.png) [`LEFT SEMI JOIN`](https://en.wikipedia.org/wiki/Relational_algebra#Semijoin_(%E2%8B%89_and_%E2%8B%8A))
  - Cannot access columns from the RIGHT table
  - Where this is not available syntactically, it can be emulated via:
    - `FROM p WHERE EXISTS (SELECT 1 FROM q WHERE q.key = p.key)`
    - `FROM p WHERE p.key IN (SELECT q.key FROM q)`
    - `FROM p LEFT OUTER JOIN q ON (q.key = p.key) WHERE q.key IS NOT NULL` (avoid for performance reasons)
- ![Venn Not Imply](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Venn0100.svg/50px-Venn0100.svg.png) [`LEFT ANTI (SEMI) JOIN`](https://en.wikipedia.org/wiki/Relational_algebra#Antijoin_(%E2%96%B7))
  - Where this is not available syntactically, it can be emulated via:
    - `FROM p WHERE NOT EXISTS (SELECT 1 FROM q WHERE q.key = p.key)`
    - `FROM p WHERE p.key NOT IN (SELECT q.key FROM q)`
    - `FROM p LEFT OUTER JOIN q ON (q.key = p.key) WHERE q.key IS NULL` (avoid for performance reasons)
- ![Venn True](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Venn1111.svg/50px-Venn1111.svg.png) `CROSS JOIN`
  - Produces a row for every combination of rows in both sources (rarely needed & be careful)

# [Set Operators](https://en.wikipedia.org/wiki/Set_operations_(SQL))

Set Operators combine rows from 2 same-type tables/queries based on equality of the entire row.

- ![Venn Or](https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Venn0111.svg/50px-Venn0111.svg.png) `UNION`
- ![Venn And](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Venn0001.svg/50px-Venn0001.svg.png) `INTERSECT`
- ![Venn Not Imply](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Venn0100.svg/50px-Venn0100.svg.png) `EXCEPT` / `MINUS`
- \* `ALL` preserves duplicates, which can improve performance when you know there are no duplicates

# Group Operators (Totals)

- `ROLLUP` creates `COUNT(terms)` levels of super aggregate rows for each `GROUP BY` term based on the order of the terms (one-to-many)
  - `GROUPING(terms)` values will always be in `POW(2, X) - 1`
  - Where this is not available syntactically, it can be emulated via:
    - ```
      SELECT [terms 1 to N], [agg func]()
       GROUP BY [terms 1 to N]
      UNION ALL
      SELECT [terms 1 to N-1], NULL, [agg func]()
       GROUP BY [terms 1 to N-1]
      ...
      UNION ALL
      SELECT [N "NULL" columns], [agg func]()
      ```
- `CUBE` creates `POW(2, COUNT(terms)) - 1` levels of super aggregate rows for every combination of `GROUP BY` terms (many-to-many)
  - Where this is not available syntactically, it can be emulated via extending the approach for emulating `ROLLUP`, but this should be avoided.
- `GROUPING SETS ((terms), etc...)` limits super aggregate rows to specified combinations of `GROUP BY` terms
- `GROUPING(terms)` identifies super aggregate rows via a bit mask with values not exceeding ` POW(2, COUNT(terms)) - 1`

# [Pivot Operators](https://en.wikipedia.org/wiki/Pivot_table)

- `PIVOT` denormalizes rows into columns for 1 metric
  - Where this is not available syntactically, it can be emulated via:
    - ```
      SELECT CASE col WHEN 'col1' THEN val END AS col1
           , CASE col WHEN 'col2' THEN val END AS col2
           -- repeat above for each value in the column being pivoted
      ```
- `UNPIVOT` normalizes columns of the same metric into rows
  - Where this is not available syntactically, it can be emulated via:
    - ```
      SELECT 'col1' as col, col1 as val
      UNION ALL
      SELECT 'col2' as col, col2 as val
      -- repeat above for each column being unpivoted
      ```

---

# [Aggregate Functions](https://en.wikipedia.org/wiki/Aggregate_function)

Aggregate Functions summarize all rows in each group into 1 result per group, comparable to a fold in functional programming or catamorphism in computer science.

When `GROUP BY` is not specified, the entire result set is considered to be 1 group, so using an Aggregate Function will yield only 1 result.

- `COUNT`
- `MIN` & `MAX`
- `SUM` & `AVG`
  - Weighted Average/Mean = `SUM(weight * value) / SUM(weight)` or `SUM(weight_normal * value)`
  - Geometric Mean
    - `POW(PRODUCT(value_pos_normal), 1.0 / COUNT(value_pos_normal))`
    - `EXP(AVG(LN(value_pos_normal)))` ( `PRODUCT` is typically not available syntactically but can be emulated via `EXP(SUM(LN(value)))` )
  - Harmonic Mean = `COUNT(value_rate) / SUM(1.0 / value_rate)`
  - Circular Mean = `ATAN2(SUM(SIN(value_rad)), SUM(COS(value_rad)))`
- `STDDEV`, `VAR`, etc...
  - [Sample vs Population](https://en.wikipedia.org/wiki/Bessel%27s_correction)
- List Concatenation (`LISTAGG`/`STRING_AGG`/`GROUP_CONCAT`)

# [Window / Analytic Functions](https://en.wikipedia.org/wiki/Window_function_(SQL))

Window Functions summarize rows in each window into 1 result per row, after `GROUP BY`.

Since they are calculated in the `SELECT` clause, using them in a `WHERE` clause requires a sub-query.

`PARTITION BY` is the window equivalent of `GROUP BY`.

`ORDER BY` changes the behavior of some Window Functions by limiting the window.

The window can also be limited dynamically based on offsets from the current row.

- Most Aggregate Functions (running total, moving average, etc...)
- `ROW_NUMBER`
- [Ranking](https://en.wikipedia.org/wiki/Ranking#Strategies_for_handling_ties)
  - Standard Competition Ranking = `RANK`
  - Modified Competition Ranking =
    - ```
      SELECT x
           , x_rank
           , x_rank_ordinal
           , MAX(x_rank_ordinal) OVER (PARTITION BY x_rank) AS x_rank_modified
        FROM(SELECT x
                  , RANK()       OVER (ORDER BY x) x_rank
                  , ROW_NUMBER() OVER (ORDER BY x) x_rank_ordinal
               FROM y
            );
      ```
  - Dense Ranking = `DENSE_RANK`
  - Ordinal Ranking = `ROW_NUMBER`
  - Fractional Ranking =
    - ```
      SELECT x
           , x_rank
           , x_rank_ordinal
           , AVG(x_rank_ordinal) OVER (PARTITION BY x_rank) AS x_rank_fractional
        FROM(SELECT x
                  , RANK()       OVER (ORDER BY x) x_rank
                  , ROW_NUMBER() OVER (ORDER BY x) x_rank_ordinal
               FROM y
            );
      ```
  - `MODE` is typically not available syntactically & does not preserve ties, but it can be calculated, preserving ties, via: 
    - ```
      SELECT x
           , x_count
        FROM(SELECT x
                  , x_count
                  , RANK() OVER (ORDER BY x_count DESC) AS x_rank
               FROM(SELECT x
                         , COUNT(1) AS x_count
                      FROM y
                     GROUP BY x
                   )
            )
       WHERE x_rank = 1
       ORDER BY x;
      ```
    - Weighted Mode (aka "get the row/group having the highest value")
      - "row" (weight-only) use case:
        - ```
          SELECT x
               , z as z_max
            FROM y
           WHERE z = (
                 SELECT MAX(z) AS z_max
                   FROM y
                )
           ORDER BY x;
          ```
      - "group" use case:
        - ```
          SELECT x
               , x_sum
            FROM(SELECT x
                      , x_sum
                      , RANK() OVER (ORDER BY x_sum DESC) AS x_rank
                   FROM(SELECT x
                             , SUM(z) AS x_sum
                          FROM y
                         GROUP BY x
                       )
                )
           WHERE x_rank = 1
           ORDER BY x;
          ```
- Absolute Selection (`FIRST_VALUE` & `LAST_VALUE` & `NTH_VALUE`)
  - Aggregate versions of these are typically not available syntactically but can be calculated via:
    - ```
      SELECT z
           , x AS x_first
        FROM(SELECT x
                  , RANK() OVER (PARTITION BY z ORDER BY w) AS x_rank
               FROM y
            )
      HAVING x_rank = 1
       ORDER BY w;
      ```
      - while it is usually best to avoid ties via strict/unique `ORDER BY` criteria, ties can be handled via either:
        - `GROUP BY z` & "List Concatenation" on `x` additionally to flatten all of them
        - `ROW_NUMBER` instead of `RANK` to pick 1 arbitrarily
- Relative Selection (`LAG` & `LEAD`)
- Partition & Collation
  - Dynamic Bucket Size & Dynamic Bucket Count
    - Partition = `DENSE_RANK() OVER (ORDER BY y)`
    - Collation = `ROW_NUMBER() OVER (PARTITION BY y ORDER BY x)`
  - Static Bucket Size & Dynamic Bucket Count (Remainder in Last Bucket)
    - Partition = `CEIL(ROW_NUMBER() OVER (ORDER BY x) / N)`
    - Collation = `(ROW_NUMBER() OVER (ORDER BY x) - 1) % N + 1`
  - Dynamic Bucket Size & Static Bucket Count (Remainder in First Buckets)
    - Partition = `NTILE(N) OVER (ORDER BY x)`
    - Collation =
      - ```
        SELECT x
             , bucket_num
             , ROW_NUMBER() OVER (PARTITION BY bucket_num ORDER BY x)
          FROM(SELECT x
                    , NTILE(N) OVER (ORDER BY x) AS bucket_num
                 FROM y
              )
         ORDER BY x;
        ```
- Quantile/Percentile (`PERCENTILE_CONT` & `PERCENTILE_DISC`)
  - Median = `PERCENTILE_CONT(0.5)` & `PERCENTILE_DISC(0.5)`
    - Weighted Median
      - ```
        SELECT AVG(val) -- Linear Interpolation to handle ties
          FROM(SELECT val
                    , weight
                    , SUM(weight) OVER () / 2.0 AS weight_median
                    ,                       SUM(weight) OVER (ORDER BY val) - weight AS weight_running_lower
                    , SUM(weight) OVER () - SUM(weight) OVER (ORDER BY val)          AS weight_running_upper
                 FROM y
              )
         WHERE weight_running_upper <= weight_median
           AND weight_running_lower <= weight_median;
        ```
- Cumulative Distribution (`CUME_DIST` & `PERCENT_RANK`)
  - This is the inverse of the Quantile/Percentile
  - Estimating the "[Pareto Joint Ratio](https://en.wikipedia.org/wiki/Pareto_principle)" (top x% of count represents (100-x)% of value) =
    - ```
      /* Linear Interpolation (y = mx + b) is usually accurate enough with sufficient data */
        WITH tbl1 AS (
             SELECT tbl.*
                  , COUNT(1)    OVER (ORDER BY weight DESC, val) AS count_running
                  , COUNT(1)    OVER ()                          AS count_total
                  , SUM(weight) OVER (ORDER BY weight DESC, val) AS weight_running
                  , SUM(weight) OVER ()                          AS weight_total
                    /* Normalize */
                  , CUME_DIST() OVER (ORDER BY weight DESC, val) AS x
                  , SUM(weight) OVER (ORDER BY weight DESC, val)
                  / SUM(weight) OVER ()
                    AS y -- Weighted Cumulative Distribution
               FROM tbl)
           , tbl2 AS (
             SELECT tbl1.*
                    /* 1st row where x + y >= 1 */
                  , x + y AS x_y
                  , RANK() OVER (ORDER BY CASE WHEN x + y >= 1 THEN x + y ELSE 3 END) AS rank_dist_gte1
                    /* m = (y_1 - y_0) / (x_1 - x_0) */
                  , (y - LAG(y) OVER (ORDER BY weight DESC, val))
                  / (x - LAG(x) OVER (ORDER BY weight DESC, val))
                    AS slope
                    /* b = y - mx */
                  , y
                  - ( x
                    * (y - LAG(y) OVER (ORDER BY weight DESC, val))
                    / (x - LAG(x) OVER (ORDER BY weight DESC, val))
                    ) AS intercept
               FROM tbl1)
      SELECT tbl2.* -- show your work
             /* 1 - x = mx + b   =>   x = (1 - b) / (m + 1) */
           ,      (1 - intercept) / (slope + 1)  AS x_pareto
             /* y = 1 - x */
           , 1 - ((1 - intercept) / (slope + 1)) AS y_pareto
        FROM tbl2
       WHERE rank_dist_gte1 = 1;
      ```
      - This can be extended to support ABC Analysis & other advanced partitioning schemes

---

# Sub-queries

## Correlated Sub-queries

- [Pareto Front](https://en.wikipedia.org/wiki/Pareto_front) (multi-objective optimization)

```
SELECT ao.*
  FROM a ao
 WHERE NOT EXISTS (
       SELECT 1
         FROM a ai
        WHERE ai.x >= ao.x -- ">=" maximizes X; "<=" minimizes X
          AND ai.y >= ao.y -- ">=" maximizes Y; "<=" minimizes Y
          -- repeat above for higher dimensions
          AND(   ai.x > ao.x -- ">" maximizes X; "<" minimizes X
              OR ai.y > ao.y -- ">" maximizes Y; "<" minimizes Y
              -- repeat above for higher dimensions
             )
      );
```

---

# Case Inensitivity

- `UPPER`/`LOWER` require a functional/function-based index or persisted generated/computed column to be performant, and they are also prone to the [Turkish "I" problem](https://en.wikipedia.org/wiki/Dotted_and_dotless_I_in_computing).
- `LIKE`/`ILIKE` ...
- A collation, whether on the permanent database or column level or the temporary session or query level, is the best way to be case-insensitive.

---

# Pagination

Though pagination can be implemented via `OFFSET` & `LIMIT`, this will not be performant if the data is not sorted via indexed columns.

Keyset pagination ...

---

# Hierarchies

- Properties
  - Level Dynamism: Is the number of levels fixed?
  - Naming: Do levels have meaningfully different names (absolute, not relative)?
  - Balance: Are all leaf nodes at the same level?
  - Raggedness: Can nodes skip levels?
    - e.g.: USA -> [No State] -> Washington, D.C.
  - "Roll Up": Is data only linked to leaf nodes?
- Tabular Representations
  - Horizontal
    - Columns: Root, ..., Parent, Child
    - For fixed-level named hierarchies, **best** all-around efficiency
  - [Adjacency List](https://en.wikipedia.org/wiki/Adjacency_list)
    - Column: Parent ID
    - For n-level unnamed hierarchies
      - worst read-time efficiency (only immediate parent relationships reified via `=`)
      - **best** write-time efficiency (change only immediate children)
      - **best** space efficiency (1 row per node; 1 column)
      - allows referential integrity
  - Path Enumeration
    - Column: Path (/Root ID/.../Parent ID/)
    - for n-level unnamed hierarchies
      - bad read-time efficiency (all ancestor relationships reified via `LIKE`)
      - good write-time efficiency (change all successors)
      - bad space efficiency (1 row per node; 1 column with full ancestry)
      - no referential integrity
  - [Nested Set](https://en.wikipedia.org/wiki/Nested_set_model) **(avoid)**
    - Columns: Left & Right Bounds
    - For n-level unnamed hierarchies
      - good read-time efficiency (all ancestor relationships reified via `>`/`<`)
      - worst write-time efficiency (change all ancestors, successors, & other nodes)
      - good space efficiency (1 row per node; 2 columns)
      - no referential integrity
  - Ancestor ([Transitive Closure](https://en.wikipedia.org/wiki/Transitive_closure))
    - Columns: ID & Ancestor ID in a separate table
    - for n-level unnamed hierarchies
      - **best** read-time efficiency (all ancestor relationships reified via `=`)
      - bad write-time efficiency (change all ancestors & successors)
      - worst space efficiency (1 row per node per ancestor; 2 columns)
      - allows referential integrity
    - May also include the [Reflexive](https://en.wikipedia.org/wiki/Reflexive_closure) and/or [Symmetric Closure](https://en.wikipedia.org/wiki/Symmetric_closure) depending on the use case, but these are trivial additions (see example below)
- [Querying](https://en.wikipedia.org/wiki/Hierarchical_and_recursive_queries_in_SQL)

Basic querying for data stored in the Adjacent List model:
```
  WITH y_hierarchy AS (
       /* Anchor */
       SELECT id
            , id_parent
            , NULL AS name_parent
         FROM y
        WHERE id_parent IS NULL
       UNION ALL
       /* Recursive */
       SELECT y.id
            , y.id_parent
            , yh.name AS name_parent
         FROM y
              JOIN y_hierarchy yh ON (yh.id = y.id_parent)
      )
SELECT *
  FROM y_hierarchy;
```

Advanced querying (replicating all of Oracle's `CONNECT BY` features) for data stored in the Adjacency List model:
```
  WITH y_hierarchy AS (
       /* Anchor */
       SELECT id
            , id_parent
            , 0 AS level -- LEVEL
            , id AS id_root
            , CASE WHEN id IN (SELECT id_parent FROM y) THEN 0 ELSE 1 END AS is_leaf -- CONNECT_BY_ISLEAF
            , 0 AS is_cycle -- CONNECT_BY_ISCYCLE
            , '/' + CAST(t.id   AS VARCHAR(MAX)) + '/' AS path_id   -- SYS_CONNECT_BY_PATH(id  , '/')
            , '/' + CAST(t.name AS VARCHAR(MAX)) + '/' AS path_name -- SYS_CONNECT_BY_PATH(name, '/')
         FROM y
        WHERE id_parent IS NULL -- START WITH id_parent IS NULL
       UNION ALL
       /* Recursive */
       SELECT y.id
            , y.id_parent
            , yh.level + 1 AS level -- LEVEL
            , yh.id_root
            , CASE WHEN y.id IN (SELECT id_parent FROM y) THEN 0 ELSE 1 END AS is_leaf -- CONNECT_BY_ISLEAF
            , CASE WHEN yh.path_id LIKE '%/' + CAST(y.id AS VARCHAR(MAX)) + '/%' THEN 1 ELSE 0 END AS is_cycle -- CONNECT_BY_ISCYCLE
            , yh.path_id   + CAST(y.id   AS VARCHAR(MAX)) + '/' AS path_id   -- SYS_CONNECT_BY_PATH(id  , '/')
            , yh.path_name + CAST(y.name AS VARCHAR(MAX)) + '/' AS path_name -- SYS_CONNECT_BY_PATH(name, '/')
         FROM y
              JOIN y_hierarchy yh ON (yh.id = y.id_parent) -- CONNECT BY PRIOR id = id_parent
        WHERE yh.is_cycle = 0 -- NOCYCLE
      )
SELECT yh.*
     , yr.name as name_root
  FROM y_hierarchy yh
       JOIN y yr ON (yr.id = yh.id_root) -- CONNECT_BY_ROOT
 ORDER BY yh.path_name; -- ORDER SIBLINGS BY name
```

Generating the [Reflexive](https://en.wikipedia.org/wiki/Reflexive_closure), [Symmetric](https://en.wikipedia.org/wiki/Symmetric_closure), & [Transitive Closure](https://en.wikipedia.org/wiki/Transitive_closure) for data stored in the Adjacent List model:
```
  WITH y_hierarchy AS (
       /* Anchor */
       /* Reflexive (A->A) */
       SELECT id
            , id AS id_other
            , 0 AS level
         FROM y
       UNION ALL
       /* Recursive */
       /* Transitive (A->B & B->C -> A->C) */
       SELECT y.id
            , yh.id_other
            , yh.level + 1 AS level
         FROM y
              JOIN y_hierarchy yh ON (yh.id = y.id_parent)
      )
SELECT id
     , id_other
     , level
  FROM y_hierarchy
UNION ALL
/* Symmetric (A->B -> B->A) */
SELECT id_other AS id
     , id       AS id_other
     , -level   AS level
  FROM y_hierarchy
 where level != 0; -- prevents duplication of Reflexive Closure
```

---

# Resources

- https://modern-sql.com/
- https://sqlity.net/en/1146/a-join-a-day-introduction/
- https://en.wikipedia.org/wiki/Level_of_measurement
- https://mystery.knightlab.com/
- ML
  - http://www.r2d3.us/visual-intro-to-machine-learning-part-1/
  - http://www.r2d3.us/visual-intro-to-machine-learning-part-2/

---
---
---

# WIP
