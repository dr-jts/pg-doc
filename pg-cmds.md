# Postgres Management Queries

## Functions

### List functions

```sql
SELECT routines.routine_name, parameters.data_type, parameters.ordinal_position
FROM information_schema.routines
    LEFT JOIN information_schema.parameters ON routines.specific_name=parameters.specific_name
WHERE routines.specific_schema='public'
AND routines.routine_name ILIKE 'svg%'
ORDER BY routines.routine_name, parameters.ordinal_position;
```

Or:
```sql
SELECT oid::regprocedure
FROM pg_proc
WHERE pg_function_is_visible(oid)  -- restrict to current search_path
AND proname ILIKE 'fname%';  -- name without schema-qualification
```

Or in `psql`:
```
\df [ pattern ]
```

### Drop function
Copy arg list from `psql` display (remove `DEFAULT` clauses)
```sql
DROP FUNCTION name(arg1, type1, ...);
```

### Create DROP FUNCTION commands

More details [here](https://stackoverflow.com/questions/7622908/drop-function-without-knowing-the-number-type-of-parameters).

```sql
SELECT oid::regprocedure
FROM pg_proc
WHERE pg_function_is_visible(oid)  -- restrict to current search_path
AND proname LIKE 'fname%';  -- name without schema-qualification
```

## Query processes

### Kill a query process

* Identify the PID of the query to terminate
```sql
SELECT pid, query FROM pg_stat_activity WHERE state = 'active'; 
```
* Kill process softly
```sql
SELECT pg_cancel_backend(PID);  
```
* Kill process hard
```sql
SELECT pg_terminate_backend(PID);
```
* Kill all queries for current user
```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE username=current_user
```

