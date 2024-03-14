---
title: Membership Management System SQL injection + Insecure File Upload = Remote Code Execution
published: true
---

### Creating and operating a demo environment

[ -> link <- ](https://github.com/Sospiro014/zday1/tree/main/Membership_Management_System_demo)


### Vulnerability Description:
[ -> SQL injection <- ](https://sospiro014.github.io/Membership-Management-System-SQL-injection)

### File upload:

The provided code is part of a Membership Management System. It contains a vulnerability known as "Insecure File Upload." Insecure File Upload vulnerabilities arise when a web application allows users to upload files without proper validation and security measures. In this context, the system accepts file uploads for elements such as logos without adequate scrutiny, potentially allowing malicious files to be uploaded.


### Vulnerable Code Section:

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





