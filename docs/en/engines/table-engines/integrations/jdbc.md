---
toc_priority: 3
toc_title: JDBC
---

# JDBC {#table-engine-jdbc}

Allows ClickHouse to connect to external databases via [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity).

To implement the JDBC connection, ClickHouse uses the separate program [clickhouse-jdbc-bridge](https://github.com/alex-krash/clickhouse-jdbc-bridge) that should run as a daemon.

This engine supports the [Nullable](../../../sql-reference/data-types/nullable.md) data type.

## Creating a Table {#creating-a-table}

``` sql
CREATE TABLE [IF NOT EXISTS] [db.]table_name
(
    columns list...
)
ENGINE = JDBC(dbms_uri, external_database, external_table)
```

**Engine Parameters**

-   `dbms_uri` β URI of an external DBMS.

    Format: `jdbc:<driver_name>://<host_name>:<port>/?user=<username>&password=<password>`.
    Example for MySQL: `jdbc:mysql://localhost:3306/?user=root&password=root`.

-   `external_database` β Database in an external DBMS.

-   `external_table` β Name of the table in `external_database`.

## Usage Example {#usage-example}

Creating a table in MySQL server by connecting directly with itβs console client:

``` text
mysql> CREATE TABLE `test`.`test` (
    ->   `int_id` INT NOT NULL AUTO_INCREMENT,
    ->   `int_nullable` INT NULL DEFAULT NULL,
    ->   `float` FLOAT NOT NULL,
    ->   `float_nullable` FLOAT NULL DEFAULT NULL,
    ->   PRIMARY KEY (`int_id`));
Query OK, 0 rows affected (0,09 sec)

mysql> insert into test (`int_id`, `float`) VALUES (1,2);
Query OK, 1 row affected (0,00 sec)

mysql> select * from test;
+------+----------+-----+----------+
| int_id | int_nullable | float | float_nullable |
+------+----------+-----+----------+
|      1 |         NULL |     2 |           NULL |
+------+----------+-----+----------+
1 row in set (0,00 sec)
```

Creating a table in ClickHouse server and selecting data from it:

``` sql
CREATE TABLE jdbc_table
(
    `int_id` Int32,
    `int_nullable` Nullable(Int32),
    `float` Float32,
    `float_nullable` Nullable(Float32)
)
ENGINE JDBC('jdbc:mysql://localhost:3306/?user=root&password=root', 'test', 'test')
```

``` sql
SELECT *
FROM jdbc_table
```

``` text
ββint_idββ¬βint_nullableββ¬βfloatββ¬βfloat_nullableββ
β      1 β         α΄Ία΅α΄Έα΄Έ β     2 β           α΄Ία΅α΄Έα΄Έ β
ββββββββββ΄βββββββββββββββ΄ββββββββ΄βββββββββββββββββ
```

## See Also {#see-also}

-   [JDBC table function](../../../sql-reference/table-functions/jdbc.md).

[Original article](https://clickhouse.tech/docs/en/engines/table-engines/integrations/jdbc/) <!--hide-->
