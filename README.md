# ST67-Lab05-ESP32-Use-LED-Component

การ build ด้วย CMake ช่วยให้โปรเจค ESP32 สามารถดึง component จากที่ต่าง ๆ มาร่วมสร้างโปรแกรมได้ ทั้งนี้โดยการสร้างไฟล์ idf_component.yml ไว้ในโฟลเดอร์ Main


```mermaid
graph RL
  A[(LED Component)] --> |idf_component.yml| B(ESP32 Project) 
  linkStyle 0   stroke-width:2px,  fill:none,stroke:red; 
```

การดึง component มาใช้งาน ช่วยให้ประหยัดเวลาและลดข้อผืดพลาดในการเขียนโปรแกรมได้มาก
## การทดลองนำเข้า component จาก github

### 1.สร้าง ESP32 project ใหม่ "ไฟกระพริบด้วย component"
1.1 ตั้งชื่อโปรเจค  `LED_Flash_with_component`
1.2 สร้างไฟล์ idf_component.yml ขึ้นใน Main พร้อมใส่เนื้อหาต่อไปนี้

```yml
dependencies:
  LED:
    git: https://github.com/koson/LED_Test.git 
    path: components/LED
```
หมายเหตุ ในบรรทัด git: ให้ใส่ชื่อ repository ของนักศึกษาที่มีคอมโพเนนท์ LED

## เว็บไซร์ บน Git Hub https://github.com/AnchisaPhetnoi/ESP32-LED-Component/tree/main/components/LED

![image](https://github.com/user-attachments/assets/c8f52c52-39d6-429a-99a7-3bc6f003432f)

![image](https://github.com/user-attachments/assets/58b90c14-dbcd-4d19-b5a4-351153e12d5a)

1.3 แก้ไขไฟล์ CMAkeLists.txt โดยเปลี่ยน  `main.c` เป็น `main.cpp` ดังต่อไปนี้

```Cmake
idf_component_register(SRCS "main.cpp"
                    INCLUDE_DIRS ".")
```
1.4 เปลี่ยนชื่อไฟล์  `main.c` เป็น `main.cpp` และแก้ code  เป็นดังนี้

``` cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "LED.h"

extern "C" void app_main(void);

void app_main(void)
{
    LED led1(5); 
    LED led2(17); 
    while(1)
    {        
        led1.ON();
        led2.OFF();
        vTaskDelay(500/portTICK_PERIOD_MS);
        led1.OFF();
        led2.ON();
        vTaskDelay(500/portTICK_PERIOD_MS);
    }
}
```

1.5 Build และทดสอบด้วยบอร์ด ESP32 ถ้ารันผ่าน ให้ commit ขึ้นบน github

จะสังเกตว่าในขั้นตอนการ build จะมีข้อความต่อไปนี้ ซึ่งเป็นการแจ้งให้ทราบว่า CMake ได้ดึง component ที่ชื่อ LED และ idf จาก internet มาร่วมสร้าง code

```
.....
Processing 2 dependencies:
[1/2] LED (838c47debbba86b33b24f9ab9ec71d2c00a4bf48)
[2/2] idf (4.4.7)
......
```

และใน project จะมีโฟลเดอร์ managed_components เพิ่มเข้ามา ด้านในจะมี LED อยู่เช่นเดียวกับตอนที่สร้างเป็น component ในโปรเจค

## บันทึกผลการทดลอง
1.บันทึกรูปภาพ

![452673405_456774763843924_4732536554693144361_n](https://github.com/user-attachments/assets/182a808b-914d-4911-bc9c-542410636822)

![453654008_335158616223189_6564999699179168474_n](https://github.com/user-attachments/assets/24c3e315-4b46-44e3-8c0a-78953aad1077)

2.บันทึกวิดิโอ




1.6 แก้ไขไฟล์ main.cpp ให้เป็นดังนี้

``` cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "LED.h"

LED led1(16); 
LED led2(17); 
LED led3(5); 
LED led4(18); 
LED led5(19); 
LED led6(21); 
LED led7(22); 
LED led8(23); 

void ALL_LED_OFF()
{
    led1.OFF();
    led2.OFF();
    led3.OFF();
    led4.OFF();
    led5.OFF();
    led6.OFF();
    led7.OFF();
    led8.OFF();
}

extern "C" void app_main(void)
{
    int i = 0;
    while(1)
    {        
        switch(i)
        {
            case 0:
            led1.ON();
            break;
            case 1:
            led2.ON();
            break;
            case 2:
            led3.ON();
            break;
            case 3:
            led4.ON();
            break;
            case 4:
            led5.ON();
            break;
            case 5:
            led6.ON();
            break;
            case 6:
            led7.ON();
            break;
            case 7:
            led8.ON();
            break;            
        }
        vTaskDelay(100/portTICK_PERIOD_MS);
        ALL_LED_OFF();
        if(i++ >= 7) i = 0;
    }
}
```

1.8 เชื่อมต่อสายไปยัง LED ตาม code ที่ปรากฏ เช่น `LED led1(16);` หมายถึงต่อ LED1 เช้ากับขา 16 เป็นต้น

1.9 Build และทดสอบด้วยบอร์ด ESP32 ถ้ารันผ่าน ให้ commit ขึ้นบน github

## บันทึกผลการทดลอง
1.บันทึกรูปภาพ

![453678739_8095534787172095_4086543282252309533_n](https://github.com/user-attachments/assets/1953bfc3-d3de-484d-83b1-da45da9a97df)

![451191454_502267235639251_7560916087866649487_n](https://github.com/user-attachments/assets/474bee7a-8ebe-4c23-887b-c4235af5fc19)


2.บันทึกวิดิโอ





1.10 แก้ไขไฟล์ main.cpp ให้เป็นดังนี้

```cpp
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "LED.h"

LED led1(16); 
LED led2(17); 
LED led3(5); 
LED led4(18); 
LED led5(19); 
LED led6(21); 
LED led7(22); 
LED led8(23); 

LED leds[] = {led1, led2, led3, led4, led5, led6, led7, led8};

extern "C" void app_main(void)
{
    int i = 0;
    while(1)
    {        
        leds[i].ON();
        vTaskDelay(100/portTICK_PERIOD_MS);
        leds[i].OFF();
        if(i++ >= 7) i = 0;
    }
}
```

1.11 Build และทดสอบด้วยบอร์ด ESP32 ถ้ารันผ่าน ให้ commit ขึ้นบน github

## บันทึกผลการทดลอง
1.บันทึกรูปภาพ

![453709396_882700150371207_5374648338742004054_n](https://github.com/user-attachments/assets/64512c6a-367e-4c17-affe-45f624b76a39)

![452002449_7723153024463071_6225827640342016856_n](https://github.com/user-attachments/assets/167b3306-a829-44d1-9a82-0dde110e87cf)


2.บันทึกวิดิโอ






## Challenge

ให้ทำไฟวิ่งเป็น pattern วิ่งวนลูป ดังต่อไปนี้ 
`*` หมายถึงหลอดไฟติด 
`.` หมายถึงหลอดไฟดับ

1. ไฟวิ่งดวงเดียว 
```
    *.......
    .*......
    ..*.....
    ...*....
    ....*...
    .....*..
    ......*.
    .......*
```

2. ไฟวิ่งสองดวงสวนกันตรงกลาง 
```
    *......*
    .*....*.
    ..*..*..
    ...**...
    ...**...
    ..*..*..
    .*....*.
    *......*
```

3. ไฟวิ่งไปกลับ 
```
    *.......
    .*......
    ..*.....
    ...*....
    ....*...
    .....*..
    ......*.
    .......*
    ......*.
    .....*..
    ....*...
    ...*....
    ..*.....
    .*......
    *.......
```
## บันทึกผลการทดลอง
1.บันทึกรูปภาพ




2.บันทึกวิดิโอ




## โปรแกรมที่ เขียนบน VS code Commit ไปบน Git Hub

https://github.com/AnchisaPhetnoi/LED_Flash_with_component.git

![image](https://github.com/user-attachments/assets/98962bc2-77f4-4a0c-bd7e-dd44bc79294e)




