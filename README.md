# Just Your Typical Github Readme 


**USER IMPORTS FOR EVERY FILE**

#include <stdio.h><br>
#include <stdbool.h><br>
#include <string.h><br>
#include <stdbool.h><br>
#include <stdlib.h><br>
#include <stdio.h><br>

**Sending Char Array Over Serial**

**Components**

Term
   ```JAVA
  char memes[6] = {'M', 'E', 'M', 'E', 'S', '\0'};
  int len = strlen(memes);
  for(;;){
	  for(int i = 0; i<len; i++){
		  Term1_SendChar(memes[i]);
  }
  ```
---------------------------------------------
  
  **Reading Light Sensor**
  
  **Components**
  
  ADC -> A/D Channel = ADC0_DM0 (Note, if wanting more then one sensor reading, add it to the channel list not a new adc) 
  Term
  
  ```java
  word light = 0;
  light_Calibrate(1);
  for(;;){
	  light_Measure(1);
	  light_GetValue(&light);
	  Term1_SendNum(light);
  }
  ```
  
  ------------------------------------------
  
  **Measuring From Multiple Sensors**
  
  **Components**
  ADC -> <br>
  A/D Channel = ADC0_DM0 - light <br>
  A/D Channel = ADC0_DM3 - temperature <br>
  Term
  
```java
word adc = 0;
byte temp;
byte light;
AD1_Calibrate(1);

for(;;){
  AD1_Measure(1);
  AD1_GetChanValue(0, &temp); // 0,1 depends on what order it is set in the channel settings
  AD1_GetChanValue(1, &light);
  Term1_SendStr("temperature: ");
  Term1_SendNum(temp);
  Term1_SendStr("Light: ");
  Term1_SendNum(light);

}
```

--------------------------------------------

**DO Task Every N Amount of Time**

**Components**

FreeCntr32<br>
Term

```JAVA
word time = 0;
FC321_Reset();
for(;;){
  FC321_GetTimeMS(&time);
  if (time >= 2000){
    BLAH();
    FC321_Reset();
    time = 0;
   }
}
```
---------------------------------------------
