---
title: InstantCMS 2.16.1 Cross Site Scripting
published: true
---

- Exploit Title: InstantCMS - Store XSS
- Application: InstantCMS
- Version: v2.16.1
- Bugs: Stored XSS
- Technology: PHP
- Vendor Homepage: https://instantcms.ru/
- Software Link: https://instantcms.ru/get
- Date: 14.09.2023
- Author: SoSPiro
- Tested on: Windows

***

### Description

I noticed that you filtered the filter very carefully. But there are still some parts you missed


### POC

1. Login with admin
2. Go to "http://localhost/o2/admin/menu/item_edit/18"
3. Insert payload in CSS class
4. Click save , and go to home page, and Detect store xss in footer

[PoC-ViDeO](https://drive.google.com/file/d/1_9QGoBnbZZrsHUgNkujja1Ptj3f8fl2W/view?usp=sharing)

### Impact

This security vulnerability has the potential to steal multiple users' cookies, gain unauthorized access to that user's account through stolen cookies, or redirect the user to other malicious websites...

### Bug fix commit

[Bug fix commit](https://github.com/instantsoft/icms2/commit/b2172a0f842fc28966b00bab3e2e9094c6bfd156)

### Reference

[Reference](https://huntr.com/bounties/18546c85-de6a-4252-a02f-c9d26f4f775e/)
