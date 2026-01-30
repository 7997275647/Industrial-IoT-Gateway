# Secure Industrial Edge-IoT Gateway
### Platform: STM32F767ZI (Nucleo-144)

## 1. Project Overview
This project transforms a **Nucleo-F767ZI** into an industry-grade **Edge-IoT Gateway**. It is designed to bridge the gap between "Southbound" industrial data (simulated via internal SoC peripherals) and "Northbound" cloud analytics via **Secure MQTT over Ethernet**. 

By leveraging the high-performance **ARM Cortex-M7** core (216 MHz), the gateway performs real-time data aggregation, JSON serialization, and secure encryption (TLS 1.2) entirely within an open-source toolchain.

---

## 2. Project Architecture



The system architecture follows a three-layer industrial model:

### A. Data Acquisition Layer (Internal Asset)
* **Internal Temperature Sensor:** Samples the die temperature to monitor SoC health in industrial environments.
* **Internal Voltage Reference ($V_{REFINT}$):** Monitors power supply stability to ensure system dependability.
* **Virtual Asset Simulation:** Uses **TIMers** in PWM/Input Capture mode to simulate motor signals or RPM telemetry without requiring external hardware.

### B. Edge Processing Layer (Middleware)
* **FreeRTOS (CMSIS-v2):** Manages concurrent tasks for high-priority sensing, data processing, and network management.
* **cJSON:** Formats raw sensor data into standardized JSON objects for enterprise cloud compatibility.
* **IWDG (Independent Watchdog):** Ensures 24/7 system recovery in case of network stack or software hangs.

### C. Communication Layer (Connectivity)
* **LwIP (Lightweight IP):** Handles the TCP/IP stack via the onboard 10/100 Ethernet MAC/PHY.
* **MbedTLS:** Provides a hardware-accelerated SSL/TLS 1.2 layer for end-to-end encrypted communication.
* **MQTT:** A robust client-subscriber model for pushing telemetry to brokers like AWS IoT, HiveMQ, or Mosquitto.

---

## 3. Technical Stack
| Component | Technology 
| :--- | :--- | :--- |
| **Hardware** | STM32F767ZI (Cortex-M7) | High-performance industrial MCU with Ethernet & FPU. |
| **OS** | FreeRTOS | Global standard for deeply embedded multitasking. |
| **Network Stack** | LwIP | Most widely used open-source TCP/IP stack for MCUs. |
| **Security** | MbedTLS (TLS 1.2) | Mandatory for secure industrial data transmission. |
| **Data Format** | JSON via cJSON | Standard for modern IoT and Web-based analytics. |
| **IDE** | STM32CubeIDE (GCC) | Professional-grade Eclipse-based development environment. |

---

## 4. Key Features
* **Zero-External-Hardware Requirement:** Utilizes internal STM32 peripherals to simulate a complete industrial environment.
* **Edge Intelligence:** Implements "Deadbanding" logic only transmitting data to the cloud when values change beyond a specific threshold to optimize bandwidth.
* **Production-Grade Security:** Full TLS handshake implementation with root CA certificate validation.
* **Resilient Connectivity:** Automated state-machine for Ethernet link-down detection and MQTT reconnection.
* **MISRA C Alignment:** Modular restructuring to ensure adherence to strict software engineering standards.

---

## 5. Getting Started

### Hardware Requirements
* 1x **Nucleo-F767ZI** Board.
* 1x **Ethernet Cable** (RJ45) connected to a router with internet access.
* 1x **Micro-USB Cable** for power and ST-LINK debugging.

### Software Setup
1. **Import to STM32CubeIDE:** Open the project and ensure the `LwIP`, `FreeRTOS`, and `MbedTLS` middlewares are enabled in the `.ioc` file.
2. **Configure Network:**
   - **For DHCP:** Ensure `LWIP_DHCP` is set to 1 in LwIP configuration.
   - **For Static IP:** Update `IP_ADDR0` to `IP_ADDR3` in `lwip.c` to match your local subnet.
3. **MQTT Broker:** Update the `MQTT_BROKER_ADDR` in your configuration header to point to your target broker.
4. **Build & Flash:** Connect the board and click **Run**.

---

## 6. Implementation Roadmap
- [ ] **Phase 1:** Initialize ADC/TIM for internal health monitoring and verify via UART.
- [ ] **Phase 2:** Configure LwIP and verify network connectivity via Ping.
- [ ] **Phase 3:** Implement MQTT client with JSON payload formatting.
- [ ] **Phase 4:** Enable MbedTLS for secure cloud communication and integrate Watchdog protection.

---

## 7. About the Author
**Sandeep Reddy Modugu**