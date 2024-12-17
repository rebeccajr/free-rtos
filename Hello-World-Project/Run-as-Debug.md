---
## Source
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682192

# Debug Settings
1. Right-click project
1. Select `Properties`
1. From sidebar click `Run/Debug Settings`
1. Select project, click `Edit` button
1. Debugger tab -> Interface -> SWD
Edit Configuration`

| Field                    | Entry
|--------------------------|--------
| Interface                | SWD radio button
| Serial Wire Viewer (SWV) | Enable check box 
| Core Clock (MHz)         | set to clock speed as verified in device configuration tool -> Clock Configuration (look at SYSCLK)


## Run Debugger
Debug As -> STM32 Cortex-M C/C++ Application

## `taskYIELD()`
- Forces a context switch
- Gives up processor

---
