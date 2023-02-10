---
sidebar_position: 1
---
# Create new project

## Setting up STM32 project

For this tutorial we will be using the [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html). This will give us all the tools to easily program any STM32 microcontroler and more.

Steps:
- Open STM32CubeIDE
- Click on create new STM32Project
- In board selector find your microcontroler
- Download the reference manual for your microcontroler (this will help us understand all the microcontroler functionality)
- Name and create your new project
- In the config file .io first configure your main clock found under RCC. 

## Library files
If in your project you decide to have library files outside the **\Src** and **\Inc** folders then we must specify under project properties the files location so the linker can find the library files.

