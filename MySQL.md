### __2026-03-15 - *Installing MySQL*__

## __*Goal:*__  
Install and configure MySQL in order to complete the LAMP stack.


## __*Context:*__  
Required for LIS624. 
LAMP stacks are frequently used for website management, including some library management systems.  
MySQL is used for relational database management, and completes the LAMP stack we've been working on in this class.  


## __*Steps:*__  

#### **Installing and setting up MySQL** 
1. Checked that machine is up to date with the following commands:  
```
sudo apt update
sudo apt upgrade
sudo apt autoremove
sudo apt clean
```  
- *10 packages were updated; none were removed*  

2. Installed MySQL commmunity server using `sudo apt install mysql-server`
- *installed succesfully*  

3. Checked the status and version using `mysql --version`  
- *Version 8.0.45; appears to be up to date*  

4. Checked status using `systemctl status mysql`  
- *mysql cited as enabled and running.*  

5. Ran post installation security check using `sudo mysql_secure_installation`  
- *ran successfully; prompt responses summarized below*  

 >Validate password component?: yes; set to low.  
 Remove anonymous users?: yes.  
 Disallow root login remotely?: yes.  
 Remove test database and access?: yes.  
 Reload privilege tables now?: yes.
 >
 
 6. Logged in to database using `sudo mysql -u root`  
 
 7. Displayed databases using `show databases;`  
 - *databases returned as follows:*  
 ```
 +--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)
```
8. Exited mysql using `\q`  

#### **Setting up regular user account**  
1. Logged back into mysql using `sudo mysql -u root`  


2. Created new user using the following:    
```
create user 'opacuser'@'localhost' identified by 'XXXXXXXX';
```
- *new user created succesfully with `Query OK` response; note that `'XXXXXXXX'` is stand-in for password*  

#### **Creating practice database**  
1. Created new database called `opacdb`, setting character encoding to UTF-8 using the following:  
```
create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;
```    
- *database created successfully with `Query OK` response.*  
- *personal note; the language used in the create command means the following:* 

> `utf8mb4` supports full range of Unicode.  
`collate` refers to rules used to compare and sort text strings.  
`0900` refers to Unicode 9.0.  
`ai` refers to accent insensitive, making db ignore accent inclusion. 
`ci` refers to case insensitive, making db ignore letter casing.
>

2. Used `show databases;` command to view new database.
- *databases return as follows:*  
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| opacdb             |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.10 sec)
```

3. Granted all privileges on the new database to `opacuser` using the following:  
```
grant all privileges on opacdb.* to 'opacuser'@'localhost';
``` 
- *privileges granted successfully with `Query OK` response.*  

4. Exited MySQL with `\q`  

#### **Logging in as regular user and creating tables**  

1. Added code in `bash` shell to aid in intuitiveness of MySQL client as follows:
> Opened text file in nano using `nano ~/.bashrc`  
Added the following code at the bottom of the file `export MYSQL_PS1="[\d]>"  
Saved and exited file, typed `source ~/.bashrc`
>

2. Logged in to MySQL as `opacuser` using  `mysql -u opacuser -p` and typing in password when prompted.  

3. Used `show databases;` then `use opacdb;` to switch to created database.
-*login and database switch over completed successfully*  

4. Created table titled `books` for testing purposes using the following command:  
```
create table books (
id int unsigned not null auto_increment,
author varchar(150) not null,
title varchar(150) not null,
copyright year not null,
primary key (id)
);
```  
- *received `Query OK` response*

5. Confirmed table creation with `show tables;` then `describe books;`  
- *tables return as follows:*  
```
[opacdb]> show tables;
+------------------+
| Tables_in_opacdb |
+------------------+
| books            |
+------------------+
1 row in set (0.00 sec)
```
- *table description returns as follows:*  
```
[opacdb]> describe books;
+-----------+--------------+------+-----+---------+----------------+
| Field     | Type         | Null | Key | Default | Extra          |
+-----------+--------------+------+-----+---------+----------------+
| id        | int unsigned | NO   | PRI | NULL    | auto_increment |
| author    | varchar(150) | NO   |     | NULL    |                |
| title     | varchar(150) | NO   |     | NULL    |                |
| copyright | year         | NO   |     | NULL    |                |
+-----------+--------------+------+-----+---------+----------------+
4 rows in set (0.01 sec)
```

6. Added records to table using the `insert` command as follows:  
```
insert into books (author, title, copyright) values
    -> ('Jennifer Egan', 'The Candy House', '2022'),
    -> ('Imbolo Mbue', 'How Beautiful We Were', '2021'),
    -> ('Lydia Millet', 'A Children\'s Bible', '2020'),
    -> ('Julia Phillips', 'Disappearing Earth', '2019');
```
- *received the following response from input:*
```
Query OK, 4 rows affected (0.05 sec)
Records: 4  Duplicates: 0  Warnings: 0
```

7. Viewed table using `select * from books;`; displayed as:
```
+----+----------------+-----------------------+-----------+
| id | author         | title                 | copyright |
+----+----------------+-----------------------+-----------+
|  1 | Jennifer Egan  | The Candy House       |      2022 |
|  2 | Imbolo Mbue    | How Beautiful We Were |      2021 |
|  3 | Lydia Millet   | A Children's Bible    |      2020 |
|  4 | Julia Phillips | Disappearing Earth    |      2019 |
+----+----------------+-----------------------+-----------+
4 rows in set (0.01 sec)
```

#### **Testing commands**  
1. Tested commands given in text book (see file notes), ran in order as follows:  
```
select author from books;  
select copyright from books;  
select author, title from books;  
select author from books where author like '%millet%';  
select title from books where author like '%mbue%';  
select author, title from books where title not like '%e';  
select * from books;  
alter table books add publisher varchar(75) after title;  
describe books;  
update books set publisher='Simon & Schuster' where id='1';  
update books set publisher='Penguin Random House' where id='2';  
update books set publisher='W. W. Norton & Company' where id='3';  
update books set publisher='Knopf' where id='4';  
select * from books;  
delete from books where author='Julia Phillips';  
insert into books
      (author, title, publisher, copyright) values
      ('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
      ('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');
select * from books;  
select author, publisher from books where copyright < '2011';  
select author from books order by copyright;  
\q
```
- *commands executed as expected; see file `MySQL-books-testcmnd.md` for command results*  

#### **Installing PHP and MySQL Support**  
1. Installed PHP support for MySQL using `sudo apt install php-mysql`  
- *some additional modules beyond basic support will be installed that may not be needed but are used for demonstrative purposes.*  

2. After successful installation, restarted Apache and MySQL using:
```
sudo systemctl restart apache2
sudo system restart mysql
```
- *restart successful*

#### **Creating and testing PHP scripts**  

1. Created login.php file in `/var/www` using the following:  
```
cd /var/www
sudo touch login.php
sudo chmod 640 login.php
sudo chown :www-data login.php
ls -l login.php
sudo nano login.php
```

2. Changed group ownership and permissions for the `login.php` file by adding the following credential info via nano:
```
<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "XXXXXXXXX";
?>
```
- *note that 'XXXXXXXX' is a stand-in for actual password*

3. Created new PHP file for website using the following commands:
```
cd /var/www/html
sudo nano opac.php
```

4. Added appropriate code to file, transcribed from textbook.
- *To save space, code wil not be included here.*  
	*Textbook will be linked in notes; refer to "Create PHP Scripts" section for code*

5. Tested syntax of `opac.php` for errors using the following commands:
```
sudo php -f /var/www/login.php
sudo php -f /var/www/html/opac.php
```
- *second input showed error on line 35; error message listed as follows:*
```
PHP Parse error:  syntax error, unexpected token "<" in /var/www/html/opac.php on line 35
```

6. Fixed syntax error noted in step 5; two lines of code had been combined into one plus  
single and double quotes had been used together incorrectly. Lines were rewritten.  
- *syntax testing inputs from Step 5 were re-ran; first input was still correct, second input printed html code as expected.*

7. Viewed site from external browser to ensure display was correct.
- *site can be viewed [here](http://35.238.136.131/opac.php)*

## __*Results:*__  
+ MySQL has been installed and configured.
+ MySQL has been successfully connected to PHP.
+ A basic database has been created, including a table.  
+ Database has been connected to VM external site, with some basic SQL queries displayed on page.

## __*Notes:*__
+ Textbook chapter on MySQL can be found [here](https://cseanburns.github.io/systems-librarianship/4c-installing-configuring-mysql.html)

## __*Reflection:*__  
+ **What confused me?**  
	As with the other LAMP assignments, there wasn't really anything that necessarily confused me. I just needed to get used to input conventions.
+ **What surprised me?**  
	Once again, nothing in particular felt surprising. If I had to pick anything, I suppose I was somewhat surprised by how neatly the table information was displayed in MySQL.
+ **What did I break and how did I fix it?**  
	When writing the site code at the end of the exercise, I accidentally skipped part of one line and sorta of smooshed it together with the one right below it as seen below:  
	- *original lines:*
	```
	echo "<p>Publisher " . htmlspecialchars($row["publisher"]) .
    " published a book by " . htmlspecialchars($row["author"]) . “.</p>";
	```
	
	- *what I wrote:*

	```
	echo "<p>Publisher " . htmlspecialchars($row["author']) . “.</p>";
    ```

    I also accidentally used a single quote after the word "author". The system let me know of the latter error when checking, so I was able to go in and fix both via nano.
+ **What would I do differently next time?**  
	I definitely think I should slow down a little when typing commands. I have a tendency to want to finish them, hit enter, and then look for errors, but this obviously doesn't work.  
+ **General**  
	Similarly to the work with PHP, I feel like I still need to let some of what I learned sink in a bit to get a better idea of the bigger picture of what I'm doing.  
	The content we were working with this week did help a lot in helping me put some more of the pieces together, but I think the whole process still feels a little bit new and clunky.
