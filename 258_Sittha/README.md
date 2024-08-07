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
CODE

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
CODE
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
    int direction = 1;

    while(1)
    {        
      
        leds[i].ON();
        leds[7 - i].ON();
        vTaskDelay(100/portTICK_PERIOD_MS);
        
        leds[i].OFF();
        leds[7 - i].OFF();

    
        if (direction == 1) {
            if (i < 3) {
                i++;
            } else {
                direction = -1;
                i++;
            }
        } else {
            if (i > 0) {
                i--;
            } else {
                direction = 1;
                i++;
            }
        }
    }
}
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
CODE 

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

LED leds[] = {led1, led2, led3, led4, led5, led6, led7, led8};

extern "C" void app_main(void)
{
    int i = 0;
    int direction = 1; 

    while(1)
    {        

        leds[i].ON();
        vTaskDelay(100/portTICK_PERIOD_MS);

        leds[i].OFF();

        if (direction == 1) {
            if (i < 7) {
                i++;
            } else {
                direction = -1;
                i--;
            }
        } else {
            if (i > 0) {
                i--;
            } else {
                direction = 1;
                i++;
            }
        }
    }
}
```
