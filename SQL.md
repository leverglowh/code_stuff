# SQL

**S**tructured **Q**uery **L**anguage.

---

#### Table of Contents

- [Table managing](#table-managing)
  - [Type constraints](#type-constraints)
- [SELECT Query](#select-query)
  - [`WHERE` operators](#where-operators)
  - [`ORDER BY`](#order-by)
  - [`LIMIT`](#limit)
  - [`CASE`](#case)
- [Aggregates](#aggregates)
  - [Computations](#computations)
  - [`GROUP BY`](#group-by)
  - [`HAVING`](#having)
- [Index](#index)
- [Multiple tables](#multiple-tables)
  - [KEYS](#keys)
  - [`..JOIN..ON..`](#joinon)
  - [`..LEFT JOIN..ON..`](#left-joinon)
  - [`CROSS JOIN`](#cross-join)
  - [`UNION`](#union)
  - [`WITH`](#with)
- [Data types](#data-types)
  - [SQLite data types](#sqlite-data-types)
  - [MySQL data types](#mysql-data-types)
  - [Postgres data types](#postgres-data-types)

---

### Table managing

```sql
CREATE TABLE table_name (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  column_1 data_type NOT NULL, 
  column_2 data_type,
  column_3 data_type
);
```

```sql
INSERT INTO table_name (column_2, column_3)
VALUES ('ciao', true);
```

```sql
ALTER TABLE table_name 
ADD COLUMN column_4 TEXT;
```

```sql
UPDATE table_name
SET column_3 = false
WHERE column_2 = 'ciao';
```

```sql
DELETE FROM table_name
WHERE column_3 IS NULL;
```

##### Type constraints

- `PRIMARY KEY`
- `CHECK`
- `UNIQUE`
- `NOT NULL`
- `DEFAULT 'Default value'`

---

### SELECT Query

```sql
SELECT * FROM users;
-- Show everything from the table
```

```sql
SELECT column_1, column_3 FROM table_name;
-- Show only column 1 and 3 of the table
```

```sql
SELECT column_1 AS 'alternate_column_name' FROM table_name; 
-- Rename column 1 only in the results
```

```sql
SELECT DISTINCT column_2, column_1 FROM table_name;
-- Do not show duplicates of the pair
```

```sql
SELECT column_1, column_2 FROM table_name WHERE column_3 = true;
-- Show only column 1 and 2, where 3 is true
```

##### `WHERE` operators

```sql
SELECT * FROM table_name WHERE column_1 = 5;
```

- `=` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to
- `LIKE`:  *<u>NOT</u>* case sensitive
  - `WHERE name LIKE 'He_lo'` selects everyone whose name starts with `He` and ends with `lo`, and has exactly *one* character in between
  - `WHERE name LIKE 'H%'` selects everyone whose name starts with `A` with anything at the tail.. (*A*, *Abc*, *A5636* are all accepted) [`A%`, `%a`, `%dada%`]
- `IS NULL`/`IS NOT NULL`
- `BETWEEN`: *<u>IS</u>* case sensitive
  - `WHERE age BETWEEN 13 AND 19` selects all teenagers
  - `WHERE name BETWEEN 'A' AND 'D'` selects all names from 'A' to, *including* 'D'.. (*Ab*, *Bc*, *Cd*, *Cz*, *D* are all accepted, *Da* isn't)
- `AND`: combining multiple  `WHERE`s
  - `WHERE name LIKE 'A%' AND age > 19`
- `OR`

##### `ORDER BY`

```sql
SELECT * FROM table_name ORDER BY column_1 ASC;
```

- `ASC`: ascending order
- `DESC`: descending order

##### `LIMIT`

```sql
SELECT * FROM table_name LIMIT 10;
-- Show only 10 rows
```

##### `CASE`

SQL way of `if..else`.. or `switch`..

```sql
SELECT column_2,
  CASE
  	WHEN column_1 > 18 THEN 'adult'
  	WHEN column_1 > 13 THEN 'teenager'
  	ELSE 'kid'
  END AS 'Role'
 FROM table_name;
-- The second column (Role) is either 'adult' or 'teenager' or 'kid'
```

---

### Aggregates

##### Computations

- `COUNT()`: n -> 1

```sql
SELECT COUNT(*) FROM table_name;
-- Counts how many rows are there
```

- `SUM()`: n -> 1

```sql
SELECT SUM(column_2) FROM table_name;
-- Shows 1 value: sum of all column_2 values
```

- `MAX()`/`MIN()`: n -> 1

```sql
SELECT MAX(column_2) FROM table_name;
-- Shows 1 value: the biggest value in the column_2
```

```sql
SELECT column_1 FROM table_name 
WHERE column_2 = (SELECT MAX(column_2) FROM table_name);
-- Shows the column 1 value that has the maximum column 2 value
```

- `AVG()`: n -> 1

```sql
SELECT AVG(column_2) FROM table_name;
-- Shows 1 value: the average value of the column 2
```

- `ROUND()`: n -> n

```sql
SELECT column_1, ROUND(column_2, 2) FROM table_name;
-- Shows 2 columns, the second column values are rounded to 2 decimal places
```

##### `GROUP BY`

```sql
SELECT column_1, COUNT(*) FROM table_name GROUP BY column_1;
-- Returns 2 columns: first one shows distinct column 1 values, the second one counts how many rows are there for every distinct column 1 value (how many times the value is repeated)
```

```sql
SELECT column_1, column_2, COUNT(*) FROM table_name
GROUP BY 2 ORDER BY 1,2;
-- Notice that I can reference columns by their position in the query
```

##### `HAVING`

Used to filter aggregated properties.

```sql
SELECT column_1, column_2, COUNT(column_3) FROM table_name
GROUP BY 1, 2
HAVING COUNT(column_3) > 5;
```

---

## Index
Index content for fast queries.
```sql
CREATE INDEX name_index on passengers (lastname)
```

---

### Multiple tables

##### KEYS

`PRIMARY KEY`

Special column that uniquely identifies its rows, it:

- Can't be `NULL`
- Must be *unique*
- There mustn't be more than *one* `PRIMARY KEY` column in a table

It is declared when the table is created:

```sql
CREATE TABLE table_name (
	column_1 INT PRIMARY KEY
);
```

`FOREIGN KEY`

When a column in *table_1* is the `PRIMARY KEY` of another table, it is called `FOREIGN KEY` in *table_1*.

Usually I make `JOIN`s between the `PRIMARY KEY` of one table and the `FOREIGN KEY` of the other.

##### `..JOIN..ON..`

Links two tables, having the same *common_column*, only the rows that matches *`common_column = column`*.

```sql
SELECT table_1.column, table_2.otherColumn
FROM table_1 JOIN table_2 
ON table_1.common_column = table_2.column;
```

##### `..LEFT JOIN..ON..`

Links two tables, having a common column, but keeping the left table rows if *`common_column = column`*  isn't verified and deletes the right table rows.

```sql
SELECT *
FROM table_1 LEFT JOIN table_2
ON table_1.common_column = table_2.column;
```

##### `CROSS JOIN`

Joins two tables, with all the possible combinations.

```sql
SELECT *
FROM table_1 CROSS JOIN table_2;
```

Usually it's used  when we need to compare each row of a table to a list of values .

```sql
SELECT month, COUNT(*)
FROM newspaper CROSS JOIN months
WHERE start_month <= month AND end_month >= month
GROUP BY month;
```

 How many users were subscribed during each month of the year. 

##### `UNION`

I use it between two `SELECT`s, when I have two `SELECT` results with the same columns, I can unite them and have a common result.

```sql
SELECT * FROM table_1 
UNION
SELECT * FROM table_2;
```

##### `WITH`

When I have to use the results of a `SELECT` to make a further query:

```sql
WITH previous_results AS (
	SELECT ...
    ...
)
SELECT * 
FROM previous_results JOIN table_2 
ON res_column = table_2.column;
```

---

## Data types

Each SQL database has its own data types.

### SQLite data types

- `TEXT`
- `NUMERIC`
- `INTEGER`
- `REAL`: real number
- `BLOB`: binary data

### MySQL data types

- `CHAR(n)`
- `VARCHAR(n)`
- `SMALLINT`
- `INT`
- `BIGINT`
- `FLOAT`
- `DOUBLE`
- ...

### Postgres data types

- `boolean`(`bool`)
- `char(n)`: fixed-length string with space padded
- `varchar(n)`: variable-length string
- `text`: a *varchar* with unlimited length
- `SMALLINT`: 2-byte signed [*-32,768* to *32,767*]
- `INT`: 4-byte signed [*-2,147,483,648* to *2,147,483,647*]
- `SERIAL`: INT, auto-generated
- `UUID`: Universal Unique Identifiers, more unique, more safe
- `float(n)`: floating-point number, precision at least `n`, at max 8 bytes
- `real`(`float8`): 4-byte floating point number
- `numeric`(`numeric(p,s)`): real number with `p` digits and `s` number after the decimal point
- `DATE`
- `TIME`
- `TIMESTAMP`: date + time
- `TIMESTAMPTZ`: timezone-aware timestamp
- `INTERVAL`: periods of time
- array: `serial number INT[]`
- `JSON`: plain JSON, must be parsed for each processing
- `JSONB`: *JSON* in binary, faster to process but slower to insert, supports <u>indexing</u>
- ...

## SQLite
```
touch mydb.sql
sqlite3 mydb.sql
// SQL STUFF HERE
.tables
.mode columns
.headers yes
```
