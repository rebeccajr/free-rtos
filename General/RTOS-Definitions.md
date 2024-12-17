Real time guarantees response time

# Definitions

Term | Definition
-|-
latency                | The time that elapses between a stimulus and the response to it
task                   | A piece of code that is schedulable
task switching latency | The time gap between a triggering of an event and time time at which the task which takes care of that event is allowed to run on the CPU
preemption             | The act of temporarily removing the task from the running state without its co-operation <br>kernel operations of RTOS are pre-emptible <br>kernel operations of GPOS are not
priority inversion    | When a lower priority task starts executing before a higher priority task
TCB                   | Task Control Block, area of memory that contains information relevant to the task

## Task States
State         | Definition
-|-
Ready         |
Blocking      |



