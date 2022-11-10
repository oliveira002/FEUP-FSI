## Format String Attack

### Task 1
In order to crash the program, we need to be able to stop or halt it's execution somehow. We know that there's one printf() function with a format string vulnerability, which we can explot in order to achieve this goal. <br>
As a first test, we decided to run the build_string.py script to generate the bad file with the string we want use and send that string to the server. <br>
This worked, but we were a bit confused as to why it did.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1039937650381049856/image.png">

Afterall, on the surface, it only looks like we were sending a random string with a few "%.8x" and a "%n" at the end. <br>
We didn't know what the "%n" did, but we knew the "%.8x" part would just print a bunch of random strings with 8 hexadecimal characters each, so we assumed the "%n" would give us something along those lines. <br>

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1039996281164152953/image.png">

Upon further investigation, we found out that "%n" is a string formater that takes a pointer as an argument and writes on that memory positon the amount of characters written or read by the functions printf and scanf, respectively, so we decided to send only the "%n" character to the program and it still worked. <br>

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1039997115570597919/image.png">

We don't know the exact inner workings, but our best informed guess is that this "%n" is either overwriting something needed for program execution, like the return address of the function that calls the myprintf() function, causing it to crash or attempting to write on the stack without permission to do so.

### Task 2

#### Task 2a



<img src="https://cdn.discordapp.com/attachments/799728570825179213/1040004247518982285/image.png">

## Week 6 CTF Challenge 
