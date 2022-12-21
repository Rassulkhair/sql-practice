# SQL PRACTICE

## SQL PRACTICE 4 - The Movies

### Relational Schema
![148116011-42c028cd-3093-4279-9618-9ecd18ef07f8](https://user-images.githubusercontent.com/90597917/208927634-a6c824cd-8ab8-48c1-bd46-d4a6c2de59dd.png)

### Table creation code

```sql
CREATE TABLE Movies
(
    Code   INTEGER PRIMARY KEY NOT NULL,
    Title  TEXT                NOT NULL,
    Rating TEXT
);

CREATE TABLE MovieTheaters
(
    Code  INTEGER PRIMARY KEY NOT NULL,
    Name  TEXT                NOT NULL,
    Movie INTEGER
        CONSTRAINT fk_Movies_Code REFERENCES Movies (Code)
);
```

### Sample dataset
```sql

INSERT INTO Movies(Code, Title, Rating)
VALUES(9, 'Citizen King', 'G'),
      (1, 'Citizen Kane', 'PG'),
      (2, 'Singin'' in the Rain', 'G'),
      (3, 'The Wizard of Oz', 'G'),
      (4, 'The Quiet Man', NULL),
      (5, 'North by Northwest', NULL),
      (6, 'The Last Tango in Paris', 'NC-17'),
      (7, 'Some Like it Hot', 'PG-13'),
      (8, 'A Night at the Opera', NULL);

INSERT INTO MovieTheaters(Code, Name, Movie)
VALUES(1, 'Odeon', 5),
      (2, 'Imperial', 1),
      (3, 'Majestic', NULL),
      (4, 'Royale', 6),
      (5, 'Paraiso', 3),
      (6, 'Nickelodeon', NULL);
      
  ```
   ### Exercises
   
1. Select the title of all movies.
2. Show all the distinct ratings in the database.
3. Show all unrated movies.
4. Select all movie theaters that are not currently showing a movie.
5. Select all data from all movie theaters and, additionally, the data from the movie that is being shown in the theater (if one is being shown).
6. Select all data from all movies and, if that movie is being shown in a theater, show the data from the theater.
7. Show the titles of movies not currently being shown in any theaters.
8. Add the unrated movie "One, Two, Three".
9. Set the rating of all unrated movies to "G".
10. Remove movie theaters projecting movies rated "NC-17".

### Answers

```sql
1. SELECT title
   FROM movies;

2. SELECT DISTINCT rating
   FROM movies;

3. SELECT title, rating
   FROM movies
   WHERE rating IS NULL;

4. SELECT name
   FROM movietheaters
   WHERE movie IS NULL;

5. SELECT *
   FROM movietheaters mt
         LEFT JOIN movies m on m.code = mt.movie;

6. SELECT *
   FROM movies m
         RIGHT JOIN movietheaters mt on m.code = mt.movie;

7. SELECT m.title
   FROM movies m
         RIGHT JOIN movietheaters mt on m.code = mt.movie
   WHERE mt.movie IS NULL;

8. INSERT INTO movies(code, title, rating)
   VALUES (10, 'One, Two, Three', NULL);

9. UPDATE movies
   SET rating = 'G'
   WHERE rating IS NULL;
    
10. DELETE
    FROM movietheaters
    WHERE movie IN (SELECT code FROM movies WHERE rating = 'NC-17');               

