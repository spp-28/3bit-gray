<!---
This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This design implements a **3-bit Gray code counter** using three D flip-flops with synchronous next-state logic.  
The Gray counter is a sequence generator where only one bit changes at a time between successive states.  

The flip-flops are mapped as follows in the schematic:  
- Bottom FF → Q0 (LSB)  
- Middle FF → Q1  
- Top FF → Q2 (MSB)  

The next-state equations are:

- D0 = (NOTQ2 NOTQ1) + (Q2 Q1)  
- D1 = (NOTQ2 Q0) + (Q1 NOTQ0)
- D2 = (Q2 Q0) + (Q1 NOTQ0) 

On each rising edge of the clock, the outputs update according to these equations, producing the 3-bit Gray sequence:

000 → 001 → 011 → 010 → 110 → 111 → 101 → 100 → (wraps back to 000).

A reset input initializes the counter to `000`. An enable input allows the counter to hold its state when disabled.

---

## How to test

The circuit can be tested in **Wokwi** simulation or on **TinyTapeout silicon**.  

Steps:

1. Connect LEDs to `uo_out[7:0]`:  
   - `uo_out[0]` → Q2 (MSB).  
   - `uo_out[1]` → Q1.  
   - `uo_out[2]` → Q0 (LSB).  
   - Remaining outputs unused or debug.  

3. To run:  
   - Set Reset=1 → all outputs = 000.  
   - Release Reset, set Enable=1.  
   - Toggle the clock (ui_in[0]) → outputs advance in Gray sequence.  
   - If Enable=0, outputs hold current value.  

---

## Testcase Truth Table

Inputs → Outputs

| Q2 Q1 Q0 (Present State) | D2 D1 D0 (Next Inputs) | Q2 Q1 Q0 (Next State) |
|---------------------------|------------------------|------------------------|
| 000                       | 0 0 1                 | 001                    |
| 001                       | 0 1 1                 | 011                    |
| 011                       | 0 1 0                 | 010                    |
| 010                       | 1 1 0                 | 110                    |
| 110                       | 1 1 1                 | 111                    |
| 111                       | 1 0 1                 | 101                    |
| 101                       | 1 0 0                 | 100                    |
| 100                       | 0 0 0                 | 000                    |

---

## External hardware

For demonstration on breadboard or FPGA dev board:

- **Clock source**: Push-button or oscillator module.  
- **Reset button**: Active-high reset line.  
- **Enable switch**: Controls whether the counter advances.  
- **3 LEDs** (with resistors) → show Q2, Q1, Q0 Gray output.  
- **Optional extra LED** → Done/Enable flag.  

In simulation (Wokwi), these are represented as virtual DIP switches (inputs) and LED indicators (outputs).

---
