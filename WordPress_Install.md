### __2026-04-05 - *WordPress Installation and Configuration*__

## __*Goal:*__  
Install and configure WordPress as first step in mock library site project. This will be the central hub of the site.

## __*Steps:*__  

#### **Routine System Updates** 

1. Checked for potential updates for machine using `sudo apt update`

	- *12 packages available for updates*  

2. Updated packages using `sudo apt upgrade` and selecing `y` when asked to confirm.

3. After installation, system recommended that a reboot be performed; initiated reboot using `sudo reboot`

	- *reboot performed successfully*

4.  Checked for any potential clean up that could be done to machine files using `sudo apt autoremove` and `sudo apt clean`
  
	- *no files needed removed*  

#### **Checking and Configuring Requirements**  

1. WordPress installation requires that system have PHP version 8.3 or greater installed; checked PHP version using `php --version`

	- *version 8.3.6 currently installed*

2. WordPress installation requires that system have MySQL version 8.0 or greater installed; checked MySQL version using `mysql --version`
	
	- *version 8.0 currently installed*

3. Installed additional php modules to ensure full functionality for WordPress using:
```
sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl
```

- When prompted: `y` to confirm.
	- *install completed successfully*

4. Restarted Apache2 and MySQL using:
```
sudo systemctl restart apache2  
sudo system restart mysql
```
	- *restart completed without issue*
	
#### **Downloading and Extracting WordPress** 

1. Changed to `/var/www/html` directory using `cd` command.

2. Downloaded latest version of WordPress using `sudo wget https://wordpress.org/latest.zip`

3. Installed unzip using `sudo apt install unzip`

4. Unzipped WordPress install file using `sudo unzip latest.zip`

	- *no apparent issues in unzip process*
	
5. Removed `latest.zip` file to free up space using `sudo rm latest.zip`

	- Checked successful removal using `ls`, *file removed successfully*

#### **Creating a Database and User**

1. Logged into MySQL using `sudo mysql -u root`

2. Created new user called `wordpress` using `create user 'wordpress'@'localhost' identified by 'XXXX';`
	- **NOTE:** `XXXX` is a stand-in for the user password.
	- *received Query OK message; user created successfully*

3. Created new database titled `wordpress` using command `create database wordpress;`
	- *received Query OK message; database created successfully*

4. Granted `wordpress` user all privileges to `wordpress` database using `grant all privileges on wordpress.* to 'wordpress'@'localhost';`
	- *received Query OK message; permissions successfully granted.*
	
5. Checked database listings using `show databases;`; confirming that `wordpress` database was there.

6. Exited MySQL using `\q`

#### **Configuring WordPress PHP Config File**

1. Changed to `wordpress` directory using `cd /var/www/html/wordpress`

2. Copied and renamed the WordPress configuration file then opened the new file using nano with the following set of commands:
```
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

3. Edited configuration file by adding the database information from the above section as follows:
	- Added `wordpress` to `DB_NAME` field
	- Added `wordpress` to `DB_USER` field
	- Added `XXXXX` to `DB_PASSWORD` field (`XXXX` is stand-in for actual password in this document)
	
4. Added `define('FS_METHOD','direct');` to the bottom of the configuration file to ensure WordPress had appropriate file writing permissions.

5. Saved changes and exited nano

#### **Running and Installing WordPress Script**

1. Went to server host in graphic browser using [server external IP](http://35.202.204.12/wordpress)

2. Followed installation prompts from site, selecting English as language and creating login credentials.
	- *login credentials created successfully*
	 

## __*Results:*__  
+ The initial WordPress site for my library website project has been successfully created.

## __*Notes:*__
+ Instructions for this process can be found in the LIS624 textbook chapter ["Install WordPress"](https://cseanburns.github.io/systems-librarianship/6a-install-wordpress.html)

## __*Reflection:*__  
+ **What confused me?**  
	- Nothing felt confusing for this project.
+ **What surprised me?**  
	- For some reason, I wasn't expecting the similarities between this and the OPAC development project.
+ **What did I break and how did I fix it?**  
	- No errors encountered.
+ **What would I do differently next time?**   
	- Honestly, this was a pretty straightforward project. For personal projects, I would likely change some defaults in the configuration file to deter security issues.
+ **General**    
	- This exercise will be helpful to look back on when working on my personal library project in the future.


	
