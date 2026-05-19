# Part C: Understanding a simple firmware library

## Table of Contents
- [Environment setup](#Environment-setup)
- [Understanding gpio.h](#Understanding-gpio.h)
- [Understanding gpio.c](#Understanding-gpio.c)
- [Understanding main.c](#Understanding-main.c)
- [Simulation Output](#Simulation-output)

---

## Environment setup
The project is done in a virtual machine, with following specifications:
- Virtual Machine environment: Oracle VirtualBox
- OS: Ubuntu 22.04
- RAM: 6 GB
- Hard disk: 10 GB

To setup the VM, [this](https://linuxvox.com/blog/install-vm-ubuntu/) guide was referred. 

After setting up, Git, GCC, and VS Code are installed using the following commands: 
```
sudo apt install git
sudo apt install build-essential
sudo snap install --classic code
```

To verify: 
```
git --version
gcc --version
code   //launches vs code software
```
Then git clone [this](https://github.com/vsdip/vsdsquadron-mini-core) repo.
Under this, the task 1 is defined. The purpose is to simulate (since no hardware is involved) a GPIO library (a library consists of the API or .h file and its implementation). 


This marks the end of setting up. 

---

## Understanding gpio.h

### Purpose: Contains function prototypes, structures, and macro definitions. Necessary to write the application code.

```
#ifndef GPIO_H
<code block>
#endif
```
Means: If GPIO_H is not defined, then execute the code block; else, skip it. 

`#define GPIO_H`

Means: Create a preprocessor macro named GPIO_H, but is not assigned any value. This just says that GPIO_H exists. 

These are called header guards. They prevent the same header file from being included multiple times during compilation, preventing duplicate function definitions and compiler errors.  
Some applications use `#pragma once` instead.  

When the file gpio.h is included for the first time `GPIO_H` is not defined, so the compiler defines it and the contents of file are included. 
If included again, `GPIO_H` already exists, so the compiler skips the file contents. 

```
/* GPIO direction definitions */
#define GPIO_OUTPUT 1
#define GPIO_INPUT  0
```
These are macros/constants. They give readable names to values. These names are much easier to use as arguments (to function calls) instead of complex register names or memory addresses that usually the values. This also serves as a way to make application code easier.

```
/* API functions */
void gpio_init(int pin, int direction);
void gpio_write(int pin, int value);
int  gpio_read(int pin);
```

These are the function declarations/prototypes. These represent the basic functions/behaviour associated with a GPIO pin: 
- initialise it as input or output
- write a value to it
- read a value from it

This entire file is considered the API. Application code can be written by using this as a reference. 

---

## Understanding gpio.c

### Purpose: Contains definitions for the functions declared in the API or `.h` file

```
#include <stdio.h>
#include "gpio.h"
```
`stdio.h` is included since the code is written in C language. `gpio.h` contains function declarations, so it must be included. 

```
void gpio_init(int pin, int direction)
{
    if (direction == GPIO_OUTPUT) {
        printf("GPIO %d initialized as OUTPUT\n", pin);
    } else {
        printf("GPIO %d initialized as INPUT\n", pin);
    }
}

void gpio_write(int pin, int value)
{
    printf("GPIO %d write value: %d\n", pin, value);
}

int gpio_read(int pin)
{
    printf("GPIO %d read value\n", pin);
    return 1; // simulated value
}
```

A real GPIO API is much more complex and involves register-level details. To read more, refer to [this](https://github.com/arduino/ArduinoCore-avr) repo, which defines the source code for functions used in Arduino boards. 

---

## Understanding main.c

### Purpose: the application code. Serves as an example to use the API.

Typically, when a board (or hardware) is provided, the vendor API and at least one example application code come as a package. The implementation files are given as precompiled binary files, used as a static library. 

A typical compilation process looks like: 
- Preprocessing: all the macros replaced with values and the preprocessor directives (like #ifdef, #ifndef) are processed.
- Compilation: Syntax checks, conversion from HLL to assembly language (produces a `.s` file). 
- Assembler: Converts assembly language to machine code to produce an object file (`.o`). 
- Linking: The linker combines multiple object files and libraries, resolves function and variable references, and generates the final executable file.
For library functions, two types of libraries may be used:
- Static library: Required library code is copied into the final executable during linking.
- Dynamic library: Library code is loaded during runtime by the operating system. For embedded systems without an OS, static libraries are used. 

In most bare-metal embedded systems (without an OS), static linking is typically used because dynamic loading support is unavailable (shared file system not present).

```
#include <stdio.h>
#include "gpio.h"
```
Including the necessary header files.

```
#define LED_PIN 5
#define BTN_PIN 3
```
Defining macros to make the code more readable and to make application code more portable. (If the hardware is changed, you only need to change the header files and pin numbers)

```
int main(void)
{
    printf("Starting firmware application\n"); 

    gpio_init(LED_PIN, GPIO_OUTPUT);  //initialise pin 5 as output
    gpio_init(BTN_PIN, GPIO_INPUT);   // intitialise pin 3 as input

    gpio_write(LED_PIN, 1);  // write value = 1 into pin 5 OR switch on the LED

    int button_state = gpio_read(BTN_PIN);  // read value from pin 3 OR the button
    printf("Button state: %d\n", button_state);

    gpio_write(LED_PIN, 0);  //write value = 0 into pin 5 OR switch off the LED

    printf("Firmware application finished\n");

    return 0;
}
```

---

## Simulation Output









