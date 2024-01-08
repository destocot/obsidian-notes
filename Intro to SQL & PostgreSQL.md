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
```
\l
```
## List of tables
```
\d
```
## Create database
```
CREATE DATABASE <database_name>
```

## Connect database
```
\c <database_name>
```

## Create table
```
CREATE TABLE <table_name> (
	id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	title VARCHAR ( 255 ) UNIQUE NOT NULL
	... other columns
);
```

## Describe table
```
\d <table_name>
```

## Insert into table
```
INSERT INTO <table_name> (column_name1, column_name2, ...)
VALUES ('value1', 'value2', ...);
```

## Drop table
```
DROP TABLE <table_name>;
```

## Drop database
```
DROP DATABSE <database_name>;
```

## Alter table
```
ALTER TABLE <table_name> ADD COLUMN <column_name> <column_type>
ALTER TABLE <table_name> DROP COLUMN <column_name>
```

> String values use single quotes ('')

> Table names and column names have optional double quotes ( "" )

> Two single quotes '' is how you escape single quotes in postgres

## ==example== Insert into table with conflict
```
INSERT INTO ingredients ( title, image, type ) VALUES ( 'watermelon', 'banana.jpg', 'this won''t be updated' ) ON CONFLICT (title) DO UPDATE SET image = excluded.image;
```

## Update row in table
```
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon';

-- Update and return updated information

UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING id, title, image;

UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING *;
```

## Delete row in table
```
DELETE FROM <table_name>
WHERE <column_name> = <column_value>
RETURNING * -- optional
;
```

## Select row(s) from table
```
SELECT * FROM <table_name>;

SELECT <column_name1>, <column_name2>, ... FROM <table_name>;
```

## Limit row(s) from table
```
SELECT * FROM <table_name>
LIMIT <limit_number>;
```

## Offset row(s) from table
```
SELECT * FROM <table_name>
LIMIT <limit_number> -- optional
OFFSET <offset_number>;
```

> Remark: When deleting a row from a table, if you are using offset to paginate the results. A user might end up not seeing a row due to a deletion, and then changing to the next page.

## Offset row(s) from table with id
```
SELECT * FROM <table_name>
WHERE id > <id_value>
LIMIT <limit_number>;
```

## Filter row(s) from table with WHERE
```
SELECT * FROM <table_name>
WHERE <column_name> = <column_value>;
```

## ==example== Filter row(s) from table
```
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
```
SELECT * FROM <table_name> ORDER BY <column_name>;

-- descending order
SELECT * FROM <table_name> ORDER BY <column_name> DESC;
```

## Search with text
```
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
```
SELECT NOW();
```

## ==example== Match a specific amount of characters
```
-- match exactly one character
SELECT * FROM ingredientsWHERE title ILIKE 'ch_rry'
```


















### SQL
```
CREATE DATABASE recipeguru;

CREATE TABLE ingredients ( id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY, title VARCHAR ( 255 ) UNIQUE NOT NULL );

ALTER TABLE ingredients ADD COLUMN image VARCHAR ( 255 ), ADD COLUMN type VARCHAR ( 50 ) NOT NULL DEFAULT 'vegetable';

INSERT INTO "ingredients" ( "title", "image", "type" -- Notice the " here 
) 
VALUES ( 'broccoli', 'broccoli.jpg', 'vegetable' -- and the ' here
);

INSERT INTO ingredients ( title, image, type ) VALUES ( 'red pepper', 'red_pepper.jpg', 'vegetable' );

INSERT INTO ingredients ( title, image, type ) VALUES ( 'avocado', 'avocado.jpg', 'fruit' ), ( 'banana', 'banana.jpg', 'fruit' ), ( 'beef', 'beef.jpg', 'meat' ), ( 'black_pepper', 'black_pepper.jpg', 'other' ), ( 'blueberry', 'blueberry.jpg', 'fruit' ), ( 'broccoli', 'broccoli.jpg', 'vegetable' ), ( 'carrot', 'carrot.jpg', 'vegetable' ), ( 'cauliflower', 'cauliflower.jpg', 'vegetable' ), ( 'cherry', 'cherry.jpg', 'fruit' ), ( 'chicken', 'chicken.jpg', 'meat' ), ( 'corn', 'corn.jpg', 'vegetable' ), ( 'cucumber', 'cucumber.jpg', 'vegetable' ), ( 'eggplant', 'eggplant.jpg', 'vegetable' ), ( 'fish', 'fish.jpg', 'meat' ), ( 'flour', 'flour.jpg', 'other' ), ( 'ginger', 'ginger.jpg', 'other' ), ( 'green_bean', 'green_bean.jpg', 'vegetable' ), ( 'onion', 'onion.jpg', 'vegetable' ), ( 'orange', 'orange.jpg', 'fruit' ), ( 'pineapple', 'pineapple.jpg', 'fruit' ), ( 'potato', 'potato.jpg', 'vegetable' ), ( 'pumpkin', 'pumpkin.jpg', 'vegetable' ), ( 'raspberry', 'raspberry.jpg', 'fruit' ), ( 'red_pepper', 'red_pepper.jpg', 'vegetable' ), ( 'salt', 'salt.jpg', 'other' ), ( 'spinach', 'spinach.jpg', 'vegetable' ), ( 'strawberry', 'strawberry.jpg', 'fruit' ), ( 'sugar', 'sugar.jpg', 'other' ), ( 'tomato', 'tomato.jpg', 'vegetable' ), ( 'watermelon', 'watermelon.jpg', 'fruit' ) ON CONFLICT DO NOTHING;
```