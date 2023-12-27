# SQL query to find table name from column name

## mysql
```
SELECT DISTINCT TABLE_NAME 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE COLUMN_NAME LIKE ('%the_column_name%')
AND TABLE_SCHEMA='table_name';
```

## ms-sql

```
SELECT
    ColumnName = c.name,
    SchemaName = SCHEMA_NAME(t.schema_id),
    TableName = t.name
FROM 
    sys.columns c
INNER JOIN 
    sys.tables t ON t.object_id = c.object_id
WHERE
    c.name = 'your-column-name-here'
```

# SQL Query to take backup of table
```
CREATE TABLE ABCD_2023_09_03 SELECT * FROM ABCD;
```


# Query to find all rows that have extra space within themselves
```
SELECT *
FROM TABLE_NAME
WHERE COLUMN_NAME LIKE '%  %';
```