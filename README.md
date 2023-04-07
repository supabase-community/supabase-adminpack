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

|       Column        |   Type   |
|---------------------|----------|
| table_id            | bigint   |
| schema              | name     |
| table               | name     |
| id                  | text     |
| ordinal_position    | smallint |
| name                | name     |
| default_value       | text     |
| data_type           | text     |
| format              | name     |
| is_identity         | boolean  |
| identity_generation | text     |
| is_generated        | boolean  |
| is_nullable         | boolean  |
| is_updatable        | boolean  |
| is_unique           | boolean  |
| enums               | json     |
| comment             | text     | 

## PGMeta Config

|     Column      |  Type   |
|-----------------|---------|
| name            | text    |
| setting         | text    |
| category        | text    |
| group           | text    |
| subgroup        | text    |
| unit            | text    |
| short_desc      | text    |
| extra_desc      | text    |
| context         | text    |
| vartype         | text    |
| source          | text    |
| min_val         | text    |
| max_val         | text    |
| enumvals        | text[]  |
| boot_val        | text    |
| reset_val       | text    |
| sourcefile      | text    |
| sourceline      | integer |
| pending_restart | boolean |

## PGMeta Extensions

|      Column       | Type |
|-------------------|------|
| name              | name |
| schema            | name |
| default_version   | text |
| installed_version | text |
| comment           | text |

## PGMeta Foreign Tables

| Column  |  Type  |
|---------|--------|
| id      | bigint |
| schema  | name   |
| name    | name   |
| comment | text   |

## PGMeta Functions

|          Column           |  Type   |
|---------------------------|---------|
| id                        | bigint  |
| schema                    | name    |
| name                      | name    |
| language                  | name    |
| definition                | text    |
| complete_statement        | text    |
| args                      | jsonb   |
| argument_types            | text    |
| identity_argument_types   | text    |
| return_type_id            | bigint  |
| return_type               | text    |
| return_type_relation_id   | bigint  |
| is_set_returning_function | boolean |
| behavior                  | text    |
| security_definer          | boolean |
| config_params             | jsonb   |

## PGMeta Materialized Views

|    Column    |  Type   |
|--------------|---------|
| id           | bigint  |
| schema       | name    |
| name         | name    |
| is_populated | boolean |
| comment      | text    |

## PGMeta Policies

|   Column   |  Type  |
|------------|--------|
| id         | bigint |
| schema     | name   |
| table      | name   |
| table_id   | bigint |
| name       | name   |
| action     | text   |
| roles      | json   |
| command    | text   |
| definition | text   |
| check      | text   |

## PGMeta Primary Keys

|   Column   |  Type  |
|------------|--------|
| schema     | name   |
| table_name | name   |
| name       | name   |
| table_id   | bigint |

## PGMeta Publications

|      Column      |  Type   |
|------------------|---------|
| id               | bigint  |
| name             | name    |
| owner            | text    |
| publish_insert   | boolean |
| publish_update   | boolean |
| publish_delete   | boolean |
| publish_truncate | boolean |
| tables           | json[]  |

## PGMeta Relationships

|       Column        |  Type  |
|---------------------|--------|
| id                  | bigint |
| constraint_name     | name   |
| source_schema       | name   |
| source_table_name   | name   |
| source_column_name  | name   |
| target_table_schema | name   |
| target_table_name   | name   |
| target_column_name  | name   |

## PGMeta Roles

|       Column        |           Type           |
|---------------------|--------------------------|
| id                  | bigint                   |
| name                | name                     |
| is_superuser        | boolean                  |
| can_create_db       | boolean                  |
| can_create_role     | boolean                  |
| inherit_role        | boolean                  |
| can_login           | boolean                  |
| is_replication_role | boolean                  |
| can_bypass_rls      | boolean                  |
| active_connections  | bigint                   |
| connection_limit    | bigint                   |
| password            | text                     |
| valid_until         | timestamp with time zone |
| config              | text[]                   |

## PGMeta Schemas

| Column |  Type  |
|--------|--------|
| id     | bigint |
| name   | name   |
| owner  | name   |

## PGMeta Tables

|       Column       |  Type   |
|--------------------|---------|
| id                 | bigint  |
| schema             | name    |
| name               | name    |
| rls_enabled        | boolean |
| rls_forced         | boolean |
| replica_identity   | text    |
| bytes              | bigint  |
| size               | text    |
| live_rows_estimate | bigint  |
| dead_rows_estimate | bigint  |
| comment            | text    |

## PGMeta Triggers

|     Column      |               Type                |
|-----------------|-----------------------------------|
| id              | oid                               |
| table_id        | oid                               |
| enabled_mode    | text                              |
| function_args   | text[]                            |
| name            | information_schema.sql_identifier |
| table           | information_schema.sql_identifier |
| schema          | information_schema.sql_identifier |
| condition       | information_schema.character_data |
| orientation     | information_schema.character_data |
| activation      | information_schema.character_data |
| events          | text[]                            |
| function_name   | name                              |
| function_schema | name                              |

## PGMeta Types

|   Column   |  Type  |
|------------|--------|
| id         | bigint |
| name       | name   |
| schema     | name   |
| format     | text   |
| enums      | jsonb  |
| attributes | jsonb  |
| comment    | text   |

## PGMeta Version

|       Column       |  Type  |
|--------------------|--------|
| version            | text   |
| version_number     | bigint |
| active_connections | bigint |
| max_connections    | bigint |

## PGMeta Views

|    Column    |  Type   |
|--------------|---------|
| id           | bigint  |
| schema       | name    |
| name         | name    |
| is_updatable | boolean |
| comment      | text    |
