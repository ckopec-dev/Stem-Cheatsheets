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
SELECT TOP 10 * FROM mytable;

-- Select top 10 rows on MySQL, PostgreSQL, or SQLite
SELECT * FROM mytable LIMIT 10;

~~~

## Filtering

~~~

-- Select films released after 1960
SELECT title FROM films WHERE release_year > 1960;

-- Select films from Japan
SELECT title FROM films WHERE country = 'Japan';

-- Select films released after 1960 and from Japan
SELECT title FROM films WHERE release_year > 1960 AND country = 'Japan';

-- Select films released after 1960 or from Japan
SELECT title FROM films WHERE release_year > 1960 OR country = 'Japan';

-- Select films released between 1960 and 1970
SELECT title FROM films WHERE release_year BETWEEN 1960 and 1970

-- Between is inclusive, so the above is identical to
SELECT title FROM films WHERE release_year >= 1960 AND release_year <= 1970

-- Select films that with "red" in the title
SELECT * FROM films WHERE title LIKE '%red%';

-- Select films that do not have "green" in the title
SELECT * FROM films WHERE title NOT LIKE '%green%';

-- Specify multiple values in a where clause
SELECT * FROM films WHERE release_year IN (1990, 2000, 2010);

-- Select films with unknown release year
SELECT * FROM films WHERE release_year IS NULL;

-- Select films with known release year
SELECT * FROM films WHERE release_year IS NOT NULL;

~~~

### Comparison operators

- &gt;: greater than
- &gt;=: greater than or equal to
- <: less than
- <=: less than or equal to
- =: equal to
- <>: not equal to

### Wildcards

- %: match zero or more characters
- _: match one character

## Aggregating

## Sorting and grouping
