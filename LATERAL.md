# JOIN LATERAL Patterns

## Ways to think about JOIN LATERAL

<https://stackoverflow.com/questions/28550679/what-is-the-difference-between-lateral-join-and-a-subquery-in-postgresql>
<https://stackoverflow.com/questions/25536422/optimize-group-by-query-to-retrieve-latest-row-per-user>

### Postgres Manual Description

Subqueries appearing in FROM can be preceded by the key word LATERAL. This allows them to reference columns provided by preceding FROM items. (Without LATERAL, each subquery is evaluated independently and so cannot cross-reference any other FROM item.)

Table functions appearing in FROM can also be preceded by the key word LATERAL, but for functions the key word is optional; the function's arguments can contain references to columns provided by preceding FROM items in any case.

### In-line SELECT

A SELECT can be used in a *select-list* (this is called a **correlated subquery**), but only if it returns a single row with a single column.
JOIN LATERAL can be thought of as a way to remove both of those restrictions.

### Looping

A LATERAL join is like a SQL foreach loop, in which the query iterates over each row in a result set 
and evaluates a subquery using that row as a parameter.



## Factor out expressions

```sql
select
    pledged_usd,
    avg_pledge_usd,
    amt_from_goal,
    duration,
    (usd_from_goal / duration) as usd_needed_daily
from kickstarter_data,
    lateral (select pledged / fx_rate as pledged_usd) pu
    lateral (select pledged_usd / backers_count as avg_pledge_usd) apu
    lateral (select goal / fx_rate as goal_usd) gu
    lateral (select goal_usd - pledged_usd as usd_from_goal) ufg
    lateral (select (deadline - launched_at)/86400.00 as duration) dr;
```

## Loop over rows

## Top-N queries

## User-Defined function returning multiple rows

## Access elements of complex data

```sql
WITH data(geom)AS (VALUES ( 'MULTIPOINT((1 1), (2 2), (3 3), (4 4))'::geometry ) )
SELECT ST_GeometryN(geom, i) FROM data 
  JOIN LATERAL generate_series(1, ST_NumGeometries(geom)) AS s(i) ON true;
```

In simple cases Postgres allows a more compact form 
by automatically doing a LATERAL join for functions returning recordsets with single values:

```sql
WITH data(geom)AS (VALUES ( 'MULTIPOINT((1 1), (2 2), (3 3), (4 4))'::geometry ) )
SELECT ST_GeometryN(geom, generate_series(1, ST_NumGeometries(geom)) ) FROM data;
```

## Examples

* Conversion Funnel: <https://heap.io/blog/postgresqls-powerful-new-join-type-lateral>


## KNN queries

<https://blog.crunchydata.com/blog/a-deep-dive-into-postgis-nearest-neighbor-search>

So how can you specify a query that finds the nearest neighbours of many features in a table out of  another table?  
This is where the recent support for LATERAL subqueries comes to the rescue.  
It essentially acts as a loop, allowing many features to be queried for their nearest neighbours.  
A typical query using LATERAL looks like:
```sql
SELECT loc.*, t.name, t.geom
  FROM locations loc
  JOIN LATERAL (SELECT name, geom FROM geonames gn 
      ORDER BY loc.geom <-> gn.geom LIMIT 10) AS t ON true;
```
