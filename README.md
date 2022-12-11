# target-HSI-and-MCO-reading

This project uses STM32CubeIDE and it's a program created to practice my C habilities during the course 'Mastering Microcontroller and Embedded Driver Development' from FastBit Embedded Brain Academy. I am using a NUCLEO-F401RE board.

MCO Master Clock Output

## Selecting the pin to MCO 

To find out which pin to use as MCO, the datasheet of the microcontroller will be used. 
By checking the alternating function table, we can see the AFx that multiplex the different functionalities for the microcontroller pins.

MSP32F401 has a MCO pin at Port A, pin 8 (PA8).

![image](https://user-images.githubusercontent.com/58916022/206924772-95419097-635c-4169-a317-81885ff8048c.png)


## Checking the required registers

To find out the register addresses, the reference manual of the microcontroller will be used.

![image](https://user-images.githubusercontent.com/58916022/206924344-dec53f3a-1f12-4b57-a049-a21baa04554c.png)
![image](https://user-images.githubusercontent.com/58916022/206924378-4604b7f3-4883-44c4-87c7-797fdbac6f09.png)
![image](https://user-images.githubusercontent.com/58916022/206924401-ec2a8229-a225-48ae-a681-81a5bb99e3e0.png)

## Programming

1 - Select the desired clock for the MCOx signal.

2 - Output the MCOx signal to the microcontroller pin.


