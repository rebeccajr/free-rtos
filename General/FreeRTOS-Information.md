This information was obtained from Lecture ##.
---
# Adding FreeRTOS Kernel to your project
2 methods
1. manually non-CMSIS RTOS
2. STM32Cube Device configuration tool

---
# CMSIS-RTOS API
From https://arm-software.github.io/CMSIS_5/RTOS2/html/genRTOS2IF.html

CMSIS-RTOS is a
- generic API
- agnostic of the underlying RTOS kernel

Application programmers call CMSIS-RTOS2 API functions in the user code to
- ensure maximum portability from one RTOS to another

Middleware using CMSIS-RTOS2 API takes advantages of this approach by avoiding unnecessary porting efforts.

```
------------------
 APPLICATION
------------------
 |
 | osThreadCreate() - CMSIS layer API to create an RTOS task
 |                  - independent of underlying RTOS
 |
------------------
 CMSIS-CORE LAYER
------------------
 |
 | vTaskCreate()    - FreeRTOS API to create an RTOS task
 |
------------------
 FREERTOS
------------------
```

## Project Layers
```
_____________________________
 Application
_____________________________
 |            |
 |    _______________________
 |     CMSIS-RTOS API Layer
 |     (Generic Layer)
 |    _______________________
 |            |
 |    _______________________
 |     Real Time Kernel
 |     (FreeRTOS, embOS, etc)
 |    _______________________
 |            |
_____________________________
 CMSIS-CORE Layer
_____________________________
 |
_____________________________
 Cortex Mx Processor
_____________________________
```



_____________________________
`FreeRTOSConfig.h`
- configuration file to customize FreeRTOS kernel
- needs to be created, it is application specific
https://www.freertos.org/a00110.html