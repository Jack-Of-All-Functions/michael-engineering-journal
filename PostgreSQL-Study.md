# Postgres study

[Link to documentation for PostgreSQL v10](https://www.postgresql.org/docs/10/index.html)

### Table of Contents

1. [What is PostgreSQL?](#what-is-postgresql)
2. [Table Basics](#table-basics)
    - [Creating a Table](#creating-a-table)
    - [Dropping a Table](#dropping-a-table)
3. [Modifying Tables](#modifying-tables)
4. [Schemas](#schemas)
5. [Data Manipulation](#data-manipulation)
    - [Inserting Data](#inserting-data)
    - [Updating Data](#updating-data)
    - [Deleting Data](#deleting-data)
    - [Querying a Table](#querying-a-table)
6. [Database Capacities](#postgresql-database-capacities)
7. [Using PostgreSQL with Node.js](#using-postgresql-with-nodejs)

### What is PostgreSQL?

PostgreSQL is an open source relational database that dates back to 1986. It was originally created in UC Berkley, as an evolution of the [Ingres](https://en.wikipedia.org/wiki/Ingres_(database)) project. Postgres is fully [ACID](https://en.wikipedia.org/wiki/ACID) compliant, and is available on nearly every platform, including macOS, Linux, FreeBSD, OpenBSD, and Windows.

### Some important features

#### Data Types

- Primitives: Integer, Numeric, String, Boolean
- Structured: Date/Time, Array, Range, UUID
- Document: JSON/JSONB, XML, Key-value (Hstore)

#### Data Integrity

- UNIQUE, NOT NULL
- Primary Keys
- Foreign Keys

#### Concurrency, Performance

- Indexing: B-tree, Multicolumn, Expressions, Partial
- Parallelization of read queries and building B-tree indicies
- Table partitioning
- Just-in-time compilation of expressions

#### Security

- Authentication: GSSAPI, SSPI, LDAP, SCRAM-SHA-256, Certificate, and more
- Multi-factor authentication with certificates and an additional method

#### Extensibility

- Stored functions and procedures
- SQL/JSON path expressions
- Foreign data wrappers: connect to other databases or streams with a standard SQL interface
- Customizable storage interface for tables
- Other extensions that provide additional functionality, including PostGIS

#### Internationalisation, Text Search

- Support for international character sets
- Case-insensitive and accent-insensitive collations
- Full-text search

### Table Basics

#### Creating a table

``` 
CREATE TABLE products (
  product_number integer,
  name text,
  price numeric
);
```

There is a limit on how many columns a table can contain. Depending on the column types, it is between 250 and 1600. Designing a table with this many columns is highly unusual, and is often a questionable design.

#### Dropping a table

` DROP TABLE products; `

Dropping a table that doesn't exist results in an error. However, it is common in SQL scripts to drop a table before attempting to create it. In this case, you can use the following command:

` DROP TABLE IF EXISTS products; `

### Modifying Tables

There are multiple functions available to modify tables. They include, but are not limited to:

- Add/Remove columns
- Change column data types
- Rename columns
- Rename tables

All of these actions are performed using the `ALTER TABLE` command.

To add a column, use the following command:

`ALTER TABLE products ADD COLUMN description text;`

To remove a column, use the following command:

`ALTER TABLE products DROP COLUMN description;`

To change the data type of a column, use the following command:

`ALTER TABLE products ALTER COLUMN price TYPE numeric;`

To rename a column, use the following command:

`ALTER TABLE products RENAME COLUMN product_no TO product_number;`

To rename a table, use the following command:

`ALTER TABLE products RENAME TO items;`

### Schemas

#### Creating a Schema

To create a schema, use the `CREATE SCHEMA` command. That may look like the following:

`CREATE SCHEMA products`

To create a new table within the schema, you may use a command like the following: 

``` 
CREATE TABLE products.images (
  ...
);
```

To drop an empty schema, use the following command:

`DROP SCHEMA products;`

To drop a schema including all contained objects, use the following command:

`DROP SCHEMA products CASCADE;`

### Data Manipulation

#### Inserting Data

To create a new row within a table, use the `INSERT` command. This command requires the table name and column values. Consider the following table schema:

```
CREATE TABLE products (
  product_number integer,
  name text,
  price numeric
);
```

If we want to insert a row into this table, we may use a command like the following:

`INSERT INTO products VALUES (1, 'Camo Onesie', 140);`

Notice that the data values are comma separated, and inserted in the order that they were created in the schema.
In the above example, you are required to know the order that the columns were inserted into the table. To avoid this, you can name the columns explicitly. That may look something like the following:

`INSERT INTO products (name, price, product_number) VALUES ('Camo Onesie', 140, 1);`

You can also insert multiple rows in the same command:

```
INSERT INTO products (name, price, product_number) VALUES
  ('Camo Onesie', 140, 1),
  ('Yeasy 350s', 120, 2),
  ('Another Cool Product', 160, 3);
```

#### Updating Data

In order to update entries, you can use the `UPDATE` command.

Updating a single entry may look like:

`UPDATE products SET price = 120 WHERE product_number = 1;`

You can also update multiple entries at a time. That may look something like:

`UPDATE products SET price = 100 WHERE price = 120;`

It is also possible to update multiple columns in an `UPDATE` command by listing multiple assignments in a `SET` command.

`UPDATE products SET name = 'Camo Onesie - SALE!!!', price = 80 WHERE product_id = 1;`

#### Deleting Data

In order to delete data, use the `DELETE` command.

If you want to drop a single row from a table, that command may look like:

`DELETE FROM products WHERE product_id = 3;`

You can also drop multiple rows at once:

`DELETE FROM products WHERE price = 100;`

If you want to drop all rows from a table, that may look like:

`DELETE FROM products;`

#### Querying a Table

Reading from a table is done with the `SELECT` command. If we want to query a product by its ID, we may use the following command:

`SELECT * FROM products WHERE product_id = 24;`

The `SELECT` command can also define a subset of the data. For instance, if you only want the name of the product, you may use a command like:

`SELECT name FROM products WHERE product_id = 24;`

### PostgreSQL database capacities

The following information is derived from [here](https://soapware.screenstepslive.com/s/documentation/m/FAQ_current/l/22071-q-what-is-the-postgresql-database-top-capacity).

| Limit                      | Value                                |
-----------------------------|--------------------------------------|
| Maximum Database Size      | Unlimited                            |
| Maximum Table Size         | 32 TB                                |
| Maximum Row Size           | 1.6 TB                               |
| Maximum Field Size         | 1 GB                                 |
| Maximum Rows per Table     | Unlimited                            |
| Maximum Columns per Table  | 250 - 1600 depending on column types |
| Maximum Indicies per table | Unlimited                            |
