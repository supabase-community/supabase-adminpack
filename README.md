# supabase-adminpack

A Trusted Language Extension containing a variety of useful databse admin queries.

Postgres database admins are often faced with a variety of issues that
require them introspecting the database state.  This extension
contains a mixed-bag of views that reflect useful information for
admins.

## Installation

Using dbdev:

```
postgres=> select dbdev.install('michelp-adminpack');
 install
---------
 t
(1 row)

postgres=> create schema adminpack;
CREATE SCHEMA
postgres=> create extension "michelp-adminpack" with schema adminpack;
CREATE EXTENSION
```

This will the extension into the `adminpack` schema, substitute with
some other schema if you want it to go somewhere else.

## Index Bloat

Index updates and querying can end up getting slower and slower over
time due to what's called "index bloat".  Understanding when this is
happening is not obvious, so there is a rather complex query you can
run to detect it.

                     View "adminpack.index_bloat"
      Column      |       Type       | Collation | Nullable | Default
------------------+------------------+-----------+----------+---------
 current_database | name             |           |          |
 schemaname       | name             |           |          |
 tblname          | name             |           |          |
 idxname          | name             |           |          |
 real_size        | numeric          |           |          |
 extra_size       | numeric          |           |          |
 extra_pct        | double precision |           |          |
 fillfactor       | integer          |           |          |
 bloat_size       | double precision |           |          |
 bloat_pct        | double precision |           |          |
 is_na            | boolean          |           |          |


## Table Bloat

## Blocking PID Tree

## Duplicate Indexes

## Table Sizes

## Index Sizes

## Schema Sizes

## Index Usage

## Last Vacuum Analyze

## Table Row Estimates

## PGMeta Columns

## PGMeta Config

## PGMeta Extensions

## PGMeta Foreign Tables

## PGMeta Functions

## PGMeta Materialized Views

## PGMeta Policies

## PGMeta Primary Keys

## PGMeta Publications

## PGMeta Relationships

## PGMeta Roles

## PGMeta Schemas

## PGMeta Tables

## PGMeta Triggers

## PGMeta Types

## PGMeta Version

## PGMeta Views
