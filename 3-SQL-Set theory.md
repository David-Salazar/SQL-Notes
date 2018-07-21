# SQL- Set Theory

Stacking up things from different tables. Akin to `rbind`. Columns are alligned by index of columns: i.e., first column with first column, second with second, et cetera. 

## UNION

Determine all (non-duplicated) country codes in either the cities or the currencies table. That is, check for which codes we either have city data or currency data.

```sql
SELECT DISTINCT country_code
 FROM cities
UNION
SELECT DISTINCT code AS country_code
 FROM currencies 
ORDER BY country_code
```

## INTERSECT

As you think about major world cities and their corresponding country, you may ask which countries also have a city with the same name as their country name?

```sql
SELECT name
 FROM countries
INTERSECT
SELECT name
 FROM cities
```

## EXCEPT 

Determine the names of capital cities that are not listed in the cities table.

```sql
SELECT capital
 FROM countries
EXCEPT
SELECT name
 FROM cities
ORDER BY capital
```

# SEMI-ANTI JOIN

Filter a table using the results from other table. 

## SEMI-JOIN

```sql
SELECT DISTINCT name
 FROM languages
 WHERE code IN (
 SELECT code
 FROM countries
 WHERE region = 'Middle East')
 ORDER BY name;
```

## ANTI-JOIN

Your goal is to identify the currencies used in Oceanian countries!

```sql
SELECT code, name
FROM countries
WHERE continent = 'Oceania'
  AND code NOT IN
  (SELECT code
   FROM currencies);
```

Use `NOT IN` and (`SELECT` code `FROM` currencies) as a subquery to get the country code and country name for the Oceanian countries that are not included in the currencies table.



