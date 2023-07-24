# Sql Cheatsheet

## Server administration

~~~

-- Check installed version and edition
SELECT @@VERSION
SELECT SERVERPROPERTY('Edition')

~~~

## Selecting

~~~

-- Select two columns from a single table
SELECT columnA, columnnB FROM mytable;

-- Count the number of records in a table
SELECT COUNT(*) AS cnt FROM mytable;

-- Count the number of non-null values in a specific column
SELECT COUNT(columnA) AS cnt FROM mytable;

-- Select distinct values in a column
SELECT DISTINCT columnA FROM mytable;

-- Count distinct values in a column
SELECT COUNT(DISTINCT columnA) AS cnt_distinct FROM mytable;

-- Select top 10 rows on MS Sql Server
SELECT TOP 10 * FROM mytable

-- Select top 10 rows on MySQL, PostgreSQL, or SQLite
SELECT * FROM mytable LIMIT 10

~~~

## Filtering

## Aggregating

## Sorting and grouping
