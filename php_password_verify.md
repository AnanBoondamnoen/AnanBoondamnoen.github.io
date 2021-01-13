![](Image/Code4Sec_Week/php_password_verify_01.png)

# PHP password_verify()

ฟังก์ชัน `password_verify()` ใช้ในการตรวจสอบว่ารหัสผ่านตรงกับค่าแฮชหรือไม่

## รูปแบบการเขียน (Syntax)
```
password_verify ( string $password , string $hash ) : bool
```
โปรดสังเกตว่าฟังก์ชัน password_hash() นั้นส่งคืน algorithm, cost และ salt ในฐานะที่เป็นส่วนของแฮชซึ่งถูกส่งคืน ดังนั้นข้อมูลทั้งหมดซึ่งจำเป็นเพื่อการตรวจสอบแฮชจะถูกรวมอยู่ภายในนั้น สิ่งนี้อนุญาตให้ฟังก์ชั่นตรวจสอบยืนยันแฮชโดยไม่จำเป็นต้องมีพื้นที่จัดเก็บแยกต่างหากสำหรับข้อมูล salt หรือ algorithm
ฟังก์ชันนี้ปลอดภัยต่อการโจมตีแบบ timing attacks

## Parameters
- $password -> รหัสผ่านของผู้ใช้งาน
- $hash -> แฮชซึ่งถูกสร้างโดยฟังก์ชัน password_hash()

## การคืนค่า
- คืนค่า **true** ถ้ารหัสผ่านและแฮชตรงกัน
- คืนค่า **false** ถ้ารหัสผ่านและแฮชไม่ตรงกัน

## ตัวอย่างการใช้งานฟังก์ชัน `password_verify()`
### ตัวอย่างที่ 1: ตรวจสอบรหัสผ่านของผู้ใช้งานว่าตรงกับแฮชหรือไม่
```
<?php
// $hash1 คือ แฮชของรหัสผ่าน(สตริงคำว่า 'Anan Boondamnoen')ของผู้ใช้งาน ซึ่งถูกสร้างโดยฟังก์ชัน password_hash()
// $hash2 คือ แฮชของรหัสผ่าน(สตริงคำว่า '#Code4Sec Week, #Day4 #NEIS0736 #NECS0736')ของผู้ใช้งาน ซึ่งถูกสร้างโดยฟังก์ชัน password_hash()
$hash1 = '$2y$10$qjIawLlyL8B1YbCmFwYLNOaWl337eTCV.1vFi4aRIJ/twZ6UjhRfm';
$hash2 = '$2y$10$8LZL1jEClXbIkrxymZVFtu58wZpe76CaFKjrhORCbaJ8re37Ww3ha';
if (password_verify('Anan Boondamnoen', $hash1)) {
    // echo 'Password is valid!';
    echo 'Password \'Anan Boondamnoen\' and $hash1 is match!';
} else {
    // echo 'Invalid password.';
    echo 'Password \'Anan Boondamnoen\' and $hash1 is not match!';
}
echo "\n";
if (password_verify('#Code4Sec Week, #Day4 #NEIS0736 #NECS0736', $hash2)) {
    // echo 'Password is valid!';
    echo 'Password \'#Code4Sec Week, #Day4 #NEIS0736 #NECS0736\' and $hash2 is match!';
} else {
    // echo 'Invalid password.';
    echo 'Password \'#Code4Sec Week, #Day4 #NEIS0736 #NECS0736\' and $hash2 is not match!';
}
?>
```
**Output: **
```
Password 'Anan Boondamnoen' and $hash1 is match!                                                                                                     
Password '#Code4Sec Week, #Day4 #NEIS0736 #NECS0736' and $hash2 is match!
```

## Reference
- [https://www.php.net/manual/en/function.password-verify.php](https://www.php.net/manual/en/function.password-verify.php)
