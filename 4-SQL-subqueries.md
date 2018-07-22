# SQL Subqueries

## Subqueries INSIDE WHERE

**Used to filter with the results from another table.**

Use your knowledge of subqueries in `WHERE` to get the urban area population for only capital cities.

```sql
-- select the appropriate fields
SELECT name, country_Code, urbanarea_pop
-- from the cities table
FROM cities
-- with city name in the field of capital cities
WHERE name IN
  (
  SELECT capital
   FROM countries
  )
ORDER BY urbanarea_pop DESC;
```

## Subqueries INSIDE SELECT

**Use it to calculate a column for us. Then, it is only a matter of aligning correctly each calculated number into our original query.** 

selects the top nine countries in terms of number of cities appearing in the cities table. 

```sql
SELECT name AS country,
  (SELECT COUNT(*)
   FROM cities
   WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```
Note how you can access the data outside the subquery within the subquery. In this case, each `name AS country` is programatically replaced with its `country_code` into the subquery and its count put into place into the result. 

## Subqueries INSIDE FROM

In this exercise, for each of the six continents listed in 2015, you'll identify which country had the maximum inflation rate (and how high it was) using multiple subqueries.

```sql
SELECT name, continent, inflation_rate
FROM countries
INNER JOIN economies
ON countries.code = economies.code
WHERE year = 2015
    AND inflation_rate IN (
        SELECT MAX(inflation_rate) AS max_inf
        FROM (
             SELECT name, continent, inflation_rate
             FROM countries
             INNER JOIN economies
             ON countries.code = economies.code
             WHERE year = 2015) AS subquery
        GROUP BY continent);
```

## Examples

In this exercise, you'll need to get the country names and other 2015 data in the economies table and the countries table for Central American countries with an official language.

```sql
SELECT DISTINCT name, total_investment, imports
 FROM countries AS c1
 LEFT JOIN economies AS e
 ON c1.code = e.code
 WHERE year = 2015 AND 
 c1.region = 'Central America'
 AND  c1.code IN (
 SELECT code
 FROM languages AS l
 WHERE l.code = c1.code
 AND official = 'true'
 )
```
You are now tasked with determining the top 10 capital cities in Europe and the Americas in terms of a calculated percentage using `city_proper_pop` and `metroarea_pop` in cities.

```sql
SELECT name , country_code, city_proper_pop, metroarea_pop,  
      city_proper_pop / metroarea_pop * 100.0 AS city_perc
FROM cities
WHERE name IN
  (SELECT capital
   FROM countries
   WHERE (continent = 'Europe'
      OR continent LIKE '%America%'))
     AND metroarea_pop IS NOT NULL
ORDER BY city_perc DESC
LIMIT 10;
```
