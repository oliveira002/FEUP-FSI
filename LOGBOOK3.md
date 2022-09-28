
# Trabalho realizado na Semana #3

##Identification:
-CVE-2021-40444 is a Microsoft Office MSHTML Remote Code Execution Vulnerability that requires no macros and only a single approval to “display content”.
-The exploit is based on a specially-crafted object, which if opened by the user will make Microsoft Office download and execute a malicious script.
-This vulnerability was also considered as a zero day vulnerability.


##Cataloguing:
-According to cve.mitre.org, the vulnerability was reported on 02/09/2021, but was only officially recognized by Microsoft on 07/09/2021.
-Has a CVSS v3.0 score of 8.8 (High)
-Since it was allegedly found by Microsoft, no bounty was collected

##Exploit:
-This zero day vulnerability is a remote code execution vulnerability in MSHTML, which is an Internet Browser engine used by Microsoft Office programs to work with web content.
-In order to take advantage of the vulnerability, attackers embed a special object in the document containing an URL to the malicious script.
-The attacking code dynamically creates a new HTML file ActiveX object in-memory, which allows the attacker to perform malicious actions on the victim’s computer.


##Attacks:
-GitHub user klezVirus managed to weaponize CVE-2021-40444, using only a few python scripts. Available at: https://github.com/klezVirus/CVE-2021-40444
-This was the same method used to attack the Ukrainian government from March to July of 2022, according to SOQ Prime, spreading Cobalt Strike Beacon Malware.
-Microsoft Threat Intelligence Center reported that, in August 2021, there were attempts at exploiting the remote code execution vulnerability.
-After public disclosure, there was a spike in attempts at exploitation. 


##CTF - Sanity Check
