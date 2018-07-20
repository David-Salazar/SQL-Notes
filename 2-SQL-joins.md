# SQL Joins

## Inner Join

Two joins on a same query. 

```SQL
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN table1
ON left_table.id = table1.id;
```

When joining tables with a common field name (e.g. countries.code = cities.code), you can use USING as a shortcut

```SQL
SELECT c.name AS country, continent, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
USING (code);
```

## CASE WHEN

```SQL
-- get name, continent, code, and surface area
SELECT name, continent, code, surface_area,
    -- first case
    CASE WHEN surface_area > 2000000
    -- first then
            THEN 'large'
    -- second case
       WHEN surface_area > 350000
    -- second then
            THEN 'medium'
    -- else clause + end
       ELSE 'small' END
    -- alias resulting field of CASE WHEN
       AS geosize_group
-- from the countries table
FROM countries;
```

## LEFT JOIN

The result of the left outer join is the set of all combinations of tuples in L and R that are equal on the chosen key, in addition (loosely speaking) to tuples in L that have no matching tuples in R.

```SQL
-- select name, region, and gdp_percapita
SELECT name, region, gdp_percapita
-- countries (alias c) on the left
FROM countries AS c
-- join with economies (alias e)
INNER JOIN economies AS e
-- match on code fields
ON c.code = e.code
-- focus on 2010 entries
WHERE year = 2010;
```

## FULL JOIN

The right outer join behaves almost identically to the left outer join, but the roles of the tables are switched. The outer join or full outer join in effect combines the results of the left and right outer joins.

```sql
SELECT c1.name AS country, region, l.name AS language,
       basic_unit, frac_unit
FROM countries AS c1
FULL JOIN languages AS l
USING (code)
FULL JOIN currencies AS cc2
USING (code)
WHERE region LIKE 'M%esia';
```

## CROSS JOIN

Do not have neither can use `ON` or `USING ()` .

Cross product between the rows of each of the tables. 

```SQL
SELECT c.name AS city, l.name AS language
 FROM cities AS c
 CROSS JOIN languages AS l
 WHERE c.name LIKE 'Hyder%'
```








