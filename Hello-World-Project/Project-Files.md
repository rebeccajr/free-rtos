# Description
This page describes the default files that are included with a new project in STM32CubeIDE and 

# Files

| File Name          | Purpose
|--------------------| -----------------
|`Core/Src/main.c`            | application code
|`Core/Src/stm32fxx_hal_msp.c`| device initialization for peripherals
|`Core/Src/stm32fxx_it.c`     | interrupt handlers and system exceptions
|`Core/Src/syscalls.c`        | implements standard library system calls e.g. write, read
|`Core/Src/sysmem.c`          | heap management / dynamic memory allocation, see Note 1
|                    |
| `FreeRTOSConfig.h` | configuration header file used to customize FreeRTOS kernel source, needs to be added externally. See Note 2.

## Note
1. The file `sysmem.c` is not needed by this project. Go to properties and check `Exclude resource from build`
2. The file `FreeRTOSConfig.h` is included with the FreeRTOS kernel source code and needs to be added to the project externally.

---
# Unneeded Files
This page describes files that are unneeded in the `Hello world` project.

## `Core/Src/sysmem.c`
Exclude this file from the build.
1. Right-click on file, go to `Properties`->`C/C++ Build`, check `Exclude resource from build`

## `heap1_.c`, `heap2_.c`, `heap3_.c`, `heap5_.c` in `third-party/free-rtos/Source/portable/MemMang/`
Heap management is done in FreeRTOS code in `portable->MemMang->heap_4.c`; delete all other `.c` files in `Memmang` directory.