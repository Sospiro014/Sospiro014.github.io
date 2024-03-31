---
title: User Registration And Login And User Management System 3.1 SQL Injection
published: true
---

- Exploit Title: User Registration & Login and User Management System With admin panel 3.1 - SQL injection
- Application: User Registration & Login and User Management System
- Date: 17.02.2024
- Bugs: SQL Injection
- Exploit Author: SoSPiro
- Vendor Homepage: https://phpgurukul.com/
- Software Link: https://phpgurukul.com/user-registration-login-and-user-management-system-with-admin-panel/
- Tested on: Windows 10 64 bit Wampserver
- CVE : CVE-2024-28323
  
* * *

### Description:
The file bwdates-report-result.php contains a potential security vulnerability related to user input validation. The script retrieves user-provided date inputs without proper validation, making it susceptible to SQL injection attacks.


### Vulnerability Details:
- **Application Name**: User Registration & Login and User Management System
- **Software Link**: [Download Link](https://phpgurukul.com/user-registration-login-and-user-management-system-with-admin-panel/)
- **Vendor Homepage**: [Vendor Homepage](https://phpgurukul.com/)

- The vulnerability lies in the following code snippet:

```php
<?php
$fdate=$_POST['fromdate'];
$tdate=$_POST['todate'];

?>

<?php $ret=mysqli_query($con,"select * from users where date(posting_date) between '$fdate' and '$tdate'");
$cnt=1;
while($row=mysqli_fetch_array($ret))
{?>
```

The script directly uses the user-supplied $fdate and $tdate variables in an SQL query without validating or sanitizing the input. This creates a potential avenue for malicious users to manipulate the query and perform SQL injection attacks.


### Vulnerability Description:

An attacker can exploit this vulnerability by crafting malicious input for the fromdate and todate parameters. By injecting SQL code into these fields, an attacker could manipulate the query to perform unauthorized actions on the database, potentially exposing sensitive information or even modifying the database contents.


### Proof of Concept (PoC):
1. Visit the application locally at http://localhost/loginsystem/admin/ and login as admin
   admin user credentials which are installed by default

```plaintext
| Username | admin
| Password |Test@12345
```

2. Go to "B/w Dates Report": http://localhost/loginsystem/admin/bwdates-report-ds.php
3. Change the "To Datev" or "From Date" values. You can use this query by manipulating proxy tools like Burp Suite payload= "' OR '1'='1'; --".

- Reproduce:
- https://packetstormsecurity.com/files/177168/User-Registration-And-Login-And-User-Management-System-3.1-SQL-Injection.html
