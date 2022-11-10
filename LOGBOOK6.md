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

We don't know the exact inner workings, but our best informed guess is that this "%n" is either overwriting something needed for program execution, like the return address of the function that calls the myprintf() function, causing it to crash or attempting to write on the stack without permission to do so. (After task 3, we're more inclined to the first one).

### Task 2

#### Task 2a

In order to print the first 4 bytes of our input, we first need to define which input we're going to search for. In our case, we chose the hex string "DEADBEEF". <br> 
Afterwards, we need to calculate how many "%x" format specifiers are needed in order to get to our string. <br>
We did this via trial and error: first, we chose a big arbitrary number, like 100, and ran the program, finding our string on the output. Then we chose another number we thought would be close to the real number, like 50, and repeated the process. Continuing this process we eventually got 64 as an answer.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1040004247518982285/image.png">

#### Task 2b

In order to read the secret message, all we have to do is instert it's address (already given to us by the server) at the beginning of our payload and repeat the process of part a) of this task with a minor change: Instead of printing out 64 hexadecimal values, we'll print 63 of them and since the 64th is our secret message's address, if we pass that address to a "%s" format specifier, it will read the contents from there.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1040290663767490600/image.png">

### Task 3

#### Task 3a and 3b

Task 3b includes task 3a, so we'll explain both under the same section. <br>

In order to rewrite a stack value we can use the "%n" format specifier, that takes a pointer and writes the number of printed characters on that address.<br>

In order to rewrite the target variable's value, we would have to give it's address (given to us by the server) to this "%n". We can achieve this using the same method we used for task 2. <br>

After that, we just have to make sure all the printed chars add up to 0x5000 (20480 in decimal). The best way to do so is to divide 20480 by 63, which gives us 325.079, meaning we have to print x62 325 char long hex strings and x1 326 char long hex string (instead of 330, because we need to remove the 4 bytes of our addr string). Since the "%x" format specifier allows us to modify the amount of hex chars we want to print we can just add "%.325x" * 62 + "%.326x" to our string. 

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1040300445782003732/image.png">

## Week 6 CTF Challenge

WIP
