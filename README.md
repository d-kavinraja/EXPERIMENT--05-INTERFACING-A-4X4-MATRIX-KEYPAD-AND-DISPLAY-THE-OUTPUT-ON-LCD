```
Name: Kavinraja D
Register Number: 212222240047
```
# EXPERIMENT 05 INTERFACING A 4X4 MATRIX KEYPAD AND DISPLAY THE OUTPUT ON LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:

![image](https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2)

4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

## Procedure : 
 ## LCD 16X2 
   16×2 LCD is named so because; it has 16 Columns and 2 Rows. There are a lot of combinations available like,
   8×1, 8×2, 10×2, 16×1, etc. But the most used one is the 16*2 LCD, hence we are using it here.

All the above mentioned LCD display will have 16 Pins and the programming approach is also the same and hence the choice is left to you. 
Below is the Pinout and Pin Description of 16x2 LCD Module:

![image](https://user-images.githubusercontent.com/36288975/233858086-7b1a88a2-f941-475c-86c2-b3bae68bdf7e.png)
![image](https://user-images.githubusercontent.com/36288975/233857710-541ac1c2-786c-4dfc-b7b5-e3a4868a9cb6.png)
![image](https://user-images.githubusercontent.com/36288975/233857733-05df5dbf-1a1e-479e-85bb-8014a39ad878.png)

4-bit and 8-bit Mode of LCD:

The LCD can work in two different modes, namely the 4-bit mode and the 8-bit mode. In 4 bit mode we send the data nibble by nibble, first upper nibble and then lower nibble. For those of you who don’t know what a nibble is: a nibble is a group of four bits, so the lower four bits (D0-D3) of a byte form the lower nibble while the upper four bits (D4-D7) of a byte form the higher nibble. This enables us to send 8 bit data.

Whereas in 8 bit mode we can send the 8-bit data directly in one stroke since we use all the 8 data lines.

 8-bit mode is faster and flawless than 4-bit mode. But the major drawback is that it needs 8 data lines connected to the microcontroller. This will make us run out of I/O pins on our MCU, so 4-bit mode is widely used. No control pins are used to set these modes. 
 LCD Commands:

There are some preset commands instructions in LCD, which we need to send to LCD through some microcontroller. Some important command instructions are given below:

Hex Code

Command to LCD Instruction Register

0F

LCD ON, cursor ON

01

Clear display screen

02

Return home

04

Decrement cursor (shift cursor to left)

06

Increment cursor (shift cursor to right)

05

Shift display right

07

Shift display left

0E

Display ON, cursor blinking

80

Force cursor to beginning of first line

C0

Force cursor to beginning of second line

38

2 lines and 5×7 matrix

83

Cursor line 1 position 3

3C

Activate second line

08

Display OFF, cursor OFF

C1

Jump to second line, position 1

OC

Display ON, cursor OFF

C1

Jump to second line, position 1

C2

Jump to second line, position 2
 
## Procedure:
 1. click on STM 32 CUBE IDE.

 2. Click on FILE, click on new stm 32 project.
 
3. Select the target to be programmed and click on next. 

4. Select the program name.


5. Corresponding ioc file will be generated automatically.

6. Select the appropriate pins as gipo, in or out, USART or required options and configure.


7. Click on cntrl+S , automaticall C program will be generated.

8. Edit the program and as per required. 

9. Add necessary library files of LCD 16x2 , write the program and use project and build.  

10. Once the project is bulild.

11. Click on debug option.


12.  Creating Proteus project and running the simulation
We are now at the last part of step by step guide on how to simulate STM32 project in Proteus.

13. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE. 

14. After creation of the circuit as per requirement. Double click on the the MCU part to open settings. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.
https://engineeringxpert.com/wp-content/uploads/2022/04/26.png

15. Click on debug and simulate using simulation as shown below. 


## STM 32 CUBE PROGRAM :
```
#include "main.h"
#include "stdbool.h"
#include "lcd.h"
bool col1,col2,col3,col4;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void keypad();

int main(void)
{

  HAL_Init();

  SystemClock_Config();

  MX_GPIO_Init();

    while (1)
  {
	  keypad();
  }

}
void keypad()
{
	Lcd_PortType ports[] = {GPIOA,GPIOA,GPIOA,GPIOA};
	Lcd_PinType pins[] = {GPIO_PIN_3,GPIO_PIN_2,GPIO_PIN_1,GPIO_PIN_0};
	Lcd_HandleTypeDef lcd;
	lcd = Lcd_create(ports,pins,GPIOB,GPIO_PIN_0,GPIOB,GPIO_PIN_1,LCD_4_BIT_MODE);

	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

	col1=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
	col2=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
	col3=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
	col4=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
	HAL_Delay(100);
	Lcd_cursor(&lcd, 0,0);
```
```

	if(!col1)
	{
		Lcd_string(&lcd, "Key pressed is:7");
	}

	else if(!col2)
	{
		Lcd_string(&lcd, "Key pressed is:8");
	}

	else if(!col3)
	{
		Lcd_string(&lcd, "Key pressed is:9");
	}

	else if(!col4)
	{
		Lcd_string(&lcd, "Key pressed is:%");
	}
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

	col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
	HAL_Delay(100);
	if(!col1)
		{
			Lcd_string(&lcd, "Key pressed is:4");
		}
	else if(!col2)
		{
			Lcd_string(&lcd, "Key pressed is:5");
		}
	else if(!col3)
		{
			Lcd_string(&lcd, "Key pressed is:6");
		}
	else if(!col4)
		{
			Lcd_string(&lcd, "Key pressed is:X");
		}

	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

	col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
	HAL_Delay(100);
	if(!col1)
		{
			Lcd_string(&lcd, "Key pressed is:1");
		}
	else if(!col2)
		{
			Lcd_string(&lcd, "Key pressed is:2");
		}
	else if(!col3)
		{
			Lcd_string(&lcd, "Key pressed is:3");
		}
	else if(!col4)
		{
			Lcd_string(&lcd, "Key pressed is:-");
		}

	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_RESET);

	col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
	HAL_Delay(100);
	if(!col1)
		{
			Lcd_string(&lcd, "Key pressed is:ON");
		}
	else if(!col2)
		{
			Lcd_string(&lcd, "Key pressed is:0");
		}
	else if(!col3)
		{
			Lcd_string(&lcd, "Key pressed is:=");
		}
	else if(!col4)
		{
			Lcd_string(&lcd, "Key pressed is:+");
		}
		else
		{
			Lcd_string(&lcd, "No key pressed");
			HAL_Delay(500);
		}
	Lcd_clear(&lcd);
}

void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);


  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);


  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0|GPIO_PIN_1, GPIO_PIN_RESET);


  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);


  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);


  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);


  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

}

void Error_Handler(void)
{

  __disable_irq();
  while (1)
  {
  }

}

#ifdef  USE_FULL_ASSERT

void assert_failed(uint8_t *file, uint32_t line)
{

}
#endif
```


## Output screen shots of proteus:
![Proteus Screenshot](https://github.com/amal-2006/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/148410730/dd27f536-e189-4c0d-b8cb-e995c15ed5ec)

 
 
 ## CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE): 
 ![Circuit Layout](https://github.com/amal-2006/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/148410730/8007ee5f-bd51-4c88-a99c-7a864bf113b0)

 
## Result :
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.
