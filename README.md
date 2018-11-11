# Just Your Typical Github Readme 


**PINS**

red-   PTC3 <br>
green- PTD4 <br>
blue-  PTA2 <br>
Light Sensor - ADC0_DM0


**I2C ACCELLEROMETER SETTINGS**
frequency - 10MHz <br>
010 and 011 <br>
SCL 93.6kHz  - Pin = PTB0 <br>
SDA 1.61 us  - Pin = PTB1 <br>
Slave address - 0x1D <br>

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
uint16 val[2];
AD1_Calibrate(TRUE);

for(;;){
  AD1_Measure(TRUE);
  AD1_GetValue16(val)
  Term1_SendStr("temperature: "\r\n);
  Term1_SendNum(val[0]);
  Term1_SendStr("Light: "\r\n);
  Term1_SendNum(val[1]);
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
  
  **Creating Semaphores**
  
  **Components**
  
  MQX-Lite -> In settings, choose how many tasks you need
  
  In main
  ```java
   _lwsem_create(&MySem1, 1);
  ```
  In mqx_tasks.c
  ```java
  extern LWSEM_STRUCT MySem1; // get semaphore from main.c
  
  void Task1_task(uint32_t task_init_data){

	// Wait for the ISR to post the semaphore
	_lwsem_wait(&MySem1);

	while(1){
		
		// Code what you need here
		
		 // say that this semaphore is ready (for other tasks)
		_lwsem_post(&MySem1);
		
		// 250ms by doing this it allows resources to be used in other tasks
		_time_delay_ticks(50);

       }
   }
   
// Task 3 prints data
void Task3_task(uint32_t task_init_data){
	while(1) {

		_lwsem_wait(&MySem1); // waits until Sem1 posts, then does code underneath
		_lwsem_wait(&MySem2);
	
		// wite code here
		
		_time_delay_ticks(50);
	}
}
  ```
  
  ------------------------------------------
