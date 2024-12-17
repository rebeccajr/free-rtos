# Idle Task
- Created automatically
- Lowest priority
- At least one task is always running on CPU (e.g. idle task)
- Responsible for freeing memory allocated by RTS to tasks that have been deleted

# Timer Service Task

- AKA timer daemon task
- Deals with "software timers"
- Created automatically when scheduler is started and `FreeRTOSConfig.h` has
    ```
    #define configUSE_TIMERS 1
    ```

- Used to manage FreeRTOS software timers
- Not necessary if not using software timers, so in `FreeRTOSConfig.h`
    ```
    #define configUSE_TIMERS 0
    ```
- Software timer callback functions execute in context of the timer daemon task