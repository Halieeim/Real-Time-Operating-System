<h1 align="center">RTOS Summary</h1>


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
> is a section of code that should not be interrupted.

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
> * is a phenomenon associated with the Priority scheduling algorithms. A process that is present in the ready state and has low priority keeps waiting for the CPU allocation because some other process with higher priority comes with due respect time.<br>
> * In a preemptive multitasking environment, if a higher priority tasks are not designed to block, CPU starvation is most likely to happen.

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
>> * __Disable and Enable Interrupts__ 
>>> __Note1:__ Disabling the interrupt for a long time affect the response time of the system which known as __Interrupt Latency__. So this should be for as little time as possible.<br>
>>> __Note2:__ It's needed when sharing data with ISR.<br>
>> * __Disabling scheduling__
>>> __Note1:__ If we do not share data with ISR, it is better to disable and enable scheduler.<br>
>>> __Note2:__ Here ISR will be executed immediately and when it is finished, the kernel will return to the interrupted task not the highest priority task, because of disabling the scheduler.<br>
>> * __Using Semaphores__
>>> ```-```Its a kernel object that one or more threads of execution can __acquire__ or __release__ for the purposes of synchronization or mutual exclusion.<br>
>>> ```-```A Semaphore is like a key that enables a task to carry out some operations or to access a resource.<br>
>>> ```-```It does not affect Interrupt Latency.<br>
>>> ```-```It runs the task with highest priority immediately.<br> 
>>> **Note:** When a Semaphore is created, the kernel assigns to it:
>>> * An associated semaphore control block (SCB)
>>> * A unique ID
>>> * A value (binary ot count according to its type)
>>> * A task-waiting list
>>>
>>> __Semaphore Types:__
>>> * **Binary Semaphore**<br>
>>> value = 0, if it is not available.
>>> value = 1, if it is available.
>>> * **Counting Semaphore**<br>
>>> value = 0, if it is not available.
>>> value > 0, if it is available.<br>
>>> **Note:** In Counting Semaphore, more than one task can acquire the semaphore at the same time, unlike binary semaphore that allows only one task to acquire the semaphore.

__Interrupt Latency:__ is the time taken by the system to respond to an interrupt.<br>
**Note:** In both Semaphore types, any task can release the semaphore even if the task did not initially acquire it.

## Dead Lock
> ```-```Also called Deadly Embrace.<br>
> ```-```Its main reason is bad design.<br>
> ```-```When two tasks are waiting the resource held by the other.<br>
> Example:
>> * Task1 has an exclusive access to Resource1.
>> * And Task2 has an exclusive access to Resource2.
>> * If task1 needs an exclusive access to Resource2.
>> * And task2 needs an exclusive access to Resource1.
>> * Both tasks will be blocked, and a DeadLock will happen.<br>
>
> **Avoiding Dead Lock**
> * Through a timeout; If the resouce is not available for a certain time, the task will resume executing.
> * Good Design.

## Resource Synchronization
> * Determines whether access to a shared resouce is safe, and if not, when it will be safe.
> * Access by multiple tasks must be synchronized to maintain the integrity of a shared resource.
> * Semaphore are useful either for synchronizing execution of multiple tasks or for coordinating access to a shared resource.

## Priority Inversion
> Is a situation in which a low-priority task executes while a higher priority task is waiting.

## Priority Inheritance
> * is a resource access control protocol that raises the priority of a task.
> * The task must has dynamic priority not static.
> * This help reducing the impact and time of inversion priority
> * Priority Inheritance does not cure priority inversion, it just minimzes its effect in some applications

## Race Condition
> When two or more tasks have access to a shared resource and they try to edit it at the same time.

## Mutex
> * It is a short for MUtual EXclusion.
> * Mutex is a special type of binary semaphore used for controling access to the shared resource.
> * Mutex include priority inheritance mechanism to avoid extended priority inversion problem.
>* Hard real time applications should be designed such that priority inversion does not happen in the first place.

## Mutex VS. Binary Semaphore
> * **Ownership**
>> * Mutex is owned by the task that takes it, so when a task locks (acquire) a mutex only this task can unlock it.
>> * Binary Semaphore has no owner, any task can unlock it.
> * **Usage**
>> * **Mutex is a locking mechanism** used to synchronize access to a shared resource.
>> * **Binary Semaphore is a signaling mechanism** ("I am done, you can carry on." kind of signal).

## Determinism VS. Responsiveness
> * **Determinism:** determine the time that every task will need to be executed.
> * **Responsiveness:** How your application be responsive to (External or Internal) events.
> * **RTOS increases Responsiveness and decreases Determinism** because most of RTOS depend on preemptive kernel.

## RTOS Advantages
> * Multitasking
> * Services
> * Structure
> * Portablitiy
> * Security
> * **Debugging** some IDEs show nice live data about you running process
> * Support

## RTOS Disadvantages
> * Resources
> * Determinism
> * Design
> * **Debugging** (complex due to race conditions on shared resources)
> * Cost

    For more details about RTOS pros & cons look at: 
    "Why RTOS1", "Why RTOS2", "Why RTOS3", and "Why Not RTOS"

## Rate Monotonic Scheduling (RMS)
> * Task priorities are based on how often tasks execute.
> * RMS makes a number of assumptions:
>> * All tasks are periodic.
>> * Tasks do not synchronize with another task (No shared resources).
>> * Preemptive scheduling must be used.

## Jitter
> The difference between when a periodic task is supposed to run and when it actually runs.

## Utilization
> is defined as the percentage of available resources that are actually used.