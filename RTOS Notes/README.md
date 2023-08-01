<p align="center">
  <h1>RTOS Summary</h1>
</p>


## Task/Thread:

> * it is a block of an app written under RTOS.
>
> * A simple program with an infinite loop.
>
> * Each task has its own CPU registers, stack, and priority.

## Multitasking:

> * is the process of switching the CPU between several tasks.
>
> * This maximize the utilization of CPU
>
> * Provides modular construction for our application, makes the app design easier.

## Resource
> is an entity used by the task, it can be:
>
> * I/O device
>
> * Variable
>
> * File

## Critical Region
> is a section of code that should not be interrupted (ex: disable interrupt and enable it again after finishing)

## Kernel
> is the core of any OS. It contains:
>
> * Scheduler: set of algorithms that determines which task should start and when. (core of the kernel).
>
> * Objects: special kernel constructs that help developers to create apps for Real-Time ES (such as "ISR").
>
> * Services: operations that the kernel performs on an object.

    Look at RTOS Task Status {Ready, Running, Blocked}.

    Look at Non-Preemptive and preemptive kernels.

## CPU starvation
> is a phenomenon associated with the Priority scheduling algorithms. A process that is present in the ready state and has low priority keeps waiting for the CPU allocation because some other process with higher priority comes with due respect time.

## What if two tasks in the ready state have the same priority?

> * Some RTOS forbidden this behaviour and in this case the number of tasks is limited to the number of priority levels.
> * Others will slice time between the two tasks (Round Robin)
> * Some will run one task until it gets blocked and then run the other.

    Look at System Tasks (The system uses it to run its internal operations)

## Task Context 
> * Each task has its own context (data), data structure called TCB, stack, and CPU registers.
>
> * Data saved in Task Context Block (TCB):
>
>> * Task status
>>
>> * ID
>>
>> * Priority
>>
>> * Stack Pointer
>>
>> * Pointer to task itself

## Context Switching 
> is how to switch the processor between the context of one task to another.
>
> * when the task is not running, its context is frozen within the TCB to be restored the next time the task runs.
>
> * The Dispatcher: is part of the scheduler which is responsible of context switching.
>
> * Frequent context switching causes a performance overhead.
>
> * USED COMPONETS:
>
>> * General Purpose Registers to store the data of the task
>>
>> * Status Register (SREG)
>>
>> * Program Counter (PC)
>>
>> * Stack Pointer

## Non-Reentrant function

> * is a function that cannot be used between more than one task, and cannot be interrupted or data loss will happen.
>
> * Must not use any global variables.

## Reentrant Function

> * is a function that can be used between more than one task and can be interrupted without fear of data loss.
>
> * Use local variables or use protected global variable.
>
> * Cannot call another Non-Reentrant function.

## Gray Area of Reentrancy

> Some functions and some operations on a shared data are processor and compiler dependent.
>
> Examples:
>>
>> * printf()
>>
>> * a++

## How to protect shared data?

> Using Mutual Exclusion Access
>
> Examples:
>
>> * Disable and Enable Interrupts (Not a good solution)
>>
>> * Disabling scheduling
>>
>> * Using Semaphores
