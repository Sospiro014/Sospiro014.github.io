---
title: Online-Library-Management-System-3-Password-Reset
published: true
---
- Exploit Title: Online Library Management System v3 - Password Reset and Email Matching Vulnerability
- Date: 12.09.2023
- Exploit Author: SoSPiro
- Vendor Homepage: https://phpgurukul.com/
- Software Link: https://phpgurukul.com/online-library-management-system/
- Version: v3
- Tested on: Windows 10 Pro 64 Bit  + Wampserver V3.3
- CVE: N/A

***

### Description:
This report outlines a security vulnerability present in the web application called [Application Name]. This vulnerability allows users to create multiple accounts with the same email address and use that email address during the password reset process. This situation can compromise the security of user accounts.

### Risk Level:
This vulnerability can lead to serious security risks, including unauthorized access to user accounts and identity theft. It has the potential to have a significant impact.

### Step-by-Step Description:
1. User creates an account as "user1," with the email address set as "user@gmail.com."
2. User creates another account as "user2" and uses the same email address, "user@gmail.com."
3. User2 initiates a password reset process when forgetting the password.
4. As a result of the password reset process, the password for the "user1" account is also reset and can be controlled by "user2."