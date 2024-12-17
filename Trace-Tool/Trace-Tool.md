## Source
Lecture 31
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682210

Download
go to segger.com/Downloads/SystemView

1. SystemView 
1. SystemView, Target Sources 

# SystemView
A software toolkit used to analyze embedded software behavior running on target
Can be used with many types of RTOS, or other OS

# Capabilities
1. anaylyze how many tasks are running, how much duration they consume
1. ISR entry and exit timing
1. behaviors like blocking, unblocking, notifying, yielding
1. analyze idle time
1. total runtime behavior

## 2 Parts
1. PC visualization software (host software)
1. SystemView target code (runs on target) - used to collect target events and send back to PC visualization software

---
# SystemView Visualization Modes

1. Real time recording (continuous recording)
    - Continuously record data, analyze and visualize in real time

2. Single-shot recording
    - recording started manually

.svdat is file extension

---
# Lecture 36: Segger SystemView Target Integration

Download target source from web, extract all

In project, create directories:
```
third-party/segger
third-party/segger/config
third-party/segger/os
third-party/segger/patch
third-party/segger/segger
```

## Step 1: Copy Files

### `third-party/segger/config`
Copy these files from downloaded directory into `third-party/segger/config`

```
SystemView_Src_V###/Config/Global.h
SystemView_Src_V###/Config/SEGGER_RTT_Conf.h
SystemView_Src_V###/Config/SEGGER_SYSVIEW_Conf.h

SystemView_Src_V###/Sample/FreeRTOSV10.4/Config/Cortex-M/SEGGER_SYSVIEW_Config_FreeRTOS.c
```

### `third-party/segger/os`
Copy these files from downloaded directory into `third-party/segger/os`

```
SystemView_Src_V###/Sample/FreeRTOSV10.4/SEGGER_SYSVIEW_FreeRTOS.c
SystemView_Src_V###/Sample/FreeRTOSV10.4/SEGGER_SYSVIEW_FreeRTOS.h
```

### `third-party/segger/patch`
Copy file from udemy course `FreeRTOSv202012.00_segger_cm4_v1.patch` into `third-party/segger/patch`

### `third-party/segger/segger`
Copy all files from downloaded directory into `third-party/segger/segger`

```
SystemView_Src_V###/Segger
```

In Syscalls, delete all files except the one related to the compiler. In the case of this class, keep only `SEGGER_RTT_Syscalls_GCC.c`

---
## Step 2: Patch File

In IDE, right-click `third-party` folder, select `Team/Apply Patch...`

---
## Step 3: FreeRTOSConfig.h Settings
1. Include `SEGGER_SYSVIEW_FreeRTOS.h` at end of `FreeRTOSConfig.h`

1. Include these macros in `FreeRTOSConfig.h`

    ```
    #define INCLUDE_xTaskGetIdleTaskHandle 1
    #define INCLUDE_pxTaskGetStackStart    1
    ```
---
## Step 4: MCU and Project Specific Settings

1. Indicate which processor core your MCU is using in `SEGGER_SYSVIEW_ConfDefaults.h`

    Choose one of these macros

    ```
    #define SEGGER_SYSVIEW_CORE_OTHER   0
    #define SEGGER_SYSVIEW_CORE_CM0     1 // Cortex-M0/M0+/M1
    #define SEGGER_SYSVIEW_CORE_CM3     2 // Cortex-M3/M4/M7
    #define SEGGER_SYSVIEW_CORE_RX      3 // Renesas RX
    ```

    In this code snippet, replace `SEGGER_SYSVIEW_CORE_OTHER` with macro from above
    ```
    #ifndef   SEGGER_SYSVIEW_CORE
      #define SEGGER_SYSVIEW_CORE SEGGER_SYSVIEW_CORE_OTHER
    #endif
    ```

1. Modify SystemView buffer size configuration in `SEGGER_SYSVIEW_ConfDefaults.h`
    ```
    #ifndef   SEGGER_SYSVIEW_RTT_BUFFER_SIZE
      #define SEGGER_SYSVIEW_RTT_BUFFER_SIZE          1024
    #endif
    ```

1. Configure application specific information in `SEGGER_SYSVIEW_Config_FreeRTOS.c`

    Replace App name 
    ```
    #define SYSVIEW_APP_NAME        "FreeRTOS Demo Application"
    
    #define SYSVIEW_DEVICE_NAME     "Cortex-M4"
    ```

---
## Step 5: Enable the ARM Cortex M3/M4 Cycle Counter


In `main.c` add

```
/* USER CODE BEGIN PV */

#define DWT_CTRL (*(volatile uint32_t*)0xE0001000)

/* USER CODE END PV */

// in int main

DWT_CTRL |= (1 << 0);
```

---
## Step 6: Start Recording Events

Call these SEGGER APIs
```
SEGGER_SYSVIEW_Conf();
SEGGER_SYSVIEW_Start();
```

Event recording stars only when `SEGGER_SYSVIEW_Start()` is called.


---
## Step 7: Compile and Debug
1. Ensure include paths are set.
1. Compile and flash FreeRTOS + SystemView
1. Enter debugging mode in IDE.
1. Run and pause after a few seconds.

```
fatal error: SEGGER_RTT_Conf.h: No such file or directory
```
Add this path to assember

Go to 
`Project properties -> C/C++ Build -> Settings -> MCU GCC Assembler -> Include paths`

add path to `third-party/segger/config`

### Priority Grouping Initialization
Priority grouping initialization required for `SEGGER_SYSVIEW_Start()`

In `Core/Src/stm32f4xx_hal_msp.c` add 

  ```

  /* USER CODE BEGIN MspInit 1 */

  vInitPrioGroupValue()

  /* USER CODE END MspInit 1 */
  ```

---
## Step 8: Collect Recorded Data from RTT Buffer

### Single-shot Recording

1. Get the SystemView RTT buffer address and number of bytes used
    ```
    _SEGGER_RTT.aUp[1].pBuffer
    _SEGGER_RTT.aUp[1].WrOff
    ```

    1. After running and suspending, go to Expressions tab (`Main Menu/Window/Show View/Expressions`)
    1. `Add new expression/_SEGGER_RTT`
    1. Expand `_SEGGER_RTT/aUp/aUp[1]/pBuffer` and obtain value, e.g. 0x200132b0
    1. Note value from `_SEGGER_RTT/aUp/aUp[1]/WrOff`

1. Export memory dump to file
    1. `Main Menu/Window/Show View/Memory Browser` paste address into search field
    1. Click export - for length, use value from `WrOff`
    1. Select RAW Binary

1. Open SystemView software -> `Main Menu/File/Load Data`

---
## Lecture 38: View Print Statements

Use instead of `printf`

```
char msg[100];
snprint(msg, 100, "%s\n", (char*) params);
SEGGER_SYSVIEW_PrintfTarget(msg)
```