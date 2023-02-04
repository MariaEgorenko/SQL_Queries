# Examples of several SQL-queries
### Create table:
```SQL
CREATE TABLE book(
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(50),
    author VARCHAR(30),
    price DECIMAL(8, 2),
    amount INT
);
```
### Result:
```
Affected rows: 0
```
___
### Fill in the table:
```SQL
INSERT INTO book (title, author, price, amount)
VALUES
    ('Мастер и Маргарита', 'Булгаков М.А.', 670.99, 3),
    ('Белая гвардия', 'Булгаков М.А.', 540.50, 5),
    ('Идиот', 'Достоевский Ф.М.', 460.00, 10),
    ('Братья Карамазовы', 'Достоевский Ф.М.', 799.01, 2),
    ('Стихотворения и поэмы', 'Есенин С.А.', 650.00, 15);
```
### Result:
```
Affected rows: 5
```
### Table *book*:
| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |
---
### Output from the table all the information about the books, and for each item calculate its cost:
```SQL
SELECT title, author, price, amount, price * amount AS total 
FROM book;
```
### Result:

| title                 | author           | price  | amount | total   | 
|-----------------------|------------------|--------|--------|---------| 
| Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      | 2012.97 | 
| Белая гвардия         | Булгаков М.А.    | 540.50 | 5      | 2702.50 | 
| Идиот                 | Достоевский Ф.М. | 460.00 | 10     | 4600.00 | 
| Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 2      | 1598.02 | 
| Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     | 9750.00 |
___
### Write an SQL query that chooses titles, authors, quantities and calculates new book prices from the *book* table (30% reduction in price):
```SQL
SELECT title, author, amount, ROUND(price * 0.7, 2) as new_price
FROM book;
```
### Result:
| title                 | author           | amount | new_price |
|-----------------------|------------------|--------|-----------|
| Мастер и Маргарита    | Булгаков М.А.    | 3      | 469.69    |
| Белая гвардия         | Булгаков М.А.    | 5      | 378.35    |
| Идиот                 | Достоевский Ф.М. | 10     | 322.00    |
| Братья Карамазовы     | Достоевский Ф.М. | 2      | 559.31    |
| Стихотворения и поэмы | Есенин С.А.      | 15     | 455.00    |
___
### Output the title, author, price, and number of all books whose price is less than 500 or more than 600, and the cost of all copies of these books is greater than or equal to 5000:
```SQL
SELECT title, author, price, amount
FROM book
WHERE (price < 500 OR price > 600)
    AND (price * amount >= 5000);
```
### Result:
| title                 | author      | price  | amount |
|-----------------------|-------------|--------|--------|
| Стихотворения и поэмы | Есенин С.А. | 650.00 | 15     |
___
### Output the title and author of those books whose title consists of two or more words, and the author's initials contain the letter "С". Sort the information by book title in alphabetical order:
```SQL
SELECT title, author
FROM book
WHERE title LIKE '%_ _%'
    AND (author LIKE '%С.%')
ORDER BY 1;
```
Result:
| title                 | author      |
|-----------------------|-------------|
| Капитанская дочка     | Пушкин А.С. |
| Стихотворения и поэмы | Есенин С.А. |
___
### Updated table *book*:
```SQL
INSERT INTO book (title, author, price, amount)
VALUES ('Игрок', 'Достоевский Ф.М.', 480.50, 10);
```
### Result:
```
Affected rows: 1
```
| book_id | title                 | author           | price  | amount |
|---------|-----------------------|------------------|--------|--------|
| 1       | Мастер и Маргарита    | Булгаков М.А.    | 670.99 | 3      |
| 2       | Белая гвардия         | Булгаков М.А.    | 540.50 | 5      |
| 3       | Идиот                 | Достоевский Ф.М. | 460.00 | 10     |
| 4       | Братья Карамазовы     | Достоевский Ф.М. | 799.01 | 3      |
| 5       | Стихотворения и поэмы | Есенин С.А.      | 650.00 | 15     |
| 6       | Игрок                 | Достоевский Ф.М. | 480.50 | 10     |

___
### Find the minimum and maximum prices of the books of all authors whose total cost of books is more than 5000. Output the result in descending order of the minimum price:
```SQL
SELECT author,
    MIN(price) AS min_price, 
    MAX(price) AS max_price
FROM book
GROUP BY author
HAVING SUM(price * amount) > 5000 
ORDER BY min_price DESC;
```
### Result:
| author           | min_price        |  max_price        |
|------------------|------------------|-------------------|
| Есенин С.А.      | 650.00           | 650.00            |
| Достоевский Ф.М. | 460.00           | 799.01            |
___
### Calculate the cost of all copies of each author excluding the books "Идиот" and "Белая гвардия". Include in the result only those authors who have the total cost of books more than 5000. Sort the result in descending order of cost:
```SQL
SELECT author, SUM(price * amount) AS cost
FROM  book
WHERE title <> 'Идиот' AND title <> 'Белая гвардия'
GROUP BY author 
HAVING SUM(price * amount) > 5000
ORDER BY 2 DESC
```
Result:
| author           | cost      |
|------------------|-----------|
| Есенин С.А.      | 9750.00   |
| Достоевский Ф.М. | 7202.03   |
___
### Output the information about the books of those authors, whose total number of copies of books is not less than 12:
```SQL
SELECT title, author, amount, price
FROM book
WHERE author IN (
        SELECT author 
        FROM book 
        GROUP BY author 
        HAVING SUM(amount) >= 12
      );
```
### Result:
| title                 | author           | amount | price  |
|-----------------------|------------------|--------|--------|
| Идиот                 | Достоевский Ф.М. | 10     | 460.00 |
| Братья Карамазовы     | Достоевский Ф.М. | 3      | 799.01 |
| Игрок                 | Достоевский Ф.М. | 10     | 480.50 |
| Стихотворения и поэмы | Есенин С.А.      | 15     | 650.00 |
___
