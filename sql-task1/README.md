# SQL PRACTICE
### SQL Practice №1 | The Manufacturers
Relational Schema
![sql1](https://user-images.githubusercontent.com/90597917/206885813-21838f36-a43e-4679-bf06-56b2047c8d36.png)
### Table creation code
```sql
CREATE TABLE Manufacturers
(
    Code INTEGER PRIMARY KEY NOT NULL,
    Name CHAR(50)            NOT NULL
);

CREATE TABLE Products
(
    Code         INTEGER PRIMARY KEY NOT NULL,
    Name         CHAR(50)            NOT NULL,
    Price        REAL                NOT NULL,
    Manufacturer INTEGER             NOT NULL
        CONSTRAINT fk_Manufacturers_Code REFERENCES Manufacturers (Code)
);
```

### Sample dataset
```sql
INSERT INTO Manufacturers(Code, Name)
VALUES(1, 'Sony'),
      (2, 'Creative Labs'),
      (3, 'Hewlett-Packard'),
      (4, 'Iomega'),
      (5, 'Fujitsu'),
      (6, 'Winchester'),
      (7, 'Bose');

INSERT INTO Products(Code, Name, Price, Manufacturer)
VALUES(1, 'Hard drive', 240, 5),
      (2, 'Memory', 120, 6),
      (3, 'ZIP drive', 150, 4),
      (4, 'Floppy disk', 5, 6),
      (5, 'Monitor', 240, 1),
      (6, 'DVD drive', 180, 2),
      (7, 'CD drive', 90, 2),
      (8, 'Printer', 270, 3),
      (9, 'Toner cartridge', 66, 3),
      (10, 'DVD burner', 180, 2);
 ```
### Exercises
1. Select the names of all the products in the store.

2. Select the names and the prices of all the products in the store.

3. Select the name of the products with a price less than or equal to $200.

4. Select all the products with a price between $60 and $120.

5. Select the name and price in cents (i.e., the price must be multiplied by 100).

6. Compute the average price of all the products.

7. Compute the average price of all products with manufacturer code equal to 2.

8. Compute the number of products with a price larger than or equal to $180.

9. Select the name and price of all products with a price larger than or equal to $180, and sort first by price (in descending order), and then by name (in ascending order).

10. Select all the data from the products, including all the data for each product's manufacturer.

11. Select the product name, price, and manufacturer name of all the products.

12. Select the average price of each manufacturer's products, showing only the manufacturer's code.

13. Select the average price of each manufacturer's products, showing the manufacturer's name.

14. Select the names of manufacturer whose products have an average price larger than or equal to $150.

15. Select the name and price of the cheapest product.

16. Select the name of each manufacturer along with the name and price of its most expensive product.

17. Select the name of each manufacturer which have an average price above $145 and contain at least 2 different products.

18. Add a new product: Loudspeakers, $70, manufacturer 2.

19. Update the name of product 8 to "Laser Printer".

20. Apply a 10% discount to all products.

21. Apply a 10% discount to all products with a price larger than or equal to $120.

### Answers
```sql
1. SELECT name
   FROM products;

2. SELECT name, price
   FROM products;

3. SELECT name
   FROM products
   WHERE price <= 200;

4. SELECT *
   FROM products
   WHERE price BETWEEN 60 AND 120;

5. SELECT name, price * 100 AS priceCents
   FROM products;

6. SELECT AVG(price)
   FROM products;

7. SELECT AVG(price)
   FROM products
   WHERE manufacturer = 2;

8. SELECT COUNT(*) AS count
   FROM products
   WHERE price >= 180;

9. SELECT name, price
   FROM products
   WHERE price >= 180
   ORDER BY price DESC, name;
    
10. SELECT *
    FROM products p,
    manufacturers m
    WHERE p.manufacturer = m.code;
   
11. SELECT p.name, p.price, m.name
    FROM products p
         INNER JOIN manufacturers m on m.code = p.manufacturer;

12. SELECT AVG(Price), Manufacturer
    FROM Products
    GROUP BY Manufacturer;

13. SELECT AVG(Price), Manufacturers.Name
    FROM Products
         INNER JOIN Manufacturers
                    ON Products.Manufacturer = Manufacturers.Code
    GROUP BY Manufacturers.Name;

14. SELECT AVG(Price), Manufacturers.Name
    FROM Products
         INNER JOIN Manufacturers
                    ON Products.Manufacturer = Manufacturers.Code
    GROUP BY Manufacturers.Name
    HAVING AVG(Price) >= 150;

15. SELECT name, price
    FROM products
    WHERE price = (SELECT MIN(price) FROM products);

16. SELECT m.name, p.name, p.price
    FROM manufacturers m
         INNER JOIN products p on m.code = p.manufacturer
    WHERE price = (SELECT MAX(price) FROM products);
    
17. Select m.name,
           AVG(p.price),
           COUNT(p.manufacturer)
    FROM manufacturers m,
         products p
    WHERE p.manufacturer = m.code
    GROUP BY p.manufacturer
    HAVING AVG(p.price) >= 150
    AND COUNT(p.Manufacturer) >= 2;
    
18. INSERT INTO products(code, name, price, manufacturer)
    VALUES (11, 'Loudspeakers', 70, 2);
    
19. UPDATE products
    SET name = 'Laser Printer'
    WHERE code = 8;
    
20. UPDATE Products
    SET price = price - (price * 0.1);
    
21. UPDATE products
    SET price = price - (price * 0.1)
    WHERE price >= 120;   
