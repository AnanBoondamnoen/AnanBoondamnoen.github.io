![](Image/Code4Sec_Week/php_preg_match_01.png)

# PHP preg_match()

ฟังก์ชัน `preg_match()` ใช้ในการตรวจสอบข้อความตาม pattern ที่กำหนด (โดยคำนึงถึงตัวอักษรพิมพ์ใหญ่และพิมพ์เล็ก) ว่าตรงกันหรือไม่ และใช้ตัวแปร keep_array ในการเก็บข้อความที่พบด้วยฟังก์ชั่น preg_match() ฟังก์ชันนี้เสริม security อย่างไร? โดยฟังก์ชัน `preg_match()` จะเสริม security ของโปรแกรมในการตรวจสอบการป้อนข้อมูลของผู้ใช้งาน (Input Validation) ว่าอินพุตที่ถูกป้อนเข้ามานั้นมีรูปแบบตรงตาม pattern ที่เรากำหนดเอาไว้หรือไม่ ฟังก์ชันนี้เหมาะสำหรับป้องกันผู้ไม่ประสงค์ดีป้อนอินพุตที่เป็นรูปแบบคำสั่งต่างๆ เช่น คำสั่งในการ Injection ของ SQL Server หรือใช้ในการป้องกันผู้ใช้งานป้อนข้อความตรงตาม pattern ที่เรากำหนด เพื่อกรองข้อความหยาบคาย คำเสียดสีต่างๆ เป็นต้น โดยฟังก์ชัน `preg_match()` นั้นประมวลผลเร็วกว่าฟังก์ชั่น `ereg()`

## รูปแบบการเขียน (Syntax)
```
preg_match ( string $pattern , string $subject , array &$matches = null , int $flags = 0 , int $offset = 0 ) : int|false
```

## Parameters
- $pattern -> รูปแบบสตริงที่ใช้ในการค้นหา
- $subject -> อินพุตสตริง
- $matches -> ถ้ามีการ **matches** เกิดขึ้น มันจึงจะถูกเติมด้วยผลลัพธ์ของการค้นหา โดย `$matches[0]` จะบรรจุข้อความซึ่งตรงกันกับรูปแบบเต็ม และ `$matches[1]` จะมีข้อความซึ่งตรงกันกับรูปแบบย่อยในวงเล็บแรกซึ่งถูกจับได้
- $flags -> **flags** สามารถเป็นการรวมกันของ flags ดังต่อไปนี้:
    - **PREG_OFFSET_CAPTURE** -> ถ้าแฟล็กนี้ถูกส่ง สำหรับทุกๆการแมตช์ที่กำลังเกิดขึ้น ค่า appendant string offset (ในหน่วยไบต์) จะถูกส่งคืนด้วย
    - **PREG_UNMATCHED_AS_NULL** -> ถ้าแฟล็กนี้ถูกส่ง รูปแบบย่อยซึ่งไม่ถูกแมตช์จะถูกรายงานว่าเป็น null มิฉะนั้นจะถูกรายงานว่าเป็นสตริงว่าง
- $offset -> โดยปกติการค้นหาเริ่มจากจุดเริ่มต้นของสตริงหัวเรื่อง พารามิเตอร์ออฟเซ็ตที่เป็นทางเลือกสามารถถูกใช้เพื่อระบุตำแหน่งทางเลือกเพื่อเริ่มต้นการค้นหา (ในหน่วยไบต์)

## การคืนค่า
- คืนค่า **1** ถ้า `$pattern` แมตช์กับ `$subject` ที่ให้มา
- คืนค่า **0** ถ้า `$pattern` ไม่แมตช์กับ `$subject` ที่ให้มา
- คืนค่า **false** ถ้าความผิดพลาดได้เกิดขึ้น

## ตัวอย่างการใช้งานฟังก์ชัน `preg_match()`
### ตัวอย่างที่ 1: หาสตริงของข้อความ "#Code4Sec"
```
<?php
// The "i" after the pattern delimiter indicates a case-insensitive search
if (preg_match("/#Code4Sec/i", "AnanBoondamnoen #Code4Sec Week, #Day3 #NEIS0736 #NECS0736")) {
    echo "A match was found.";
} else {
    echo "A match was not found.";
}
?>
```
**Output: **
```
A match was found.
```

### ตัวอย่างที่ 2: หาคำว่า "Anan"
```
<?php
/* The \b in the pattern indicates a word boundary, so only the distinct
 * word "Anan" is matched, and not a word partial like "AnanBoondamnoen" */
if (preg_match("/\bAnan\b/i", "Anan Code4Sec Week, #Day3 #NEIS0736 #NECS0736")) {
    echo "A match was found.";
} else {
    echo "A match was not found.";
}
echo "\n";
if (preg_match("/\bAnan\b/i", "AnanBoondamnoen Code4Sec Week, #Day3 #NEIS0736 #NECS0736.")) {
    echo "A match was found.";
} else {
    echo "A match was not found.";
}
?>
```
**Output: **
```
A match was found.
A match was not found.
```

### ตัวอย่างที่ 3: การนำชื่อโดเมนออกจาก URL
```
<?php
// get hostname from URL
preg_match('@^(?:http://)?([^/]+)@i',
    "http://msit.mut.ac.th", $matches);
$host = $matches[1];

// get last two segments of hostname
preg_match('/[^.]+\.[^.]+\.[^.]+$/', $host, $matches);
echo "domain name is: {$matches[0]}\n";
?>
```
**Output: **
```
domain name is: mut.ac.th
```

### ตัวอย่างที่ 4: การใช้รูปแบบย่อยที่ถูกตั้งชื่อ
```
<?php
$str = 'AnanBoondamnoen: 6317810009';
/* This also works in PHP 5.2.2 (PCRE 7.0) and later, however 
 * the above form is recommended for backwards compatibility */
preg_match('/(?P<name>\w+): (?P<digit>\d+)/', $str, $matches);
print_r($matches);
?>
```
**Output: **
```
Array
(
    [0] => AnanBoondamnoen: 6317810009
    [name] => AnanBoondamnoen
    [1] => AnanBoondamnoen
    [digit] => 6317810009
    [2] => 6317810009
)
```

## Reference
- [https://www.php.net/manual/en/function.preg-match.php](https://www.php.net/manual/en/function.preg-match.php)
- [https://www.mindphp.com/%E0%B8%84%E0%B8%B9%E0%B9%88%E0%B8%A1%E0%B8%B7%E0%B8%AD/63-%E0%B8%9F%E0%B8%B1%E0%B8%87%E0%B8%81%E0%B9%8C%E0%B8%8A%E0%B8%B1%E0%B9%88%E0%B8%99-php/512-preg_match.html](https://www.mindphp.com/%E0%B8%84%E0%B8%B9%E0%B9%88%E0%B8%A1%E0%B8%B7%E0%B8%AD/63-%E0%B8%9F%E0%B8%B1%E0%B8%87%E0%B8%81%E0%B9%8C%E0%B8%8A%E0%B8%B1%E0%B9%88%E0%B8%99-php/512-preg_match.html)