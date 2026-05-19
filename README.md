# Embedded Firmware Guide

## Table of Contents
- [What is embedded firmware?](#what-is-embedded-firmware)
- [Difference between application code and firmware](#difference-between-application-code-and-firmware)
- [Role of firmware in microcontroller-based systems](#role-of-firmware-in-microcontroller-based-systems)
- [What is a firmware library / API?](#what-is-a-firmware-library--api)
- [Why functions like digitalWrite(), delay(), Serial.write() exist](#why-functions-like-digitalwrite-delay-serialwrite-exist)
- [How libraries hide register-level complexity](#how-libraries-hide-register-level-complexity)
- [Why reusable APIs are critical in industry](#why-reusable-apis-are-critical-in-industry)
- [What “bare-metal” means](#what-bare-metal-means)
- [Why no RTOS is used in this program](#why-no-rtos-is-used-in-this-program)
- [How timing, polling, and interrupts are handled without an OS](#how-timing-polling-and-interrupts-are-handled-without-an-os)

---

## What is embedded firmware?
<!-- Describe embedded firmware at a high level: purpose, location (flash/ROM), tight coupling with hardware, boot responsibilities, lifecycle. -->
Firmware is low-level software that is required to get a device (hardware) up and running before being used by an application. It exists in a read-only memory location of the device (flash memory) and is usually the first set of instructions that the device executes after power on. In general purpose computers, firmware is used to load the OS kernel from hard-drive to RAM and as support for other software. In embedded systems, firmware is the only software present to control the system. Firmware is specific to the hardware it is responsible for. 

---

## Difference between application code and firmware
<!-- Contrast responsibilities, abstraction levels, portability, update frequency, and dependencies. Provide examples where boundaries blur. -->
Application code answers the question: what to do? (logic)

Firmware answers the question: how to do it? (hardware)

Suppose we want to blink an LED using arduino. Application code is the code written in Arduino IDE - first set the pin mode (setup), then the digital write function is called to set the pin value. Functions like digitalWrite and pinMode provide a way for applications to use the GPIO peripherals without knowing the register-level details. 

Application code decides what to do with the given hardware. Firmware actually gets it done in that hardware. 

Since application code is more logical and general, it is highly portable and not specific to a board or MCU. Firmwares are specific to a board and MCU (they can be used for devices of the same MCU family though). Firmware is essentially an interface between the application code and the hardware.

---

## Role of firmware in microcontroller-based systems
<!-- Explain hardware init, drivers, board support, boot, power management, safety, diagnostics, and real-time control. -->
Firmware in a microcontroller-based system is the low-level software responsible for initializing hardware, controlling peripherals, managing real-time operations, and enabling communication between hardware and application software. It is typically organized into layers such as drivers, the Hardware Abstraction Layer (HAL), and the Board Support Package (BSP), where the HAL provides generic access to MCU peripherals and the BSP contains board-specific hardware mappings. Firmware also handles system startup, boot processes, power management, diagnostics, safety mechanisms, interrupt handling, and communication protocols, forming the essential interface between the physical hardware and higher-level system functionality.

---

## What is a firmware library / API?
<!-- Define libraries, HAL, SDKs, middleware. Explain API surfaces, versioning, and how they enable modularity. -->
An Application Programming Interface (API) is a set of rules and functions to exchange data between two applications. For eg. if a food delivery app needs to display real-time location and maps, it would use Google Maps API to get the required data. This ensures the heavy lifting for that specific task is done by a well-established software instead of building one from scratch. It decreases the development time and allows the incorporation of different features into an application without much effort.

Similarly, to ensure easy use of hardware, the low-level implementation details are hidden under functions that can be used directly in the application. These functions form a Firmware API/library. For eg, an application can communicate with the uart transciever module using functions like uart_send, uart_recieve, etc. These procedures reflect what the module's functionality and with the knowlegde of the required parameters, users don't need to know the underlying complexities to use the module in their application.

---

## Why functions like digitalWrite(), delay(), Serial.write() exist
<!-- Motivate simplicity, portability, teaching, rapid prototyping, and mapping to underlying peripherals/timers/UARTs. -->
The concept of abstraction is very powerful in any engineering discipline. By dividing a domain into multiple layers, multiple engineers can focus on multiple layers at a time, increasing productivity. And by having a clear boundary between layers, finding and solving bugs becomes easier as well. 

Any hardware/device has engineers who design it and those who use it. Each of this is a big task on it's own, and requires years of study and experience. To decouple the design and utility aspects of a device is, hence, extremely beneficial. Design engineers create the firmware library, and the function prototypes (or the interface) is provided to the application engineers. Now, the application engineers don't have to know the device-specific details to use that hardware, they can fully focus on the appropriate application. 

An example would be the Arduino IDE. One doesn't need to know about the ATmega MCUs to get started with the arduino board. This decreases development time, and helps in rapid prototyping. 

---

## How libraries hide register-level complexity
<!-- Show conceptually how macros, inline functions, and drivers abstract registers, bitfields, and clock trees. Mention trade-offs. -->
Firmware libraries (the .h files) contain function prototypes (or declarations), macros and structs. The function definition is defined in a separate .c or .cpp file (or it may be precompiled, like stdio.h library definition). The register level details are present in the definitions. When a code file is compiled, the linker replaces the function calls by their definitions to create the final executable or binary file. Thus, to write an application code, register-level details need not be known. Macros rename the register addresses and other specific values to a more general word, so they can be used in the application code. 

---

## Why reusable APIs are critical in industry
<!-- Cover maintainability, time-to-market, testing, certification, team scaling, vendor independence, and long-term support. -->
Having reusable APIs ensures the application code is not changed if the underlying hardware needs to be changed. APIs are just function prototypes, they don't include the implementation. Hence, for different devices with the same functionality, only the implementation needs to be changed, keeping the API as is. For e.g., the basic function of a uart transciever module is to send and recieve data. So it's API must have uart_send and uart_recieve. Different MCUs might have different registers mapped to the uart interface, but the function remains same. This also helps maintain a standard that every manufacturer must ensure for that device. 

---

## What “bare-metal” means
<!-- Define no OS/RTOS, direct hardware control, cooperative scheduling, deterministic timing expectations. -->
Bare-metal refers to software running directly on the hardware without an Operating System. There is no OS or RTOS scheduling tasks or managing memory. The programmer has complete control over all the hardware resources (memory, clock, power). The overall software is simple, takes up less memory and deterministic - essential for real-time systems. 

---

## Why no RTOS is used in this program
<!-- Justify by simplicity, resource limits, latency needs, certification scope, or static scheduling sufficiency. -->
Choosing bare-metal over an Real-Time Operating System (RTOS) is usually because of: 
- Resource Constraints: Small MCUs don't have enough memory to store and run the RTOS. 
- Simplicity: If the application code is simple, then OS is not required to manage it. In fact, it's faster to run it directly on hardware.
- Latency: Task-switching in an OS takes time (context switching). Bare-metal reacts to hardware events with the absolute minimum delay.
  
---

## How timing, polling, and interrupts are handled without an OS
<!-- Explain busy-wait vs hardware timers, periodic ISRs, event flags, debouncing, priority, latency, and critical sections. -->
In bare-metal embedded systems, there is no operating system to schedule tasks, so the firmware directly controls execution using a main loop, hardware timers, polling, and interrupts. 
- The processor checks device status registers periodically (eg. TX_READY is checked before transmitting data using UART transmitter).
- When an interrupt occurs (seen in the interrupt registers), the CPU pauses the `main()` program, shifts to the respective ISR (interrupt service routine), executes that, then resumes the `main()` program.
-  Delays can be executed as a loop in the CPU, but it's inefficient. Timer peripherals are used for timed interruptions, generating events etc. 
