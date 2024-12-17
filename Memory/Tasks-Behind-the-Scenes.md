https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682254

Task consumes heap memory
A microcontroller has RAM 

`Total RAM = SRAM1 + SRAM 2` e.g, `112 KB + 16 KB = 128 KB `

A portion of this RAM is used for heap memory, `configTOTAL_HEAP_SIZE`. This must be configured appropriately per microcontroller you are running your application on.


```
| <------------------- Heap ------------------->|<-------------------------------------->|
|                                               | Global data, arrays, static variables  |
|                                               | Kernel stack                           |
Low                                                                                   High
```

In `FreeRTOSConfig.h`, 

```cpp
#define configTOTAL_HEAP_SIZE ( ( size_t ) ( 75 * 1024 ) ) // set the total heap size to 75 KB
```

Example
If we change heap size to 130, you will get an error:

In `FreeRTOSConfig.h`, 
```cpp
#define configTOTAL_HEAP_SIZE ( ( size_t ) ( 130 * 1024 ) ) // set the total heap size to 130 KB
```

Error when building:
```
C:/ST/STM32CubeIDE_1.10.1/STM32CubeIDE/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.11.3.rel1.win32_1.1.0.202305231506/tools/bin/../lib/gcc/arm-none-eabi/11.3.1/../../../../arm-none-eabi/bin/ld.exe: hello.elf section `.bss' will not fit in region `RAM'
C:/ST/STM32CubeIDE_1.10.1/STM32CubeIDE/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.11.3.rel1.win32_1.1.0.202305231506/tools/bin/../lib/gcc/arm-none-eabi/11.3.1/../../../../arm-none-eabi/bin/ld.exe: region `RAM' overflowed by 4488 bytes
collect2.exe: error: ld returned 1 exit status
make: *** [makefile:67: hello.elf] Error 1
```

The error stating
```
.../ld.exe: hello.elf section `.bss' will not fit in region `RAM
.../ld.exe: region `RAM' overflowed by 4488 bytes
```

When creating a task using `xTaskCreate()`, memory for TCB (task control block) is allocated in the heap.

```
| TCB1 | STACK-1 |
|      |         |
|      |         |
```

```cpp
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


xTaskCreate() Actions

1. Memory for TCB allocated in heap section of RAM
2. Memory for Task private stack allocated in heap
3. If heap allocation is successful, task is placed in kernel ready list


TCB
- Data structure that contains different elements about the stack
- `pxTopOfStack`



PSP process stack pointer
MSP main stack pointer














