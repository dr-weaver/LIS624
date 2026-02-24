# Repository for Systems Librarianship class
## Contents
+ Documentation outline
+ 2026-02-08 - *Repository Creation*
+ 2026-02-08 - *VM Connection*
+ 2026-02-14 - *grep*
+ 2026-02-22 - *Library searches with yaz*
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
