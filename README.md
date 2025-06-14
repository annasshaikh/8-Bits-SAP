# Project Report: Digital Logic Design - 8-Bit SAP Computer

**Group Name:** 8 BITS [1]

## Group Members [3]

*   **Lead:** Muhammad Annas Shaikh - 26919
*   **Co-Lead:** Noor Un Nisa Shaukat - 26971
*   Kisa Fatima - 27076
*   Mahnoor Adeel - 26913
*   Muhammad Ahsanuddin Ahmed - 27134
*   Muhammad Maaz Siddiqui - 27070
*   Muhammad Ibrahim Farid - 27098
*   Muhammad Musab Sohail - 26923

## WOKWI Link [3]

[8 Bit SAP Computer (With Output) - Wokwi ESP32, STM32, Arduino Simulator]([https://wokwi.com/projects/your_project_link_here](https://wokwi.com/projects/385723689877905409))


## Table of Contents

*   [Our Understanding of the SAP-1](#our-understanding-of-the-sap-1)
*   [Components](#components)
*   [Instruction Format](#instruction-format)
*   [Instruction Set](#instruction-set)
*   [The SAP Cycle (Fetch & Execute)](#the-sap-cycle-fetch--execute)
*   [Inclusions](#inclusions)
*   [Exclusions](#exclusions)
*   [Sample Codes to Try on Wokwi](#sample-codes-to-try-on-wokwi)

## Our Understanding of the SAP-1 [4]

The SAP-1 (Simple As Possible) architecture is an educational 8-bit computer model. SAP-1 comprises essential components such as an Arithmetic Logic Unit (ALU) for basic calculations, a set of registers for temporary data storage, a Control Unit to manage instruction execution, and a Memory Unit for storing data and instructions. The current instruction is stored in the Instruction Register (IR), and the control unit decodes the instructions and then it is executed.

## Components [5]

1.  **Program Counter (PC)**
    *   **Size:** 4 bits
    *   **Function:** Keeps track of the memory address of the next program instruction to be fetched. It is incremented after an instruction is fetched. [6]
2.  **Instruction Register (IR)**
    *   **Size:** 8 bits
    *   **Function:** Stores the value of the program instruction that is being executed currently. The first 4 bits are the opcode and the other 4 bits are the memory address. [7]
3.  **Accumulator (A)**
    *   **Size:** 8 bits
    *   **Function:** Holds the results of Arithmetic and Logic operations. Acts as a temporary workspace. [8]
4.  **Arithmetic and Logic Unit (ALU)**
    *   **Size:** 8 bits
    *   **Function:** Performs Arithmetic (add, subtract) and Logical (bitwise AND, bitwise XOR) operations. [9]
5.  **Control Unit (CU)**
    *   **Function:** Directs operations of different components to execute instructions. Decodes instructions, controls data flow, and generates control signals. It's majorly a 4-bit decoder, decoding the opcode to 15 control wires. Clock synchronization is crucial. [10]
6.  **Clock**
    *   **Function:** Synchronizes the operations of the computer components. Coordinates the fetch and execution phases using regular on/off pulses. Performs FETCH/EXECUTE and FETCH WRITE/EXECUTE WRITE operations. Implemented using a Python loop in the Pi Pico. [11, 12]
7.  **Output Component**
    *   **Function:** Represents an output device (LEDs or seven-segment display) where the 8-bit result of the computation from the accumulator is displayed. [12, 13]
8.  **Controller:** Raspberry Pi Pico [5]

## Instruction Format [15]

The instruction set is divided into a 4-bit opcode and a 4-bit data field.

*   **Opcode (4 bits):** Specifies the operation to be performed.
*   **Address (4 bits):** Contains memory address or additional information, depending on the instruction.

**Example:**
Instruction: `00111001`
*   **Opcode:** `0011` (e.g., XOR)
*   **Address:** `1001`

This would be decoded as, "Perform XOR operation on the contents of the data register (at memory address `1001`) and the accumulator and store the result in the accumulator.”

## Instruction Set [14]

| INSTRUCTION | OPCODE | FUNCTION                                                                  |
| :---------- | :----- | :------------------------------------------------------------------------ |
| HALT        | 000    | Stops the execution of Program                                            |
| ADD         | 001    | Adds contents of memory location to Accumulator                           |
| SUB         | 010    | Subtracts contents of memory location from Accumulator                    |
| XOR         | 011    | Performs XOR operation on the contents of memory and the accumulator.     |
| AND         | 100    | Performs AND operation on the contents of memory and the accumulator.     |
| LOAD        | 101    | Loads the contents of the memory into the accumulator.                    |
| STORE       | 110    | Stores the contents of the accumulator in a memory location.              |
| JUMP        | 111    | Load The Address to PC                                                    |

**For Output:** The 4th most significant bit of an instruction (if used for this purpose) can be turned `0` for Output screen to be on, but `1` for closing output screen. [14]

## The SAP Cycle (Fetch & Execute) [16]

### FETCH Cycle
The FETCH cycle is the first stage of instruction execution.
1.  The address multiplexer (ADDR MUX) selects the program counter (PC) as the source of the address for the memory.
2.  The memory outputs the instruction stored at the address given by the PC to the output bus.
3.  The IR write control line is activated, causing the instruction on the output bus to be written into the IR.
4.  The PC increment control line is activated, causing the PC to increment by one and point to the next instruction.

*(Refer to diagrams on Page 16 for control signals active during Fetch)*

### Execute Cycle
The Execute Cycle is the stage where the decision occurs based on the instruction in IR. The Opcode in the IR is decoded using the control unit and the corresponding control lines are active.

*(Refer to table on Page 17 for control signals active during Execute for different instructions)*

## Inclusions [17]

*   ALU can perform two arithmetic operations: addition and subtraction.
*   ALU can perform two logical operations: bitwise XOR and AND.
*   Instruction set allows direct addressing.
*   Seven-segment display will be used to display the result of ALU operations.

## Exclusions [17]

*   Overflows and Underflows are not catered if the output produced by ALU requires more than 8 bits.
*   The subtraction operation doesn't generate a borrow bit along with output.
*   It doesn't include operations for floating-point numbers.
*   The computer cannot handle any interrupts generated by the user while the program is running.
*   It doesn't incorporate conditional instructions.
*   There are no instructions for indirect or immediate addressing.
*   The control unit cannot process multiple instructions simultaneously.
*   Memory is not implemented through physical circuits but represented through a microcontroller (Raspberry Pi Pico).

## Sample Codes to Try on Wokwi [18, 19]

*(Note: These codes are represented as arrays of 8-bit values, where the first 4 bits are the opcode and the last 4 bits are the address/data. Comments explain the intended operation.)*

```python
#Print 2^n  f = 1
mem = [
    [0,1,0,1 ,0,1,1,0], # LOAD MEM[0110]        #0
    [0,0,0,1 ,0,1,1,1], # ADD R = R + MEM[0111] #1
    [0,1,1,0 ,0,1,1,1], # Store AT MEM[0111]    #2
    [1,0,0,1 ,0,1,1,1], # ADD R = R + MEM[0111] #3
    [0,1,1,1 ,0,0,1,0], # Jump to MEM[0010]     #4
    [0,0,0,0 ,0,0,0,0],                         #5
    [0,0,0,0, 0,0,0,1], ##[DATA]                #6
    [0,0,0,0, 0,0,0,1], ##[DATA]                #7
]


#print 4^n      f = 2


mem = [
    [0,1,0,1 ,1,1,1,0], # LOAD MEM[1110]        #0
    [0,0,0,1 ,1,1,1,1], # ADD R = R + MEM[1111] #1
    [0,1,1,0 ,1,1,1,1], # Store AT MEM[1111]    #2
    [1,0,0,1 ,1,1,1,1], # ADD R = R + MEM[1111] #3
    [0,1,1,0 ,1,1,1,1], # Store AT MEM[0111]    #4
    [1,0,0,1 ,1,1,1,1], # ADD R = R + MEM[1111] #5
    [0,1,1,1 ,0,0,1,0], # Jump to MEM[0010]     #6
    [0,0,0,0 ,0,0,0,0],                         #7
    [0,0,0,0 ,0,0,0,0],                         #8
    [0,0,0,0 ,0,0,0,0],                         #9
    [0,0,0,0 ,0,0,0,0],                         #10
    [0,0,0,0 ,0,0,0,0],                         #11
    [0,0,0,0 ,0,0,0,0],                         #12
    [0,0,0,0 ,0,0,0,0],                         #13
    [0,0,0,0 ,0,0,0,0],                         #14
    [0,0,0,0 ,0,0,0,1],                         #15
]


# Check if its Odd or Even (Print 0 for Even and 1 for ODD)
mem = [
    [1,1,0,1 ,1,1,1,1], # LOAD MEM[1111]        #0
    [1,1,0,0 ,1,1,1,0], # AND R = R & MEM[1110] #1
    [0,0,0,0 ,0,0,0,0], # HALT WITH OUTPUT      #2
    [0,0,0,0 ,0,0,0,0],                         #3
    [0,0,0,0 ,0,0,0,0],                         #4
    [0,0,0,0 ,0,0,0,0],                         #5
    [0,0,0,0 ,0,0,0,0],                         #6
    [0,0,0,0 ,0,0,0,0],                         #7
    [0,0,0,0 ,0,0,0,0],                         #8
    [0,0,0,0 ,0,0,0,0],                         #9
    [0,0,0,0 ,0,0,0,0],                         #10
    [0,0,0,0 ,0,0,0,0],                         #11
    [0,0,0,0 ,0,0,0,0],                         #12
    [0,0,0,0 ,0,0,0,0],                         #13
    [0,0,0,0 ,0,0,0,1],                         #14
    [0,0,0,0 ,0,1,1,1],# DATA                   #15
]


# fabinoci Sequuence


mem = [
    [1, 1, 0, 1, 1, 0, 0, 0], #LOAD 8
    [1, 0, 0, 1, 0, 1, 1, 1], #ADD 7
    [1, 1, 1, 0, 1, 0, 0, 0], #STORE 8
    [1, 0, 1, 0, 0, 1, 1, 1], #SUB 7
    [0, 1, 1, 0, 0, 1, 1, 1], #STORE 7
    [0, 1, 1, 1, 0, 0, 0, 0], #JUMP TO
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 1],
    [0, 0, 0, 0, 0, 0, 0, 1]
]
```
