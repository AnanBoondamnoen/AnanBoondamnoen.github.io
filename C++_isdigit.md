![](Image/Code4Sec_Week/C++_isdigit_01.png)

# C++ isdigit()

ฟังก์ชัน `isdigit()` ใช้สำหรับตรวจสอบว่าค่าอาร์กิวเมนต์ที่ถูกส่งเข้ามานั้นเป็นอักขระตัวเลขหรือไม่ โดยถ้าใช่จะคืนค่าเป็น ค่าที่ไม่ใช่ศูนย์ (non-zero values) แต่ถ้าไม่ใช่จะคืนค่าเป็น 0 ตัวอย่างอักขระที่ไม่ใช่ตัวเลขก็เช่น ตัวอักษร (A-Z, a-z หรือ อักขระ Unicode) ช่องว่าง (space) ! # % & ? และอักขระอื่นๆ ที่นอกเหนือจากตัวเลข โดยฟังก์ชัน `isdigit()` จะเสริม security ของโปรแกรมในการตรวจสอบการป้อนข้อมูลของผู้ใช้งาน (Input Validation) ว่าอินพุตที่ถูกป้อนเข้ามานั้นเป็นรูปแบบที่ถูกกำหนดเอาไว้หรือไม่ เช่น โปรแกรมรับค่าหมายเลขบัตรเครดิต หรือโปรแกรมรับค่าหมายเลขโทรศัพท์มือถือ ซึ่งจะมีแต่อักขระตัวเลขเท่านั้น หากผู้ใช้งานหรือผู้ไม่ประสงค์ดีกรอกอักขระตัวอักษรหรืออักขระพิเศษเข้ามา ฟังก์ชันนี้จะสามารถตรวจสอบความถูกต้องของอินพุตให้ได้

## รูปแบบการเขียน (Syntax)
```
int isdigit(int argument);
```
ถูกกำหนดไว้ในไฟล์ส่วนหัว `cctype`

## Parameters
- argument -> อักขระที่จะตรวจสอบ

## การคืนค่า
- คืนค่า **non-zero values** ถ้าอาร์กิวเมนต์เป็นอักขระตัวเลข
- คืนค่า **0** ถ้าอาร์กิวเมนต์ไม่เป็นอักขระตัวเลข

## ตัวอย่างการใช้งานฟังก์ชัน `isdigit()`

### ตัวอย่างที่ 1: ตรวจสอบว่าอักขระทั้งหมดเป็นอักขระตัวเลขหรือไม่
```
#include <cctype>
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
    int CountIfNotDigit1 = 0;
    char text1[] = "AnanBoondamnoen"; //อักขระทั้งหมดในสตริงเป็นตัวอักษร
    for(int i=0; i<strlen(text1); i++) {if(isdigit(text1[i])!=0){} else{CountIfNotDigit1++;}}
    if(CountIfNotDigit1==0){printf("\"AnanBoondamnoen\" is isdigit()\n");} else{printf("\"AnanBoondamnoen\" is not isdigit()\n");}
    
    int CountIfNotDigit2 = 0;
    char text2[] = "6317810009"; //อักขระทั้งหมดในสตริงเป็นตัวเลข
    for(int i=0; i<strlen(text2); i++) {if(isdigit(text2[i])!=0){} else{CountIfNotDigit2++;}}
    if(CountIfNotDigit2==0){printf("\"6317810009\" is isdigit()\n");} else{printf("\"6317810009\" is not isdigit()\n");}
    
    int CountIfNotDigit3 = 0;
    char text3[] = "Code4Sec"; //อักขระในสตริงเป็นตัวอักษรผสมตัวเลข
    for(int i=0; i<strlen(text3); i++) {if(isdigit(text3[i])!=0){} else{CountIfNotDigit3++;}}
    if(CountIfNotDigit3==0){printf("\"Code4Sec\" is isdigit()\n");} else{printf("\"Code4Sec\" is not isdigit()\n");}
    
    int CountIfNotDigit4 = 0;
    char text4[] = "63178 10009"; //อักขระในสตริงมีอักขระช่องว่าง (space)
    for(int i=0; i<strlen(text4); i++) {if(isdigit(text4[i])!=0){} else{CountIfNotDigit4++;}}
    if(CountIfNotDigit4==0){printf("\"63178 10009\" is isdigit()\n");} else{printf("\"63178 10009\" is not isdigit()\n");}
    
    int CountIfNotDigit5 = 0;
    char text5[] = "çå"; //อักขระในสตริงเป็นอักขระ Unicode
    for(int i=0; i<strlen(text5); i++) {if(isdigit(text5[i])!=0){} else{CountIfNotDigit5++;}}
    if(CountIfNotDigit5==0){printf("\"çå\" is isdigit()\n");} else{printf("\"çå\" is not isdigit()\n");}

    return 0;
}
```
**Output: **
```
"AnanBoondamnoen" is not isdigit()
"6317810009" is isdigit()
"Code4Sec" is not isdigit()
"63178 10009" is not isdigit()
"çå" is not isdigit()
```

### ตัวอย่างที่ 2: ตรวจสอบว่าในสตริงมีอักขระตัวเลขอะไรบ้าง
```
#include <cctype>
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
    char str[] = "AnanBoondamnoen 6317810009 Code4Sec";

    cout << "The digit in the string are:" << endl;
    for (int i=0; i<strlen(str); i++)
    {
        if (isdigit(str[i]))
            cout << str[i] << " ";
    }

    return 0;
}
```
**Output: **
```
The digit in the string are:
6 3 1 7 8 1 0 0 0 9 4
```

## Reference
- [https://www.programiz.com/cpp-programming/library-function/cctype/isdigit](https://www.programiz.com/cpp-programming/library-function/cctype/isdigit)
- [https://www.geeksforgeeks.org/isalpha-isdigit-functions-c-example](https://www.geeksforgeeks.org/isalpha-isdigit-functions-c-example/)