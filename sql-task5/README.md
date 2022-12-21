# SQL PRACTICE

## SQL PRACTICE 5 - The Pieces

### Relational Schema
![148441901-0c23c4d6-931e-4d47-829f-2ee16eb92ee7](https://user-images.githubusercontent.com/90597917/208929072-5ea09842-b960-4784-a4b3-bced424598a8.png)

### Table creation code

```sql
CREATE TABLE Pieces
(
    Code INTEGER PRIMARY KEY NOT NULL,
    Name TEXT                NOT NULL
);

CREATE TABLE Providers
(
    Code TEXT PRIMARY KEY NOT NULL,
    Name TEXT             NOT NULL
);

CREATE TABLE Provides
(
    Piece    INTEGER
        CONSTRAINT fk_Pieces_Code REFERENCES Pieces (Code),
    Provider TEXT
        CONSTRAINT fk_Providers_Code REFERENCES Providers (Code),
    Price    INTEGER NOT NULL,
    PRIMARY KEY (Piece, Provider)
);
```

### Sample dataset
```sql
INSERT INTO Providers(Code, Name)
VALUES('HAL', 'Clarke Enterprises'),
      ('RBT', 'Susan Calvin Corp.'),
      ('TNBC', 'Skellington Supplies');

INSERT INTO Pieces(Code, Name)
VALUES(1, 'Sprocket'),
      (2, 'Screw'),
      (3, 'Nut'),
      (4, 'Bolt');

INSERT INTO Provides(Piece, Provider, Price)
VALUES(1, 'HAL', 10),
      (1, 'RBT', 15),
      (2, 'HAL', 20),
      (2, 'RBT', 15),
      (2, 'TNBC', 14),
      (3, 'RBT', 50),
      (3, 'TNBC', 45),
      (4, 'HAL', 5),
      (4, 'RBT', 7);
```

### Exercises

1. Select the name of all the pieces.
2. Select all the providers' data.
3. Obtain the average price of each piece (show only the piece code and the average price).
4. Obtain the names of all providers who supply piece 1.
5. Select the name of pieces provided by provider with code "HAL".
6. For each piece, find the most expensive offering of that piece and include the piece name, provider name, and price ( note that there could be two providers who supply the same piece at the most expensive price).
7. Add an entry to the database to indicate that "Skellington Supplies" (code "TNBC") will provide sprockets (code "1") for 7 cents each.
8. Increase all prices by one cent.
9. Update the database to reflect that "Susan Calvin Corp." (code "RBT") will not supply bolts (code 4).
10. Update the database to reflect that "Susan Calvin Corp." (code "RBT") will not supply any pieces (the provider should still remain in the database).

### Answers

```sql
1. SELECT name
   FROM pieces;

2. SELECT *
   FROM providers;

3. SELECT piece, AVG(price)
   FROM provides
   GROUP BY piece
   ORDER BY piece;

4. SELECT pr.name
   FROM providers pr
         INNER JOIN provides p on pr.code = p.provider
   WHERE p.piece = 1;

5. SELECT p.name
   FROM pieces p
         INNER JOIN provides p2 on p.code = p2.piece
   WHERE p2.provider = 'HAL';

6. SELECT pieces.name, providers.name, price
   FROM pieces
         INNER JOIN provides ON pieces.code = piece
         INNER JOIN providers ON providers.code = provider
   WHERE price =
      (
          SELECT MAX(price)
          FROM provides
          WHERE piece = pieces.code
      );

7. UPDATE provides
   SET price = 0.7
   WHERE provider = 'TNBC'
    AND piece = 1;

8. UPDATE provides
   SET price = price + 0.1;

9. DELETE
   FROM provides
   WHERE provider = 'RBT'
    AND piece = 4;
    
10. DELETE
    FROM Provides
    WHERE Provider = 'RBT';              
