# Description
This of page describes how to start a new project with two simple tasks. The full 'Hello world' project can be found [here](https://dev.azure.com/rebeccajennifer/rtos/_git/freertos-udemy-class?path=/projects/hello). The code changes can be found on the page [Project Source Code](/Hello-World-Project/Project-Source-Code)

[[_TOC_]]

---
# Set Up Directories

Create two folders
1. Workspace directory
1. Software and toolchains

---
---
# New Project
This section describes the process for starting a new project in the IDE.

## Source
Lecture 19
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681022

## Procedure
1. Open `STM32CubeIDE` and create / select workspace
1. Create new project
1. Select `Board Selector` tab and search for board name. Click `Next`.
1. Select the following, then click `Next`.
   Field                 | Selection
   -|-
   Project Name          | Name of your project, e.g. `free-rtos-project`
   Use default location  | Uncheck this box if you would like to save your source files in a different directory. Click `Browse...` to select an empty directory of where to save the source code for the project.
   Targeted Language     | C
   Targeted Binary Type  | Executable
   Targeted Project Type | `STM32Cube`<br> Note: <br>`Empty` indicates that you will write microcontroller code from scratch.<br>`STM32Cube` will prompt you with a device configuration tool enabling you to modify various configurable items.

1. Select `Copy only necessary library files`. Click `Finish`.  
1. When prompted to initialize peripherals, select `No`.
1. When prompted to open `Device Configuration Tool` perspective, select `Yes`.  

---
---
# Adding FreRTOS Kernel Source

## Source
Lecture 20
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681026

## Methods to add FreeRTOS Kernel
1. Manually (what we use in this example)
1. With STM32 Cube Device Configuration Tool

## Manually add FreeRTROS Kernel

1. Create new folder in project called `third-party`, and a subfolder called `free-rtos`
1. Copy following folders from `sw-and-toolchains/.../FreeRTOS` into project folder `third-party/free-rtos`
    1. `License`
    1. `Source`  

1. In project folder `third-party/free-rtos/Source/portable`, delete all, except:
    1. `GCC` directory
    1. `MemMang` directory
    1. `readme.txt` file  

1. In project folder `third-party/free-rtos/Source/portable/GCC`, delete all other folders not related to processor, i.e. all except `ARM_CM4F`

1. Refresh project in IDE. There are two ways to do this:
    1. Right-click project in IDE select `Refresh`
    1. Select project in IDE, press `f5`  

1. Go to properties for `third-party` folder, sidebar -> `C/C++ Build`, uncheck `Exclude resource from build`

---
---
# Initial Build Errors
After setting up a project with the FreeRTOS kernel source, you will likely run into several build errors. This page describes how to remedy these errors. **Note:** The base project mentioned above has already resolved the errors listed below:

## Source
Lecture 22
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681032

## Include path settings

---
### Can't find `FreeRTOS.h`
Error:
```
fatal error: FreeRTOS.h: No such file or directory
```
Add `third-party/free-rtos/include` to include paths:
1. Go to `project properties -> C/C++ Build -> Settings -> Tool Settings -> (left sidebar) - MCU GCC Compiler -> Include paths` and click the add button
1. Click `Workspace`  
1. Expand project, click on `third-party/FreeRTOS/include` then `OK`, then `OK`  
1. Repeat for `third-party/free-rtos/portable/GCC/ARM_CM4F`

---
### Can't find `FreeRTOSConfig.h`

Error:
```
`fatal error: FreeRTOSConfig.h: No such file or directory
```
The file `FreeRTOSConfig.h` is used to customize the FreeRTOS kernel. It needs to be created and added to the project.

#### Obtain `FreeRTOSConfig.h`
1. Go to [freertos.org](http://freertos.org/) -> kernel -> Developer Docs -> FreeRTOSConfig.h
    1. Alternatively, copy from: `FreeRTOS/Demo/CORTEX_M4F_STM32F407zG-SK`
    - Note the directory in the demo folder should correspond to your processor
1. Put `FreeRTOSConfig.h` in directory `third-party/free-rtos`

#### Add `FreeRTOSConfig.h` to path
1. Go to project `properties -> C/C++ Build -> Settings -> Tool Settings -> (left sidebar) - MCU GCC Compiler -> Include paths`
1. Click add button in top right corner of window
1. Click `Workspace`
1. Expand project, click on `third-party/free-rtos` then `OK`, then `OK`

#### More information on `FreeRTOSConfig.h`
FreeRTOS.org -> Kernel menu -> Developer Docs -> FreeRTOSConfig.h
https://freertos.org/a00110.html


### SystemCoreClock undeclared
Find where it is declared, e.g. in `Core/Src/system_stm32f4xx.c` and extern it. It might be compiled out...

```cpp
#ifdef __ICCARM__
    #include <stdint.h>
    extern uint32_ SystemCoreClock;
#endif
```

to 

```cpp
#if definded (__ICCARM__) || defined (__GNUC__) || defined (__CC_ARM)
    #include <stdint.h>
    extern uint32_ SystemCoreClock;
#endif
```

---
### Multiple definition of SVC_Handler, PendSV_Handler, SysTick_Handler
These handlers are defined in `stm32f4xx_it.c` and `third-party/FreeRTOS/portable/GCC/ARM_CM4F/port.c`

To resolve error, remove definitions from `stm32f4xx_it.c`
using device configuration tool

1. Open `.ioc` file

1. From left sidebar, go to `System Core/NVIC/Code generation/`, uncheck the following from `Generate IRQ handler`:
    - `System service call via SWI instruction`
    - `Pendable request for system service`
    - `Time base: System tick timer`


1. From main menu, go to `Project/Generate Code` or just `ctrl + s` to save .ioc

---
### Undefined references

In `FreeRTOSConfig.h` change

| Undefined Reference             | Modification 
|---------------------------------|--------------------------
| `vApplicationIdleHook`          | `configUSE_DL_HOOK              0`
| `vApplicationTickHook`          | `configUSE_TICK_HOOK            0`
|                                 | `configUSE_MALLOC_FAILED_HOOK   0`
| `vApplicationStackOverflowHook` | `configCHECK_FOR_STACK_OVERFLOW 0`

--
---
---
# Time Base Selection

## Source
Lecture 23
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681034

## Set Stuff
hardware abstraction layer - allows in interact with peripherals

From device configuration tool

| Field                             | Entry
|-----------------------------------|-------------
| `System Core/SYS/Timebase Source` | `TIM6`
| `System Core/NVIC/Priority Group` | `4 bits for pre-emption priority 0 bits for subpriority`

Save the `.ioc` file to regenerate the code.

## Note
After saving the `.ioc` file, a new file in `Core/Src/` should appear called `stm324xx_hal_timebase.tim.c`
