
# Stuck in HardFault_Handler
In `stm32f4xx_hal_msp.c`
```cpp
void HAL_MspInit(void)
{
  //...

  //----------------------------------------------------
  // change NVIC_PRIORITYGROUP_0 to NVIC_PRIORITYGROUP_4
  //----------------------------------------------------
  HAL_NVIC_SetPriorityGrouping(NVIC_PRIORITYGROUP_4);

  /* System interrupt init*/

  /* USER CODE BEGIN MspInit 1 */

  /* USER CODE END MspInit 1 */
}
```