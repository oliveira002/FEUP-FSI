## Public-Key Infrastructure (PKI) Lab

### Task 1

First, we copied the openssl.cnf config file to our working directory

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049810971654377522/image.png">

Then, we changed the config file, created the necessary auxiliary files and generated the self-signed CA file

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811174780313713/image.png">

Next, we ran the commands to look at the decoded content of the files:  `openssl x509 -in ca.crt -text -noout` and `openssl rsa -in ca.key -text -noout`

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811390870859837/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811637684666542/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811751874609202/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811913023967302/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049811973119938560/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049812044678963261/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049812128489545749/image.png">
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049812194801504276/image.png">

- Q: What part of the certificate indicates this is a CA’s certificate?
    - R: In the file ca.crt, the line that says `CA=TRUE`
<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049813061017546845/image.png">

- Q: What part of the certificate indicates this is a self-signed certificate?
    - R: The fact that the Authority (Issuer) Key Identifier is the same as the Subject Key Identifier

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1049813886381068338/image.png">

- Q: In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq. Please identify the values for these elements in your certificate and key files.
    - R: 

<img src=""> 

### Task 2

### Task 3

### Task 4

### Task 5

### Task 6

## Week 11 CTF Challenge
### Challenge 1

### Challenge 2
