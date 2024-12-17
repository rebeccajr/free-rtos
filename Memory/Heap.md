Elements that are created on the heap:
- Task control block
- Stack for task
- Semaphore
- Queues
   - Queue control block
   - Item list


```cpp
xSemaphoreCreateBinary()
xQueueCreate()
```


heap memory managed in `heap_#.c` e.g. `third-party/FreeRTOS/portable/MemMang/heap_4.c`

In `heap_4.c`
Heap is an array. This file manages memory allocation in this array.

```cpp
    extern uint8_t ucHeap[ configTOTAL_HEAP_SIZE ];
```
