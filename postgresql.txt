# Learn PostgreSQL Basics and Commands

## What is Database.
## What is SQL and Relational Database
## What is PostgreSQL AKA Postgres
PostgreSQL Installation
Setup PSQL (MacOS/Windows/Linux)
How to create Database
How to connect to Database
A Very dangerous command
How to create Tables
Creating tables with constraints
Insert into Example
Generate 1000 Rows with Mockaroo
Select from
Order by
Distinct
Where clause and AND
Comparison Operators
Limit, Offset & Fetch
IN
Between
Like and iLike
Group By 
Group By Having
Adding New table and Data Using Mockaroo
Calculating Min, Max & Average.
SUM
Basics of Arithmetic Operators.
Arithmetic Operators (ROUND)
Alias
Coalesce
NULLIF
Timestamps and Dates course
Adding and Subtracting With dates.
Extracting fields from Timestamp.
Age function
What are Primary Keys.
Understanding Primary Keys.
Adding Primary key.
Unique constraints.
Check Constraints
How to delete Records.
How to update Records.
On conflict Do nothing.
Upsert.
What is a relationship/foreign keys.
Adding relationship between tables.
Updating foreign keys columns.
Inner joins
Left joins
Deleting records with foreign keys.
Exporting query results to CSV.
Serial & Sequences.
Extensions
Understanding UUID Data Type.
UUID as Primary keys.



Create a DATABASE.

``` CREATE DATABASE test; ```

``` \l ```

Connect to a Database.
``` psql --help ```
Run this command billow from terminal out of psql terminal.
``` psql -h localhost -p 5432 -U user databaseName ```

``` \c databaseName ```

A dangerous command.
``` DROP DATABASE databaseName; ```

Create Table with postgres.
CREATE TABLE table_name (
	column_name + data type + constraint if any
	)
Create table without constraints.

CREATE TABLE person (
	id int,
	first_name VARCHAR(255),
	last_name VARCHAR(255),
	gender VARCHAR(10),
	date_of_birth TIMESTAMP,
	)

Create table with constraints.
``` CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	gender VARCHAR(5) NOT NULL,
	date_of_birth DATE NOT NULL
);
```

Insert records into tables.
INSERT INTO person (
	first_name,
	last_name,
	gender,
	date_of_birth) VALUES ('anne','smith','female',DATE '1988-01-09');
	
	Generate records with mockaroo.
	
	Select from
	Order by - orders columns by asc or desc.
	select * from person order by country_of_birth ASC | DESC;
	
	Distinct keyword removes duplicates to your query.
	SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;
	
	
Where clause and AND
we can use where clause to filter based on column.
select * from person where gender = 'male' AND (country_of_birth = 'poland' OR country_of_birth = 'china') AND last_name = 'Pietersma';

Comparison operators.
select 1 = 1;

Limit, Offset & Fetch.
select * from person LIMIT 5;
select * from person OFFSET 5 LIMIT 5;
offset and limit are the same thing but fetch is sql standard.

SELECT * FROM person OFFSET 5 FETCH FIRST 5 ROW ONLY.

IN
instead of this
select * from person where country_of_birth = 'china' OR country_of_birth = 'france' OR country_of_birth='Brazil';
do this
select * from person where country_of_birth IN ('china', 'Brazil', 'France')

BETWEEN - selects data from a range.
SELECT * FROM person WHERE date_of_birth BETWEEN DATA '2000-01-01' AND '2015-12-31';

LIKE and iLike
SELECT * FROM person WHERE email LIKE '%@google.%';
iLike ignores case sensitivity.
SELECT * FROM person WHERE country_of_birth iLIKE 'p%';

Group By
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth ORDER BY country_of_birth;

Group by Having
having keyword must be before order by right after group by.
select country_of_birth, count(*) from person group by country_of_birth having count(*) > 40 order by country_of_birth;

Adding New Table and data using mockaroo.

Calculating Min, Max & Average.
select MAX|MIN|AVG(price) FROM car;
select ROUND(AVG(price)) FROM car;

SELECT make,model, MIN(price) FROM car GROUP BY make, model;
SELECT make, SUM(price) FROM car GROUP BY make;

Basic of Arithmetic Operators
select 10 +2;

Arithmetic operators round
SELECT id, make, model, price, ROUND(price * .10, 2) FROM car;

Alias
select price as original_price;

Coalesce
coalesce keyword allows us to have default value incase the first one is not present.
SELECT email FROM person;
SELECT COALESCE(email, 'Email was not provided') FROM person;

NULLIF
SELECT COALESCE(10 / NULLIF(0,0), 0); 
this is how you can handle divided by zero.

Timestamps and dates.
SELECT NOW();

Adding and Subtracting with Dates.
SELECT NOW() - INTERVAL '1 YEAR';
SELECT NOW() - INTERVAL '10 MONTHS';
SELECT NOW() - INTERVAL '10 DAY';
SELECT NOW() + INTERVAL '10 YEARS';
SELECT NOW() + INTERVAL '10 MONTHS')::DATE;

Extracting Fields
SELECT EXTRACT(YEAR FROM NOW());
SELECT EXTRACT(MONTH|YEAR|DAY|DOW|CENTURY FROM NOW());

Age function
SELECT first_name, last_name, gender, country_of_birth, date_of_birth, AGE(NOW(), date_of_birth) AS Age FROM person;

Primary Keys;
Primary keys (PK) Uniquely identify a record in tables;

Understanding Primary keys.

remove primary keys constraint
ALTER TABLE person DROP CONSTRAINT person_pkey;
now you can add duplicate columns.

Adding primary key.
ALTER TABLE person add PRIMARY KEY(id); - we cannot add a primary key, when the rows are not unique in our table.
DELETE FROM person WHERE id = 1;
SELECT * FROM person WHERE id = 1
ALTER TABLE person add PRIMARY KEY(id);
\d person; to describe table.
remeber if you want to add a primary key you've to make sure the column that you want to be the primary key is unique in every row.

Unique constraint.
SELECT email, count(*) FROM person GROUP BY email;
SELECT email, count(*) FROM person GROUP BY email HAVING COUNT(*) > 1;

ALTER TABLE person ADD CONSTRAINT unique_email_address UNIQUE (email) 
DELTE FROM person WHERE id = 1002;

\d person;

remove unique constraint
ALTER TABLE person DROP CONSTRAINT unique_email_address;
other way of adding constraint.
ALTER TABLE person ADD UNIQUE (email); - this way the constraint name defined by postgres by default.
 
unique constraints allows us to have a unique value per column. is not the same as primary key because pk's job is to identify a record in a table.

Check constraints - Allows us to add a constraint based on boolean condition.
SELECT DISTINCT gender FROM person;
ALTER TABLE person ADD CONSTRAINT gender_constraint CHECK (gender = 'Female' OR gender = 'Male');

Delete Records.
DELTE FROM person - deletes all records on the table.
DELTE FROM person WHERE gender='female' AND country_of_birth = 'Poland'

Update Records
UPDATE person SET email = 'omar@gmail.com' WHERE id=1
UPDATE person SET first_name='omar', last_name = 'adan', email = 'omar.adan@hotmail.com' WHERE id = 2011

Onconflict do nothing.
INSERT INTO person(id, first_name, last_name, gender, email, date_of_birth, country_of_birth)
VALUES (2017, 'Russ', 'Ruddoch', 'Male', 'ruddoch07@hhs.gov', DATE '1952-09-25', 'Norway')
ON CONFLICT (id) DO NOTHING;
when you want to use ON CONFLICT make sure to use with unique constraint column.

Upsert
INSERT INTO person(id, first_name, last_name, gender, email, date_of_birth, country_of_birth)
VALUES (2017, 'Russ', 'Ruddoch', 'Male', 'ruddoch07@hhs.gov', DATE '1952-09-25', 'Norway')
ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email, 
last_name = EXCLUDED.last_name, first_name = EXCLUDED.first_name;
this is how we can do ON CONFLICT do update. this allows you to perform an update or insert hence the Upsert.
Upsert allows you override existing data if present otherwise inserts a new row.

Foreign Keys, Joins & Relationships.
Foreign keys is a column that references a primary key in another table.
in order for this to work the types have to be the same.
Foreign key syntax: car_id BIGINT REFERENCES car(id) UNIQUE (car_id)

Adding Relationships Between Tables.

CREATE TABLE car (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	make VARCHAR(100) NOT NULL,
	model VARCHAR (100) NOT NULL,
	price NUMERIC(19, 2) NOT NULL
);

CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	gender VARCHAR (7) NOT NULL,
	email VARCHAR (100),
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50) NOT NULL,
	car_id BIGINT REFERENCES car (id),
	UNIQUE(car_id)
);

Updating Foreign Keys Columns.
UPDATE person SET car_id = 1 WHERE id = 2;

Inner Joins
Inner joins is an effective way of actually combining two tables.
the way it works is that you table A and table B and what you want to do is to combine two tables.
inner join takes whatever is common in both tables.
if you've a record inside table A and also a record of table B if you've a foreign key which present in both tables,
then it takes those two records and gives you results of both which we call into c.

SELECT * FROM person
JOIN car ON person.car_id = car.id;
\x

LEFT JOINS.
LEFT JOIN includes all the rows from left table i.e table A as well as
the records from table B that have a corresponding relationship  and also the ones that don't have a corresponding relationship.
it returns all the records even if there's not a much. and then you get a result C.

SELECT * FROM person
LEFT JOIN car ON car.id = person.car_id;

basically means that you want join both tables including records which don't a foreign key relationship.

Deleting Records with Foreign Keys.

SELECT * FROM car WHERE id = 13;
UPDATE person SET car_id = 13 WHERE id = 9000;
DELETE FROM car WHERE id = 13; - this will work.
because whenever you try to delete individual records make sure that if there's a foreign key constraint you need to remove fk constraint before you perform the actual deletion.
if we want to delete the record first we need to remove the card_id foreign key from the person table, 
then we can savely remove the car record. or we can delete the person table record on the person for example omar record on the table
because there's no fk constraint b/w john and some other table. or we can update the car_id column to NULL.

Exporting Query results to CSV
\copy (SELECT * FROM person LEFT JOIN car ON car.id = person.car_id) TO '/PATH/TO/FILESYSTEM/filename.csv' DELIMETER ',' CSV HEADER;
 
SERIAL AND SEQUENCES.
SELECT * FROM person_id_seq;
SELECT nextval('person_id_seq'::regclass);
if you invoke this function it will increments to the next value.
and after that if you insert another record that record will take value after invoked the function.

change value of sequence.
ALTER SEQUENCE person_id_seq RESTART WITH 10;

Extensions.
Extensions are functions that can add extra functionality to your database
view all available extensions.
SELECT * FROM pg_available_extensions;

Understanding UUID Data Type.
global unique identifier its used your mac address and date time to generate global unique identifier for you.

to install it in postgresql.
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
SELECT * FROM pg_available_extensions; - view installed column.
\df
SELECT uuid_generate_v4();
its very good practice to use uuid as primary key in our tables.
one of the benefits of using uuid as keys is that it makes it very hard,
for attackers to try and mine our database. i.e if we had an API for our /users and user-id,
attackers could exploit all numbers and tries to update/delete information so on and so forth.
but with uuid its very very difficult for them to guess which person is in your database.
other benefit is that because its global unique that means you can migrate data accross databases
without any conflicts.

UUID As primary key.
CREATE TABLE person (
	person_uid UUID NOT NULL PRIMARY KEY,
	first_name VARCHAR(100) NOT NULL,
	last_name VARCHAR(100) NOT NULL,
	gender,
	email,
	dob,
	cob
	car_uid UUID REFERENCES car(car_uid),
	UNIQUE(car_id),
	UNIQUE(email)
);

CREATE TABLE car (
	car_uid UUID NOT NULL PRIMARY KEY,
	make VARCHAR(100) NOT NULL,
	model VARCHAR(100) NOT NULL,
	price NUMERIC(19, 2) NOT NULL CHECK (price > 0)
);

INSERT INTO person(person_uid, firstname, all_columns)
VALUES (uuid_generate_v4(), 'omar' 'blah-blah');

SELECT * FROM person LEFT JOIN car USING (car_uid);
SELECT * FROM person LEFT JOIN car USING (car_uid) 
	WHERE car.* IS NULL;
