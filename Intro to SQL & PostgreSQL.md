```
docker run -e POSTGRES_PASSWORD=lol --name=pg --rm -d -p 5432:5432 postgres:14
```

```
docker exec -u postgres -it pg psql
```

# SQL - Structured Query Language
---
# psql

## List of databases
```sql
\l
```
## List of tables
```sql
\d
```
## Create database
```sql
CREATE DATABASE <database_name>
```

## Connect database
```sql
\c <database_name>
```

## Create table
```sql
CREATE TABLE <table_name> (
	id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	title VARCHAR ( 255 ) UNIQUE NOT NULL
	... other columns
);
```

## Describe table
```sql
\d <table_name>
```

## Insert into table
```sql
INSERT INTO <table_name> (column_name1, column_name2, ...)
VALUES ('value1', 'value2', ...);
```

## Drop table
```sql
DROP TABLE <table_name>;
```

## Drop database
```sql
DROP DATABASE <database_name>;
```

## Alter table
```sql
ALTER TABLE <table_name> ADD COLUMN <column_name> <column_type>
ALTER TABLE <table_name> DROP COLUMN <column_name>
```

> String values use single quotes ('')

> Table names and column names have optional double quotes ( "" )

> Two single quotes '' is how you escape single quotes in postgres

## ==example== Insert into table with conflict
```sql
INSERT INTO ingredients ( title, image, type ) VALUES ( 'watermelon', 'banana.jpg', 'this won''t be updated' ) ON CONFLICT (title) DO UPDATE SET image = excluded.image;
```

## Update row in table
```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon';

-- Update and return updated information

UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING id, title, image;

UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING *;
```

## Delete row in table
```sql
DELETE FROM <table_name>
WHERE <column_name> = <column_value>
RETURNING * -- optional
;
```

## Select row(s) from table
```sql
SELECT * FROM <table_name>;

SELECT <column_name1>, <column_name2>, ... FROM <table_name>;
```

## Limit row(s) from table
```sql
SELECT * FROM <table_name>
LIMIT <limit_number>;
```

## Offset row(s) from table
```sql
SELECT * FROM <table_name>
LIMIT <limit_number> -- optional
OFFSET <offset_number>;
```

> Remark: When deleting a row from a table, if you are using offset to paginate the results. A user might end up not seeing a row due to a deletion, and then changing to the next page.

## Offset row(s) from table with id
```sql
SELECT * FROM <table_name>
WHERE id > <id_value>
LIMIT <limit_number>;
```

## Filter row(s) from table with WHERE
```sql
SELECT * FROM <table_name>
WHERE <column_name> = <column_value>;
```

## ==example== Filter row(s) from table
```sql
SELECT * FROM ingredients
WHERE type = 'vegetable'
	AND id < 20;

SELECT * FROM ingredients
WHERE id <= 10
	OR id >= 20;

-- <> represents not
SELECT * FROM ingredients
WHERE type <> 'fruit';
```

## Order row(s) in table
```sql
SELECT * FROM <table_name> ORDER BY <column_name>;

-- descending order
SELECT * FROM <table_name> ORDER BY <column_name> DESC;
```

## Search with text
```sql
SELECT * FROM <table_name> WHERE <column_name> LIKE '%<query>%'

-- insensitive case search
SELECT * FROM <table_name> WHERE <column_name> ILIKE '%<query>%'

-- search multiple columns
SELECT * FROM <table_name> 
WHERE CONCAT(<column_name1>, <column_name2>) 
ILIKE '%<query>%'
```

> %query ==> string ends in query
> query% ==> string starts with query
> %query% ==> query is anywhere in the string

## Get current time
```sql
SELECT NOW();
```

## ==example== Match a specific amount of characters
```sql
-- match exactly one character
SELECT * FROM ingredientsWHERE title ILIKE 'ch_rry'
```

# Relationships & Joins

# ONE-TO-MANY RELATIONSHIPS

## Naive Approach 1 - Using Separate Queries
### Select data from two tables using a common ID.
```sql
SELECT <col_1>, <col_2> FROM <table_1> WHERE <col_name> = <col_value>;
SELECT <col_1> FROM <table_2> WHERE <col_name> = <col_value>;
```

## ==example==
```sql
SELECT title, body FROM recipes WHERE recipe_id = 4;
SELECT url FROM recipes_photos WHERE recipe_id = 4;
```

## Naive Approach  2 - Using Single Query

## ==example==
```sql
SELECT recipe.title, recipe.body, photo.url
FROM recipes_photos photo
INNER JOIN recipes recipe
ON photo.recipe_id = recipe.recipe_id;
```

# Joins

![[SQL_Joins.jpg]]

# ==example== Include all recipes regardless if they have a photo
```sql
SELECT recipe.title, recipe.body, photo.url
FROM recipes recipe
LEFT OUTER JOIN recipes_photos photo
ON recipe.recipe_id = photo.recipe_id;
```

> the `OUTER` keyword is optional for LEFT | RIGHT | FULL joins

# ==example== Natural Join
- since both the recipes_photos and recipes table have the refence column recipe_id named the same, PostgreSQL is smart enough to figure out what to join on.
```sql
SELECT * FROM recipes_photos NATURAL JOIN recipes;
SELECT * FROM recipes_photos NATURAL LEFT JOIN recipes;
SELECT * FROM recipes_photos NATURAL RIGTH JOIN recipes;
```

# Cross Join
- CROSS JOIN allows you to make every permutation and combination of multiple tables.
```sql
SELECT r.title, r.body, rp.url
FROM recipes_photos rp
CROSS JOIN recipes r;
```

# Foreign Keys

- REFERENCES defines that the column is a foreign key, in this example recipes is the table and recipe_id is the name of the column it'll match.
- `ON DELETE CASCADE` if the other row gets deleted than delete this one as well.
```sql
CREATE TABLE recipes (
  recipe_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  title VARCHAR ( 255 ) UNIQUE NOT NULL,
  body TEXT
);

CREATE TABLE recipes_photos (
  photo_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  url VARCHAR(255) NOT NULL,
  recipe_id INTEGER REFERENCES recipes(recipe_id) ON DELETE CASCADE
);
```

# `ON` Constraints
```sql
ON DELETE CASCADE
ON DELETE SET NULL
ON DELETE NO ACTION
ON UPDATE ...
```

> Foreign Keys also prevent `INSERTS` that violate the constraint

# MANY-TO-MANY RELATIONSHIPS























---

### SQL (Seed Data)

```sql
CREATE DATABASE recipeguru;

CREATE TABLE ingredients ( id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY, title VARCHAR ( 255 ) UNIQUE NOT NULL );

ALTER TABLE ingredients ADD COLUMN image VARCHAR ( 255 ), ADD COLUMN type VARCHAR ( 50 ) NOT NULL DEFAULT 'vegetable';

INSERT INTO "ingredients" ( "title", "image", "type" -- Notice the " here 
) 
VALUES ( 'broccoli', 'broccoli.jpg', 'vegetable' -- and the ' here
);

INSERT INTO ingredients ( title, image, type ) VALUES ( 'red pepper', 'red_pepper.jpg', 'vegetable' );

INSERT INTO ingredients ( title, image, type ) VALUES ( 'avocado', 'avocado.jpg', 'fruit' ), ( 'banana', 'banana.jpg', 'fruit' ), ( 'beef', 'beef.jpg', 'meat' ), ( 'black_pepper', 'black_pepper.jpg', 'other' ), ( 'blueberry', 'blueberry.jpg', 'fruit' ), ( 'broccoli', 'broccoli.jpg', 'vegetable' ), ( 'carrot', 'carrot.jpg', 'vegetable' ), ( 'cauliflower', 'cauliflower.jpg', 'vegetable' ), ( 'cherry', 'cherry.jpg', 'fruit' ), ( 'chicken', 'chicken.jpg', 'meat' ), ( 'corn', 'corn.jpg', 'vegetable' ), ( 'cucumber', 'cucumber.jpg', 'vegetable' ), ( 'eggplant', 'eggplant.jpg', 'vegetable' ), ( 'fish', 'fish.jpg', 'meat' ), ( 'flour', 'flour.jpg', 'other' ), ( 'ginger', 'ginger.jpg', 'other' ), ( 'green_bean', 'green_bean.jpg', 'vegetable' ), ( 'onion', 'onion.jpg', 'vegetable' ), ( 'orange', 'orange.jpg', 'fruit' ), ( 'pineapple', 'pineapple.jpg', 'fruit' ), ( 'potato', 'potato.jpg', 'vegetable' ), ( 'pumpkin', 'pumpkin.jpg', 'vegetable' ), ( 'raspberry', 'raspberry.jpg', 'fruit' ), ( 'red_pepper', 'red_pepper.jpg', 'vegetable' ), ( 'salt', 'salt.jpg', 'other' ), ( 'spinach', 'spinach.jpg', 'vegetable' ), ( 'strawberry', 'strawberry.jpg', 'fruit' ), ( 'sugar', 'sugar.jpg', 'other' ), ( 'tomato', 'tomato.jpg', 'vegetable' ), ( 'watermelon', 'watermelon.jpg', 'fruit' ) ON CONFLICT DO NOTHING;

-- recipes table

DROP TABLE IF EXISTS recipes;
DROP TABLE IF EXISTS recipes_photos;
CREATE TABLE recipes (
  recipe_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  title VARCHAR ( 255 ) UNIQUE NOT NULL,
  body TEXT
);
INSERT INTO recipes
  (title, body)
VALUES
  ('cookies', 'very yummy'),
  ('empanada','ugh so good'),
  ('jollof rice', 'spectacular'),
  ('shakshuka','absolutely wonderful'),
  ('khachapuri', 'breakfast perfection'),
  ('xiao long bao', 'god I want some dumplings right now');

-- recipes_photos table

CREATE TABLE recipes_photos (
  photo_id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  url VARCHAR(255) NOT NULL,
  recipe_id INTEGER REFERENCES recipes(recipe_id) ON DELETE CASCADE
);

INSERT INTO recipes_photos
  (recipe_id, url)
VALUES
  (1, 'cookies1.jpg'),
  (1, 'cookies2.jpg'),
  (1, 'cookies3.jpg'),
  (1, 'cookies4.jpg'),
  (1, 'cookies5.jpg'),
  (2, 'empanada1.jpg'),
  (2, 'empanada2.jpg'),
  (3, 'jollof1.jpg'),
  (4, 'shakshuka1.jpg'),
  (4, 'shakshuka2.jpg'),
  (4, 'shakshuka3.jpg'),
  (5, 'khachapuri1.jpg'),
  (5, 'khachapuri2.jpg');
-- no pictures of xiao long bao


```

