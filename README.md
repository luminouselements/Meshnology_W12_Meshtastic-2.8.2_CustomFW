# Meshnology W12 Meshtastic Variant
[Meshtastic Firmware 2.7.26.54e0d8d Beta](https://github.com/meshtastic/firmware/tree/develop/variants/esp32s3/meshnology-w12)

![Meshtastic](https://img.shields.io/badge/Meshtastic-Compatible-purple)
![ESP32-S3](https://img.shields.io/badge/MCU-ESP32--S3-blue)
![16MB Flash](https://img.shields.io/badge/Flash-16MB-green)
![8MB PSRAM](https://img.shields.io/badge/PSRAM-8MB-green)
![LR2021](https://img.shields.io/badge/Radio-LR2021-orange)
![SSD1315](https://img.shields.io/badge/OLED-SSD1315-yellow)

Meshtastic support for the Meshnology W12 (WiFi LoRa 32 V5).
![](https://wiki.meshnology.com/assets/images/w12new-03b97ecfaa4bbb52ef1a24875fbb1ca2.jpg)

---

<details>
<summary><strong>📋 Hardware Summary</strong></summary>

| Component | Specification |
|------------|--------------|
| MCU | ESP32-S3R8 |
| Flash | 16MB |
| PSRAM | 8MB OPI |
| Radio | LR2021 |
| Display | SSD1315 128x64 OLED |
| USB | Native USB-C |
| WiFi | 2.4GHz |
| BLE | BLE 5 |
| Variant | meshnology_w12 |
| Hardware Model | 145 |

</details>

---

<details>
<summary><strong>📚 Table of Contents</strong></summary>

- [Overview](#overview)
- [Features](#features)
- [Hardware Specifications](#hardware-specifications)
- [Quick Reference](#quick-reference)
- [Complete Hardware Pin Mapping](#complete-hardware-pin-mapping)
- [OLED Configuration](#oled-configuration)
- [LR2021 Radio Configuration](#lr2021-radio-configuration)
- [Battery Monitoring](#battery-monitoring)
- [Pin Conflict Warnings](#pin-conflict-warnings)
- [Hardware Validation Checklist](#hardware-validation-checklist)
- [Build Instructions](#build-instructions)
- [Flashing](#flashing)
- [Recovery](#recovery)
- [Known Limitations](#known-limitations)

</details>

# Overview

Support files for the Meshnology W12 Meshtastic variant.

# Features

- ESP32-S3R8
- 16MB Flash
- 8MB OPI PSRAM
- LR2021 LoRa radio
- SSD1315 OLED
- Native USB CDC
- OTA support

# Hardware Specifications

| Item | Value |
|------|------|
| MCU | ESP32-S3R8 |
| Flash | 16MB |
| PSRAM | 8MB OPI |
| Display | SSD1315 |
| Radio | LR2021 |

# Quick Reference

| Function | GPIO |
|-----------|--------|
| USER BUTTON | GPIO0 |
| BATTERY ADC | GPIO1 |
| BATTERY ENABLE | GPIO2 |
| LR2021 CS | GPIO8 |
| LR2021 SCK | GPIO9 |
| LR2021 MOSI | GPIO10 |
| LR2021 MISO | GPIO11 |
| LR2021 RESET | GPIO12 |
| LR2021 BUSY | GPIO13 |
| LR2021 IRQ | GPIO14 |
| OLED SDA | GPIO17 |
| OLED SCL | GPIO18 |
| USB D- | GPIO19 |
| USB D+ | GPIO20 |
| OLED CS | GPIO21 |
| VEXT ENABLE | GPIO45 |
| RGB LED | GPIO46 |
| OLED RESET | GPIO47 |
| GNSS CTRL | GPIO48 |

# Complete Hardware Pin Mapping

| Pin | GPIO | Function |
|------|------|------|
| 5 | GPIO0 | User Button |
| 6 | GPIO1 | Battery ADC |
| 7 | GPIO2 | Battery Enable |
| 12 | GPIO7 | LR2021 DIO7 |
| 13 | GPIO8 | LR2021 CS |
| 14 | GPIO9 | LR2021 SCK |
| 15 | GPIO10 | LR2021 MOSI |
| 16 | GPIO11 | LR2021 MISO |
| 17 | GPIO12 | LR2021 RESET |
| 18 | GPIO13 | LR2021 BUSY |
| 19 | GPIO14 | LR2021 IRQ |
| 22 | GPIO17 | SDA |
| 23 | GPIO18 | SCL |
| 24 | GPIO19 | USB D- |
| 25 | GPIO20 | USB D+ |
| 26 | GPIO21 | OLED CS |
| 36 | GPIO45 | VEXT |
| 37 | GPIO46 | RGB |
| 28 | GPIO47 | OLED RESET |
| 27 | GPIO48 | GNSS CTRL |

# OLED Configuration

```cpp
#define USE_SSD1306
#define I2C_SDA 17
#define I2C_SCL 18
#define VEXT_ENABLE 45
```

# LR2021 Radio Configuration

```cpp
#define LORA_CS 8
#define LORA_SCK 9
#define LORA_MOSI 10
#define LORA_MISO 11
#define LR2021_NRESET_PIN 12
#define LR2021_BUSY_PIN 13
#define LORA_DIO1 14
#define LR2021_IRQ_DIO_NUM 8
```

# Battery Monitoring

```cpp
#define BATTERY_PIN 1
#define ADC_CTRL 2
```

# Pin Conflict Warnings

Do not repurpose GPIOs used by:

- USB (19,20)
- OLED (17,18,21,45,47)
- LR2021 (7-14)
- Battery monitor (1,2)

# Hardware Validation Checklist

- [ ] USB detected
- [ ] OLED initializes
- [ ] LR2021 initializes
- [ ] TX/RX verified
- [ ] Battery reporting works
- [ ] OTA update works
- [ ] 24-hour stability test completed

# Build Instructions

```bash
pio run -e meshnology_w12
```

# Flashing

```bash
pio run -e meshnology_w12 -t upload
```

Factory Image:

```bash
esptool.py --chip esp32s3 write_flash 0x0000 firmware-meshnology_w12.factory.bin
```

# Recovery

Erase:

```bash
esptool.py --chip esp32s3 erase_flash
```

Reflash the factory image.

# Known Limitations

- RGB LED disabled
- GNSS not implemented
- Future LR2021 features may require driver updates

# Build Validation Results

```text
Environment : meshnology_w12
Status      : SUCCESS
RAM Usage   : 39.6%
Flash Usage : 34.7%
```
