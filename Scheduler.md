The scheduler dispatches tasks to run. It is a block of code that runs in the privileged mode of the processor.

## Source
https://www.udemy.com/course/mastering-rtos-hands-on-with-freertos-arduino-and-stm32fx/learn/lecture/25682178

[[_TOC_]]


To invoke in FreeRTOS, run
```
vTaskStartScheduler();
```

# Scheduling Policies
1. Pre-emptive scheduling
2. Co-operative scheduling

In `FreeRTOSConfig.h` macro `configUSE_PREEMPTION` determines scheduling policy.

Definition                 | Scheduling Policy
-|-
`configUSE_PREEMPTION = 1` | pre-emptive scheduling
`configUSE_PREEMPTION = 0` | co-operative scheduling

## Pre-emptive Scheduling
 Pre-emption is when a running task is replaced with another task.

During pre-emption, the running task gives up the processor even if it hasn't finished its work. When this happens, the previously running task returns to ready state.

### Types of pre-emptive scheduling
1. Cyclic aka Round-robin
2. Priority-based

### Cyclic

Tasks are scheduled without priority; each task runs for an equal amount of time in circular order. You can implement this with a timer interrupt.

This macro in `FreeRTOSConfig.h` sets the **tick rate** (the timer interrupt) to 1000 ms; i.e. the scheduler is invoked every 1000 ms.
```cpp
#define configTICK_RATE_HZ( ( TickType_t ) 1000 )       // sets the tick rate to 1000 ms
```

### Priority-based 
Scheduler uses task priority to determine which ready-state task to run. The higher the priority value, the higher the priority.

A task with the highest priority will continue to run indefinitely unless it leaves voluntarily to allow other tasks to run, gets deleted, blocked or suspended.

## Cooperative
Task voluntarily yields the processor. No pre-emption of tasks by scheduler. RTOS tick still occurs, but does not invoke pre-emption of other tasks.

---
# Task States

State     | Definition
-|-
Ready     | Task is the waiting room, ready to run when the scheduler says so.
Blocking  | Temporarily yielding the CPU until a specified event is met.

---
# Other Terminology

Term | Definition
-|-
Tick              | A timer interrupt. Any timer peripheral can be used for an RTOS tick.
Context Switching | When the scheduler pre-empts a task with another task, it saves the context of the running task (the one that is pre-empted) and retrieves the context of the pre-empting task.