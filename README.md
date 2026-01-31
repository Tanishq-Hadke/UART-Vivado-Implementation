# UART Protocol Implementation using Vivado (Verilog)

This repository contains a complete **UART (Universal Asynchronous Receiver Transmitter)** implementation designed using **Verilog HDL** and developed in **Xilinx Vivado**.  
The project includes baud rate generation, UART transmitter, and UART receiver modules suitable for FPGA-based communication.

---

## 📌 Features

- Configurable system clock frequency
- Configurable UART baud rate
- UART Transmitter (TX)
- UART Receiver (RX)
- Standard UART frame format:  
  **1 Start bit, 8 Data bits, No parity, 1 Stop bit (8N1)**
- Fully synthesizable design
- Modular and reusable Verilog code
- FPGA-ready implementation

---

---

## ⏱️ System Clock

The UART design uses the FPGA system clock.

Typical clock frequencies:
- 100 MHz (Basys 3 / Nexys A7)
- 50 MHz (Other FPGA boards)

The system clock is internally divided to generate the UART baud clock.

---

## 🔢 UART Baud Rate

UART communication is asynchronous, meaning no shared clock exists between the transmitter and receiver.  
Both devices must be configured to operate at the same baud rate.

Common baud rates:
- 9600
- 19200
- 38400
- 57600
- 115200

---

## 🧮 Baud Rate Calculation


This divider value is used in the baud rate generator module.

---

## ⏲️ Baud Rate Generator

### File: baud_rate_generator.v

**Function:**  
Generates a baud tick by dividing the system clock.

**Inputs:**
- clk – System clock
- rst – Reset

**Output:**
- baud_tick – Single-cycle pulse at baud frequency

**Operation:**
- Counter increments on each clock
- When counter reaches divider value, baud_tick is asserted
- Counter resets and repeats

---

## 📤 UART Transmitter (TX)

### File: uart_tx.v

**Function:**  
Converts 8-bit parallel data into serial UART data.

### UART Frame Format (8N1)

| Field      | Bits |
|-----------|------|
| Start Bit | 1    |
| Data Bits | 8    |
| Parity    | 0    |
| Stop Bit | 1    |

**Transmission Order:**
- Start bit (LOW)
- Data bits (LSB first)
- Stop bit (HIGH)

**Inputs:**
- clk
- rst
- baud_tick
- tx_start
- tx_data[7:0]

**Outputs:**
- tx
- tx_busy

**TX State Machine:**
1. IDLE
2. START
3. DATA
4. STOP
5. DONE

---

## 📥 UART Receiver (RX)

### File: uart_rx.v

**Function:**  
Receives serial UART data and reconstructs 8-bit parallel data.

**RX Operation:**
1. Detect start bit
2. Sample at baud intervals
3. Receive 8 data bits
4. Verify stop bit
5. Assert data valid

**Inputs:**
- clk
- rst
- baud_tick
- rx

**Outputs:**
- rx_data[7:0]
- rx_done

**RX State Machine:**
1. IDLE
2. START
3. DATA
4. STOP
5. DONE

---


---

## 🧩 Top-Level Module

### File: uart_top.v

**Function:**  
Integrates the baud rate generator, UART transmitter, and UART receiver into a single module for FPGA deployment.

---

## 🧪 Simulation

### File: uart_tb.v

**Simulation Checks:**
- Baud timing accuracy
- TX data correctness
- RX data correctness
- Reset behavior

**Supported Simulators:**
- Vivado Simulator
- ModelSim
- QuestaSim

---

Pins must be adjusted based on the FPGA board.

---

## 💻 How to Use

1. Open Vivado
2. Create a new RTL project
3. Add Verilog files from `src/`
4. Add constraints from `constraints/`
5. Set `uart_top.v` as top module
6. Run synthesis and implementation
7. Program the FPGA
8. Use a serial terminal (PuTTY / Tera Term)

---

## 🚀 Applications

- FPGA to PC serial communication
- Debug message transmission
- Embedded system interfaces
- Sensor data communication
- Microcontroller to FPGA communication

---

## ⚠️ Limitations

- No parity bit
- Fixed 8-bit data length
- Single stop bit
- No FIFO buffering

---

## 🔮 Future Enhancements

- Parity bit support
- Configurable data length
- FIFO buffers
- Interrupt-based UART
- AXI-stream interface

---

## 🛠️ Tools Used

- Verilog HDL
- Xilinx Vivado
- FPGA development board
- UART terminal software

---
