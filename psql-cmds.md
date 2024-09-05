## psql Commands

### Show query time

`\timing`

### Formatting

* `\pset format aligned` - show table formatting
* `\pset format unaligned` - do not show table formatting
* `\x auto` - format tables as single column

### Command-line usage

Run SQL file sending raw output to another file
```
psql -A -t -o file.out  < query.sql
```
