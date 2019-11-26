# BT_IAP_STM32F4
#Main function 
using a Qt  app as a service and STM32  as a client ,update program of stm32


#folder introduction
<./App demo> it's a application demo , this project could generate a binary file(such :app_demo.bin) ,the BT_IAP.exe could send this **.bin file to STM32F407**.
<./BT_IAP> it's QT source code.
<./IAP_BT-STM32F407ZG> it's STM32f407 main IAP code.

#hardware introduction
												 S|---------------------------------|
|--------------------|B						  B| E|                           		|
|  STM32F407ZG       |T	(Uart6)               T| R|	QT driver the serial port 		|
|                    |	(HC05)		    	   | I|			                  		|
| IAP code at flash  |M	--------------------- M| A|through the HC05 send *.bin file |
|0x8000000~ 0x8010000|O	<<<<<<<<<<<<<<<<<<<<  O| L|                           		|
|					 |D	>>>>>>>>>>>>>>>>>>>>  D|  |                           		|
|  APP code			 |U	--------------------- U| P|                           		|
|0x8010000~0x80f0000 |L	              (HC05)  L| O|                           		|
|--------------------|E                       E| R|                           		|	
												 T|---------------------------------|
#software tools
QTcreator
keil IDE
												 

#configuration 
For application runing at specific flash address ,we need setup the flash address In keil :
  project -> option for target -> Target -> read/Only memory Areas -> On chip ->IROM1:
  filling with :start-0x8010000	(keep Consistency whit ./IAP_BT-STM32F407ZG/IAP/iap.h line-17 :#define FLASH_APP1_ADDR		0x08010000)
				size -oxf0000	(STM32F407ZG flash size(1024KB) - IAP region(64K) = (APP region)960K)
				
& For application compliting a binary file, In keil:
  project -> option for target -> USer -> After build /rebuild ,checking the first checkbox(Run #1), and filling:
  D:\keil\ARM\ARMCC\bin\fromelf.exe --bin -o ..\OBJ\pro.bin ..\OBJ\pro.axf       -this is my path.
  Your keil instaalling directory path\ARM\ARMCC\bin\fromelf.exe  --bin -o <You want create .bin at ?> <your .axf path>   	-this is your need set
