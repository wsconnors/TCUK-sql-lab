# TCUK-SQL

The following lab is to be completed using the H2 Console. 

To get to the console, start your application like any normal Spring app, and go to
`localhost:8080/console` in a browser.  Your defaults should be fine, but just to be safe the following are the settings
you should use for the login:
* Driver Class:`org.h2.Driver`
* JDBC URL:`jdbc:h2:mem:testdb`
* User Name:`sa`
* Password:{leave blank}

Once you have figured out the correct queries for each step, 
save a copy in a file called `src/main/resources/script.sql`. This will be how you submit this assignment. 
If at any time you need to reset the database, you can restart your Spring Boot server.

## Guided Walkthrough

Proceed through the sections below, testing out the queries shown and observing the result. 
Feel free to experiment with your own variations on these queries. 
This will help to build your familiarity with the process of working with SQL queries.

#### Insert people into People table

```SQL
INSERT INTO people (last_name, first_name, mobile, birthday)
VALUES ('Smith', 'John', '230-4293', '1973-01-23');
```

After doing that insert, go ahead and run all the statements in the `data-h2.sql` file.  That should build out your
database enough to get started.  Feel free to add more people, though.

My suggestion for the following stuff is, in the H2 console, leave the first line as a `SELECT *` from whatever tables
you're querying.  Then, after that, run your queries.  That way you can compare the results of your query with the 
data in the table.

#### Selecting all rows from table 

```SQL
SELECT * FROM people;
```

#### Updating rows

Update firstname for person whose id is 1

```SQL
UPDATE people SET first_name = 'Tony' WHERE id = 1;
```

Update mobile where last names are Smith

```SQL
UPDATE people SET mobile = '152-9854' WHERE last_name = 'Smith';
```

Update multiple columns with multiple conditions

```SQL
UPDATE people SET birthday = '1955-01-25' 
WHERE last_name = 'Smith' 
AND id = 4;
```

```SQL
UPDATE people SET mobile = '333-3333', last_name = 'Johnson' 
WHERE first_name = 'Noelle' OR first_name = 'Raj';
```

#### Basic Functions

```SQL
SELECT * FROM people;
```

```SQL
SELECT COUNT(homenumber) FROM homes;
```

```SQL
SELECT homenumber FROM homes WHERE id = 1;
```

```SQL
SELECT COUNT(*) FROM homes;
```

```SQL
SELECT COUNT(DISTINCT last_name) FROM people;
```

```SQL
SELECT  SUM(id), AVG(id) FROM people;
```

```SQL
SELECT SUM(id) AS sum, AVG(id) AS avg FROM people;
```
Notice the difference in the returned table with the aliases?

```SQL
SELECT MIN(birthday) FROM people;
```

## Strings

```SQL
SELECT UPPER (first_name), LOWER(last_name) FROM people;
```

```SQL
SELECT REPLACE(last_name, 'a', '1') FROM people;
```

```SQL
SELECT last_name FROM people;
```

```SQL
INSERT INTO people (first_name, last_name, mobile) 
VALUES ('Otto', 'Von Count', '656-6548');
```

```SQL
SELECT CONCAT(first_name, last_name) FROM people
WHERE last_name = 'Smith';
```

```SQL
SELECT CONCAT(first_name, ' ', last_name) 
FROM people 
WHERE last_name = 'Smith';
```

```SQL
SELECT CONCAT_WS(' ',first_name, last_name, mobile) 
FROM people WHERE last_name= 'Smith';
```

```SQL
SELECT homenumber, LEFT(homenumber, 3), RIGHT(homenumber, 2) FROM homes;
```

```SQL
SELECT LENGTH(address), CHAR_LENGTH(address) FROM homes;
```

## Compare

```SQL
SELECT first_name, last_name, YEAR(birthday) FROM people WHERE birthday >= '1970-07-06' AND birthday<='1987-07-06';
```

```SQL
SELECT first_name, birthday FROM people WHERE first_name='Thomas' OR first_name='Raj' OR first_name='Sheeri';
```

```SQL
SELECT first_name, birthday FROM people WHERE first_name IN ('Noelle', 'Thomas', 'Raj');
```

#### Wild Cards

```SQL
SELECT first_name FROM people WHERE RIGHT(first_name,1)='e';
```

```SQL
SELECT first_name FROM people WHERE first_name LIKE '%j'; 
```

```SQL
SELECT first_name FROM people WHERE first_name LIKE '%o%';
```

```SQL
SELECT first_name FROM people WHERE first_name NOT LIKE '%o%';
```

```SQL
SELECT COUNT(*) FROM people;
```

```SQL
SELECT last_name, COUNT(*) FROM people GROUP BY last_name;
```

```SQL
SELECT last_name, GROUP_CONCAT(mobile) FROM people GROUP BY last_name;
```

```SQL
SELECT last_name, GROUP_CONCAT(mobile SEPARATOR ' and ') FROM people GROUP BY last_name;
```

```SQL
SELECT last_name, GROUP_CONCAT(mobile SEPARATOR ' and ') FROM people GROUP BY last_name  HAVING COUNT(*)>1;
```

```SQL
SELECT last_name, GROUP_CONCAT(mobile SEPARATOR ' and ') FROM people WHERE last_name != 'Cabral' GROUP BY last_name  HAVING COUNT(*)>1;
```

#### Sorting 

```SQL
SELECT first_name, birthday FROM people ORDER BY birthday;
```

```SQL
SELECT first_name, birthday FROM people ORDER BY birthday DESC;
```

```SQL
SELECT first_name, last_name FROM people ORDER BY last_name, first_name;
```

```SQL
SELECT first_name, birthday FROM people ORDER BY birthday DESC LIMIT 3;
```

```SQL
SELECT first_name, MONTHNAME(birthday) as mon, birthday FROM people ORDER BY MONTH(birthday);
```

```SQL
SELECT last_name, COUNT(*) FROM  people GROUP BY last_name;
```

```SQL
SELECT last_name, COUNT(*) FROM  people GROUP BY last_name ORDER BY NULL;
```

## Inserting and Replacing Records

```SQL
INSERT INTO people (first_name, last_name, birthday, home_id)
VALUES ('John', 'Smith', '1998-04-07', 4),
('Maya', 'Wasserman' , NULL, 4),
('Paul', 'Thompson', '1996-05-27', 1);
```

#### Deleting

```SQL
DELETE FROM people WHERE first_name='Maya';
```

```SQL
SELECT * FROM people;
```

## JOIN

```SQL
INSERT INTO people (first_name, last_name, birthday)
VALUES ('Eli', 'Kramer', '1984-01-15');
```
	
	
```SQL
SELECT * FROM people;
```

```SQL
SELECT * FROM homes;
```

```SQL
SELECT p.first_name, h.address 
FROM people p
INNER JOIN homes h on (p.home_id = h.id);
```

```SQL
SELECT first_name, last_name
FROM people p
INNER JOIN homes h on (p.home_id = h.id)
WHERE p.HOME_ID = 1;
```

```SQL
SELECT p.*, h.address, h.homenumber
FROM people p
INNER JOIN homes h  on (p.home_id = h.id)
WHERE p.first_name  LIKE '%e%';
```

##### Exercise:

Devise a report:

Show all the people in your address table, only if you know their birthday.

Show their name, address and birthday ordered by birthday month, so January birthdays are first.

Devise a report:

Output all information for all people and their home information


### Mini Movie Database

NOTE:
The schema for all the tables is in `src/main/resources/schema-h2.sql` and can be viewed from the H2 console.
Don't submit any changes to the schema file.  And, in all reality, you shouldn't be doing anything in there outside of
experimenting for your own curiosity.

Add the following movies to the `movies` table using an insert statement:

| Title | Runtime | Genre | IMDB Score | Rating |
| ----- | ------- | ----- | ----------- | ----- |
| Howard the Duck | 110 | Sci-Fi | 4.6 | PG |
| Lavalantula | 83 | Horror | 4.7 | TV-14 |
| Starship Troopers | 129 | Sci-Fi | 7.2 | PG-13 |
| Waltz With Bashir | 90 | Documentary | 8.0 | R |
| Spaceballs | 96 | Comedy | 7.1 | PG |
| Monsters Inc. | 92 | Animation | 8.1 | G |

Add a few more movies of your choosing.

Create a query to find all movies in the Sci-Fi genre.

Create a query to find all films that scored at least a 6.5 on IMDB

For parents who have young kids, but who don't want to sit through long children's movies, 
create a query to find all of the movies rated G or PG that are less than 100 minutes long.

Create a query to show the average runtimes of movies scoring below a 7.5 on imdb, grouped by their respective genres.

There's been a data entry mistake; Starship Troopers is actually rated R, not PG-13. 
Create a query that finds the movie by its title and changes its rating to R.

Show the ID number and rating of all of the Horror and Documentary movies in the database. Do this in only one query.

This time let's find the average, maximum, and minimum IMDB score for movies of each rating.

That last query isn't very informative for ratings that only have 1 entry. 
Use a `HAVING COUNT(*) > 1` clause to only show ratings with multiple movies showing.

Let's make our movie list more child-friendly. Delete all entries that have a rating of R. 
Remember to record your query in `script.sql`.