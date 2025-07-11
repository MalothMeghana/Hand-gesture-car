# Hand Gesture Controlled Car

## 🔧 Hardware Overview

### 🚗 Receiver Side (Car)
- **Arduino UNO** – Main controller  
- **L298N Motor Driver** – Controls motor directions  
- **nRF24L01 Module** – Receives wireless commands  
- **DC Motors** – Drives the car  

### ✋ Transmitter Side (Hand Gesture)
- **Arduino Nano** – Main controller  
- **MPU6050** – Detects hand tilt (gesture input)  
- **nRF24L01 Module** – Sends commands wirelessly  
- **Push Button** – Enables/disables transmission  
- **LED** – Indicates transmission status  

---

## 🔄 Working Logic Breakdown

### ✅ STEP 1: MPU6050 Reads Hand Movement
The MPU6050 sensor detects hand tilt using X (ax) and Y (ay) accelerometer values.

**Tilt Thresholds:**

| Direction | Condition      |
|-----------|----------------|
| Forward   | `ay < -4000`   |
| Backward  | `ay > 4000`    |
| Left      | `ax < -4000`   |
| Right     | `ax > 4000`    |
| Stop      | Other values   |

---

### ✅ STEP 2: Speed Control Logic
- Speed increases with more tilt.
- Speed index ranges from **1 to 5** based on tilt intensity.

---

### ✅ STEP 3: nRF24L01 Sends Data
- Data packet: `[Tx_command, Speed_index]`  
- Transmitted **only when button is pressed** (`Tx_Enable_Flag == 1`)

---

### ✅ STEP 4: Receiver Gets the Command
- Receiver reads:
  - `Rx_Array[0]` → Direction  
  - `Rx_Array[1]` → Speed Index  

---

### ✅ STEP 5: Car Acts Based on Command

| `Received_Command` | Action       |
|--------------------|--------------|
| 1                  | Forward      |
| 2                  | Backward     |
| 3                  | Turn Left    |
| 4                  | Turn Right   |
| 0                  | Stop         |

- Run/stop mode controlled using `Run_Stop_Mode`
- Speed delay handled by `Run_Stop_Counter`

---

## 📊 Gesture Mapping Table

| Hand Tilt        | Tx Command | Action      | Speed Based on Tilt |
|------------------|------------|-------------|----------------------|
| ay < -4000       | 1          | Forward     | Yes                  |
| ay > 4000        | 2          | Backward    | Yes                  |
| ax < -4000       | 3          | Turn Left   | Yes                  |
| ax > 4000        | 4          | Turn Right  | Yes                  |
| No Tilt          | 0          | Stop        | No                   |

---

## ⚙️ Important Libraries Used

- `Wire.h` – I2C communication for sensors  
- `MPU6050.h` – Motion sensing with MPU6050  
- `RF24.h` – Wireless communication with nRF24L01

## 🛠️ Tools & IDE Used

- **Arduino IDE** – Used to write, compile, and upload code to Arduino UNO and Nano boards  
- **Windows OS** – For development and testing  
- **Git & GitHub** – For version control and project documentation   
