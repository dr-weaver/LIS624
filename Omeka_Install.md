### __2026-04-12 - *Omeka Installation and Configuration*__

## __*Goal:*__  
Install and configure Omeka and begin to build a collection on it. This assignment is largely self guided and meant to test our ability to apply what we've learned in previous assignments.

## __*Steps:*__  

#### **Routine System Updates** 

1. Checked for potential updates for machine using `sudo apt update`

	- *12 packages available for updates*  

2. Updated packages using `sudo apt upgrade` and selecing `y` when asked to confirm.

3. No containers needed to be restarted after update.

4.  Checked for any potential clean up that could be done to machine files using `sudo apt autoremove` and `sudo apt clean`
  
	- *no files needed removed*  

#### **Prerequisites**  

1. Checked that current version of PHP meets Omeka requirements (v7.1 or higher) using `php --version`

	- *version 8.3.6 currently installed*

2. Checked that current version of MySQL meets Omeka requirements (v5.5.5 or higher) using `mysql --version`
	
	- *version 8.0.45 currently installed*

3. Confirmed that php modules `mysqli` and `exif` are installed using `php -m`

4. Installed `imagemagick` using `sudo apt install imagemagick`, confirming install with `y` when prompted.

	- *install completed successfully*
	
5. Enabled Apache `mod_rewrite` using `sudo a2enmod rewrite`
	
	- *installed, Apache2 restart recommended*
	
6. Restart Apache2 with `sudo systemctl restart apache2`
	
	- *no signs of error with restart*

#### **Creating a Database and User**

1. Logged into MySQL using `sudo mysql -u root`

2. Created new user called `omeka` using `create user 'omeka'@'localhost' identified by 'XXXX';`
	
	- **NOTE:** `XXXX` is a stand-in for the user password.
	- *received Query OK message; user created successfully*

3. Created new database titled `omeka` using command `create database omeka;`
	
	- *received Query OK message; database created successfully*

4. Granted `omeka` user all privileges to `omeka` database using `grant all privileges on omeka.* to 'omeka'@'localhost';`
	
	- *received Query OK message; permissions successfully granted.*
	
5. Checked database listings using `show databases;`; confirming that `omeka` database was there.

6. Exited MySQL using `\q`

#### **Downloading Omeka Classic** 

1. Changed to `/var/www/html` directory using `cd` command.

2. Downloaded latest version of Omeka Classic using `sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip`
	
	- *file saved as* `omeka-3.2.zip`
	
3. Unzipped Omeka install file using `sudo unzip omeka-3.2.zip`

	- *no apparent issues in unzip process*
	
4. Removed `omeka-3.2.zip` file to free up space using `sudo rm omeka-3.2.zip`

	- *checked successful removal using* `ls`*, file removed successfully*
	
5. Renamed `omeka-3.2` directory to `omeka` using `sudo mv /var/www/html/omeka-3.2 /var/www/html/omeka`
	
	- *checked with* `ls` *to confirm that name change was successful*

#### **Adding Database Credentials to Omeka**

1. Changed to `omeka` directory using `cd /var/www/html/omeka`
	

2. Opened `dp.ini` file to add MySQL database information using `sudo nano dp.ini`

3. Edited configuration file by adding the database information as follows:
	- Added `localhost` to `host` field
	- Added `omeka` to `username` field
	- Added `XXXXX` to `password` field (XXXX is stand-in for actual password in this document)
	- Added `omeka` to `database` field.

4. Saved changes and exited nano


#### **Configuring Apache Group Access for Omeka**

1. Changed ownership of `/omeka` using `sudo chmod -R g+w *` while still in the `omeka` directory.

	- *no apparent issue in change*

2. Went to server host in graphic browser using [server external IP](http://34.27.105.35/omeka/install/)

	- **ERROR 1:** Browser site claimed that there was an error with the following text:
	```
	Omeka  
	Installation Error  
	mod_rewrite is not enabled.  
	Apache's mod_rewrite extension must be enabled for Omeka to work properly. Please enable mod_rewrite and try again.
	```
3. Realized that error was likely due to issue with `apache2.conf` file (with help of post in LIS624 Teams group)

4. Used `cd` command to change to `/etc/apache2` directory.

5. Opened Apache2 config file using `sudo nano apache2.conf` and scrolled down to the access information for the `var/www` directory.

6. Added the following text beneath the script for the directory mentioned above:
	```
	<Directory /var/www/html/omeka/>
  Options Indexes FollowSymLinks
  AllowOverride All       
  Require all granted
	</Directory>
	```

7. Saved and exited `nano`

8. Restarted Apache2 using `sudo systemctl restart apache2`

9. Retested server by going back to [server external IP](http://34.27.105.35/omeka/install/) to check functionality.

	- *Solution worked, site loaded as intended*
	
#### __*Setting up Omeka and adding an item*__

1. Follow the prompts for installation by setting username, password, email, and item parameters.

	- *installation successful*
	
2. Added image to site following basic DublinCore prompts.

	- *item added successfully*

## __*Results:*__  
+ Omeka has  been set up and a photo has been uploaded.

## __*Notes:*__
+ Instructions for this process can be found in the LIS624 textbook chapter ["Install Omeka"](https://cseanburns.github.io/systems-librarianship/6b-install-omeka.html)

+ Omeka site can be reached [here](http://34.27.105.35/omeka)

## __*Reflection:*__  
+ **What confused me?**  
	- I wouldn't say that anything felt confusing, but only have vague instructions had me feeling a little more unsure throughout the process than I've felt with the last few exercises.
+ **What surprised me?**  
	- Nothing in particular!
+ **What did I break and how did I fix it?**  
	- There was an issue with getting the site to connect with Apache and thus register that `mod_rewrite` had been enabled.
		Fixed by updating the `apache2` config file.
+ **What would I do differently next time?**   
	- Nothing in particular, I felt that this went generally well.
+ **General**    
	- I'm curious to see if I did anything wrong without realizing it.
