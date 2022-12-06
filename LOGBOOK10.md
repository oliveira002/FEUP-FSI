## Cross-Site Scripting Attack Lab

### Task 1
In this task we're asked to inject javascript code into a user's profile such that when that profile is loaded, an alert popup will show up with the message "XSS". <br>
In order to do so, firstly, we need to login into an user account. We chose Alice.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049736125780070481/image.png">

Then, we changed her Brief Description section to contain our javascript code: `<script>alert('XSS');</script>`

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049741154406633533/image.png">

Upon submiting the edit form, we're met with the expected popup, since the page reloads. Because the privacy of Alice's Brief Description section is set to public, any user that loads her profile should be met with the exact same popup.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049741154767347732/image.png">

### Task 2

### Task 3

### Task 4

## Week 10 CTF Challenge
### Challenge 1

### Challenge 2

