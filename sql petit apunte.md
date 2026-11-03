# SQL

Podemos usar pgAdmin o sqlshell.

## CREATE and DROP TABLE

Para crear tablas usamos `CREATE TABLE [nombre] ([columna] [tipo]);`

```sql
CREATE TABLE cars (
brand VARCHAR(255),
model VARCHAR(255),
year INT
);
```

The `DROP TABLE` statement is used to drop an existing table in a database, erasing all the records at the same time.

```sql
DROP TABLE cars;
```

## INSERT and DELETE records

Para añadir filas (o records) a la tabla usamos `INSERT INTO [tabla] ([columna], [...]) VALUES ([value], 'Mustang', 1964);`

```sql
INSERT INTO cars (brand, model, year)
VALUES ('Ford', 'Mustang', 1964);

/* varias filas a la vez: */

INSERT INTO cars (brand, model, year)
VALUES
  ('Volvo', 'p1800', 1968),
  ('BMW', 'M1', 1978),
  ('Toyota', 'Celica', 1975);
```

The `DELETE` statement is used to delete existing **records** in a table.

> Note: Be careful when deleting **records** in a table! Notice the `WHERE` clause in the `DELETE` statement. The `WHERE` clause specifies which record(s) should be deleted.

```sql
/* Esto solo borra los records que la brand sea volvo. */

DELETE FROM cars
WHERE brand = 'Volvo';
```

To erase all records from a table we can use two different statemnents

```sql
DELETE FROM cars;

/* o sino tambien */

TRUNCATE TABLE cars;
```

## SELECT

To retrieve data from a data base, we use the `SELECT` statement. The following will be getting everything from table _cars_.

```sql
SELECT * FROM cars;
```

If we want to be specific with the column

```sql
SELECT brand, year FROM cars;
```

## ALTER TABLE

To **add, delete, or modify** columns in an existing table, we have to use the `ALTER TABLE` statement. Is also used to add and drop various constraints on an existing table.

When adding columns we must also specify the data type of the column.

```sql
ALTER TABLE cars ADD color VARCHAR(255);
```

If We want to change the data type of the _year_ column of the _cars_ table from **INT** to **VARCHAR(4)**. To modify a column, use the `ALTER COLUMN` statement and the `TYPE` keyword followed by the new data type:

```sql
ALTER TABLE cars
ALTER COLUMN year TYPE VARCHAR(4);
```

> Note: Some data types cannot be converted if the column has value. E.g. numbers can always be converted to text, but text cannot always be converted to numbers.

To remove a column we use `DROP COLUMN`:

```SQL
ALTER TABLE cars
DROP COLUMN color;
```

The `UPDATE` statement is used to modify the value(s) in existing records in a table.

```sql
UPDATE cars
SET color = 'red'
WHERE brand = 'Volvo';
```

> Note: Be careful with the WHERE clause, in the example above ALL rows where brand = 'Volvo' gets updated. If you omit the WHERE clause, ALL records will be updated!

Update several columns at the same time

```sql
UPDATE cars
SET color = 'white', year = 1970
WHERE brand = 'Toyota';
```

## WHERE

We can operate with different operators in the WHERE clause:

`=` | Equal to
`<` | Less than
`>` | Greater than
`<=` | Less than or equal to
`>=` | Greater than or equal to
`<>` | Not equal to
`!=` | Not equal to
`LIKE` | Check if a value matches a pattern (case sensitive)
`ILIKE` | Check if a value matches a pattern (case insensitive)
`AND` | Logical AND
`OR` | Logical OR
`IN` | Check if a value matches any value within a provided list
`BETWEEN` | Check if a value is within a specified range
`IS NULL` | Check if a value is NULL
`NOT` | Makes a negative result e.g. NOT LIKE, NOT IN, NOT BETWEEN

# TBD>

- Diseño y normalización de bases de datos.
- Claves primarias y foráneas.
- Relaciones entre tablas.
- Índices y optimización de consultas.
- Transacciones y propiedades ACID.
- Integración de PostgreSQL con un backend en Node.js.
- Migraciones y herramientas ORM, como Prisma o Drizzle.
