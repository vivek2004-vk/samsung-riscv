# 2-Bit Comparator using VSDSquadron Mini

## Overview
This project involves implementing a **2-bit comparator circuit** using the VSDSquadron Mini RISC-V development board. A 2-bit comparator compares two binary numbers (`A[1:0]` and `B[1:0]`) and outputs:
1. **A > B (Green LED)**: Indicates that `A` is greater than `B`.
2. **A == B (Yellow LED)**: Indicates that `A` is equal to `B`.
3. **A < B (Red LED)**: Indicates that `A` is less than `B`.

This project showcases the practical application of digital logic and RISC-V GPIO programming using the PlatformIO IDE.

---

## Components Required
- **VSDSquadron Mini Board**.
- **4 Push Buttons** (2 for `A[1:0]` and 2 for `B[1:0]`).
- **3 LEDs** (Green, Yellow, Red).
- **Breadboard**.
- **Jumper Wires**.
- **VS Code** and **PlatformIO IDE**.

---

## Logical Diagram and Expressions
The comparator logic uses Boolean algebra to determine the three outputs:

1. **A > B**:  
   \( G = A_1B_1' + (A_1 \cdot B_1)(A_0B_0') \)

2. **A == B**:  
   \( E = (A_1 \oplus B_1)' \cdot (A_0 \oplus B_0)' \)

3. **A < B**:  
   \( S = A_1'B_1 + (A_1 \cdot B_1)(A_0'B_0) \)

---

## Hardware Connections

| **Signal**     | **GPIO Pin** | **Connection**       |
|-----------------|--------------|----------------------|
| **A[1]**       | PD1          | Push Button Input    |
| **A[0]**       | PD2          | Push Button Input    |
| **B[1]**       | PD3          | Push Button Input    |
| **B[0]**       | PD4          | Push Button Input    |
| **A > B (G)**  | PC4          | Green LED Output     |
| **A == B (E)** | PC5          | Yellow LED Output    |
| **A < B (S)**  | PC6          | Red LED Output       |
| **VCC**        | 3.3V         | Power Supply         |
| **GND**        | GND          | Ground               |

---
![Alt text](snapshots/vsd1.png)

---

## 2-Bit Comparator Truth Table

| **A1** | **A0** | **B1** | **B0** | **A > B** | **A == B** | **A < B** |
|--------|--------|--------|--------|----------|------------|----------|
| 0      | 0      | 0      | 0      | 0        | 1          | 0        |
| 0      | 0      | 0      | 1      | 0        | 0          | 1        |
| 0      | 0      | 1      | 0      | 0        | 0          | 1        |
| 0      | 0      | 1      | 1      | 0        | 0          | 1        |
| 0      | 1      | 0      | 0      | 1        | 0          | 0        |
| 0      | 1      | 0      | 1      | 0        | 1          | 0        |
| 0      | 1      | 1      | 0      | 0        | 0          | 1        |
| 0      | 1      | 1      | 1      | 0        | 0          | 1        |
| 1      | 0      | 0      | 0      | 1        | 0          | 0        |
| 1      | 0      | 0      | 1      | 1        | 0          | 0        |
| 1      | 0      | 1      | 0      | 0        | 1          | 0        |
| 1      | 0      | 1      | 1      | 0        | 0          | 1        |
| 1      | 1      | 0      | 0      | 1        | 0          | 0        |
| 1      | 1      | 0      | 1      | 1        | 0          | 0        |
| 1      | 1      | 1      | 0      | 1        | 0          | 0        |
| 1      | 1      | 1      | 1      | 0        | 1          | 0        |


---

## HOW TO PROGRAM

```c
#include <ch32v00x.h>
#include <debug.h>

// Define input pins for push buttons (PD1, PD2 for A; PD3, PD4 for B)
#define A0_PIN GPIO_Pin_1
#define A1_PIN GPIO_Pin_2
#define B0_PIN GPIO_Pin_3
#define B1_PIN GPIO_Pin_4

// Define output pins for LEDs (PC3 for A < B, PC4 for A == B, PC5 for A > B)
#define LT_PIN GPIO_Pin_3  // Red LED (A < B)
#define EQ_PIN GPIO_Pin_4  // Green LED (A == B)
#define GT_PIN GPIO_Pin_5  // Yellow LED (A > B)

void GPIO_Config(void) {
    GPIO_InitTypeDef GPIO_InitStructure;

    // Enable GPIO clocks for port D and port C
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);

    // Configure push button input pins (PD1, PD2, PD3, PD4)
    GPIO_InitStructure.GPIO_Pin = A0_PIN | A1_PIN | B0_PIN | B1_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;  // Internal pull-up
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // Configure LED output pins (PC3, PC4, PC5)
    GPIO_InitStructure.GPIO_Pin = LT_PIN | EQ_PIN | GT_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
}

void CompareNumbers(void) {
    uint8_t A = ((GPIO_ReadInputDataBit(GPIOD, A1_PIN) << 1) | GPIO_ReadInputDataBit(GPIOD, A0_PIN));
    uint8_t B = ((GPIO_ReadInputDataBit(GPIOD, B1_PIN) << 1) | GPIO_ReadInputDataBit(GPIOD, B0_PIN));

    // Reset all LEDs
    GPIO_ResetBits(GPIOC, LT_PIN | EQ_PIN | GT_PIN);

    // Comparison logic
    if (A > B) {
        GPIO_SetBits(GPIOC, GT_PIN);  // Yellow LED (A > B)
    } else if (A == B) {
        GPIO_SetBits(GPIOC, EQ_PIN);  // Green LED (A == B)
    } else {
        GPIO_SetBits(GPIOC, LT_PIN);  // Red LED (A < B)
    }
}

int main(void) {
    // Initialize the system and GPIO
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();

    while (1) {
        CompareNumbers();
        Delay_Ms(100);  // Debouncing delay
    }
}

