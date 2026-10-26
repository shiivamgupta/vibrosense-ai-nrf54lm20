# VibroSense AI: Acoustic & Vibration Motor Fault Predictor with BLE Mesh & Hands-Free Alerts

**Hackster.io "Sustain the System" Contest with Nordic Semiconductor**

## Overview
**VibroSense AI** is an ultra-low-power, retrofittable Edge AI predictive maintenance node built on the **Nordic nRF54LM20 DK**. It combines 3-axis vibration analysis and ultrasonic acoustic sensing with on-device TinyML inference (leveraging the nRF54LM20 Cortex-M33 & Axon NPU / Neuton AI) to detect bearing wear, shaft misalignment, and pump cavitation before equipment failure or energy waste occurs.

Health indices and actionable maintenance recommendations are broadcast via **Bluetooth LE (BLE Mesh / Periodic Advertising)** to an Android companion app (with hands-free technician audio alerts) and a local Raspberry Pi 4 monitoring gateway, complemented by **NFC tap-to-inspect** diagnostic logging.

---

## Initial Bill of Materials (BOM)

| Component | Part / Model | Interface to nRF54LM20 DK | Purpose |
| :--- | :--- | :--- | :--- |
| **Primary Dev Kit** | **Nordic Semiconductor nRF54LM20 DK** | Core SoC / Onboard PMIC / BLE / NFC | Edge AI inference (Axon NPU / Cortex-M33), BLE Mesh/Advertising, 8MB QSPI fault logging, NFC OOB diagnostics |
| **Vibration Sensor** | ST LSM6DSO or InvenSense MPU6050 6-Axis IMU Breakout | I2C / SPI (EasyDMA) + GPIO Wake Interrupt | High-frequency 3-axis vibration sampling for FFT bearing & misalignment analysis |
| **Acoustic Sensor** | InvenSense INMP441 Digital MEMS Microphone | I2S / PDM (EasyDMA) | Ultrasonic & acoustic signature capture for early bearing pitting and pump cavitation |
| **Temperature Probe** | 10k NTC Thermistor Probe | 14-bit SAADC Analog Input | Motor/pump housing thermal drift monitoring |
| **Mobile Client** | Android Smartphone (+ optional BLE Smartglasses/Audio) | Bluetooth LE 5.4 / NFC-A | Field technician dashboard, NFC tap readout, and hands-free voice maintenance alerts |
| **Edge Gateway (Optional)** | Raspberry Pi 4 | Bluetooth LE Mesh / Periodic Advertising Scanner | Local facility dashboard and long-term trend aggregation |

---

## Planned Architecture
1. **Sense:** Hardware wake-on-vibration interrupt triggers synchronized IMU (I2C/SPI) + MEMS Microphone (I2S/PDM) + NTC (14-bit SAADC) capture via EasyDMA.
2. **Process & Infer:** CMSIS-DSP extracts time/frequency features (RMS, Kurtosis, FFT bins) -> On-device TinyML model (Axon NPU / Neuton) classifies machine health state and estimates Remaining Useful Life (RUL).
3. **Store:** Fault events and spectral snapshots are logged to the nRF54LM20 DK's onboard **8 MB QSPI Flash**.
4. **Communicate:** Broadcasts compact health vectors over **BLE Periodic Advertising / BLE Mesh** and provides instant **NFC tap-to-inspect** readout for maintenance personnel.

## Software Toolchain
* **Firmware:** nRF Connect SDK (Zephyr RTOS), C/C++, CMSIS-DSP
* **IDE & Debugging:** nRF Connect for VS Code, SEGGER J-Link OB, Wireshark BLE Sniffer
* **Edge AI:** Nordic Neuton TinyML / Axon NPU Toolchain
* **Mobile App:** Android Studio (Kotlin/Java BLE GATT & Periodic Advertising Scanner)
