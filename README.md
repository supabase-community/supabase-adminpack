# supabase-adminpack

A Trusted Language Extension containing a variety of useful database admin queries.

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
time due to what's called "index bloat".  This is where frequent
updates and deletes can cause space in index data structures to go
unused.  Understanding when this is happening is not obvious, so there
is a rather complex query you can run to detect it.

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

Like index bloat, tables with high update and delete rates can also
end up containing lots of allocated but unused space.  This query
shows which tables have the most bloat.

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

Postgres queries can block each other, and this blocking relationship
can form a tree, where A blocks B, which blocks C, and so on.  This
query formats blocking queries into a tree structure so it's easy to
see what query is causing the blockage.

|  Column   | Type | 
|-----------|------|
| PID       | text |
| Lock Info | text |
| State     | text |


## Duplicate Indexes

If a table contains duplicate indexes, then unecessary work is done
updating and storing them, this query will show up to 4 duplicate
indexes per table if they exist.

| Column |   Type   |
|--------|----------|
| size   | text     |
| idx1   | regclass |
| idx2   | regclass |
| idx3   | regclass |
| idx4   | regclass |


## Table Sizes

This view shows tables and their sizes.

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

This view shows schemas and their sizes, which is the sum of the sizes
of all the tables and indexes in the schema.

|   Column    |  Type   |
|-------------|---------|
| schemaname  | name    |
| table       | text    |
| index       | text    |
| table_index | text    |
| sum         | numeric |


## Index Usage

This view shows index size and usage.  An unused index still needs to
be updated and that takes time an storage, so it's a good candidate to
drop.

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

This views shows the last time a table was vacuumed an analyzed.

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

This view shows estimates for the number of rows in a table.  This is
just an estimate and depends on up to date table statistics.

|   Column   |  Type  |
|------------|--------|
| schemaname | name   |
| relname    | name   |
| n_live_tup | bigint |

## PGMeta Columns

This view shows infromation for the columns of tables in the system.

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

This views shows the configuration of the database.

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

This view shows installed extensions in the database.

|      Column       | Type |
|-------------------|------|
| name              | name |
| schema            | name |
| default_version   | text |
| installed_version | text |
| comment           | text |

## PGMeta Foreign Tables

This view shows foreign tables in the database.

| Column  |  Type  |
|---------|--------|
| id      | bigint |
| schema  | name   |
| name    | name   |
| comment | text   |

## PGMeta Functions

This view shows functions in the database.

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

This view shows materialized views in the database.

|    Column    |  Type   |
|--------------|---------|
| id           | bigint  |
| schema       | name    |
| name         | name    |
| is_populated | boolean |
| comment      | text    |

## PGMeta Policies

This view shows Row Level Security Policies in the database.

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

This view shows primary keys in the database.

|   Column   |  Type  |
|------------|--------|
| schema     | name   |
| table_name | name   |
| name       | name   |
| table_id   | bigint |

## PGMeta Publications

This view shows logical replication publishers in the database.

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

This view shows foreign key relationships in the database.

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

This view shows roles in the database system.  Note that roles are
global objects and apply to all databases.

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

This view shows all schemas in the database.

| Column |  Type  |
|--------|--------|
| id     | bigint |
| name   | name   |
| owner  | name   |

## PGMeta Tables

This view shows all tables in the database.

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

This view shows all triggers in the database.

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

This view shows all types in the database.

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

This view shows the current database version.

|       Column       |  Type  |
|--------------------|--------|
| version            | text   |
| version_number     | bigint |
| active_connections | bigint |
| max_connections    | bigint |

## PGMeta Views

This view shows all views in the database.

|    Column    |  Type   |
|--------------|---------|
| id           | bigint  |
| schema       | name    |
| name         | name    |
| is_updatable | boolean |
| comment      | text    |
