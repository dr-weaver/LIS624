### __2026-03-29 - *Basic ILS Creation*__

## __*Goal:*__  
Create a simple OPAC and cataloging module in order to famliarize with how library ILS's work behind the scenes.

## __*Steps:*__  
#### **Routine System Updates**  

1. Checked for potential updates for machine using `sudo apt update`

- *all packages up to date*  

2. Checked for any potential clean up that could be done to machine files using `sudo apt autoremove` and `sudo apt clean`
  
- *no files needed removed*  

#### **Database Creation and Configuration**  
1.  Logged in as MySQL root user using `sudo mysql -u root`

2. Created new database called `DinnerDB` using `crate database DinnerDB;`

- *received Query OK message, database created successfully*  

3. Granted all priviliges on `DinnerDB` to `opacuser` from previous module using the following command:  
```
grant all privileges on DinnerDB.* to 'opacuser'@'localhost';
```

- *received Query OK message, privileges granted successfully*  

4.   Exited MySQL using `\q`

5. Logged back into MySQL as `opacuser` using ` mysql -u opacuser -p`, entered user password when prompted.

- *login successful*

6. Confirmed user access to database using `show databases;`, returned the following:  
```
+--------------------+
| Database           |
+--------------------+
| DinnerDB           |
| information_schema |
| opacdb             |
| performance_schema |
+--------------------+
4 rows in set (0.08 sec)
```

- *database is listed, confirming access*

7. Accessed database and created `Meals` table using the following commands:  
```
use DinnerDB;
create table Meals (
    meal_id int auto_increment primary key,  
    meal_name varchar(100) not null,  
    cuisine varchar(50),  
    cooking_time int not null default 1 check (cooking_time > 0),  
    vegetarian boolean  
);
```

> the table created has the following features:  
	- `meal_id`: auto increment integer; primary key.  
	- `meal_name`: variable-length string up to 100 chars.; cannot be empty.  
	- `cuisine`: variable-length string up to 50 chars.  
	- `cooking_time`: integer, cannot be empty, 0, or negative value.  
	- `vegetarian`: BOOLEAN; must be **TRUE** or **FALSE**.
>

- *initial typo in second line, "auto-increment" instead of "auto_increment"; received error message, re-typed command correctly, received Query OK*

8. Created `Ingredients` table using the following command:
```
create table Ingredients (
    ingredient_id int auto_increment primary key,
    meal_id int not null,
    ingredient_name varchar(100) not null,
    quantity varchar(50),
    foreign key (meal_id) references Meals(meal_id) on delete cascade
);
```

> the table created has the following features:  
	- `ingredient_id`: auto increment integer; primary key.  
	- `meal_id`: non-null integer, foreign key from `Meals`.  
	- `ingredient_name`: variable-length string up to 100 chars.; cannot be empty.  
	- `quantity`: variable-length string up to 50 chars.  
	- per `on delete cascade` clause, corresponding ingredients will be removed from `Ingredients` table when meal entries are removed from `Meals`.
>

- *received Query OK, table created successfully*

9. Inserted data into `Meals` table using the following command:
```
insert into Meals (meal_name, cuisine, cooking_time, vegetarian) values
    ('Spaghetti Bolognese', 'Italian', 45, FALSE),
    ('Vegetable Stir Fry', 'Chinese', 20, TRUE),
    ('Chicken Curry', 'Indian', 50, FALSE),
    ('Mushroom Risotto', 'Italian', 35, TRUE);
```

- *received Query OK, data added successfully*

10. Inserted data into `Ingredients` table using the following command:
```
insert into Ingredients (meal_id, ingredient_name, quantity) values
    (1, 'Spaghetti', '200g'),
    (1, 'Ground Beef', '250g'),
    (1, 'Tomato Sauce', '1 cup'),
    (2, 'Broccoli', '100g'),
    (2, 'Carrots', '50g'),
    (2, 'Soy Sauce', '2T'),
    (3, 'Chicken Breast', '300g'),
    (3, 'Curry Powder', '2T'),
    (3, 'Coconut Milk', '1 cup'),
    (4, 'Arborio Rice', '1 cup'),
    (4, 'Mushrooms', '1 cup'),
    (4, 'Parmesan Cheese', '1/2 cup');
```

- *received Query OK, data added successfully* 

11. Ran several test Queries using tables, following prompts in "Querying Data" section of LIS624 textbook chapter ["Introduction to Relational Databases"](https://cseanburns.github.io/systems-librarianship/5a-introduction-to-relational-databases.html)

- **Note:** See `DinnerDB_Queries.md` file to view queries and results.
- *Queries ran without issue*

12. Logged out of MySQL using `\q`

#### **Practice with User Privilege Management**
1. Logged into MySQL as root user using `sudo mysql -u root`

2. reviewed `opacuser` user privileges with `show grants for 'opacuser'@'localhost';`

- *system returned the following:*
```
+----------------------------------------------------------------+
| Grants for opacuser@localhost                                  |
+----------------------------------------------------------------+
| GRANT USAGE ON *.* TO `opacuser`@`localhost`                   |
| GRANT ALL PRIVILEGES ON `DinnerDB`.* TO `opacuser`@`localhost` |
| GRANT ALL PRIVILEGES ON `opacdb`.* TO `opacuser`@`localhost`   |
+----------------------------------------------------------------+
3 rows in set (0.00 sec)
```

3. Revoked all privileges using `revoke all privileges on DinnerDB.* from 'opacuser'@'localhost';`

- *received Query OK; re-used `show grants` command from step 2 and confirmed that there was no mention of `DinnerDB`*

4. Reviewed users by using `select user, host from mysql.user;`; received the following results:
```
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| opacuser         | localhost |
| root             | localhost |
+------------------+-----------+
6 rows in set (0.00 sec)
```

5. Returned privileges for `DinnerDB` to `opacuser` using `grant all privileges on DinnerDB.* to 'opacuser'@'localhost';`  

- *Received Query OK, used `show grants` command from steps 2 and 3 again to confirm that privileges had returned.*

#### **OPAC Creation**  
1. Logged into MySQL as `opacuser` using `mysql -u opacuser -p`, entering password once prompted.

-*login successful*  

2. Altered `books` table in `opacdb` to adjust copyright data and allow date filters to work more effectively using the following:
```
use opacdb;
alter table books add publication_date date;
update books set publication_date = str_to_date(concat(copyright, '-01-01'), '%Y-%m-%d');
alter table books drop column copyright;
alter table books change publication_date copyright date not null;
```

-*Received "Query OK, 5 rows affected" message*

3. Ran `select * from books;` command to check date display, printed following results confirming correction:
```
+----+---------------+-----------------------+-------------------------+------------+
| id | author        | title                 | publisher               | copyright  |
+----+---------------+-----------------------+-------------------------+------------+
|  1 | Jennifer Egan | The Candy House       | Simon & Schuster        | 2022-01-01 |
|  2 | Imbolo Mbue   | How Beautiful We Were | Penguin Random House    | 2021-01-01 |
|  3 | Lydia Millet  | A Children's Bible    | W. W. Norton & Company  | 2020-01-01 |
|  5 | Emma Donoghue | Room                  | Little, Brown & Company | 2010-01-01 |
|  6 | Zadie Smith   | White Teeth           | Hamish Hamilton         | 2000-01-01 |
+----+---------------+-----------------------+-------------------------+------------+
5 rows in set (0.01 sec)
```

4. Logged out of MySQL using `\q`

5. Changed to `/html` directory using `cd /var/www/html/`

6. Used nano to create new HTML file titled `mylibrary.html`

7. Created an HTML form that allows for query entries for searching the OPAC.

- **Note:** Code taken from the LIS624 textbook chapter ["Creating a Bare Bones OPAC"](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html), can be found in "HTML Form" section.

8. Used nano to create new PHP file titled `search.php`

9. Created PHP script that connects to HTML form and allows for functional searching of `books` database.

- **Note:** Code taken from "PHP Search Script" section of the same textbook chapter linked above.

10. Logged into MySQL as `opacuser` using `mysql -u opacuser -p` and typing password when prompted.

- *logged in successfully*

11. Reviewed `books` database using the following commands:
```
show tables;
select * from books;
```

- *correctly printed the following response:*
```
+----+---------------+-----------------------+-------------------------+------------+
| id | author        | title                 | publisher               | copyright  |
+----+---------------+-----------------------+-------------------------+------------+
|  1 | Jennifer Egan | The Candy House       | Simon & Schuster        | 2022-01-01 |
|  2 | Imbolo Mbue   | How Beautiful We Were | Penguin Random House    | 2021-01-01 |
|  3 | Lydia Millet  | A Children's Bible    | W. W. Norton & Company  | 2020-01-01 |
|  5 | Emma Donoghue | Room                  | Little, Brown & Company | 2010-01-01 |
|  6 | Zadie Smith   | White Teeth           | Hamish Hamilton         | 2000-01-01 |
+----+---------------+-----------------------+-------------------------+------------+
5 rows in set (0.00 sec)
```

12. Tested that both files worked correctly using [server IP address](http://35.202.204.12/mylibrary.html) and performing a few test searches based on the information in the above table.

- *searches for "candy", "Donoghue", and "Company", combined with inclusive date ranges of "01-01-2000"-"01-01-2026" correctly retrieved accurate entities from database*

13. Returned to MySQL, inserted new data to `books` using the following command:
```
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010-01-01'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-01'),
('Stanley Dean Rider Jr', 'Death Feigning Beetles of the United States and Mexico', 'Stanley Dean Rider Jr', '2024-01-01'),
('Peter Kuper', 'Insectopolis', 'W. W. Norton & Company', '2025-01-01'),
('Maisy Valais', 'Sunflowers and Lavender', 'Yellow Jacket', '2026-01-01');
```

- *received Query OK message; used select all command from step 11 to confirm added rows*

14. Exited MySQL using `\q`

#### **Cataloging Module Creation**  
1. Returned to `/html` directory, created new `cataloging` directory using `sudo mkdir cataloging`

- *confirmed directory was successfully created using `ls` command and seeing it listed among html directory contents*

2. Changed to `/cataloging` directory using `cd cataloging`.

3. Created and opened new HTML file using `sudo nano index.html`.

4. Added HTML script to create form for adding record information to the OPAC; saved and exited nano.

- **Note:** Code taken from the LIS624 textbook chapter ["Creating a Bare Bones Cataloging Module"](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html), can be found in "Creating the HTML Page and a PHP Cataloging Page" section

5. Created and opened new PHP file using `sudo nano insert.php`.

6. Added PHP script connecting HTML form from step 4 to MySQL database `books`; saved and exited nano.

- **Note:** Code taken from "PHP Insert Script" section of the same textbook chapter linked in step 4.

7. Created simple authentication file in apache2's configuration directory, utilizing htpasswd mechanism.
```
sudo htpasswd -c /etc/apache2/.htpasswd libcat
```

- **Note:** "libcat" references the username chosen for the auth. credentials. Was prompted to add and confirm password, and did so accordingly.

- *password added successfully, no apparent errors in credential creation*

8. Opened Apache config file using `sudo nano /etc/apache2/apache2.conf`

9. Scrolled to `<Directory /var/www/>` script block and added the following script below in order to set `/cataloging` directory to require authorization; saved and exited.
```
<Directory /var/www/html/cataloging/>
  Options Indexes FollowSymLinks
  AllowOverride AuthConfig
  Require all granted
</Directory>
```

10. Created and opened hashed password file using `sudo nano .htaccess`; added the following code to set credentials created in step 7 to `/cataloging` directory; saved and exited.
```
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```

11. Checked that configuration was successful using `sudo apachectl configtest`

- *received Syntax OK message, confirming success*

12. Restarted `apache2` using `sudo systemctl restart apache2`

13. Checked system status using `sudo systemctl status apache2`

- *confirmed system was `enabled` and `active`*

14. Changed ownership of `/var/www/html` directory to user `www-data` using ` sudo chown :www-data /var/www/html`

15. Set setgid bit on `/var/www/html` so that `www-data` inherits shared ownership of future files created in directory using ` sudo find /var/www/html -type d -exec chmod g+s {} +`

16. Tested cataloging interface by visiting server IP address.

- *correctly prompted for login credentials, responded to the right ones, page looks as expected*

17. Added a few items to the "catalog" using interface.

- *data submitted without issue*

18. Tested that data was correctly added to OPAC by going to [OPAC interface](http://35.202.204.12/mylibrary.html) and searching for author name from one of the added titles.

- *search found and displayed correct data*
 
## __*Results:*__  
+ I now have an operating OPAC and cataloging interface through which I can add and search for bibliographic data.

## __*Notes:*__
+ Textbook chapter ["Introduction to Relational Databases"](https://cseanburns.github.io/systems-librarianship/5a-introduction-to-relational-databases.html)
+ Textbook chapter ["Creating a Bare Bones OPAC"](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html)
+ Textbook chapter ["Creating a Bare Bones Cataloging Module"](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html)

## __*Reflection:*__  
+ **What confused me?**  
	- I was a bit confused trying to parse out what adding text to the config file did vs making the `.htaccess` file during authorization configuration. Once I actually completed the final step of that, though, it made sense.
+ **What surprised me?**  
	- Honestly? That the above was the only thing that confused me!
+ **What did I break and how did I fix it?**  
	- Initially typed "auto-increment" instead of "auto_increment" when building `Meals` table; re-typed command correctly to fix.
+ **What would I do differently next time?**   
	- These weren't really options in this scenario since this was a class exercise, but:
	- Flesh out the CSS script for the OPAC and cataloging interface to make them look a bit more interesting.
	- Include more data elements in the `books` DB/cataloging module.
+ **General**    
	- I wanted to work on a person project involving CI and LAMP stacks after graduation, I believe I will take this specific concept and flesh it out further to develop a catalog for my personal library.
