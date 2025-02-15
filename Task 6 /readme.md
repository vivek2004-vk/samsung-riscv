# 2-Bit Comparator using VSDSquadron Mini

## Overview
This project implements a **2-bit comparator** using the **VSDSquadron Mini** board. It compares two 2-bit binary numbers (A and B) and determines whether:  
- **A > B**  
- **A = B**  
- **A < B**  

The result is displayed using **three LEDs**.

## Components Used
- **VSDSquadron Mini Board**
- **4 push buttons** (for inputs: A1, A0, B1, B0)
- **3 LEDs** (for output indicators: A > B, A = B, A < B)
- **1kΩ Resistors** (for LEDs)
- **Breadboard & Jumper Wires**

## Pin Configuration
| Function | Pin |
|----------|----|
| A1 (Input) | PD0 |
| A0 (Input) | PD1 |
| B1 (Input) | PD2 |
| B0 (Input) | PD3 |
| A > B (LED) | PC4 |
| A = B (LED) | PC5 |
| A < B (LED) | PC6 |

-----

## How It Works
1. The push buttons allow users to set 2-bit values for A and B.
2. The program reads the input and determines the comparison result.
3. The corresponding LED lights up for **A > B**, **A = B**, or **A < B**.

------
## Truth Table
| **A1** | **A0** | **B1** | **B0** | **A > B (PC4 LED )** | **A = B (PC5 LED )** | **A < B (PC6 LED ** |
|:------:|:------:|:------:|:------:|:----------------------:|:----------------------:|:----------------------:|
| 0 | 0 | 0 | 0 | 0 | 1 | 0 |
| 0 | 0 | 0 | 1 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 1 | 0 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 0 | 1 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| 1 | 0 | 0 | 1 | 1 | 0 | 0 |
| 1 | 0 | 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 | 1 | 0 |

------
##PROGRAM


