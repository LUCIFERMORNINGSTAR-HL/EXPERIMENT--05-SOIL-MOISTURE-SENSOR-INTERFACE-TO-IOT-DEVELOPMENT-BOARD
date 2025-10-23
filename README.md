# EXPERIMENT-05-SOIL-MOISTURE-SENSOR-INTERFACE-TO-IOT-DEVELOPMENT-BOARD

## Aim: 

To Interface a Analog Input  (soil moisture sensor) to ARM IOT development board and write a  program to obtain  the data on the com port 
## Components required: 

STM32 CUBE IDE, ARM IOT development board,  STM programmer tool,Soil Moisture Sensor.
## Theory 
#### Hardware Overview
A typical soil moisture sensor consists of two parts.

The Probe
The sensor includes a fork-shaped probe with two exposed conductors that is inserted into the soil or wherever the moisture content is to be measured.

As previously stated, it acts as a variable resistor, with resistance varying according to soil moisture.

![image](https://github.com/vasanthkumarch/EXPERIMENT--05-SOIL-MOISTURE-SENSOR-INTERFACE-TO-IOT-DEVELOPMENT-BOARD-/assets/36288975/00e1751d-44e6-41e3-b261-717a657861be)

The Module
In addition, the sensor includes an electronic module that connects the probe to the Arduino.

The module generates an output voltage based on the resistance of the probe, which is available at an Analog Output (AO) pin.

The same signal is fed to an LM393 High Precision Comparator, which digitizes it and makes it available at a Digital Output (DO) pin.

![image](https://github.com/vasanthkumarch/EXPERIMENT--05-SOIL-MOISTURE-SENSOR-INTERFACE-TO-IOT-DEVELOPMENT-BOARD-/assets/36288975/85f21aed-ce9b-416c-8ad2-d0919eb32dbf)

The module includes a potentiometer for adjusting the sensitivity of the digital output (DO).

You can use it to set a threshold, so that when the soil moisture level exceeds the threshold, the module outputs LOW otherwise HIGH.

This setup is very useful for triggering an action when a certain threshold is reached. For example, if the moisture level in the soil exceeds a certain threshold, you can activate a relay to start watering the plant.


Soil Moisture Sensor Pinout
The soil moisture sensor is extremely simple to use and only requires four pins to connect.

soil moisture sensor pinout
AO (Analog Output) generates analog output voltage proportional to the soil moisture level, so a higher level results in a higher voltage and a lower level results in a lower voltage.

DO (Digital Output) indicates whether the soil moisture level is within the limit. D0 becomes LOW when the moisture level exceeds the threshold value (as set by the potentiometer), and HIGH otherwise.

VCC supplies power to the sensor. It is recommended that the sensor be powered from 3.3V to 5V. Please keep in mind that the analog output will vary depending on the voltage supplied to the sensor.

GND is the ground pin.

![image](https://github.com/vasanthkumarch/EXPERIMENT--05-SOIL-MOISTURE-SENSOR-INTERFACE-TO-IOT-DEVELOPMENT-BOARD-/assets/36288975/bf4f99ab-6c72-4d9b-9e65-40669401ce04)

## Procedure:
 1. click on STM 32 CUBE IDE, the following screen will appear
  
 ![image](https://user-images.githubusercontent.com/36288975/226189166-ac10578c-c059-40e7-8b80-9f84f64bf088.png)

 2. click on FILE, click on new stm 32 project
  
 ![image](https://user-images.githubusercontent.com/36288975/226189215-2d13ebfb-507f-44fc-b772-02232e97c0e3.png)
![image](https://user-images.githubusercontent.com/36288975/226189230-bf2d90dd-9695-4aaf-b2a6-6d66454e81fc.png)

3. Select the target to be programmed as shown below and click on next
   
![Screenshot 2025-03-11 134231](https://github.com/user-attachments/assets/09e61f3d-224f-4ca8-96d4-7336869df5c7)

4. Select the program name
   
![image](https://user-images.githubusercontent.com/36288975/226189316-09832a30-4d1a-4d4f-b8ad-2dc28f137711.png)

5. Corresponding ioc file will be generated automatically
   
![Screenshot 2025-03-11 134528](https://github.com/user-attachments/assets/df427edd-e24a-4612-a858-aeae859b379f)


6. Select the appropriate pins as GPIO, in or out, USART or required options and configure
   
![Screenshot 2025-03-11 134617](https://github.com/user-attachments/assets/125ee548-30b1-4c88-932f-adf07984522f)
![Screenshot 2025-03-11 134642](https://github.com/user-attachments/assets/0adfbb58-4cad-408a-9300-f4808b53cac4)


7. Click on Ctrl+S, automatically C program will be generated
   
![image](https://github.com/user-attachments/assets/1e9a3494-c750-44d2-9a64-145b2b7bf8f1)

8. Edit the program and as per required 

![image](https://user-images.githubusercontent.com/36288975/226189461-a573e62f-a109-4631-a250-a20925758fe0.png)


9. Use project and build all 

![image](https://user-images.githubusercontent.com/36288975/226189554-3f7101ac-3f41-48fc-abc7-480bd6218dec.png)

10. Once the project is build 

![image](https://user-images.githubusercontent.com/36288975/226189577-c61cc1eb-3990-4968-8aa6-aefffc766b70.png)

11. Open STM32Cube Programmer

![Screenshot 2025-03-11 135208](https://github.com/user-attachments/assets/bb67ab6b-81a5-450c-b170-4276a9b87ef2)

12. Connect the STM board through the COM port, then upload the corresponding project ELF file while ensuring the board is in flash mode, and click on 'Start Program.' After the file download is complete, switch your board to run mode and press the reset button to see the output
  
13.Open serial port utility 

![image](https://github.com/user-attachments/assets/c7fb1ee4-814b-4589-92c3-080442637265)

14. Check for execution of the output using run option.


## STM 32 CUBE PROGRAM :
```
#include "main.h"
#include "stdbool.h"

bool IRSENSOR;
void irpair();

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  while (1)
  {
    irpair();
  }
}

void irpair()
{
  IRSENSOR = HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_3);
  if (IRSENSOR == 0)
  {
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_SET);
    HAL_Delay(1000);
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);
    HAL_Delay(1000);
  }
  else
  {
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);
    HAL_Delay(1000);
  }
}

void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = RCC_MSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_6;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK3 | RCC_CLOCKTYPE_HCLK
                              | RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_PCLK1
                              | RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.AHBCLK3Divider = RCC_SYSCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOA_CLK_ENABLE();

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);

  GPIO_InitStruct.Pin = GPIO_PIN_0;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}

void Error_Handler(void)
{
  __disable_irq();
  while (1)
  {
  }
}

#ifdef USE_FULL_ASSERT
void assert_failed(uint8_t *file, uint32_t line)
{
}
#endif

```

## OUTPUT
```
FLASH ON AND OFF
```
![WhatsApp Image 2025-09-15 at 11 20 44_0ddacf06](https://github.com/user-attachments/assets/1f946b9c-1c5a-433e-aba2-ff2e6dc16a5c)
![WhatsApp Image 2025-09-15 at 11 20 44_1a136ddd](https://github.com/user-attachments/assets/1f1cb5cd-2370-4e30-b699-9c9aa8768a58)


## Result

Interfacing a digital Input (ir pair) with ARM microcontroller based IOT development is executed and the results are verified.
