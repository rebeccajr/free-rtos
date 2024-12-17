Lecture 29
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682186


## ITM (Instrumentation Trace Macrocell)
ITM is an optional application-driven trace source that supports printf style debugging.

To implement, modify `syscalls.c` as noted below.

```
/* Includes */
#include <sys/stat.h>
#include <stdlib.h>
#include <errno.h>
#include <stdio.h>
#include <signal.h>
#include <time.h>
#include <sys/time.h>
#include <sys/times.h>

//------------------------------------------------------------------------------
// Start ITM code
//------------------------------------------------------------------------------
//
// Source:
// https://github.com/niekiran/Embedded-C/blob/master/All_source_codes/target/itm_send_data.c
//------------------------------------------------------------------------------
//
// Implementation of printf like feature using ARM Cortex M3/M4/ ITM functionality
// This function will not work for ARM Cortex M0/M0+
// If you are using Cortex M0, then you can use semihosting feature of openOCD
//------------------------------------------------------------------------------

// Debug Exception and Monitor Control Register base address


#define DEMCR        			*((volatile uint32_t*) 0xE000EDFCU )

/* ITM register addresses */
#define ITM_STIMULUS_PORT0   	*((volatile uint32_t*) 0xE0000000 )
#define ITM_TRACE_EN          	*((volatile uint32_t*) 0xE0000E00 )

void ITM_SendChar(uint8_t ch)
{

	//Enable TRCENA
	DEMCR |= ( 1 << 24);

	//enable stimulus port 0
	ITM_TRACE_EN |= ( 1 << 0);

	// read FIFO status in bit [0]:
	while(!(ITM_STIMULUS_PORT0 & 1));

	//Write to ITM stimulus port0
	ITM_STIMULUS_PORT0 = ch;
}

//------------------------------------------------------------------------------
// End ITM code
//------------------------------------------------------------------------------

//...
__attribute__((weak)) int _write(int file, char *ptr, int len)
{
  (void)file;
  int DataIdx;

  for (DataIdx = 0; DataIdx < len; DataIdx++)
  {
    //--------------------------------------------------------------------------
    // The call to _io_putchar funtion is commented out and replaced with a call to
    // ITM_SendChar to use the ITM Macrocell to print to the ITM console
    //--------------------------------------------------------------------------
    //__io_putchar(*ptr++);
    ITM_SendChar(*ptr++);
    //--------------------------------------------------------------------------

  }
  return len;
}

```
In `main.c` include the standard C IO header file:

```
#include <stdio.h>
```

## Show Printing

`Main Menu -> Window -> Show View -> SWV -> SWV ITM Data Console`

Go to settings when in tab for SWV ITM Data Console

Select ITM Stimulus Ports 0  

Start trace  

