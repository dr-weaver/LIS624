### __2026-03-08 - *Configuring Apache with PHP*__

## __*Goal:*__  
Install and configure PHP.  
Familiarize with server-side language and how it interacts with the browser end of things.


## __*Context:*__  
Required for LIS624.  
Most people are familiar with client-side programming languages like JavaScript, which rely more heavily on letting the browser do a lot of the work. 
Server-side languages, like PHP, don't directly translate in a graphical browser view in the same way, but instead rely on the web server (in this case Apache) to bridge that translation gap.  
Because of this, server-side languages have to be configured within the web browser in order to make them work correctly.  
PHP is used to work with databases to create data-driven content, making it helpful in many scenarios.


## __*Steps:*__  
#### **Installing Packages**  
1.  Install php and libapache2-mod-php packages using the following command:  
```
sudo apt install php libapache2-mod-php
```  
-*Install was completed successfully*  

2. Restart system using the following command:  
```
sudo systemctl restart apache2
```  
-*Restart completed without issue*  

3. Check the status and version of PHP using `php -v` 

-*Version noted as 8.3.6, appears to be up to date.*  

4. Check status of apache2 using `systemctl status apache2`  

-*apache2 cited as enabled and running.*  

#### **Check Installation Functionality**  
1. Create PHP file in web document root using the following commands:  
```
cd /var/www/html/  
sudo nano info.PHP
```  
-*Commands ran without any apparent issues.*  

2. Add following text to command file:  
```
<?PHP
phpinfo();
?>
```

-*Text added, file saved successfully.*  

3. Visit file from browser using VM public IP: `http://35.238.136.131/info.php`

-*File generated in browser successfully.*  

4. Remove info file using `sudo rm /var/www/html/info,php` due to systems info being displayed in it.  

-*File removed, checked with `ls` command that it was no longer listed in directory.*  

#### **Configurating Apache to prioritize PHP files**  
+ *Note:
Apache defaults to prioritizing HTML when no file type is specified in a URL.  
Because of this, I need to configure it to prioritize PHP instead by editing the `dir.conf` file.*   

1. Create a backup of the `dir.conf` file before modification using the following commands:  
```
cd /etc/apache2/mods-available/
sudo cp dir.conf dir.conf.bak
```  
-*Back up successfully created, evident by listing for it in directory.*  

2. Open the dir.conf in nano using `sudo nano dir.conf`.  
    Change text so that `index.php` is the first line and save.  

-*Changes implemented and saved correctly.* 

3. Check configuration after changes using `apachectl configtest`.  

-*Configtest successful, received `Syntax Ok` message.*  

4. Reload Apache configuration and check status using the following commands:  
```
sudo systemctl reload apache2  
systemctl status apache2
```
-*System confirmed to be enabled and active.*  

#### **Creating index.php file**  

1. Change back to Apache document root directory using `cd` and create file using:
```
cd /var/www/html/
sudo nano index.PHP
```  
-*File opened in nano successfully.*  

2. Create file utilizing HTML and PHP, including PHP that functions as a browser detector.  
-*Code copied from course textbook, link to chapter will be included in Notes.*  

3. Test success by using browser to access VM public IP without file type specification.  
-*Public IP navigates to index.php file.*  


## __*Results:*__  
+ Apache has been installed and correctly configured to default to PHP files when opening server in browser.

## __*Notes:*__
+ Textbook chapter on PHP configuration can be found [here](https://cseanburns.github.io/systems-librarianship/4b-installing-configuring-php.html)

## __*Reflection:*__  
+ **What confused me?**  
	The process wasn't confusing, but I don't feel like I have a strong grasp on what PHP actually *is*.  
	Each step made sense, but my mind is still trying to consolidate the "bigger picture".
+ **What surprised me?**  
	Honestly, the fact that the whole thing felt so intuitive and smooth was the most surprising part.  
+ **What did I break and how did I fix it?**  
	Nothing seems to be broken!  
+ **What would I do differently next time?**  
	I think I would like to read through the process again without a deadline looming behind me.  
	I feel like I generally know what I did, but not at the level of depth that I would like.  
+ **General** 	
	I don't feel like my understanding of this process is as concrete as I'd like, although I'm not sure why.  
	It's like I can tell there's a missing element in my understanding, but I can't pinpoint what that is.  
	I'm hoping that next week's exercises will build on this, which will sort of fill in those missing pieces.
