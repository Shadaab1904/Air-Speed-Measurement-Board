# Air-Speed-Measurement-Board
This project is a custom-designed PCB that integrates a **Sensirion SDP810 differential pressure sensor** with an **STM32F030F4Px microcontroller**. The board is designed for use in airspeed measurement applications, such as on drones and UAVs. It measures differential pressure via I²C, applies onboard filtering, converts it into airspeed (in m/s), and provides the result via an SPI interface to a master controller. An **interrupt pin** signals the availability of new data.

## 📦 Overview

- **Sensor**: Sensirion SDP810-500PA (differential pressure sensor)
- **Microcontroller**: STM32F030F4Px (ARM Cortex-M0)
- **Power**: 5V input with onboard 3.3V regulation
- **Interfaces**:
  - I²C (sensor side)
  - SPI (to master controller)
  - Read Interrupt Pin (signals new data ready)
  - SWD (for programming/debugging)
 
 ## 🎯 Purpose

This board acts as an **airspeed signal processing unit**. It reads differential pressure caused by the difference between total and static air pressure (using a pitot tube setup), computes the **dynamic pressure**, and derives the **airspeed** using Bernoulli’s principle.

## 🧠 Functional Description

### 1. **Sensor Interface (I²C)**
- The STM32 communicates with the SDP810 sensor using I²C.
- The sensor provides differential pressure readings in Pascal (Pa).

### 2. **Signal Processing**
- Raw pressure readings are digitally filtered.
- The airspeed \( v \) is calculated using:

\[
v = \sqrt{\frac{2 \cdot \Delta P}{\rho}}
\]

where:
- \( \Delta P \) = differential pressure in Pa
- \( \rho \) = air density (typically 1.225 kg/m³ at sea level)

### 3. **Data Output (SPI + Interrupt)**
- Filtered airspeed data is provided to an SPI master.
- An interrupt output toggles to notify the master of new data availability.

## 🔧 Key Components
- STM32F030F4Px: ARM Cortex-M0 MCU
- SDP810-500PA: Differential pressure sensor with I²C output
- HT75xx-1 (SOT89): Low dropout 3.3V voltage regulator
- B5819W: Reverse polarity protection diode
- TVS Diode: ESD protection on power lines
- Crystal. External clock source for STM32
- Conn_01x10_Pin: Main interface header
- Capacitors, Resistors: Decoupling, pull-ups, filtering
- Status Indicator: Optional debug/status indicator
- Ferrite Bead: Noise suppression on power line

## 📐 Pin Interface 

- VCC (5V): Input power (to LDO regulator)
- GND: Ground
- SWDIO: SWD programming interface
- SWCLK: SWD clock
- SPI_MISO: SPI Slave Output (to master)
- SPI_MOSI: SPI Slave Input (from master)
- SPI_SCK: SPI clock
- SPI_CS: Chip Select
- INT_OUT: Interrupt pin (new data ready)
- GND: Ground

## ⚡ Power and Programming
### 🔋 Power Supply

- Supply 5V through pin 1.
- The HT75xx LDO steps it down to 3.3V for STM32 and sensor.

### 🧠 Programming (SWD)

- Use **ST-Link V2** or equivalent SWD debugger:
-  STM32 Pin | ST-Link Pin
-  SWDIO     | SWDIO
-  SWCLK     | SWCLK
-  GND       | GND
-  NRST      | NRST
- 3.3V (ref) | Vcc

## 📈 Applications

- Fixed-wing drone airspeed measurement
- Feedback loop for fan/blower control
- Wind tunnel experiments
- HVAC airflow monitoring

## 📂 Project structure
- `Pressure sensor.kicad_sch` – Schematic file
- `Pressure sensor_pcb` – PCB layout file
- `Pressure sensor_pro` – KiCad project file
- `STEP/` - Sensor 3D model
- `gerber/` – Gerber and drill files for fabrication
- `bom.csv` – Bill of Materials

- ## ⚙️ Tools
- Designed with [KiCad](https://kicad.org/)

- ## 📦 Fabrication
Use the files in the `gerber/` directory to order PCBs from manufacturers (e.g., JLCPCB etc.).


- ## 📄 License
MIT License – feel free to modify and use.

## 🙌 Contributions
Pull requests welcome! If you spot issues or have improvements, please open an issue or PR.
