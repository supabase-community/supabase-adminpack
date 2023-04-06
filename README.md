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

|      Column      |       Type       | 
|------------------|------------------|
| current_database | name             | 
| schemaname       | name             | 
| tblname          | name             |
| idxname          | name             |
| real_size        | numeric          |
| extra_size       | numeric          |
| extra_pct        | double precision |
| fillfactor       | integer          |
| bloat_size       | double precision |
| bloat_pct        | double precision |
| is_na            | boolean          |


## Table Bloat

|      Column      |       Type       | 
|------------------|------------------|
| current_database | name             |
| schemaname       | name             |
| tblname          | name             |
| real_size        | numeric          |
| extra_size       | double precision |
| extra_pct        | double precision |
| fillfactor       | integer          |
| bloat_size       | double precision |
| bloat_pct        | double precision |
| is_na            | boolean          |


## Blocking PID Tree

|  Column   | Type | 
|-----------|------|
| PID       | text |
| Lock Info | text |
| State     | text |


## Duplicate Indexes

| Column |   Type   |
|--------|----------|
| size   | text     |
| idx1   | regclass |
| idx2   | regclass |
| idx3   | regclass |
| idx4   | regclass |


## Table Sizes

|      Column      |       Type       |
|------------------|------------------|
| table_schema     | name             |
| table_name       | name             |
| row_estimate     | real             |
| total            | text             |
| index            | text             |
| toast            | text             |
| table            | text             |
| total_size_share | double precision |


## Schema Sizes

|   Column    |  Type   |
|-------------|---------|
| schemaname  | name    |
| table       | text    |
| index       | text    |
| table_index | text    |
| sum         | numeric |


## Index Usage

|     Column      |  Type  |
|-----------------|--------|
| schemaname      | name   |
| tablename       | name   |
| num_rows        | bigint |
| table_size      | text   |
| index_name      | name   |
| index_size      | text   |
| unique          | text   |
| number_of_scans | bigint |
| tuples_read     | bigint |
| tuples_fetched  | bigint |


## Last Vacuum Analyze

|       Column        |           Type           |
|---------------------|--------------------------|
| relname             | name                     |
| last_vacuum         | timestamp with time zone |
| n_mod_since_analyze | timestamp with time zone |
| last_analyze        | timestamp with time zone |
| last_autoanalyze    | timestamp with time zone |
| analyze_count       | bigint                   |
| autoanalyze_count   | bigint                   |

## Table Row Estimates

|   Column   |  Type  |
|------------|--------|
| schemaname | name   |
| relname    | name   |
| n_live_tup | bigint |

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
