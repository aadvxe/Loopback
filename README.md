# Loopback (STM32F767ZI Multi-Tasking FreeRTOS & CAN/UART Controller)

An embedded firmware project for the **NUCLEO-F767ZI** featuring multi-threaded **FreeRTOS** execution, **USART3 DMA** command parsing, and **CAN1** bus integration to orchestrate multi-channel onboard LED control.

---

## 🎯 Project Overview

This project was developed for testing embedded multi-tasking patterns and communication interfaces (including collaboration/research experiments associated with BRIN - National Research and Innovation Agency of Indonesia). It controls three independent LEDs on the NUCLEO board via distinct FreeRTOS threads dispatched by UART or CAN bus commands.

- **MCU:** STM32F767ZIT6 (ARM Cortex-M7 @ 216 MHz)
- **Board:** NUCLEO-F767ZI
- **RTOS:** FreeRTOS
- **Serial Interface:** USART3 with DMA RX/TX (`115200` baud)
- **CAN Interface:** CAN1 (bxCAN)
- **Actuators / Indicators:** Onboard User LEDs:
  - `PB0` (Green LED / `LD1`)
  - `PB7` (Blue LED / `LD2`)
  - `PB14` (Red LED / `LD3`)

---

## 🧵 RTOS Tasks & State Machine

The firmware spawns dedicated FreeRTOS threads governed by the global `taskControl` state:

| Task Name | State Trigger (`taskControl`) | Hardware Action |
| :--- | :---: | :--- |
| **`tasklampone`** | `1` | Resets PB7 & PB14, periodically toggles **Green LED** (`PB0`) every 1000 ms. |
| **`tasklamputwo`** | `2` | Resets PB0 & PB14, periodically toggles **Blue LED** (`PB7`) every 1000 ms. |
| **`tasklamputhree`** | `3` | Resets PB0 & PB7, periodically toggles **Red LED** (`PB14`) every 1000 ms. |
| **`StartAll`** | `4` | Concurrently toggles **all three LEDs** (`PB0`, `PB7`, `PB14`) every 1000 ms. |
| **`StopAll`** | `5` | Turns **OFF all three LEDs**. |

---

## 📁 Folder Structure

```
Loopback/
├── main.c                      # Standalone source reference implementation with FreeRTOS threads
└── Loopback/                   # Complete STM32CubeIDE project directory
    ├── Core/
    │   ├── Inc/                # FreeRTOSConfig.h, main.h
    │   └── Src/                # main.c, freertos.c, stm32f7xx_it.c
    ├── Drivers/                # STM32F7xx HAL & CMSIS
    ├── Middlewares/            # FreeRTOS source
    ├── Loopback.ioc            # STM32CubeMX configuration
    └── STM32F767ZITX_FLASH.ld  # Linker script
```

---

## 🛠️ Building & Running

1. Open **STM32CubeIDE**.
2. Select **File > Open Projects from File System...** and open `Loopback/Loopback`.
3. Connect your **NUCLEO-F767ZI** via USB.
4. Build the project (`Ctrl+B`) and flash (`Ctrl+F11`).
5. Send control commands via serial terminal (115200 baud) or CAN frames to switch active LED task states.
