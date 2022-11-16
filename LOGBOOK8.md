## SQL Injection Attack Lab

### Task 1

### Task 2

### Task 3

## Week 8 CTF Challenge
### Challenge 1

In this challenge, we're told that if we login into the ctf-fsi.fe.up.pt:5003 website using the admin username, we'll get access to the flag. <br>
Glancing over the part of the index.php code that queries the database, we noticed that it was vulnerable to SQL Injection attacks. <br>
Since the inputs are not sanitized whatsoever, one could build a string such that the query asks the database for all the information of the admin user, without checking if the password is valid.

<img src="https://cdn.discordapp.com/attachments/1021902913079103488/1042503044295819384/image.png">

Now that we know this, we need to build our input to exploit this vulnerability. <br>
- There's many things we can do:
    - Cut the query short, selecting only the username = 'admin' and discarding the password
        - Inputs:
        - username: admin'; 
        - password (can be anything): abc 
    - Change the query to something similar to "Select * from user where username = 'admin' OR True;"
        - Inputs:
        - username: admin' or '=
        - password (can be anything): abc

In both cases, the main difficulty in building the input is the number of quotes we need to include, in order to match with other quotes present in the query string. <br>
All that's left to do is pwn the website, obtaining the flag (flag{21e977547fb42f61bb1c94035c0faf64}).

<img src="https://cdn.discordapp.com/attachments/1021902913079103488/1042509560226795620/image.png">
<img src="https://cdn.discordapp.com/attachments/1021902913079103488/1042508682845499453/image.png">



### Challenge 2
