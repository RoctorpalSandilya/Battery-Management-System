# Battery Management System (BMS)

## Overview

This repository contains the firmware for a **Battery Management System (BMS)** developed for the Formula Style Electric Vehicle (EV) of the IIT Kharagpur Formula Student team. The system is designed to monitor, manage, and protect the vehicle's battery pack with real-time performance tracking and safety mechanisms.

## Project Details

### Organization
- **Team**: Formula Student Team - IIT Kharagpur
- **Vehicle Type**: Formula Style Electric Vehicle (EV)
- **Purpose**: High-performance battery management for competitive motorsport applications

### System Architecture

#### Hardware Components
- **Master Controller**: STM32 MCU - Handles primary control logic, data processing, and communication
- **Slave ICs**: ADBMS6830 - Battery monitoring and balancing integrated circuits for individual cell monitoring
- **Communication Protocol**: I2C/SPI (configurable via ADBMS6830 interface)

#### Software Environment
- **Development Framework**: Mbed OS
- **IDE**: Keil Studio
- **Primary Languages**: C++ (51.4%) and C (48.6%)

## Key Features

### Battery Monitoring
- **Real-time Cell Voltage Monitoring**: Continuous tracking of individual cell voltages via ADBMS6830 slaves
- **Temperature Monitoring**: Thermal sensor integration for safe operating conditions
- **Current Monitoring**: High-precision current measurement for SOC (State of Charge) calculation
- **Pack Voltage Tracking**: Aggregate pack voltage and health metrics

### Active Battery Management
- **Cell Balancing**: Automatic passive/active balancing through ADBMS6830 balancing circuits
- **State of Charge (SOC) Estimation**: Coulomb counting with temperature compensation
- **State of Health (SOH) Monitoring**: Battery degradation tracking over vehicle lifetime
- **Thermal Management**: Temperature-based throttling and cooling system control

### Safety Features
- **Over-Voltage Protection**: Individual cell and pack-level protection
- **Under-Voltage Protection**: Prevention of deep discharge
- **Over-Current Protection**: Rapid detection and response to fault currents
- **Temperature Monitoring**: Over-temperature and under-temperature alarms
- **Isolation Monitoring**: Battery isolation fault detection
- **Emergency Shutdown**: Fail-safe mechanisms for critical faults

### Vehicle Integration
- **CAN Bus Interface**: Communication with vehicle control units
- **Data Logging**: Onboard storage of critical parameters for post-race analysis
- **Performance Metrics**: Real-time efficiency and power delivery tracking
- **Telemetry Support**: Wireless data transmission to pit crew monitoring systems

## Hardware Architecture

### Block Diagram
```
┌─────────────────────────────────────────┐
│        STM32 MCU (Master)               │
│   - Control Logic                       │
│   - Data Processing                     │
│   - CAN Communication                   │
└────────────────────┬────────────────────┘
                     │
         (I2C/SPI Bus)
                     │
        ┌────────────┴────────────┐
        │                         │
   ┌────▼────┐             ┌─────▼────┐
   │ADBMS6830│   ...       │ADBMS6830 │
   │ Slave 1 │             │ Slave N  │
   └────┬────┘             └─────┬────┘
        │                        │
    ┌───┴─────────────────────────┴────┐
    │   Battery Pack (Series Cells)    │
    │   Cell1 - Cell2 - ... - CellN    │
    └────────────────────────────────────┘
```

### ADBMS6830 Features
- Monitors up to 12 cells per IC
- Daisy-chainable for multi-IC systems
- Integrated passive balancing circuitry
- Integrated voltage and temperature measurements
- I2C and SPI communication options

## Software Architecture

### Firmware Modules

#### Core Modules
- **`main.cpp`**: Firmware entry point and main control loop
- **`BMS_Core.cpp/h`**: Primary BMS control and state machine
- **`Cell_Monitor.cpp/h`**: Individual cell monitoring and data collection
- **`Pack_Monitor.cpp/h`**: Aggregate pack-level monitoring

#### Communication Modules
- **`ADBMS_Driver.cpp/h`**: ADBMS6830 IC communication and control
- **`CAN_Interface.cpp/h`**: CAN bus communication with vehicle systems
- **`Telemetry.cpp/h`**: Wireless/wired telemetry output

#### Safety & Control Modules
- **`Safety_Manager.cpp/h`**: Fault detection and response
- **`Balancer.cpp/h`**: Cell balancing algorithm implementation
- **`Thermal_Manager.cpp/h`**: Temperature-based control

#### Utility Modules
- **`Data_Logger.cpp/h`**: Onboard logging functionality
- **`Config.h`**: System configuration and calibration parameters
- **`Utils.cpp/h`**: Common utility functions

### State Machine
The BMS operates through distinct states:
1. **INIT**: System initialization and hardware verification
2. **IDLE**: Standby mode with minimal monitoring
3. **ACTIVE**: Normal vehicle operation with full monitoring
4. **BALANCING**: Cell balancing in progress
5. **THERMAL_THROTTLE**: Reduced performance due to temperature
6. **FAULT**: Error state with safe shutdown procedures
7. **SHUTDOWN**: Controlled system shutdown

## Getting Started

### Prerequisites
- **Keil Studio** IDE installed
- **STM32CubeMX** for MCU configuration (optional)
- **Mbed OS 6.x** or compatible version
- STM32 compatible development board
- ADBMS6830 evaluation board or custom PCB

### Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/RoctorpalSandilya/Battery-Management-System.git
   cd Battery-Management-System
   ```

2. **Setup Keil Studio Project**
   - Open Keil Studio
   - Import the project from the cloned directory
   - Select your target MCU (STM32 series)

3. **Configure Mbed OS**
   - Ensure Mbed OS libraries are properly configured
   - Update board-specific settings in `mbed_app.json`

4. **Build the Project**
   ```bash
   mbed build -t GCC_ARM -m STM32XXXXX
   ```

5. **Flash to MCU**
   - Use ST-Link or DAPLink programmer
   - Follow your IDE's flashing procedure

### Configuration

Key configuration parameters in `Config.h`:
- Number of ADBMS6830 ICs connected
- Cell voltage limits (min/max)
- Pack voltage limits
- Temperature thresholds
- Balancing parameters
- CAN bus settings

## Development & Debugging

### Build Options
- **Debug Build**: Full debugging symbols and logging
- **Release Build**: Optimized for performance
- **Test Build**: With diagnostic/telemetry endpoints

### Debugging Tools
- **Serial Terminal**: UART debugging interface
- **CAN Bus Analyzer**: Monitor CAN traffic
- **Data Logger**: Review historical data via SD card or FLASH

### Testing
- Unit tests for individual modules
- Integration tests with ADBMS6830 simulator
- Vehicle-in-the-loop testing during competition

## Performance Specifications

### Electrical Specifications
- **Cell Voltage Monitoring Accuracy**: ±10mV
- **Pack Voltage Range**: 48V - 800V (configurable)
- **Current Monitoring Range**: ±500A (adjustable)
- **Temperature Range**: -20°C to +80°C

### Timing Specifications
- **Cell Monitoring Frequency**: 10Hz - 1kHz (configurable)
- **Balancing Update Rate**: 1Hz
- **CAN Message Rate**: 100Hz typical
- **Data Logging Rate**: 10Hz

### Safety Response Times
- **Over-current Detection**: <10ms
- **Over-voltage Detection**: <20ms
- **Thermal Shutdown**: <50ms

## Competition Performance

This BMS system has been engineered for:
- **Endurance Races**: Multi-hour reliability
- **Acceleration Events**: High power delivery monitoring
- **Efficiency Challenges**: Real-time energy tracking
- **Autonomous Driving**: Vehicle integration compatibility

## Maintenance & Updates

### Regular Maintenance
- Monitor cell balance drift over vehicle lifetime
- Update calibration parameters as battery ages
- Review data logs for anomaly detection
- Firmware updates for bug fixes and improvements

### Firmware Updates
- Over-the-air (OTA) update capability via CAN
- Bootloader implementation for seamless updates
- Version tracking and rollback support

## Known Limitations

- Current firmware supports up to 12 ADBMS6830 slaves (144 cells)
- Cell balancing is passive through resistive dissipation
- Communication latency dependent on I2C/SPI bus speed
- Thermal sensors limited to onboard ADC resolution

## Future Enhancements

- [ ] Active balancing circuit integration
- [ ] Machine learning-based SOH prediction
- [ ] Enhanced telemetry with cloud integration
- [ ] Multi-level safety redundancy
- [ ] Predictive maintenance algorithms
- [ ] WiFi/Bluetooth connectivity for pit crew monitoring

## Project Structure

```
Battery-Management-System/
├── README.md
├── mbed_app.json
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── BMS_Core.cpp
│   ├── Cell_Monitor.cpp
│   ├── Pack_Monitor.cpp
│   ├── ADBMS_Driver.cpp
│   ├── CAN_Interface.cpp
│   ├── Safety_Manager.cpp
│   ├── Balancer.cpp
│   ├── Thermal_Manager.cpp
│   ├── Data_Logger.cpp
│   └── Telemetry.cpp
├── include/
│   ├── BMS_Core.h
│   ├── Cell_Monitor.h
│   ├── Pack_Monitor.h
│   ├── ADBMS_Driver.h
│   ├── CAN_Interface.h
│   ├── Safety_Manager.h
│   ├── Balancer.h
│   ├── Thermal_Manager.h
│   ├── Data_Logger.h
│   ├── Telemetry.h
│   ├── Config.h
│   └── Utils.h
├── lib/
│   └── Mbed_OS/ (Mbed OS libraries)
├── docs/
│   ├── Hardware_Setup.md
│   ├── Communication_Protocol.md
│   ├── Calibration_Guide.md
│   └── Troubleshooting.md
└── tests/
    ├── unit_tests/
    └── integration_tests/
```

## Documentation

Detailed documentation available in the `docs/` directory:
- **Hardware Setup Guide**: Wiring and PCB layout
- **Communication Protocol**: CAN and I2C/SPI specifications
- **Calibration Guide**: System calibration procedures
- **Troubleshooting**: Common issues and solutions
- **API Reference**: Function documentation and usage

## Contributing

This firmware is maintained by the IIT Kharagpur Formula Student team. For contributions:

1. Create a feature branch
2. Make your improvements with clear commit messages
3. Ensure code follows the project style guide
4. Submit a pull request with detailed description

## References & Resources

### Technical Documentation
- [ADBMS6830 Datasheet](https://www.analog.com/en/products/adbms6830.html)
- [STM32 MCU Reference Manual](https://www.st.com/en/microcontrollers/stm32-32-bit-arm-cortex-m-mcus.html)
- [Mbed OS Documentation](https://os.mbed.com/docs/)
- [Keil Studio Documentation](https://www.keil.com/products/keilstudio/)

### External Resources
- Formula SAE Official Rules and Guidelines
- CAN Bus Protocol Specifications (ISO 11898)
- Battery Management System Best Practices

## License

This project is licensed under the **MIT License** - see the LICENSE file for details.

## Contact & Support

**Developer**: RoctorpalSandilya

**Team**: Formula Student Team - IIT Kharagpur

For questions, bug reports, or suggestions:
- Open an issue on GitHub
- Contact the development team through IIT Kharagpur Formula Student channels

## Acknowledgments

- IIT Kharagpur Formula Student Team members and advisors
- Analog Devices (ADBMS6830 support)
- STMicroelectronics (STM32 MCU support)
- Mbed OS and Keil Studio communities

---

**Last Updated**: May 2026
**Firmware Version**: 1.0.0
**Status**: Active Development
