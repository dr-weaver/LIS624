## __2026-03-22 - *Server Setup Documentation*__

### **Introduction**  
+ The following documentation breaks down the process I followed to develop a LAMP stack for my Systems Librarianship class.  

### **Finished Environment**
+ OS: Ubuntu 24.04.4 LTS
+ VM provider: Google Cloud
+ Web server: Apache 2.4
+ PHP Version: 8.3.6
+ MySQL version: 8.0.45
+ Testing Browser Used: Google Chrome

### **What is a LAMP Stack?**  
+ A LAMP stack is made up of four open source software components (Linux, Apache, MySQL, and PHP) that, when combined, can be used to host websites that utilize databases in some capacity. 
+ Since each piece of software is open-source and (for the most part) free. Furthermore, each element of a LAMP stack allows for a fairly high degree of customization, and have been around long enough that documentation on their use can be found easily online.  
These features make this type of structure especially accessible for anyone wanting to build a webserver or online database, making LAMP stacks popular for use in personal and professional projects alike.  
+ The different components of software each have their own role to play, which can be broken down as follows:  
	> Linux serves as an operating system;  
	Apache serves as an HTTP server;  
	MySQL serves as a database management system;  
	and PHP serves as a server-side programming language.
	>
When combined, all four components provide a well rounded, cohesive structure through which one can host a website with ease.

### **Installing and Configuring Apache**  

#### *Steps*
+ Below are the steps I used for installing and configuring Apache for use in my LAMP stack.  

1. Checked for and installed any available updates using:  
```
sudo apt update
sudo apt -y upgrade
```

2. Searched for apache using the `apt search apache2 | head` command, found `apache2`.

3. Installed using `sudo apt install apache2`, confirming installation with `y` when prompted.

4. Checked that `apache2` was running correctly using `systemctl status apache2`; confirmed that status showed it as Active and Loaded.

5. Installed `w3m` as a text-based browser using `sudo apt install w3m`.

6. Checked the functionality of the web server by using the command `w3m localhost` to visit my default site, which returned the correct response of:
> Apache2 Ubuntu Default Page  
It works!
>  

7. Navigated to document root using in order to back-up the `index.html` file in the document root and renamed it to `index.original.html` using the following commands:
```
cd /var/www/html/
sudo mv index.html index.original.html
```

8. Confirmed that files displayed correctly by using the `ls` command, the files listed correctly, displayed as seen below:
```
index.html  index.original.html  index.php  opac.php
```

9. Created new `index.html` file using `sudo nano index.html` and added some basic HTML code.  

10. After saving file changes, tested that changes were applied correctly by using the external IP address to visit my webpage in a graphical browser [here](http://35.202.204.12/index.html).  
 
#### *Issues*  
+ No real issues encountered during this process.


### **Installing and Configuring PHP with Apache** 
+ The following notes were adapted from the `PHP.md` file which is also located in this repository.

#### *Steps for PHP Installation and Solo Configuration*
1. Installed `php` and `libapache2-mod-php` packages using the following command, confirming installation with `y` when prompted:  
```
sudo apt install php libapache2-mod-php
```

2. Restarted `apache2` using `sudo systemctl restart apache2`; the restart completed without issue.

3. Checked the status and version of `php` using `php -v`, noting that it was enabled and running, and using version 8.3.6.

4. Checked the status and version of `apache2` using `systemctl status apache2`, noting that it was enabled and running, and using version 2.4.

5. Created a `PHP` test file in the web document root using the following commands:  
```
cd /var/www/html/  
sudo nano info.PHP
```

6. Added the following text to the `PHP` file:
```
<?PHP
phpinfo();
?>
```

7. Saved changes, confirmed successful implementation by visiting site through external IP.

8. After confirming that the file saved and loaded correctly, I removed it from my system using `sudo rm /var/www/html/info.php`

9. Confirmed removal of file using `ls` command and noting that it wasn't in the file directory.

#### *Steps for Configuring PHP with Apache*
1. Navigated to the `apache2` `mods` directory and created a backup of the `dir.conf` file using:
```
cd /etc/apache2/mods-available/
sudo cp dir.conf dir.conf.bak
```

2. Opened the dir.conf in `nano` using `sudo nano dir.conf` and changed the text so that `index.php` is the first line; saved changes.

3. After saving changes, checked configuration with `apachectl configtest`, receiving a message stating `Syntax Ok` to confirm success.

4. Reloaded `apache2` using `sudo systemctl reload apache2` then confirmed that system was enabled and active using `systemctl status apache2`.

5. Navigated to the root document using `cd /var/www/html/` and created a new php index file using `sudo nano index.php`.

6. Added the following code, which allows site to function as a browser detector, to the new `index.php` file:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Browser Detector</title>
</head>
<body>
    <h1>Browser & OS Detection</h1>
    <p>You are using the following browser to view this site:</p>

    <?php
    $user_agent = $_SERVER['HTTP_USER_AGENT'];

    // Browser Detection
    if (stripos($user_agent, 'Edg') !== false || stripos($user_agent, 'Edge') !== false) {
        $browser = 'Microsoft Edge';
    } elseif (stripos($user_agent, 'Firefox') !== false) {
        $browser = 'Mozilla Firefox';
    } elseif (stripos($user_agent, 'Chrome') !== false && stripos($user_agent, 'Chromium') === false) {
        $browser = 'Google Chrome';
    } elseif (stripos($user_agent, 'Chromium') !== false) {
        $browser = 'Chromium';
    } elseif (stripos($user_agent, 'Opera Mini') !== false) {
        $browser = 'Opera Mini';
    } elseif (stripos($user_agent, 'Opera') !== false || stripos($user_agent, 'OPR') !== false) {
        $browser = 'Opera';
    } elseif (stripos($user_agent, 'Safari') !== false && stripos($user_agent, 'Chrome') === false) {
        $browser = 'Safari';
    } else {
        $browser = 'Unknown Browser';
    }

    // OS Detection
    if (stripos($user_agent, 'Windows') !== false) {
        $os = 'Windows';
    } elseif (stripos($user_agent, 'iOS') !== false || stripos($user_agent, 'iPhone') !== false || stripos($user_agent, 'iPad') !== false) {
        $os = 'iOS';
    } elseif (stripos($user_agent, 'Android') !== false) {
        $os = 'Android';
    } elseif (stripos($user_agent, 'Mac') !== false || stripos($user_agent, 'Macintosh') !== false) {
        $os = 'Mac';
    } elseif (stripos($user_agent, 'Linux') !== false) {
        $os = 'Linux';
    } else {
        $os = 'Unknown OS';
    }

    // Output Result
    echo "<p>Your browser is <strong>$browser</strong> and your operating system is <strong>$os</strong>.</p>";
    ?>

</body>
</html>
```

7. Saved changes and verified successful implementation by using the system's external IP to visit the site [here](http://35.202.204.12/index.php).

#### *Issues*  
+ Once again, no real issues were encountered during this exercise.

### **Installing and configuring MySQL with PHP and Apache**
+ The final step in completing the LAMP stack. The following notes were adapted from the `MySQL.md` file that can also be found in this repository.

#### *Steps for Installing and Configuring MySQL*
1. Given that this was a new instance of work, checked for and installed updates for the system with the following commands, confirming installation with `y` when prompted:
```
sudo apt update
sudo apt upgrade
sudo apt autoremove
sudo apt clean
```  

2. Installed MySQL commmunity server using `sudo apt install mysql-server`

3. Checked the version of `mysql` using `mysql --version`; version stated as 8.0.45.

4. Checked status of `mysql` using `systemctl status mysql`; system was enabled and running.

5. Ran post installation security check using `sudo mysql_secure_installation`; security check required multiple prompt responses, summarized as follows:
 >Validate password component?: yes; set to low.  
 Remove anonymous users?: yes.  
 Disallow root login remotely?: yes.  
 Remove test database and access?: yes.  
 Reload privilege tables now?: yes.
 >
 
6. Logged in to MySQL using `sudo mysql -u root` and displayed databases using `show databases;`, which provided the following list:
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

7. Exited `mysql` using `\q`

#### *Steps for Configuring Account and Database in MySQL*
1. Logged back into `mysql` and successfully created new user with the following commands:
```
sudo mysql -u root
create user 'opacuser'@'localhost' identified by 'XXXXXXXX';
```

2. Created `opacdb` database using the following command:
```
create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;
```

3. Confirmed that database was created successfully by using `show databases;` and finding it listed in directory, which was printed as follows:
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

4. Granted all privileges on `opacdb` to `opacuser` with the command seen below, `Query Ok` response confirmed successful implementation.
```
grant all privileges on opacdb.* to 'opacuser'@'localhost';
``` 

5. Exited MySQL with `\q`, then added code through `bash` shell to increase functionality of MySQL client as follows:
	- Opened text file in nano using `nano ~/.bashrc`  
	- Added the following code at the bottom of the file `export MYSQL_PS1="[\d]>"  
	- Saved and exited file, typed `source ~/.bashrc`


+ **Note:** *At this point, I worked in `mysql` as `opacuser` to create some tables for the `opacdb` database.*  
*Since this process is not technically related to configuration of the software itself, it will not be included for the sake of brevity.*  
*Details on this process, however, can be found in the `MySQL.md` file as well as the `MySQL-books-testcmnd.md` file on this repository.*

#### *Steps for Configuring Relationship with PHP and Testing PHP Scripts*
1. Installed PHP support using `sudo apt install php-mysql`, completed successfully.

2. Restarted `apache2` using `sudo systemctl restart apache2`, restart successful.

3. Restarted `mysql` using `sudo system restart mysql`, restart successful.

4. Created login.php file in `/var/www` and opened it in `nano` using the following:
```
cd /var/www
sudo touch login.php
sudo chmod 640 login.php
sudo chown :www-data login.php
ls -l login.php
sudo nano login.php
```

5. Added the following code to `login.php` in order to change ownership and permissions:
```
<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "XXXXXXXXX";
?>
```

6. Changed to `/html/` directory using `cd /var/www/html`, confirmed location change with `pwd`

7. Opened new file in `nano` using `sudo nano opac.php`, adding the following code:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MySQL Server Example</title>
</head>
<body>

    <h1>A Basic OPAC</h1>
    <p>We can retrieve all the data from our database and book table using a couple of different queries.</p>

    <?php
    // Load MySQL credentials securely
    require_once '/var/www/login.php';

    // Enable detailed MySQL error reporting
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

    // Establish database connection
    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);

    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    echo "<h2>Query 1: Retrieving Publisher and Author Data</h2>";

    // Query using prepared statement
    $stmt = $conn->prepare("SELECT publisher, author FROM books");
    $stmt->execute();
    $result = $stmt->get_result();

    while ($row = $result->fetch_assoc()) {
        echo "<p>Publisher " . htmlspecialchars($row["publisher"]) .
             " published a book by " . htmlspecialchars($row["author"]) . ".</p>";
    }

    $stmt->close();

    echo "<h2>Query 2: Retrieving Author, Title, and Date Published Data</h2>";

    $stmt2 = $conn->prepare("SELECT author, title, copyright FROM books");
    $stmt2->execute();
    $result2 = $stmt2->get_result();

    while ($row = $result2->fetch_assoc()) {
        echo "<p>A book by " . htmlspecialchars($row["author"]) .
             " titled <em>" . htmlspecialchars($row["title"]) .
             "</em> was released in " . htmlspecialchars($row["copyright"]) . ".</p>";
    }

    $stmt2->close();
    $conn->close();
    ?>

</body>
</html>

```

8. Tested for errors in `login.php` with `sudo php -f /var/www/login.php`, results were successful.

9. Tested for errors in `opac.php` using `sudo php -f /var/www/html/opac.php`
+ **Issue 1** Running the above command resulted in the following error message, indicating syntax error on file line 35:
```
PHP Parse error:  syntax error, unexpected token "<" in /var/www/html/opac.php on line 35
```

10. **Resolved Issue 1** by opening the file with `nano` using `sudo nano opac.php` and fixing syntax error, which will be explained in next section.

11. Saved changes and exited `nano`

12. Re-ran `sudo php -f /var/www/html/opac.php` to check for further errors, none found and html code was printed as expected.

13. Ensured that website display was correct by using external IP address in graphical browser to view the page [here](http://35.238.136.131/opac.php)

#### *Issues*
1. Combined the following lines of code:
```
echo "<p>Publisher " . htmlspecialchars($row["publisher"]) .
" published a book by " . htmlspecialchars($row["author"]) . “.</p>";
```

Into the following:
```
	echo "<p>Publisher " . htmlspecialchars($row["author']) . “.</p>";
```

+ This caused a typo where `"author"` became `"author'`, and resulted in a syntax error as the incorrect code did not contain a complete query.
+ Issue fixed by re-typing both lines correctly in `nano`. Confirmed fixed with successful completion of `sudo php -f /var/www/html/opac.php` command.
