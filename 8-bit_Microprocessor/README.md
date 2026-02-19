# Custom 8-Bit Microprocessor in Verilog

## Overview
This repository contains the RTL design of a custom 8-bit microprocessor implemented in Verilog HDL. The processor utilizes a Von Neumann architecture with a unified 32-byte memory space for both instructions and data. It features an accumulator-based datapath and a custom-defined Instruction Set Architecture (ISA).



## Architecture Highlights
* **Data Width:** 8-bit
* **Memory:** 32-byte integrated RAM (5-bit address space)
* **Registers:** * `acc`: 8-bit Accumulator
    * `pc`: Program Counter
    * `instr`: Instruction Register
* **Execution:** Unified fetch-decode-execute cycle.

## Instruction Set Architecture (ISA)
The instruction format is 8 bits wide, divided into a 3-bit Opcode and a 5-bit Operand/Address.

| Bit Range | [7:5]  | [4:0]           |
| :---      | :---   | :---            |
| **Field** | Opcode | Operand/Address |

### Supported Instructions

| Mnemonic | Opcode | Description | Operation |
| :--- | :---: | :--- | :--- |
| **LOAD** | `000` | Load to Accumulator | `ACC <- RAM[Address]` |
| **STORE** | `001` | Store from Accumulator | `RAM[Address] <- ACC` |
| **ADD** | `010` | Add to Accumulator | `ACC <- ACC + RAM[Address]` |
| **SUB** | `011` | Subtract from Accumulator | `ACC <- ACC - RAM[Address]` |
| **JMP** | `100` | Unconditional Jump | `PC <- Address` |
| **ANDOP** | `101` | Bitwise AND | `ACC <- ACC & RAM[Address]` |
| **OROP** | `110` | Bitwise OR | `ACC <- ACC \| RAM[Address]` |

## Example Program
The module initializes with a test program pre-loaded into memory. The program performs a series of arithmetic and logical operations, storing intermediate results back into RAM before entering an infinite halt loop.

```assembly
// Assembly equivalent of the pre-loaded memory block
LOAD  10  // ACC = RAM[10] (5)
ADD   11  // ACC = 5 + RAM[11] (3) = 8
STORE 12  // RAM[12] = 8
LOAD  20  // ACC = RAM[20] (12)
ANDOP 21  // ACC = 12 & RAM[21] (10) = 8
OROP  22  // ACC = 8 | RAM[22] (6) = 14
SUB   11  // ACC = 14 - RAM[11] (3) = 11
STORE 13  // RAM[13] = 11
JMP   8   // Jump to self (Halt)
