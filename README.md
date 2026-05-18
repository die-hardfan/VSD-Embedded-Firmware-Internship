# Embedded Firmware Guide

## Table of Contents
- [What is embedded firmware?](#what-is-embedded-firmware)
- [Difference between application code and firmware](#difference-between-application-code-and-firmware)
- [Role of firmware in microcontroller-based systems](#role-of-firmware-in-microcontroller-based-systems)
- [What is a firmware library / API?](#what-is-a-firmware-library--api)
- [Why functions like digitalWrite(), delay(), Serial.write() exist](#why-functions-like-digitalwrite-delay-serialwrite-exist)
- [How libraries hide register-level complexity](#how-libraries-hide-register-level-complexity)
- [Why reusable APIs are critical in industry](#why-reusable-apis-are-critical-in-industry)
- [Bare-metal firmware basics](#bare-metal-firmware-basics)
- [What “bare-metal” means](#what-bare-metal-means)
- [Why no RTOS is used in this program](#why-no-rtos-is-used-in-this-program)
- [How timing, polling, and interrupts are handled without an OS](#how-timing-polling-and-interrupts-are-handled-without-an-os)

---

## What is embedded firmware?
<!-- Describe embedded firmware at a high level: purpose, location (flash/ROM), tight coupling with hardware, boot responsibilities, lifecycle. -->

---

## Difference between application code and firmware
<!-- Contrast responsibilities, abstraction levels, portability, update frequency, and dependencies. Provide examples where boundaries blur. -->

---

## Role of firmware in microcontroller-based systems
<!-- Explain hardware init, drivers, board support, boot, power management, safety, diagnostics, and real-time control. -->

---

## What is a firmware library / API?
<!-- Define libraries, HAL, SDKs, middleware. Explain API surfaces, versioning, and how they enable modularity. -->

---

## Why functions like digitalWrite(), delay(), Serial.write() exist
<!-- Motivate simplicity, portability, teaching, rapid prototyping, and mapping to underlying peripherals/timers/UARTs. -->

---

## How libraries hide register-level complexity
<!-- Show conceptually how macros, inline functions, and drivers abstract registers, bitfields, and clock trees. Mention trade-offs. -->

---

## Why reusable APIs are critical in industry
<!-- Cover maintainability, time-to-market, testing, certification, team scaling, vendor independence, and long-term support. -->

---

## Bare-metal firmware basics
<!-- Outline startup code, linker script, ISR vectors, main loop patterns, memory maps, and peripheral configuration. -->

---

## What “bare-metal” means
<!-- Define no OS/RTOS, direct hardware control, cooperative scheduling, deterministic timing expectations. -->

---

## Why no RTOS is used in this program
<!-- Justify by simplicity, resource limits, latency needs, certification scope, or static scheduling sufficiency. -->

---

## How timing, polling, and interrupts are handled without an OS
<!-- Explain busy-wait vs hardware timers, periodic ISRs, event flags, debouncing, priority, latency, and critical sections. -->
