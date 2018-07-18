# SQL Notebook

To understand what is SQL, at a high level, we must first understand what is Database Management System (DBMS).

## DBMS

A DBMS provides 

> Efficient, reliable, convenient, and safe multi-user storage and access to massive amounts of persistent data
> 

To set up a DBMS, we must think through the following key concepts:

- Data Model. e.g. Relation: data is a set of records. XML: Hierarchical.
- Schema versus Data. e.g., the types of data that each attribute is gonna hold.
- Data Definition Langugae. To find an implementation of the database.
- Data Manipulation Language (DML).

**SQL is a DML for the Relational Data Model.**

### Relational Model


In the relational model, a Database = A set of named relations (table).

A relation, in turn, is defined by a set of attributes (columns: name and type) and a set of rows. A set of records that has a specific value for each attribute. 

Schema: a structural description of the relations in the database. 
Instance: actual contents in the database. 

A special attribute (or set of attributes) in each relation is a key: an attribute that uniquely identifies that specific row in the given relation. 

# Structured Query Language SQL

## Selecting Attributes

### General

To select attributes A1,..., An from the relation R1

```SQL
SELECT A1, A2,..., AN
FROM R1
```

### Examples

```SQL
SELECT name, birthdate
FROM people;
```
Sometimes, you may want to select all columns from a table. Typing out every column name would be a pain, so there's a handy shortcut:

```SQL
SELECT *
FROM people;
```

If you only want to return a certain number of results, you can use the LIMIT keyword to limit the number of rows returned:

```SQL
SELECT *
FROM people
LIMIT 10;
```
## Count

Count all rows in a relation

```Sql
SELECT COUNT(*)
FROM R1
```

 However, if you want to count the number of non-missing values in a particular column, you can call COUNT on just that column.
 
 ```SQL
 SELECT COUNT(birthdate)
FROM people;
 ```
 
 It's also common to combine COUNT with DISTINCT to count the number of distinct values in a column.

For example, this query counts the number of distinct birth dates contained in the `people` table:

```SQL
SELECT COUNT(DISTINCT birthdate)
FROM people;
```

## Filtering Rows

Filter rows that hold a given condition. 

```SQL
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

### Using BETWEEN 

```SQL
SELECT title
FROM films
WHERE (release_year BETWEEN 1994 AND 1999)
AND (certification = 'PG' OR certification = 'R');
```
Nota bene: `BETWEEN` is inclusive. i.e., includes limits.

### Using IN

```SQL
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

### Using NULL, IS NOT NULL

```SQL
SELECT COUNT(*)
 FROM films
 WHERE language IS NULL
```

### Using LIKE, NOT LIKE

In SQL, the LIKE operator can be used in a WHERE clause to search for a pattern in a column. To accomplish this, you use something called a wildcard as a placeholder for some other values. There are two wildcards you can use with LIKE:

The `%` wildcard will match zero, one, or many characters in text. For example, the following query matches companies like 'Data', 'DataC' 'DataCamp', 'DataMind', and so on:

```SQL
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```

The `_` wildcard will match a single character. For example, the following query matches companies like 'DataCamp', 'DataComp', and so on:

```SQL
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```

## Aggregations

Get the maximum amount grossed by any film:

```SQL
SELECT MAX(gross)
 FROM films
```
Get the amount grossed by the best performing film between 2000 and 2012, inclusive.

```SQL
SELECT SUM(gross)
 FROM films
 WHERE release_year >= 2000
```

### A NOTE ON ARITHMETIC

4%3 goes to 1. One of the numbers must be float to make it go to 1.3333

### ALIASING

```SQL
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

Get the percentage of people who are no longer alive. Alias the result as percentage_dead. Remember to use 100.0 and not 100!

```sql
SELECT COUNT(deathdate) * 1.0 / COUNT(*) * 100.0 AS percentage_dead
FROM people;
```

Nota bene: Note the use of 1.0 such that the counts division go to floats. 

## ORDER BY

Lexicographical order and Descending order for `birthdate`. Default is Ascedning order.

```SQL
SELECT birthdate, name
 FROM people
 ORDER BY birthdate DESC, name;
```
## GROUP BY

Get the country, release year, and lowest amount grossed per release year per country. Order your results by country and release year.

```SQL
SELECT release_year, country, MIN(gross)
 FROM films
 GROUP BY release_year, country
 ORDER BY country, release_year
```

### HAVING 

Nota Bene: `HAVING` used to filter by the result of one or more aggregations used on each group of the `GROUP BY`

Now you're going to write a query that returns the average budget and average gross earnings for films in each year after 1990, if the average budget is greater than $60 million.

```SQL
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
 FROM films
 WHERE release_year > 1990
 GROUP BY release_year
 HAVING AVG(budget) > 60000000
```



