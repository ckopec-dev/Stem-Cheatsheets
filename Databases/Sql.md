# Sql Cheatsheet

## Server administration

~~~sql
-- Check installed version and edition
SELECT @@VERSION
SELECT SERVERPROPERTY('Edition')

-- Create an index
CREATE INDEX IDX_Name ON account (name);

-- Reindex a table
DBCC DBREINDEX('dbo.mytable');
~~~

## Selecting

~~~sql
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

~~~sql
-- Select films released after 1960
SELECT title FROM films WHERE release_year > 1960;

-- Select films from Japan
SELECT title FROM films WHERE country = 'Japan';

-- Select films released after 1960 and from Japan
SELECT title FROM films WHERE release_year > 1960 AND country = 'Japan';

-- Select films released after 1960 or from Japan
SELECT title FROM films WHERE release_year > 1960 OR country = 'Japan';

-- Select films released between 1960 and 1970
SELECT title FROM films WHERE release_year BETWEEN 1960 AND 1970

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
- !&gt;: not greater than
- <: less than
- <=: less than or equal to
- !<: not less than
- =: equal to
- <>, !=: not equal to
- BETWEEN: between two specified values
- IS NULL: is a null value
- LIKE: for use with wildcard operators

### Wildcards

- %: match zero or more characters
- _: match one character
- []: specify a set of characters

## Functions

### Strings

~~~sql
-- Left 3 chars and right 4 chars
SELECT LEFT(phone, 3), RIGHT(phone, 4) FROM company

-- The length of the field
SELECT LEN(city) FROM company

-- Lower/upper case of field
SELECT LOWER(name), UPPER(state) FROM company

-- Trims whitespace from left and right of field
SELECT LTRIM(name), RTRIM(city) FROM company

-- Returns soundex value (4-character code used to evaluate similarity of strings
SELECT SOUNDEX('banana')
~~~

### Dates and times

~~~sql
-- Get the year of a datetime
SELECT DATEPART(yy, order_date) FROM orders
~~~

### Math

- ABS()
- COS()
- EXP()
- PI()
- SIN()
- SQRT()
- TAN()
- ROUND(number_to_round, decimal_places): if decimal_places is negative, the number will round to the left of the decimal point
  
## Aggregating

### Common aggregate functions

- AVG()
- SUM()
- MIN()
- MAX()
- COUNT()

~~~sql
-- To use aggregate on unique values, use the DISTINCT argument
SELECT AVG(DISTINCT price) FROM product
~~~

## Sorting and grouping

~~~sql
-- Order films by title
SELECT * FROM films ORDER BY title;

-- Same sort except by position
SELECT * FROM films ORDER BY 1;

-- Order films by most largest gross
SELECT * FROM films ORDER BY gross DESC;

-- Order films by most recent release year then title
SELECT * FROM films ORDER BY release_year DESC, title ASC;

-- Show count of films by certification (G, PG, etc)
SELECT certification, COUNT(title) AS title_count FROM films GROUP BY certification;

-- Show count of films by certification and language, sorted by number of titles from most to least
SELECT certification, language, COUNT(title) AS title_count
FROM films
GROUP BY certification, language
ORDER BY title_count DESC;

-- Show count of films with a minimum of over ten films by release year
SELECT release_year, COUNT(title) AS title_count FROM films GROUP BY release_year HAVING COUNT(title) > 10; 
~~~

## Select ordering

- SELECT
- FROM
- WHERE
- GROUP BY
- HAVING
- ORDER BY

## Order of execution

- FROM
- WHERE
- GROUP BY
- HAVING
- SELECT
- ORDER BY
- LIMIT

## Having vs where

- WHERE can't filter aggregate functions.
- WHERE filters individual records, HAVING filters grouped records.

## Case

- Similar to case statements in other languages.

~~~sql
SELECT
  home_goal,
  away_goal,
  CASE
    WHEN home_goal > away_goal THEN 'Home Team Win'
    WHEN home_goal < away_goal THEN 'Away Team Win'
    ELSE 'Tie' END AS outcome
FROM match;
~~~

## Aliases

~~~sql
SELECT first_name + ' ' + last_name AS full_name
SELECT unit_price * quantity AS cost
~~~

## Joins

### Inner join

- Looks for records in both tables that match on given field(s).
- It is generally easier to construct the query by building the joins first.

~~~sql
-- Inner join of presidents and prime_ministers, joining on country
SELECT prime_ministers.country, prime_ministers.continent, prime_minister, president
FROM prime_ministers
INNER JOIN presidents
ON prime_ministers.country = presidents.country;

-- Same query with aliases
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
ON p1.country = p2.country;

-- Same query with USING
SELECT p1.country, p1.continent, prime_minister, president
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country);

-- Chaining joins
SELECT p1.country, p1.continent, prime_minister, president, pm_start
FROM prime_ministers AS p1
INNER JOIN presidents AS p2
USING(country)
INNER JOIN prime_minister_terms AS p3
USING(prime_minister);
~~~

### Left join

- Returns all records in the left table, and records in the right table that match on given field(s).
- This is a type of outer join.

~~~sql
-- Left join of presidents and prime_ministers, joining on country
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
LEFT JOIN presidents AS p2
USING(country);
~~~

### Right join

- Similar to left join, except in the other direction.
- This is another type of outer join.
- Uncommon, since it can be rewritten as an equivalent left join.

~~~sql
-- Right join of presidents and prime_ministers, joining on country
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
RIGHT JOIN presidents AS p2
USING(country);
~~~

### Full join

- Combines a left join and a right join.
- This is another type of outer join.

~~~sql
-- Full join of presidents and prime_ministers, joining on country
SELECT p1.country, prime_minister, president
FROM prime_ministers AS p1
FULL JOIN presidents AS p2
USING(country);
~~~

### Cross join

- Creates all possible combinations of two tables.

~~~sql
CREATE TABLE Colors (
    ColorName VARCHAR(50)
);

CREATE TABLE Sizes (
    SizeLabel VARCHAR(10)
);

INSERT INTO Colors (ColorName) VALUES
('Red'),
('Blue'),
('Green');

INSERT INTO Sizes (SizeLabel) VALUES
('S'),
('M'),
('L'),
('XL');

SELECT
    c.ColorName,
    s.SizeLabel
FROM
    Colors AS c
CROSS JOIN
    Sizes AS s;
~~~

### Self join

- Used to join a table with itself.
- Aliasing is required.

~~~sql
-- Self join of every country with every other country on the same continent
SELECT p1.country AS country1, p2.country AS country2, p1.continent
FROM prime_ministers AS p1
INNER JOIN prime_ministers AS p2
ON p1.continent = p2.continent AND p1.country <> p2.country
~~~

## Set operations

### Union

- Takes two tables and returns all rows from both tables (unless the rows are identical, in which case only one of them is returned).

~~~sql
SELECT * FROM left_table
UNION
SELECT * FROM right_table;
~~~

### Union All

- Same as union except duplicates are included.

~~~sql
SELECT * FROM left_table
UNION ALL
SELECT * FROM right_table;
~~~

### Intersect

- Takes two tables and returns rows that are identical in both tables.

~~~sql
SELECT id, val FROM left_table
INTERSECT
SELECT id, val FROM right_table;
~~~

### Except

- Takes two tables and returns rows in the left table that are not in the right table.

~~~sql
SELECT id, val FROM left_table
EXCEPT
SELECT id, val FROM right_table;
~~~

## Subqueries

### Semi join

- Selects records in the first table table where a condition is met in the second table.
- Unlike other joins, a semi join is implemented via a subquery.

~~~sql
SELECT president, country, continent
FROM presidents
WHERE country IN (SELECT country FROM states WHERE indep_year < 1800);
~~~

### Anti join

- Similar to semi join, except records are excluded when a condition is met.
- Also implemented via a subquery.

~~~sql
SELECT president, country, continent
FROM presidents
WHERE country NOT IN (SELECT country FROM states WHERE indep_year < 1800);
~~~

## Inserting

~~~sql
-- Values for all fields in given table must be provided if not explicitly specified.
INSERT INTO customers
VALUES ('Bob', 'Smith', '555-555-1234')

-- Explicitly specify fields
INSERT INTO customers(firstname, lastname)
VALUES ('Bob', 'Smith')

-- Insert values using the result of a select
INSERT INTO customers(firstname, lastname)
SELECT firstname, lastname FROM customer_staging

-- Create a new populated table using the result of a select
SELECT * INTO customers FROM customer_import
~~~

## Updating

~~~sql
UPDATE customers SET active = 0 WHERE firstname = 'Frank' 
~~~

## Deleting

~~~sql
DELETE FROM customers WHERE lastname = 'Smith'
~~~

## Creating tables

~~~sql
CREATE TABLE customers
(
  customer_num  INTEGER      NOT NULL,
  firstname     VARCHAR(20)  NOT NULL,
  lastname      VARCHAR(20)  NOT NULL,
  created       DATETIME     NOT NULL    DEFAULT GETDATE()
);
~~~

## Altering tables

~~~sql
-- Add a column
ALTER TABLE customers
ADD phone VARCHAR(20) 

-- Remove a column
ALTER TABLE customers
DROP COLUMN created

-- Remove the table
DROP TABLE customers
~~~

## Views

~~~sql
-- Views are essentially saved select queries
CREATE VIEW customers_formatted AS
SELECT firstname + ' ' + lastname AS 'Name'
FROM customers

-- Use the view
select * from customers_formatted
~~~

## Stored procedures

~~~sql
CREATE PROCEDURE spActiveCustomers AS
SELECT * FROM customers WHERE active = 1
GO;

-- Use the sproc
EXEC spActiveCustomers

-- Enable data access
SELECT @@ServerName
EXEC sp_serveroption @@ServerName, 'DATA ACCESS', TRUE

-- Save the sproc results to a temp table
SELECT * INTO #tmp
FROM OPENQUERY(YOURSERVERNAME, 'EXEC {db name}.{schema name}.{sproc name}')

-- Select from the temp table
SELECT * FROM #tmp

-- The temp table lives in tempdb and will be dropped automatically when the session ends.
~~~

## Transactions

~~~sql
-- Everything happens
BEGIN TRANSACTION
-- Do some inserts, updates, or deletes
COMMIT TRANSACTION

-- Nothing happens
BEGIN TRANSACTION
-- Do some inserts, updates, or deletes
ROLLBACK TRANSACTION

-- Use savepoints to do partial rollbacks 
BEGIN TRANSACTION
INSERT {some data}
SAVEPOINT delete1
DELETE {some data}
-- Undo the delete
ROLLBACK TO delete1 
~~~
