![](Image/Code4Sec_Week/php_preg_match_01.png)

# PHP preg_match()

ฟังก์ชัน `preg_match()` ใช้ในการตรวจสอบข้อความตาม pattern ที่กำหนด (โดยคำนึงถึงตัวอักษรพิมพ์ใหญ่และพิมพ์เล็ก) ว่าตรงกันหรือไม่ และใช้ตัวแปร keep_array ในการเก็บข้อความที่พบด้วยฟังก์ชั่น preg_match() ฟังก์ชันนี้เสริม security อย่างไร? โดยฟังก์ชัน `preg_match()` จะเสริม security ของโปรแกรมในการตรวจสอบการป้อนข้อมูลของผู้ใช้งาน (Input Validation) ว่าอินพุตที่ถูกป้อนเข้ามานั้นมีรูปแบบตรงตาม pattern ที่เรากำหนดเอาไว้หรือไม่ ฟังก์ชันนี้เหมาะสำหรับป้องกันผู้ไม่ประสงค์ดีป้อนอินพุตที่เป็นรูปแบบคำสั่งต่างๆ เช่น คำสั่งในการ Injection ของ SQL Server หรือใช้ในการป้องกันผู้ใช้งานป้อนข้อความตรงตาม pattern ที่เรากำหนด เพื่อกรองข้อความหยาบคาย คำเสียดสีต่างๆ เป็นต้น โดยฟังก์ชัน `preg_match()` นั้นประมวลผลเร็วกว่าฟังก์ชั่น `ereg()`

## รูปแบบการเขียน (Syntax)
```
preg_match ( string $pattern , string $subject , array &$matches = null , int $flags = 0 , int $offset = 0 ) : int|false
```
Searches **subject** for a match to the regular expression given in **pattern**.

## Parameters
- $pattern - The pattern to search for, as a string.
- $subject - The input string.
- $matches - If **matches** is provided, then it is filled with the results of search. `$matches[0]` will contain the text that matched the full pattern, `$matches[1]` will have the text that matched the first captured parenthesized subpattern, and so on.
- $flags - **flags** can be a combination of the following flags:
    - PREG_OFFSET_CAPTURE - 
    - PREG_UNMATCHED_AS_NULL - 
- $offset - Normally, the search starts from the beginning of the subject string. The optional parameter **offset** can be used to specify the alternate place from which to start the search (in bytes).

## การคืนค่า
```
preg_match() returns 1 if the pattern matches given subject, 0 if it does not, or false if an error occurred.
```

## ตัวอย่างการใช้ `preg_match()`
### ตัวอย่างที่ 1 : หาสตริงของข้อความ "php"
```
<?php
// The "i" after the pattern delimiter indicates a case-insensitive search
if (preg_match("/php/i", "PHP is the web scripting language of choice.")) {
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

### ตัวอย่างที่ 2 : หาคำว่า "web"
```
<?php
/* The \b in the pattern indicates a word boundary, so only the distinct
 * word "web" is matched, and not a word partial like "webbing" or "cobweb" */
if (preg_match("/\bweb\b/i", "PHP is the web scripting language of choice.")) {
    echo "A match was found.";
} else {
    echo "A match was not found.";
}

if (preg_match("/\bweb\b/i", "PHP is the website scripting language of choice.")) {
    echo "A match was found.";
} else {
    echo "A match was not found.";
}
?>
```
**Output: **
```
A match was found.A match was not found.
```

### ตัวอย่างที่ 3 : 
```
<?php
// get host name from URL
preg_match('@^(?:http://)?([^/]+)@i',
    "http://www.php.net/index.html", $matches);
$host = $matches[1];

// get last two segments of host name
preg_match('/[^.]+\.[^.]+$/', $host, $matches);
echo "domain name is: {$matches[0]}\n";
?>
```
**Output: **
```
domain name is: php.net
```

### ตัวอย่างที่ 4 : 
```
<?php

$str = 'foobar: 2008';

preg_match('/(?P<name>\w+): (?P<digit>\d+)/', $str, $matches);

/* This also works in PHP 5.2.2 (PCRE 7.0) and later, however 
 * the above form is recommended for backwards compatibility */
// preg_match('/(?<name>\w+): (?<digit>\d+)/', $str, $matches);

print_r($matches);

?>
```
**Output: **
```
Array
(
    [0] => foobar: 2008
    [name] => foobar
    [1] => foobar
    [digit] => 2008
    [2] => 2008
)
```

## References
- [https://www.php.net/manual/en/function.preg-match.php](https://www.php.net/manual/en/function.preg-match.php)
- [https://www.mindphp.com/%E0%B8%84%E0%B8%B9%E0%B9%88%E0%B8%A1%E0%B8%B7%E0%B8%AD/63-%E0%B8%9F%E0%B8%B1%E0%B8%87%E0%B8%81%E0%B9%8C%E0%B8%8A%E0%B8%B1%E0%B9%88%E0%B8%99-php/512-preg_match.html](https://www.mindphp.com/%E0%B8%84%E0%B8%B9%E0%B9%88%E0%B8%A1%E0%B8%B7%E0%B8%AD/63-%E0%B8%9F%E0%B8%B1%E0%B8%87%E0%B8%81%E0%B9%8C%E0%B8%8A%E0%B8%B1%E0%B9%88%E0%B8%99-php/512-preg_match.html)
