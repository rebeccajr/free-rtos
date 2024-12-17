# Description
This page contains sample code for a 'Hello world' project. This application has two running tasks that print 'Hello from Task-1' and 'Hello from Task-2' to the SWV (Serial Wire Viwere) Data ITM Console over the SWO pin. The full project can be found [here](https://github.com/rebeccajr/rtos/tree/main/projects/hello).

## Source
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682166
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682186
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682192

Always include your code between the `USER CODE BEGIN` and `USER CODE END` comments. E.g. in `main.c`


```
//------------------------------------------------------------------------------
/* USER CODE BEGIN Includes */
//------------------------------------------------------------------------------
#include "FreeRTOS.h"
#include "task.h"
//------------------------------------------------------------------------------
/* USER CODE END Includes */
//------------------------------------------------------------------------------

// template code...

//------------------------------------------------------------------------------
/* USER CODE BEGIN PFP */
//------------------------------------------------------------------------------
static void task1_handler(void* parameters);
static void task2_handler(void* parameters);
//------------------------------------------------------------------------------
/* USER CODE END PFP */
//------------------------------------------------------------------------------

// template code...

int main (void)
{
  //----------------------------------------------------------------------------
  /* USER CODE BEGIN 1 */
  //----------------------------------------------------------------------------
  TaskHandle_t task1_handle;
  TaskHandle_t task2_handle;

  BaseType_t status;
  //----------------------------------------------------------------------------
  /* USER CODE END 1 */
  //----------------------------------------------------------------------------

  // template code...

  //----------------------------------------------------------------------------
  /* USER CODE BEGIN 2 */
  //----------------------------------------------------------------------------
  status = xTaskCreate(
    task1_handler, "Task-1", 200, "Hello from Task-1", 2, &task1_handle);
  configASSERT(status == pdPASS);

  status = xTaskCreate(
    task2_handler, "Task-2", 200, "Hello from Task-2", &task2handle);
  configASSERT(status == pdPASS);

  // Start scheduler
  vTaskStartScheduler();

  // If the program reaches here, then the launch of the scheduler has
  // failed due to insufficient memory in the heap

  //----------------------------------------------------------------------------
  /* USER CODE END 2 */
  //----------------------------------------------------------------------------

  /* Infinite loop */
  while (1)
  {
    //--------------------------------------------------------------------------
    /* USER CODE BEGIN WHILE */
    //--------------------------------------------------------------------------

    //--------------------------------------------------------------------------
    /* USER CODE END WHILE */
    //--------------------------------------------------------------------------
  }

  //----------------------------------------------------------------------------
  /* USER CODE BEGIN 3 */
  //----------------------------------------------------------------------------

  //----------------------------------------------------------------------------
  /* USER CODE END 3 */
  //----------------------------------------------------------------------------
}

// template code...

//------------------------------------------------------------------------------
/* USER CODE BEGIN 4 */
//------------------------------------------------------------------------------
static void task1_handler(void* parameters)
{
	while (1)
		printf("%s\n", (char*) parameters);

	vTaskDelete(NULL);
	return;
}

static void task2_handler(void* parameters)
{

	while (1)
		printf("%s\n", (char*) parameters);

	vTaskDelete(NULL);
	return;
}
//------------------------------------------------------------------------------
/* USER CODE END 4 */
//------------------------------------------------------------------------------

```

# Troubleshooting

If you try to build the project and get the error `undefined reference to xTaskCreate`, verify that the definition of this function is included in the build paths.

In this example, the definition to `xTaskCreate` is in `tasks.c`, contained in `third-party/free-rtos/Source`

Go to project properties -> `C/C++ General` (left sidebar) -> `Paths and Symbols` -> `Source Location` (tab). If the directory containing the file with the definition has not been added, click the `Add Folder` button, then navigate to `third-party/free-rtos/Source`  
![image](https://github.com/rebeccajr/rtos/assets/26588191/fe832445-297e-446d-b303-228fdd369c1a)

![image](https://github.com/rebeccajr/rtos/assets/26588191/3956c6d2-537f-4395-a898-1c18ef88c008)
