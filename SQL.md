SQL

**S**tructured **Q**uery **L**anguage.

---

#### Table of Contents

- [Table managing](#table-managing)
  - [Type constrains](#type-constrains)
- [Queries](#query)
  - [WHERE](#where-operators)
  - [ORDER BY](#order-by)
  - [LIMIT](#limit)
  - [CASE](#case)
- [Aggregates](#aggregates)
  - [Computations](#computations)
  - [GROUP BY](#group-by)
- [Postgres data types](#postgres-data-types)

---

### Table managing

```sql
CREATE TABLE table_name (
    column_1 data_type,
    column_2 data_type,
    column_3 data_type
);
```

```sql
INSERT INTO table_name (column_2, column_3)
VALUES ('ciao', true);
```

```plsql
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
- `UNIQUE`
- `NOT NULL`
- `DEFAULT 'Default value'`

---

### Query

```sql
SELECT * FROM users;
/* Show everything from the table */
```

```sql
SELECT column_1, column_3 FROM table_name;
/* Show only column 1 and 3 of the table */
```

```sql
SELECT column_1 AS 'alternate_column_name' FROM table_name; 
/* Rename column 1 only in the results */
```

```sql
SELECT DISTINCT column_2, column_1 FROM table_name;
/* Do not show duplicates of the pair */
```

```sql
SELECT column_1, column_2 FROM table_name WHERE column_3 = true;
/* Show only column 1 and 2, where 3 is true */
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
SELECT * FROM table_name LIMIT 10; /* Show only 10 rows */
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
 /* The second column (Role) is either 'adult' or 'teenager' or 'kid' */
```

---

### Aggregates

##### Computations

- `COUNT()`: n -> 1

```sql
SELECT COUNT(*) FROM table_name; /* Counts how many rows are there */
```

- `SUM()`: n -> 1

```sql
SELECT SUM(column_2) FROM table_name;
/* Shows 1 value: sum of all column_2 values */
```

- `MAX()`/`MIN()`: n -> 1

```sql
SELECT MAX(column_2) FROM table_name;
/* Shows 1 value: the biggest value in the column_2 */
```

```sql
SELECT column_1 FROM table_name 
WHERE column_2 = (SELECT MAX(column_2) FROM table_name);
/* Shows the column 1 value that has the maximum column 2 value */
```

- `AVG()`: n -> 1

```sql
SELECT AVG(column_2) FROM table_name;
/* Shows 1 value: the average value of the column 2 */
```

- `ROUND()`: n -> n

```sql
SELECT column_1, ROUND(column_2, 2) FROM table_name;
/* Shows 2 columns, the second column values are rounded to 2 decimal places */
```

##### `GROUP BY`

```sql
SELECT column_1, COUNT(*) FROM table_name GROUP BY column_1;
/* Returs 2 columns: first one shows distinct column 1 values, the second one counts how many rows are there for every distinct column 1 value (how many times the value is repeated) */
```

```sql
SELECT column_1, column_2, COUNT(*) FROM table_name
GROUP BY 2 ORDER BY 1,2;
/* Notice that I can reference columns by their position in the query */
```

##### `HAVING`

Used to filter aggregated properties.

```sql
SELECT column_1, column_2, COUNT(column_3) FROM table_name
GROUP BY 1, 2
HAVING COUNT(column_3) > 5;
```

---

##### Postgres data types

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
- `JSONB`: *JSON* in binary, faster to process but slower to insert, supports <u>indexing</u>.