# Task
- A piece of code that is schedulable
- Each task has its own stack

## Source

Lecture    | URL
-|-
24 | https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/6383704  
25 | https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681056
31 | https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682254

## 2 Steps
1. Create task
1. Provide task handler

## Task Creation
Tasks are created dynamically, i.e. dynamic memory allocations
reference:
[xTaskCreate socumentation](https://www.freertos.org/a00125.html)

```
BaseType_t xTaskCreate (
TaskFunction_t               pvTaskCode,     // addr of task handler
                                             // i.e. function pointer
const char* const            pcName,         // name identifying the task
const configSTACK_DEPTH_TYPE usStackDepth,   // amt of stack mem (in words)
                                             // allocated to task
void*                        pvParameter,    // ptr to data to be passed to
                                             // task handler
UBaseType_t                  uxPriority,     // task priority value
TaskHandle_t* const          pxCreatedTask   // the addr of the task
);

```

### uBaseType_t
'The most efficient, natural type for the architecture', e.g. a 32-bit type for 32-bit type architecture. `U` indicates unsigned.

[freertos.org reference](https://www.freertos.org/FreeRTOS-Coding-Standard-and-Style-Guide.html)

### configSTACK_DEPTH_TYPE
This type is defined in `FreeRTOS.h`, e.g.
```
#define configSTACK_DEPTH_TYPE uint16_t
```
### Task Priority
Priority value is used by scheduler to determine which task to run. Lower value implies lower priority (urgency).

Values range from 0 to `configMAX_PRIORITIES - 1`

Determine `configMAX_PRIORITIES` based on application. Note that too many priorities can lead to RAM overconsumption due to high level of context switching. A good guideline is to use 5 unless you need more.

### Return value
Value | Description
-|-
`pdPASS` | Task was created successfully
`errCOULD_NOT_ALLOCATION_REQUIRED_MEMORY` | Task was not created successfully



## Task Implementation

```
void vATaskFunction(void *pvParameters)
{
    for(;;)
    {
        // task application code
    }

    vTaskDelete(NULL);
}
```


```
void vATaskFunction(void *pvParameters)
{
    // local variable of task handler
    // if it were static, then only one copy would exist and
    // would be shared by all instances of task
    int iVariableExampleLocal = 0;

    // one copy of this variable exists and is shared by all
    // instances of this task
    static int iVariableExampleStatic = 0;
    
    // task is an infinite loop
    for(;;)
    {
        // task application code
    }

    vTaskDelete(NULL);
}
```



