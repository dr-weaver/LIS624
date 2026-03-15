## **Intro**  
The following commands were ran in MySQL as part of the exercises outlined in MySQL.md  
In particular, this fiel corresponds to the **Testing commands** section of that file.  
The goal of this portion of the exercise was to practice using SQL commands to search and edit a relational database.  
The prompts will be broken down into basic sections here. A command will be listed, then the results of it's execution will be listed directly after.  

### *Selecting specific entities in a relation*  
1. `select author from books;`
```
+----------------+
| author         |
+----------------+
| Jennifer Egan  |
| Imbolo Mbue    |
| Lydia Millet   |
| Julia Phillips |
+----------------+
4 rows in set (0.17 sec)
```

2. `select copyright from books;`
```
+-----------+
| copyright |
+-----------+
|      2022 |
|      2021 |
|      2020 |
|      2019 |
+-----------+
4 rows in set (0.00 sec)
```

3. `select author, title from books;`
```
+----------------+-----------------------+
| author         | title                 |
+----------------+-----------------------+
| Jennifer Egan  | The Candy House       |
| Imbolo Mbue    | How Beautiful We Were |
| Lydia Millet   | A Children's Bible    |
| Julia Phillips | Disappearing Earth    |
+----------------+-----------------------+
4 rows in set (0.00 sec)
```

4. `select author from books where author like '%millet%';`
```
+--------------+
| author       |
+--------------+
| Lydia Millet |
+--------------+
1 row in set (0.02 sec)
```

5. `select title from books where author like '%mbue%';`
```
+-----------------------+
| title                 |
+-----------------------+
| How Beautiful We Were |
+-----------------------+
1 row in set (0.01 sec)
```

6. `select author, title from books where title not like '%e';`
```
+----------------+--------------------+
| author         | title              |
+----------------+--------------------+
| Julia Phillips | Disappearing Earth |
+----------------+--------------------+
1 row in set (0.01 sec)
```

7. `select * from books;`
```
+----+----------------+-----------------------+-----------+
| id | author         | title                 | copyright |
+----+----------------+-----------------------+-----------+
|  1 | Jennifer Egan  | The Candy House       |      2022 |
|  2 | Imbolo Mbue    | How Beautiful We Were |      2021 |
|  3 | Lydia Millet   | A Children's Bible    |      2020 |
|  4 | Julia Phillips | Disappearing Earth    |      2019 |
+----+----------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

### *Adding publisher column to table*  
1. `alter table books add publisher varchar(75) after title;`
```
Query OK, 0 rows affected (0.18 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

2. `describe books;`
```
+-----------+--------------+------+-----+---------+----------------+
| Field     | Type         | Null | Key | Default | Extra          |
+-----------+--------------+------+-----+---------+----------------+
| id        | int unsigned | NO   | PRI | NULL    | auto_increment |
| author    | varchar(150) | NO   |     | NULL    |                |
| title     | varchar(150) | NO   |     | NULL    |                |
| publisher | varchar(75)  | YES  |     | NULL    |                |
| copyright | year         | NO   |     | NULL    |                |
+-----------+--------------+------+-----+---------+----------------+
5 rows in set (0.02 sec)
```

3. `update books set publisher='Simon & Schuster' where id='1';`
```
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

4. `update books set publisher='Penguin Random House' where id='2';`
```
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

5. `update books set publisher='W. W. Norton & Company' where id='3';`
```
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

6. `update books set publisher='Knopf' where id='4';`
```
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

7. `select * from books;`
```
+----+----------------+-----------------------+------------------------+-----------+
| id | author         | title                 | publisher              | copyright |
+----+----------------+-----------------------+------------------------+-----------+
|  1 | Jennifer Egan  | The Candy House       | Simon & Schuster       |      2022 |
|  2 | Imbolo Mbue    | How Beautiful We Were | Penguin Random House   |      2021 |
|  3 | Lydia Millet   | A Children's Bible    | W. W. Norton & Company |      2020 |
|  4 | Julia Phillips | Disappearing Earth    | Knopf                  |      2019 |
+----+----------------+-----------------------+------------------------+-----------+
4 rows in set (0.00 sec)
```

### *Deleting and inserting entities from a relation*
1. `delete from books where author='Julia Phillips';`
```
Query OK, 1 row affected (0.01 sec)
```

2.```insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');```

```
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

3. `select * from books;`
```
+----+---------------+-----------------------+-------------------------+-----------+
| id | author        | title                 | publisher               | copyright |
+----+---------------+-----------------------+-------------------------+-----------+
|  1 | Jennifer Egan | The Candy House       | Simon & Schuster        |      2022 |
|  2 | Imbolo Mbue   | How Beautiful We Were | Penguin Random House    |      2021 |
|  3 | Lydia Millet  | A Children's Bible    | W. W. Norton & Company  |      2020 |
|  5 | Emma Donoghue | Room                  | Little, Brown & Company |      2010 |
|  6 | Zadie Smith   | White Teeth           | Hamish Hamilton         |      2000 |
+----+---------------+-----------------------+-------------------------+-----------+
5 rows in set (0.00 sec)
```

### *Selecting and/or sorting based on copyright range*
1. `select author, publisher from books where copyright < '2011';`
```
+---------------+-------------------------+
| author        | publisher               |
+---------------+-------------------------+
| Emma Donoghue | Little, Brown & Company |
| Zadie Smith   | Hamish Hamilton         |
+---------------+-------------------------+
2 rows in set (0.00 sec)
```

2. `select author from books order by copyright;`
```
+---------------+
| author        |
+---------------+
| Zadie Smith   |
| Emma Donoghue |
| Lydia Millet  |
| Imbolo Mbue   |
| Jennifer Egan |
+---------------+
5 rows in set (0.00 sec)
```
