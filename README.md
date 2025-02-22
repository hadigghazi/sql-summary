# SQL Summary

This repository provides a comprehensive SQL summary with examples based on a simple library management system database.

## Database Structure

### Tables

- `books`
  - `book_id`: INT, PRIMARY KEY
  - `title`: VARCHAR
  - `author_id`: INT, FOREIGN KEY
  - `year_published`: INT

- `authors`
  - `author_id`: INT, PRIMARY KEY
  - `name`: VARCHAR

- `borrowers`
  - `borrower_id`: INT, PRIMARY KEY
  - `name`: VARCHAR
  - `book_id`: INT, FOREIGN KEY

## Basic SQL Commands

### SELECT

Retrieve data from the `books` table:

```sql
SELECT title, year_published FROM books;
```

### INSERT

Insert a new record into the `books` table:

```sql
INSERT INTO books (book_id, title, author_id, year_published) VALUES (1, 'book1', 1, 2005);
```

### UPDATE

Modify an existing record in the `books` table:

```sql
UPDATE books SET year_published = 2006 WHERE book_id = 1;
```

### DELETE

Delete a record from the `books` table:

```sql
DELETE FROM books WHERE book_id = 1;
```

### CREATE TABLE

Create a new `books` table:

```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(255),
    author_id INT,
    year_published INT,
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
);
```

### ALTER TABLE

Add a new column to the `books` table:

```sql
ALTER TABLE books ADD genre VARCHAR(255);
```

### DROP TABLE

Delete the `books` table:

```sql
DROP TABLE books;
```

### SELECT with Condition

Retrieve data from the `books` table where the year published is after 2000:

```sql
SELECT title FROM books WHERE year_published > 2000;
```

### INSERT Multiple Records

Insert multiple records into the `authors` table:

```sql
INSERT INTO authors (author_id, name) VALUES (1, 'writer1'), (2, 'writer2');
```

### UPDATE Multiple Records

Modify the author name in the `authors` table:

```sql
UPDATE authors SET name = 'writer3' WHERE author_id = 2;
```

### DELETE with Condition

Delete records from the `borrowers` table where the borrower name is 'borrower1':

```sql
DELETE FROM borrowers WHERE name = 'borrower1';
```

## Constraints

### PRIMARY KEY

Define a primary key in the `books` table:

```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(255),
    author_id INT,
    year_published INT
);
```

### FOREIGN KEY

Define a foreign key in the `borrowers` table:

```sql
CREATE TABLE borrowers (
    borrower_id INT PRIMARY KEY,
    name VARCHAR(255),
    book_id INT,
    FOREIGN KEY (book_id) REFERENCES books(book_id)
);
```

### CHECK

Ensure values meet a specific condition in the `books` table:

```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(255),
    author_id INT,
    year_published INT CHECK (year_published > 1900)
);
```

## Joins

#### `books` Table

| book_id | title                | author_id | year_published |
|---------|-----------------------|-----------|----------------|
| 1       | The Great Gatsby      | 1         | 1925           |
| 2       | 1984                  | 2         | 1949           |
| 3       | To Kill a Mockingbird | 3         | 1960           |
| 4       | The Catcher in the Rye| NULL      | 1951           |

#### `authors` Table

| author_id | name                |
|-----------|---------------------|
| 1         | F. Scott Fitzgerald |
| 2         | George Orwell       |
| 3         | Harper Lee          |
| 4         | J.D. Salinger       |

### INNER JOIN

An **INNER JOIN** returns only the rows that have matching values in both tables.

```sql
SELECT books.title, authors.name 
FROM books 
INNER JOIN authors ON books.author_id = authors.author_id;
```

#### Result

| title                | name                |
|----------------------|---------------------|
| The Great Gatsby     | F. Scott Fitzgerald |
| 1984                 | George Orwell       |
| To Kill a Mockingbird| Harper Lee          |

### LEFT JOIN

A **LEFT JOIN** returns all records from the left table (books), and the matched records from the right table (authors). The result is NULL from the right side if there is no match.

```sql
SELECT books.title, authors.name 
FROM books 
LEFT JOIN authors ON books.author_id = authors.author_id;
```

#### Result

| title                | name                |
|----------------------|---------------------|
| The Great Gatsby     | F. Scott Fitzgerald |
| 1984                 | George Orwell       |
| To Kill a Mockingbird| Harper Lee          |
| The Catcher in the Rye| NULL                |

### RIGHT JOIN

A **RIGHT JOIN** returns all records from the right table (authors), and the matched records from the left table (books). The result is NULL from the left side if there is no match.

```sql
SELECT books.title, authors.name 
FROM books 
RIGHT JOIN authors ON books.author_id = authors.author_id;
```

#### Result

| title                | name                |
|----------------------|---------------------|
| The Great Gatsby     | F. Scott Fitzgerald |
| 1984                 | George Orwell       |
| To Kill a Mockingbird| Harper Lee          |
| NULL                 | J.D. Salinger       |

### FULL OUTER JOIN

A **FULL OUTER JOIN** returns all records when there is a match in either left (books) or right (authors) table records. If there is no match, the result is NULL on the side that doesn't have a match.

```sql
SELECT books.title, authors.name 
FROM books 
FULL OUTER JOIN authors ON books.author_id = authors.author_id;
```

#### Result

| title                | name                |
|----------------------|---------------------|
| The Great Gatsby     | F. Scott Fitzgerald |
| 1984                 | George Orwell       |
| To Kill a Mockingbird| Harper Lee          |
| The Catcher in the Rye| NULL                |
| NULL                 | J.D. Salinger       |

## Aggregate Functions

### COUNT

Return the number of books:

```sql
SELECT COUNT(book_id) FROM books;
```

### SUM

Return the total number of years all books were published:

```sql
SELECT SUM(year_published) FROM books;
```

### AVG

Return the average year books were published:

```sql
SELECT AVG(year_published) FROM books;
```

### MIN

Return the earliest year a book was published:

```sql
SELECT MIN(year_published) FROM books;
```

### MAX

Return the latest year a book was published:

```sql
SELECT MAX(year_published) FROM books;
```

## Miscellaneous

### GROUP BY

Group rows by author and count the number of books for each:

```sql
SELECT authors.name, COUNT(books.book_id) 
FROM books 
INNER JOIN authors ON books.author_id = authors.author_id 
GROUP BY authors.name;
```

### ORDER BY

Sort the `books` table by year published in descending order:

```sql
SELECT * FROM books ORDER BY year_published DESC;
```

### HAVING

Specify a condition for groups, return authors with more than one book:

```sql
SELECT authors.name, COUNT(books.book_id) 
FROM books 
INNER JOIN authors ON books.author_id = authors.author_id 
GROUP BY authors.name 
HAVING COUNT(books.book_id) > 1;
```

### DISTINCT

Select unique publication years from the `books` table:

```sql
SELECT DISTINCT year_published FROM books;
```
