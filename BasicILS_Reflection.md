### __2026-04-01 - *Basic OPAC and Cataloging Module Creation - Reflection*__

## __*Context:*__  
+ This is part of a required assignment for LIS624. The goal of the assignment is to reflect on the process used to create the basic ILS structures that are documented in the `DIY_ILS.md` file.  
+ This documentation is intended to provide a more narrative and descriptive guide to this process, which can be used in conjunction with the above mentioned document to provide a more robust and well-rounded guide.

## __*Introduction:*__  
An OPAC (Online Public Access Catalog) is an online interface that allows library users to search and review the bibliographic data of the items in a library’s collection. 
It can be used to check for the availability of specific items, or as a way to find materials that cover specific subjects or genres. 
This information is accessed through a relational database, which stores the bibliographic data that has been supplied by librarians, often through a cataloging module. 
The OPAC allows for the conversion of patron searches to SQL syntax which retrieves data from the relational database.  
This type of system typically uses a LAMP stack as its foundation, as these types of structures are both accessible and versatile.

## __*Steps:*__ 
#### **Building a database**  
+ Since we set up our LAMP stack server in the last module, our first step in creating a basic ILS was to develop a database that would house cataloging information using MySQL.
Below is a basic outline of how this process went, without adding in the exact code used. Please refer to the `DIY_ILS.md` file for the specific code used in this process.

1. In order to create the databse you'll be working with, you'll initially need to log into MySQL as the root user, as the other user ID you'll be operating under likely won't have the privilieges to do so.
	+ Login as the root user using the `sudo mysql -u root` command

2. As the root user, create a database using the `create database` command, and grant full access privileges to the user ID you'll be working under with the `grant all privileges` command.

3. Once privileges have been granted, you can exit MySQL as the root user, and log back in under the above mentioned user ID.
	+ **NOTE:** As a security measure, this user account should be password protected, see `MySQL.md` for further information on how to set this up

4. Next, create a table to store book information using `create table` command, which includes the table name, the values you want included (author, title, publishing date, etc), and how you want the data in each value to be treated (integer vs characters, required vs nont required, etc.)
	+ **NOTE:** When setting the parameters for how you want data to be entered into a field, it is important to consider not only the character type (numbers, letters, a combination) but also how integral the data is in describing an entity, and how it may influence or be influenced by data elements in future tables.
	
	
3. Once the table is confirmed as created, you can begin adding data for different books that you want in the system using the `insert into [table]` command, including which values you’re entering information for and grouping that information for each item in parentheses.
  
4. Once the information is added correctly, it can be searched using SQL querying syntax. 
	+ **EXAMPLES:** `select * from [table]` pulls all info from all items and values for display; using `select [column name] COUNT (*) FROM [table]` provides a count of different values within a given column, etc.
	
5. Having created the table, added some initial entries to it, and testing that it works as intended through some querying, you've now finished the initial set up of your relational database.

#### **Creating a Basic OPAC**  
1.  If you followed the processes outlined in this repo when creating your book table, you'll have to alter the copyright column to aid in the search functionality of your OPAC. In order to do so, follow these steps:
	In order to do this, you'll login to MySQL with your relevant user ID, access the database you created, and use the `alter table` command to do the following:  
	- Login with the relevant MySQL user ID and access the database for your OPAC;
	 - Use the `alter table` command and add a new publication date column;
	 - Use the `update` command to change the new column to be in a specified date format using a YYYY-MM-DD structure, with the month and day assumed to be "01-01"
	 - Use the `alter table` command to remove the original copyright date column;
	 - Use the `alter table` command again to rename the new publication date column to match that of the one you just deleted.>

2. Now that the copyright date can be more easily searched, you will start to build your OPAC by utilizing some HTML and PHP code to develop a search page. 
	+ **NOTE:** It's important to note that the HTML provides the viewing structure through which searches can be performed, while the PHP script is what actually initiates the database search and retrieves the results; in essence, the HTML provides the viewed form while the PHP does the actual searching.
	+ **NOTE:** The following outline will break down the function of the HTML and PHP code, and is based on the code provided in the `HTML Form` and `PHP Search Script` sections of the LIS624 textbook chapter ["Creating a Bare Bones OPAC"](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html)
	
3. You'll start by using a text editor (I used `nano`) to create an HTML document to serve as the site page. The HTML code used provides a basic structure that includes the following elements:
	+ An introduction that explains how the search interface works, noting that the start and end date data is required.
	+ Further explanation of the pared down nature of both the toy OPAC and the records it searches.
	+ A reference to the PHP script which it will be linked to, ensuring that interacting with the HTML triggers the PHP script to act as intended.
	+ A blank space for search terms, labeled appropriately.
	+ Two date-input boxes, one labeled for start date, and one labeled for end date.
	
4. Again, this code is meant to provide the graphical view of the OPAC, meaning that it doesn't actually search the database, but provides the structure through which browser users can interact the the PHP script which will actually do so.

5. Once that code has been written and saved, you'll create a PHP document with your text editor, naming it the same as the PHP file name you reference in the HTML code. The PHP script includes the following parameters:
	 + Basic elements for a table structure in which results will be organized.
	+ A command to login to MySQL as the user ID you've been using for the project, including automatic inputs for the login credentials to establish a connection.
	+ Enabling of error reporting, should it be needed.
	+ Request parameters that outline how each input box should be allowed to interact with the server.
	+ A preparared SQL query which selects all non-date columns for the initial text search box, and looks for dates that fall between those given in the start and end date boxes.
	+ Permission to use wildcard results and instructions for how to display the results within the table format outlined above.
	+ A "no results found" statement to retrieve if there are no matches on the SQL query.
	+ A command to close the connection.
	+ A link that allows the user to return to the OPAC search page, labeled appropriately.

6. Once these files are saved, they should be live on your server's site. You can visit the site and test some basic searches to ensure that everything is functioning properly.

7. So long as everything works as intended, you've officially set up the OPAC! Going forward, any items you add to the table connecting to the OPAC should be searchable through this interface.

#### **Creating a Basic Cataloging Module**
1. The process for creating the cataloging module is similar to the one used to create the OPAC. The goal here is to make an input form in which you can add new title information that can be logged into your relational database.

2. Before making the two files, you'll want to make a new, dedicated cataloging directory in your server using the `mkdir` command. It is here that you'll use the text editor to create both files as you did for the OPAC.
	+ **NOTE:** The following outline will break down the function of the HTML and PHP code, and is based on the code provided in the `Creating the HTML Page and a PHP Cataloging Page` section of the LIS624 textbook chapter ["Creating a Bare Bones Cataloging Module"](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html)
	
3. The HTML file will serve, once again, as a general structure through which individuals can interact with the PHP script; it has the following components:
	+ A discription for the page and its use as a way for catalogers to input records, along with a warning that non-catalogers should not use it.
	+ A reference to the PHP script file that will be used to actually add the data to your table.
	+ Individual blank input boxes for each element recorded in the table (author, title, publisher, copyright date), labeled appropriately.
	+ A submit button.
		
4. After saving the HTML file, you'll create a new PHP file (just as you did when creating the OPAC) and add the code which will actually allow for the data submitted on the site to be added to the table. It will contain the following elements:
	+ A command to connect to login to the relevant directory of the server, establishing connection with the database using appropriate login credentials.
	+ The ability to produce an error message should it be needed.
	+ Server requests that outline what each input box is recording.
	+ A command that enforces the requirement that all data elements be added, and that dates be given in the correct format.
	+ Error messages that explain that the above two requirements are put in place.
	+ An SQL `insert` query that adds the data to the appropriate parts of the table.
	+ A result statement to provide should the query execute successfully, as well as one to provide should it fail.
	+ A command to close the connection to the server.
	+ A link that allows users to go back to the input form and add another record, labeled appropriately.
	
5. Once both files are saved, the enxt step will be adding a serurity measure to the page so that only those who have appropriate login credentials can add records through the page.

6. In order to do this, you'll use the `sudo` command to create a new authentication file in the Apache2 directory that stores its configuartion files and signal that you'll be utilizing a hashed password while also providing a username to associate this password with.
	+ **NOTE:** Setting up a simple username and password system in this scenario wouldn't do much good, as the login credentials for the site would be visible in the site URL. Using a hashed password essentially tells Apache to scramble the password information, making it harder for unauthorized site visitors to find the credentials needed to access the site.
	
7. After creating the authentication file, you'll open Apache's `.conf` file to add code that only allows for configuration changes to be focused on the cataloging directory alone, rather than it's parent directory.

8. Next, you'll use `sudo` to open a new text editor file in the cataloging directory titled `.htaccess`. Here, you'll add code that specifies the need for the hashed password and its username in order to access anything in the cataloging directory via a web request.

9. Once the files have been created and saved, you'll run a configuration test to ensure Apache is configured correctly using `sudo apachectl configtest`.
	+ **NOTE:** Should any syntax errors exist in the code you provided in the config files, you should get an error message.
	
10. If you get a `Syntax OK` message after running the config test, you've configured everything correctly and can now restart Apache and check its status using the `systemctl restart` and `systemctl status` commands.

11. Next, grant the Apache2 web server account (`www-data`) permissions and ownership to the `cataloging` directory using the `chown` command to set it as the owner of `/var/www/html`, which is the parent directory of your `cataloging` one.
	
12. Finally, you'll set the `setgid bit` on the above parent directory, making it so that any new or changed files inherit the appropriate levels of group ownership. To do so, you'll use the `chmod` command to change the modification specifications for the directory.

13. Now that everything has been configured, you can use a web browser and your server's IP address to access the cataloging module, provide the login credentials, and start cataloging.

## __*Wrap-Up and Further Insight*__

+ It's important to recognize that this ILS structure is for demonstrative purposes and simplistic compared to it's real-world counterparts. In order to develop a more robust OPAC and cataloging module, one would need to add features such as (but not limited to):
	+ More in-depth search functionality;
	+ More data fields with less strict requirements;
	+ Authority control functionality;
	+ Increased security measures;
	+ Copy cataloging and record import abilities;
	+ Linked data capabilities;
	+ A connection to a circulation module.
Still, the basic structure here can provide a good starting point upon which one could build in order to build a more robust system.

+ While this document is laid out as instructions given in the second person perspective, it is based directly on my experience in carrying out this project. During my time developing this system, I heavily utilized the course textbook for the LIS624 course that the assignment is a part of. In particular, I followed along with the instructions given in the following chapters, as well as their accompanying lecture videos provided in the course shell:
	+ ["Introduction to Relational Databases"](https://cseanburns.github.io/systems-librarianship/5a-introduction-to-relational-databases.html)
	+ ["Creating a Bare Bones OPAC"](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html)
	+ ["Creating a Bare Bones Cataloging Module"](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html)  
	
Furthermore, I frequently found myself referring to both the textbook chapters on setting up the LAMP stack, and my previous documentation in this repo on that process. The `PHP.md` and `MySQL.md` files were especially useful as I worked with the PHP scripts for both forms, and created and edited the database in MySQL.

Outside of course related materials, the only external resource I used was this [SuperTokens blog post](https://supertokens.com/blog/password-hashing-salting) on hashed passwords.
