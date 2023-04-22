# FRERTOS

FreeRTOS is a real-time operating system kernel for embedded devices. It is designed to be small, simple, and easy to use. It is distributed under the MIT license.

## Preemptive vs. Non preemptive multitasking
Preemptive multitasking is when the operating system can interrupt a task at any time. Non preemptive multitasking is when the operating system can only interrupt a task when it is blocked.

## Real time vs. Non real time operating system
A real time operating system is an operating system that guarantees that a task will be completed within a certain time. A non real time operating system is an operating system that does not guarantee that a task will be completed within a certain time.

## What is a task?
A task is a unit of execution. A task can be a thread, a process, or a function. A task can be preempted by another task.

## Getting started with FreeRTOS on STM32

To get started with FreeRTOS on STM32 we will be using the STM32CubeIDE. The STM32CubeIDE is an integrated development environment (IDE) for STM32 microcontrollers. It is based on Eclipse and is available for Windows, Linux, and macOS. It is free to use and can be downloaded from [here](https://www.st.com/en/development-tools/stm32cubeide.html).

### Using FreeRTOS with STM32CubeIDE

Using FreeRTOS with STM32CubeIDE is very easy. First we need to create a new project. We can do this by clicking on `File -> New -> STM32 Project`. We will be using the NUCLEO-F401RE microcontroller. We can then select the `FreeRTOS` template. This will create a new project with FreeRTOS already configured, where freeRTOS is abstracted behind the CMSIS-RTOS API. There are a few difrences between the CMSIS-RTOS API and the FreeRTOS API. The main difference is that what is called a task in FreeRTOS is called a thread in the CMSIS-RTOS API. 




