# JOIN LATERAL Patterns

## Ways to think about JOIN LATERAL

### In-line SELECT

A SELECT can be used in a select-list, but only if it returns a single row with a single column.
JOIN LATERAL can be thought of as a way to remove both of those restrictions 

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
