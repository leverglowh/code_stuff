## SQL

**S**tructured **Q**uery **L**anguage.

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

##### Constraints

- `PRIMARY KEY`
- `UNIQUE`
- `NOT NULL`
- `DEFAULT 'Default value'`



### Query

```sql
SELECT * FROM users;
```

```sql
SELECT (column_1, column_3) FROM table_name;
```

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