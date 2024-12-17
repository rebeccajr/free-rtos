# Description
This page describes the directories contained in the FreeRTOS download described in this wiki.

Lecture 18: https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25681012

# Directory Summary
Directory | Description
-|-
`FreeRTOS`                 | Free RTOS kernel files
`FreeRTOS\Source`          | Source code for FreeRTOS kernel
`FreeRTOS\Source\include`  | Header files for FreeRTOS kernel
`FreeRTOS\Source\portable` | Processor architecture specific code
`FreeRTOS-Plus`            | Middleware
`FreeRTOS-Plus\CLI`        | Command line interface libraries

# `Source`
These files are architecture independent.  
![image](https://github.com/rebeccajr/rtos/assets/26588191/68c87137-f421-444d-a905-776bdf0ba2e1)


# `portable`
This directory contains the processor architecture specific code. All subfolders refer to different compilers.  
![image](https://github.com/rebeccajr/rtos/assets/26588191/33e250a7-35b9-45e4-abb8-85b9c2e24adc)
The subfolders within the compiler directories correspond with the different processor architectures.  
![image](https://github.com/rebeccajr/rtos/assets/26588191/e7b7044d-3f17-4b2e-9f89-f6a2b12a4a8e)


When adding this source code to a project, it is only necessary to include the directories corresponding to the compiler(s) you will be using.

# `Demos`
Demo projects for different architectures.
