


## Week 4 CTF Challenge 
Steps:
- Open the website
- Go through the available options skimming for information
- On the WordPress Hosting product in the shop notice the additional information tab
- Check Additional information tab and find Wordpress version 5.8.1 and WooCommerce v5.7.1 and Booster for WooCommerce v5.4.3 plugins
- In the product reviews, note that there's an admin user, likely with full system permission
- On the footer of the website, notice that there's the phrase "Build with Storefront & WooCommerce", so they're probably using Wordpress v5.8.1 and the  WooCommerce v5.7.1 and Booster for WooCommerce v5.4.3 plugins. Confirmed via adding the sufix /feed to the url (http://ctf-fsi.fe.up.pt:5001/feed/) in <generator>https://wordpress.org/?v=5.8.1</generator>
- While searching for vulnerabilities in CVE databases for the different plugins versions, we noticed that for the WooCommerce Plugin v.5.4.3 there was a vulnerability that lets you bypass the authentication system via the password recovery process
- The corresponding CVE is CVE-2021-34646 (flag1: flag{CVE-2021-34646}) and in exploit-db there's a python script to exploit it (https://www.exploit-db.com/exploits/50299)
- Follow the instructions on the python script:
1.  Goto: http://ctf-fsi.fe.up.pt:5001//wp-json/wp/v2/users/
2. Find that the user id for the admin is 1
3. Attack with python ./50299.py http://ctf-fsi.fe.up.pt:5001/ 1
4. See which of the 3 links that were output work and allow to bypass the authentication.
- After clicking the correct link and being logged as an admin, go to http://ctf-fsi.fe.up.pt:5001/wp-admin/edit.php and find the private post with the second flag: flag{please don't bother me}
