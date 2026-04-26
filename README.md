# Electric-Bicycle-Conversion-System

> A conventional bicycle retrofitted into a smart electric vehicle with regenerative pedal charging, real-time telemetry display, and energy-efficient drive control.

## Table of Contents

- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Components Used](#components-used)
- [Circuit Description](#circuit-description)
- [Working Principle](#working-principle)
- [Display & Telemetry](#display--telemetry)
- [Performance Specifications](#performance-specifications)
- [How to Replicate](#how-to-replicate)
- [Project Photos](#project-photos)
- [Demo Video](#demo-video)
- [Future Improvements](#future-improvements)
- [Author](#author)
- [References](#references)

---

## Project Overview

This project converts a standard mechanical bicycle into a fully functional electric vehicle using off-the-shelf components and custom integration. The goal was to design a low-cost, sustainable urban transport solution that:

- Eliminates fuel cost for short-distance commuting
- Reuses an existing bicycle frame (circular economy approach)
- Extends battery range through regenerative pedal energy recovery
- Provides real-time rider feedback via an embedded telemetry display

The system consumes energy **only during active motor acceleration** — coasting and pedalling draw zero motor current, making it highly efficient compared to conventional hub-motor e-bikes that run continuously.

---

## Key Features

| Feature | Description |
|--------|-------------|
| **Motor Drive** | DC/BLDC hub motor activated only on throttle input |
| **Regenerative Charging** | Pedalling generates electricity fed back into the battery |
| **Key Switch Ignition** | Main circuit isolated when key is off — prevents accidental activation |
| **Throttle Regulator** | PWM-based speed controller for smooth acceleration control |
| **LCD Telemetry Display** | Real-time speed, odometer, battery %, date and time |
| **~50 km Range** | Per full charge under mixed motor + pedal conditions |
| **Zero Idle Consumption** | Motor draws no current when not accelerating |

---

## System Architecture

```
 ┌──────────────────────────────────────────────────────────────┐
 │                    ELECTRIC BICYCLE SYSTEM                   │
 └──────────────────────────────────────────────────────────────┘

  ┌─────────┐    ┌──────────┐    ┌────────────┐    ┌─────────┐
  │   Key   │───▶│  Battery │───▶│  PWM Speed │───▶│  Motor  │
  │ Switch  │    │   Pack   │    │ Controller │    │ (Drive) │
  └─────────┘    └──────────┘    └────────────┘    └─────────┘
                      ▲                 ▲
                      │                 │
              ┌───────┴──────┐  ┌───────┴──────┐
              │  Regenerative│  │   Throttle   │
              │  Pedal Unit  │  │  Regulator   │
              └──────────────┘  └──────────────┘
                      
  ┌──────────────────────────────────────────┐
  │               LCD DISPLAY                │
  │   Speed (km/h) | Odometer | Battery %    │
  │   Date | Time  (via RTC module)          │
  └──────────────────────────────────────────┘
```

---

## Components Used

### Mechanical
- Standard adult bicycle (26-inch wheels)
- Rear wheel hub motor (integrated or external mount)

### Electrical & Electronics

| Component | Purpose |
|-----------|---------|
| BLDC / DC Hub Motor | Wheel drive — activated by throttle only |
| Li-ion / SLA Battery Pack | Energy storage (~36V or 48V depending on motor spec) |
| PWM Speed Controller | Variable speed via throttle input |
| Throttle (twist-grip or thumb) | Rider-controlled acceleration input |
| Key Switch | Main circuit ON/OFF and safety isolation |
| Generator / Dynamo Circuit | Converts pedal energy to electrical charge |
| Rectifier + Charge Circuit | Converts AC dynamo output to DC for battery |
| LCD Display Module | Visual feedback for rider |
| RTC Module (DS3231 or similar) | Real-time clock for date/time on display |
| Hall Effect / Reed Switch Sensor | Speed sensing for speedometer |
| Microcontroller (Arduino Uno/Nano) | Reads sensors, drives display |
| Wiring, connectors, fuse, mounting hardware | System integration |

---

## Circuit Description

### Power Path
```
Battery (+) → Key Switch → PWM Controller → Motor → Battery (-)
```

When the key is OFF, the entire drive circuit is de-energized. The throttle sends a 0–5V signal to the PWM controller which adjusts motor speed proportionally. The motor receives power **only when the throttle is engaged** — no current flows during coasting.

### Regenerative Charging Path
```
Pedals → Chain → Dynamo/Generator → Rectifier → Charge Circuit → Battery
```

Pedalling rotates a small generator coupled to the drivetrain. The AC output is rectified and voltage-regulated before being fed into the battery. This occurs in parallel with the drive circuit — riding and charging can happen simultaneously.

### Display Circuit
```
Hall Sensor (wheel) → Microcontroller → LCD Display
Battery Voltage Divider → ADC on Microcontroller
RTC Module (I2C) → Microcontroller
```

---

## Working Principle

1. **Start**: Rider inserts key → main circuit is energized
2. **Idle**: No throttle input → motor draws zero current → display shows stats
3. **Acceleration**: Rider engages throttle → PWM controller drives motor → bicycle accelerates
4. **Coasting**: Throttle released → motor disengages → no energy consumed
5. **Pedalling**: Rider pedals → dynamo generates electricity → battery charges
6. **Combined mode**: Rider pedals + uses throttle → partial regen + partial motor drive

This creates a system that actively recovers energy during pedalling, behaving similarly to regenerative braking in electric cars — except the energy source is human pedal power rather than braking kinetic energy.

---

## Display & Telemetry

The LCD display shows the following in real time:

```
┌─────────────────────────┐
│  SPD: 24 km/h  BAT: 78% │
│  ODO: 1234 km           │
│  2025-04-26  10:43 AM   │
└─────────────────────────┘
```

| Parameter | Source | Update Rate |
|-----------|--------|-------------|
| Speed (km/h) | Hall effect sensor on wheel | ~1 sec |
| Odometer (km) | Accumulated pulse count from speed sensor | Continuous |
| Battery % | Voltage divider → ADC → mapped to % | ~5 sec |
| Date & Time | RTC module (DS3231) over I2C | 1 sec |

---

## Performance Specifications

| Specification | Value |
|--------------|-------|
| Operating Voltage | 36V / 48V (motor-dependent) |
| Estimated Range | ~50 km per charge |
| Top Speed (motor assisted) | ~25–30 km/h (regulator limited) |
| Charging Method | Regenerative pedalling + grid charging |
| Motor Activation | Throttle-only (zero idle draw) |
| Display | 16×2 or 20×4 LCD |
| Ignition | Key switch (main circuit isolation) |

---

## How to Replicate

### Step 1 — Gather Components
Refer to the [Components Used](#components-used) table. Total estimated cost: ₹4,000–₹8,000 depending on motor/battery spec.

### Step 2 — Mount the Motor
Attach the hub motor to the rear wheel axle (or replace rear wheel with a motorized hub wheel). Ensure the motor wires are routed safely along the frame.

### Step 3 — Mount Battery Pack
Secure the battery on the frame (down tube or rear rack). Use zip ties and a mounting bracket.

### Step 4 — Wire the Control Circuit
- Connect battery → key switch → PWM controller → motor
- Connect throttle signal wire to PWM controller input
- Add an inline fuse (10A–20A) on the positive battery line

### Step 5 — Wire the Regenerative Unit
- Mount dynamo near the drivetrain
- Connect dynamo output → rectifier → voltage regulator → battery charge input
- Ensure the charge voltage does not exceed battery's rated charge voltage

### Step 6 — Set Up the Display
- Connect Hall sensor to microcontroller interrupt pin
- Connect RTC module via I2C (SDA/SCL pins)
- Connect battery voltage divider to ADC pin
- Upload the display firmware (see `code/display-firmware/`)
- Mount LCD display at handlebar level

### Step 7 — Test
1. Test key switch: circuit should be dead when key is off
2. Test throttle: motor should respond smoothly from 0 to max
3. Test pedal charging: multimeter on battery terminals while pedalling should show slight voltage rise
4. Test display: all parameters should update correctly

---

## Project Photos

> *(Add your actual photos here)*

| View | Description |
|------|-------------|
| `images/full-build.jpg` | Complete assembled electric bicycle |
| `images/display-closeup.jpg` | LCD showing speed, battery, date/time |
| `images/motor-mount.jpg` | Hub motor on rear wheel |
| `images/battery-mount.jpg` | Battery pack mounted on frame |
| `images/controller-wiring.jpg` | PWM controller and wiring |

---

## Demo Video

> *(Add your demo video link here)*

[![Demo Video](https://img.shields.io/badge/Watch%20Demo-YouTube-red)](https://youtube.com/your-video-link)

The demo video shows:
- Key switch ignition sequence
- Live throttle-to-motor response
- LCD display updating speed and battery in real time
- Pedal charging (voltmeter reading while pedalling)

---

## Future Improvements

- **Bluetooth data logging**: Send speed/battery/odometer data to a mobile app
- **Solar panel integration**: Add a small solar panel on the rear rack for passive charging
- **Regenerative braking**: Recover energy during braking in addition to pedalling
- **Battery Management System (BMS)**: Add cell-level protection and balancing
- **GPS tracking**: Add GPS module to display live route and total distance on app
- **IoT dashboard**: Cloud-connected telemetry viewable remotely

---

## Author

Boni Adi Vishnu  
B.Tech in Electronics and Communication Engineering, Raghu Engineering College.
Email: adivishnu8977@gmail.com  
LinkedIn: [linkedin.com/in/adivishnu(https://www.linkedin.com/in/adi-vishnu-boni-589894271/)  

---

## References

1. Chapman, S. J. *Electric Machinery Fundamentals*. McGraw-Hill, 5th ed.
2. Mohan, N. *Power Electronics: Converters, Applications, and Design*. Wiley.
3. Arduino Documentation — [https://docs.arduino.cc](https://docs.arduino.cc)
4. DS3231 RTC Datasheet — Maxim Integrated
5. BLDC Motor Control Theory — Texas Instruments Application Note SLVA704

---

*This project was developed as part of an undergraduate engineering final year project. All component choices, circuit designs, and integration decisions were made independently.*
