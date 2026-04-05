# Repository for Systems Librarianship class
## Contents
+ Documentation outline
+ 2026-02-08 - *Repository Creation*
+ 2026-02-08 - *VM Connection*
+ 2026-02-14 - *grep*
+ 2026-02-22 - *Library searches with yaz*
+ 2026-02-24 - *System Reboot*
+ 2026-03-02 - *Working with Apache HTTP Server*
+ 2026-03-08 - *Update to Documentation Structure*
+ 2026-03-08 - *Configuring Apache with PHP*
+ 2026-03-15 - *MySQL*
+ 2026-03-22 - *Server Setup Documentation*
+ 2026-03-29 - *Basic ILS Creation*
+ 2026-04-01 - *Basic OPAC and Cataloging Module Creation - Reflection*
+ 2026-04-05 - *WordPress Installation and Configuration*
## Documentation outline:
A simple outline for documenting the work done on a project.  
The first line should be preceded with the appropriate heading level, depending on the structure of the file it is held within.

__YYYY-MM-DD - *Topic*__

+ Goal:  
+ Context:  
+ Steps:  
+ Results:  
+ Verification:  
+ Notes:

## __2026-02-08 - *Repository Creation*__

+ *Goal:*  
  Create a repository that will be used for LIS624
+ *Context:*  
  This is a required assignment for LIS624, as this repository will be used for the rest of the course.
+ *Steps:*  
  1. Create github account
  2. Create Repository
  3. Create README file for the repository
  4. Develop structure for README and subsequent documentation 
+ *Results:*  
  A repository has been created for future work for LIS624 and has a clear, organized structure for future documentation.
+ *Verification:*  
  Returning to the project several days after initial construction found that it was still in place as intended.
+ *Notes:*  
  - Documentation structure may change as the needs of the project become more clear.
  - [Section 2d] (https://cseanburns.github.io/systems-librarianship/2d-documenting-git-github-markdown.html) of the course text book outlines different code inputs for markdown language and can be referred to as needed.

## __2026-02-08 - *VM Connection*__

+ *Goal:*  
 Connect VM to git repo
+ *Context:*  
 This is a required assignment for LIS624. SSH connection must be established between the VM and repository in order to edit repo files within VM.
+ *Steps:*
 1. Configure git settings on VM.
 2. Create SSH key and add to github repository.
 3. Clone repo to VM.
 4. Stage, Commit, and Push repo.
+ *Results:*  
 VM and repo should now be connected, and files edited or created using the VM can be successfully pushed out to the repo.
+ *Verification:*  
 After completing each of the steps outlined, the repo is checked to see if the edits done with the VM command line are evident.
+ *Notes:*  
  - Text formatting is slightly off when done in VM. Need to continue to work with this at a later date to determine where issue lies.

## __2026-02-14 - *grep*__

+ *Goal:*  
Familiarize with searching files using grep.
+ *Context:*  
This is required work for LIS624. Searching files using grep can be very powerful/helpful.
+ *Steps:*
 1. Export BibTex file from Web of Science and upload it to the CLI.
 2. Review file using the "less" command to get a general idea of its contents.
 3. Use different grep commands and keys to explore the file in depth and famliarize with how different prompts can be utilized.
+ *Results:*  
I have a general understanding of how grep works, but still need to practice using it to become more proficient.
+ *Verification:*  
N/A
+ *Notes:*  
 - The breadth of grep’s capabilities makes it a bit daunting and hard to fully grasp.
 - I should try breaking up the commands into general concepts and slowly build up my understanding that way rather than try to tackle everything at once.

## __2026-02-22 - *Library searches with yaz*__

+ *Goal:*  
Familiarize with yaz and library searches.
+ *Context:*  
Required for week 6 of LIS624. In order to learn the “behind the scenes” of library search retrievals using z39.50 protocol.
+ *Steps:*
1. Download yaz and open yaz client.
2. Use the yaz client to establish a z39.50 connection with a library catalog
3. Search using various attributes and PQF operator phrases to find records for items.
4. Practice retrieving, viewing, and downloading records.
5. Practice formatting records to json via the jq command and editing records in nano.
+ *Results:*  
I became familiar with a new method of retrieving and viewing records. I also got some practice with json, which I haven’t used much yet. 
+ *Notes:*
- This was a lot of fun, and I can see it being helpful in a variety of ways.

## __2026-02-24 - *System Reboot*__

+ *Goal:*  
Learn how to perform a system restart.
+ *Context:*  
I got multiple restart suggestion messages during the module 3 check in process and learned the process for doing so.
+ *Steps:*
1. Type the command: sudo reboot.
2. Wait for reboot to happen, eventually you will get a message that the CLI shell lost connection.
3. Retry connection, you should be able to reauthorize and have things open up again as usual.
+ *Results:*  
I was able to learn a new command that will be helpful in the future.
+ *Notes:*
- Be sure to save any open documents before doing this, as it will results in the loss of any unsaved work.

## __2026-03-02 - *Working with Apache HTTP Server*__

+ *Goal:*  
Practice creating web pages with Apache.
+ *Context:*  
Required for LIS624 as a way to learn about web functionality and become familiar with web services.4
+ *Steps:*
1. Check system for updates, install any available.
2. Search for and install apache, ensure it is running correctly with systemctl command.
3. Install and open a text-based web browser, for this project I used w3m.
4. Check functionality of web server via both the loopback address and the external IP address associated with the VM.
5. Backup index.html file, then create a new one to work with in order to build web page.
6. Open new file in nano, write html code for web page.
7. Save changes, check that the page opens correctly in graphical browser.
+ *Results:*  
I was able to successfully create a simple html webpage.
+ *Verification:*  
I opened the external IP address for my VM, the changes I had made to the web page could be seen here.
+ *Notes:*  
It may be interesting to try out some of the html and css code from my LIS690 assignments, particularly the beetle web page I made, in this context.

## __2026-03-08 - *Update to Documentation Structure*__

In order to provide more in depth documentation, I will begin to change the way that this README file is used.  
Going forward, tasks will be added to the file with the same title format, but information in this file will be briefer.  
Instead, I will create a separate file that is dedicated to that specific tasks or topic, the name of which I will provide in the README entry.  
This should make it so that the README continues to be manageable as more entries are added, but that meaningful data is not lost for the sake of brevity.

## __2026-03-08 - *Configuring Apache  with PHP*__

#### *Context:*  
Required for LIS624. 
PHP is used to work with databases to create data-driven content. Since it's a server side language, it has to be configured in order to display appropriately in browser.  
Further documentation can be found in "PHP.md"

#### *Environment:*  
+ OS: Ubuntu 24.04.4 LTS  
+ VM provider: Google Cloud  
+ Web server: Apache2  
+ PHP version: 8.3.6  

#### *Steps Completed:*  
1. Installed php and apache2 packages.
2. Checked installation functionality.
3. Configured Apache to prioritize PHP files.
4. Created index.php file

#### *Issues Encountered:*
+ None!

#### *What I learned:*
+ I learned about the differences between client-side and server-side languages and how each is handled differently and used for different purposes.  
I also learned how PHP displays in browser, and the steps to configure a server to prioritize a non-default file type.  
Finally, I got more practice with using text editors and checking how changes look in browser view after being implemented from the server-side of things.

## __2026-03-15 - *MySQL*__

#### *Context:*
Required for LIS624. The final part of configuring a LAMP stack.  
MySQL is used for relational database management, and as frequently used for library management systems and other sites.  
Further documentation can be found in `MySQL.md`

#### *Environment:*
+ OS: Ubuntu 24.04.4 LTS  
+ VM provider: Google Cloud  
+ Web server: Apache2  
+ PHP Version: 8.3.6  
+ MySQL version: 8.0.45  

#### *Steps Completed:*
1. Installed and set up MySQL
2. Set up MyDQL regular user account
3. Created a practice database
4. Logged in as regular user and created a table
5. Tested SQL commands
6. Installed PHP and MySQL support
7. Created and tested PHP scripts

#### *Issues Encountered:*
+ Brief typos with code at the end of the exercise resulted in a syntax error that wouldn't allow for proper display.  
	Error fixed in nano; further info in `MySQL.md` file.

#### *What I learned:*
I learned how to work with databases and tables using MySQL in the CLI.  
I also learned how to connect MySQL with PHP, thus completing the LAMP stack I've been working on.

## __2026-03-22 - *Server Setup Documentation*__

#### *Context:*  
As we finish up the LAMP stack project in LIS624, we are required to create a specified document which detials the setup process.

#### *Results:*  
+ Given that this was just the development of written instructions, I felt that there wasn't much need to follow the normal structure in this file.
+ I wanted to include a brief mention in here, however, since the documentation will be included in the repository.
+ The file has been added as `ServerSetupDocumentation.md` and work in conjunction with the `PHP.md` and `MySQL.md` files.

## __2026-03-29 - *Basic ILS Creation*__

#### *Context:*
+ Required for LIS624. In this module, we worked to create a bare bones OPAC as well as a basic cataloging module. 
+ The work utilized the LAMP stack from the last module.
+ The steps listed here are meant to provide a very minimal overview, further details can be found in the `DIY_ILS.md` file.

#### *Environment:*
+ OS: Ubuntu 24.04.4 LTS  
+ VM provider: Google Cloud  
+ Web server: Apache2  
+ PHP Version: 8.3.6  
+ MySQL version: 8.0.45  

#### *Steps Completed:*
1. Practiced database and table creation, querying, and database privilege management in MySQL.

2. Created an HTML form to allow for searching, and PHP script that made it operable, resulting in ability to search `books` database from LAMP module.

3. Added additional titles to `books` database.

4. Created a cataloging interface to add more items to the `books` database that is connected to the OPAC that was created.

#### *Issues Encountered:*
+ Typo when created a database table resulting in syntax error; retyped command correctly to fix.

#### *What I learned:*
+ More information on creating and working with MySQL databases.
+ How to create a searchable, functional OPAC.
+ How to create a functional cataloging interface.
+ How to set authorization using apache2's `htpsswd` function.
+ How to connect all of the elements listed above.

## __2026-04-01 - *Basic OPAC and Cataloging Module Creation - Reflection*__

#### *Context:*
+ Required for LIS624. A reflection of the creation of a basic OPAC and cataloging module.

#### *Results:*
+ As with the module 4 LAMP reflection document, this is structured as a written document rather than a full exercise, meaning there doesn't feel like as much need to follow regular structure in this antry.
+ The file provides more functional context to the set up process, and can be used alongside the more procedural `DIY_ILS.md` file to give a bigger picture of the process used to create these ILS modules.
+ The document itself is titled `BasicILS_Reflection.md` and can be found in this repository. 

## __2026-04-05 - *WordPress Installation and Configuration*__

#### *Context:*
+ Required for LIS624. The first step in creating the mock library site.
+ In this exercise, we installed and configured WordPress so that it can serve as the main access point for our library sites.
+ More detailed documentation can be found in the `WordPress_Install.md` file.

#### *Environment:*
+ OS: Ubuntu 24.04.4 LTS  
+ VM provider: Google Cloud  
+ Web server: Apache2  
+ PHP Version: 8.3.6  
+ MySQL version: 8.0.45  

#### *Steps Completed:*
1. Checked for routine system updates, installed files.

2. Confirmed that system requirements were met to install WordPress, installed extra PHP modules to increase functionality.

3. Downloaded and extraceted WordPress installation file.

4. Created new MySQL database and user for project.

5. Configured WordPress config PHP file.

6. Used external IP to visit WordPress site in browser and finish login creation process.

#### *Issues Encountered:*
+ None

#### *What I learned:*
+ How to install and configure WordPress using a CLI
+ How to connect it to a specific database in MySQL
+ How to extract zip files in a CLI
