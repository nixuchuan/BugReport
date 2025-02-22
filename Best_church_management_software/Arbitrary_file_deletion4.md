### Vulnerability file address

In line 56 of `church/admin/app/product.php`, `@unlink("../../assets/images/" . htmlspecialchars($_POST['old_photo_img'], ENT_QUOTES, 'UTF-8'));`,` $_POST['old_photo_img']` parameters have not been restricted, which causes any file to be deleted

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

        $filepath = "../../assets/images/" . htmlspecialchars($_FILES["banner"]["name"], ENT_QUOTES, 'UTF-8');
        if (move_uploaded_file($_FILES["banner"]["tmp_name"], $filepath)) {
            $img1 = htmlspecialchars($_FILES["banner"]["name"], ENT_QUOTES, 'UTF-8');
        } else {
            echo "Error !!";
        }

        $stmt = $conn->prepare("INSERT INTO `product`(`category`,`heading`, `shortcontent`, `content`,`photo`, `banner`,`metatitle`,`metadescription`,`keywords`,`robots`) VALUES (?,?,?,?,?,?,?,?,?,?)");

        $stmt->execute([
            htmlspecialchars($_POST['category'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['heading'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['shortcontent'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['content'], ENT_QUOTES, 'UTF-8'),
            $img,
            $img1,
            htmlspecialchars($_POST['metatitle'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['metadescription'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['keywords'], ENT_QUOTES, 'UTF-8'),
            htmlspecialchars($_POST['robots'], ENT_QUOTES, 'UTF-8')
        ]);

        $_SESSION['success'] = "success";

        header("location:../manage_product.php");
    }

    if (isset($_POST['update'])) {

        if ($_FILES['photo']['tmp_name'] != '') {
            $file_extension = pathinfo($_FILES["photo"]["name"], PATHINFO_EXTENSION);
            $new_filename = uniqid() . '.' . $file_extension;
            $filepath = "../../assets/images/" . $new_filename;

            if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
                $img = $new_filename;

                @unlink("../../assets/images/" . htmlspecialchars($_POST['old_photo_img'], ENT_QUOTES, 'UTF-8'));
            }
        }
```

### POC

```http

```

### Attack results pictures

First, we create a `1.txt` file in the `church/` directory

![image-20250222112132614](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502221121074.png)



Then we execute the above poc to attack

![image-20250222112350465](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502221123501.png)



Files are deleted after the attack is successful

![image-20250222112400130](https://raw.githubusercontent.com/nixuchuan/imgs/main/202502221124163.png)