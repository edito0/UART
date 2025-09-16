# UART Communication System (Verilog)

## 📌 Overview
This project implements a **Universal Asynchronous Receiver Transmitter (UART)** system in Verilog.  
The design supports **serial data communication** with the following key features:
- Baud rate generation
- UART receiver
- UART transmitter
- FIFO buffering for TX and RX paths
- Button-controlled read/write operations
- Debug outputs on LEDs and 7-segment displays

The project is organized into modular RTL files to ensure reusability and clarity.

---

## 📂 Data Tranfer on the UART
![Data Tranfer on the UART](imgs/Picture2.png)


1. Wait until incoming signal becomes 0 (start bit), then start the sampling tick counter

2. When tick counter reaches 7 (middle of start bit), clear tick counter and restart

3. When counter reaches 15 (middle of first data bit), shift bit value into register & restart tick counter

4. Repeat step 3 (N – 1) more times to retrieve the remaining data bits

5. Repeat step 3 (M) more times to obtain stop bits

---

### Architecture

## 1. UART Tx FSM
![UART Tx FSM](imgs/Picture6.png)

## 2. UART Rx FSM
![UART Rx FSM](imgs/Picture5.png)

## 3. Complete Architecture
![UART Architecrure](imgs/Picture7.png)

---

## 📐 System Architecture
The design consists of the following components:

1. **Baud Rate Generator (Timer)**  
   - Generates sampling ticks (`s_tick`) for UART operation.  
   - Configured for **9600 baud** with `FINAL_VALUE = 650`.

2. **UART Receiver**  
   - Receives serial data (`rx`).  
   - Generates `rx_done_tick` when a byte is received.  
   - Stores received data in **RX FIFO**.

3. **UART Transmitter**  
   - Loads data from **TX FIFO**.  
   - Transmits serial data (`tx`).  
   - Signals transmission completion with `tx_done_tick`.

4. **FIFO Buffers**  
   - RX FIFO stores incoming data until read.  
   - TX FIFO buffers outgoing data before transmission.  
   - Prevents data loss during continuous transfers.

5. **Control & User Interface**  
   - **Push buttons**: trigger read/write operations.  
   - **LEDs**: indicate FIFO empty/full status.  
   - **Switches + 7-segment display**: show transmitted/received data.  

---

## 🚀 How It Works
1. The **receiver** collects serial data from `rx` and pushes it into the **RX FIFO**.  
2. The **user** can press a button to read data, which will be displayed on **7-segment displays**.  
3. To transmit, the user loads data from switches into the **TX FIFO** using a button.  
4. The **transmitter** pulls data from TX FIFO and sends it serially via `tx`.  
5. LEDs provide FIFO status feedback.

---

## ⚙️ Parameters
- **Baud Rate**: 9600  
- **Data Width**: 8-bit  
- **FIFO Depth**: Determined by Xilinx IP configuration  

---

## 🛠️ Simulation & Testing
1. Simulate each RTL module independently (UART RX, UART TX, FIFO).  
2. Run integration test with `terminal_demo.v` as top-level.  
3. Verify using a serial terminal (e.g., Putty, TeraTerm) via FPGA board UART port.  

---
