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

First, we need to find the boundary addresses of the registers that we will need to set. Since we are using PA8, the initial register address for Port A and also RCC inicial register address.

![image](https://user-images.githubusercontent.com/58916022/206925828-74a675fa-0149-477c-8a97-24c3e13548cb.png)

```
#define GPIOA_BASE_ADDR            0x40020000UL
#define RCC_BASE_ADDR              0x40023800UL
```

![image](https://user-images.githubusercontent.com/58916022/206924344-dec53f3a-1f12-4b57-a049-a21baa04554c.png)

Let's add the offset for the RCC_CFGR register.
```
#define RCC_CFGR_REG_OFFSET        0x08UL
#define RCC_CFGR_REG_ADDR          (RCC_BASE_ADDR + RCC_CFGR_REG_OFFSET )
```

![image](https://user-images.githubusercontent.com/58916022/206924378-4604b7f3-4883-44c4-87c7-797fdbac6f09.png)
![image](https://user-images.githubusercontent.com/58916022/206924401-ec2a8229-a225-48ae-a681-81a5bb99e3e0.png)

I also add the options for clock input of MCO2, but just to compare with MCO1. MCO2 will not be used.

And to configure the PA8 pin, we need to enable the clock for Port A. To do so, we go to RCC_AHB1ENR. This register has an offset of 0x30.

![image](https://user-images.githubusercontent.com/58916022/206926212-a03a800d-1bcb-4052-bb74-3b278abb41ac.png)

```
	 uint32_t *pRCCAhb1Enr = (uint32_t*)(RCC_BASE_ADDR + 0x30);
	*pRCCAhb1Enr |= ( 1 << 0); //Enable GPIOA peripheral clock
```

Now, we change the mode configuration of the pin, so PA8 can work by using a altertative function mode.

![image](https://user-images.githubusercontent.com/58916022/206926408-4f54e490-2b3c-4e9a-8d1f-1c595b6952ab.png)

```
	uint32_t *pGPIOAModeReg = (uint32_t*)(GPIOA_BASE_ADDR + 00);
	*pGPIOAModeReg &= ~( 0x3 << 16); //clear
	*pGPIOAModeReg |= ( 0x2 << 16);  //set
```

Now, choose the mode as being AF0.

![image](https://user-images.githubusercontent.com/58916022/206926499-f001fb33-538a-4b01-b475-81cd1b8a7a91.png)

```
	uint32_t *pGPIOAAltFunHighReg = (uint32_t*)(GPIOA_BASE_ADDR + 0x24);
	*pGPIOAAltFunHighReg &= ~( 0xf << 0);
```

## Programming

The final code: 

![image](https://user-images.githubusercontent.com/58916022/206926620-a2c4a97c-1ffe-43ff-8705-24af939b6f3c.png)

## Results

