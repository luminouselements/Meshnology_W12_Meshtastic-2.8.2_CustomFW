# Meshnology W12 Meshtastic Variant

## Overview

Meshtastic variant for the Meshnology W12 (WiFi LoRa 32 V5).

### Hardware
- ESP32-S3R8
- 16MB Flash
- 8MB OPI PSRAM
- Semtech LR2021 LoRa radio
- SSD1315 128x64 OLED
- Native USB-C
- BLE + Wi-Fi

## Build Configuration

```ini
[env:meshnology_w12]
board = esp32-s3-devkitc-1
board_build.partitions = default_16MB.csv
board_upload.flash_size = 16MB
board_build.flash_mode = qio
board_build.psram_type = opi
board_build.arduino.memory_type = qio_opi
-D ARDUINO_USB_CDC_ON_BOOT=1
```

## Key Pin Assignments

### LoRa (LR2021)
| Signal | GPIO |
|----------|----------|
| CS/NSS | 8 |
| SCK | 9 |
| MOSI | 10 |
| MISO | 11 |
| RESET | 12 |
| BUSY | 13 |
| IRQ/DIO8 | 14 |
| DIO7 | 7 |

### OLED
| Signal | GPIO |
|----------|----------|
| SDA | 17 |
| SCL | 18 |
| CS | 21 |
| DC | 6 |
| RESET | 47 |
| VEXT Enable | 45 |

### Battery Monitor
| Signal | GPIO |
|----------|----------|
| Battery ADC | 1 |
| Divider Enable | 2 |

### USB
| Signal | GPIO |
|----------|----------|
| USB D- | 19 |
| USB D+ | 20 |

### GNSS Header
| Signal | GPIO |
|----------|----------|
| RX | 38 |
| TX | 39 |
| WAKE | 40 |
| PPS | 41 |
| RESET | 42 |
| CTRL | 48 |

## Critical Warnings

### LR2021 IRQ Mapping

```cpp
#define LR2021_IRQ_DIO_NUM 8
```

The W12 routes the radio interrupt through DIO8. Using the RadioLib default DIO5 configuration can cause channel scanning failures.

### Battery Monitor

GPIO2 enables the battery divider. Reusing GPIO2 will break battery measurements.

### OLED Power

GPIO45 controls the VEXT rail.

```cpp
digitalWrite(45, LOW);   // Enable
```

## Flashing

### Build

```bash
pio run -e meshnology_w12
```

### Upload

```bash
pio run -e meshnology_w12 -t upload
```

### Serial Monitor

```bash
pio device monitor -b 115200
```

## Recovery

### Erase Device

```bash
esptool.py --chip esp32s3 erase_flash
```

### Flash Factory Image

```bash
esptool.py --chip esp32s3 write_flash 0x0000 firmware-meshnology_w12.factory.bin
```

## Validation Checklist

- [ ] USB CDC detected
- [ ] OLED initializes
- [ ] LR2021 initializes
- [ ] Channel scan completes
- [ ] BLE pairing works
- [ ] Battery voltage reports correctly
- [ ] OTA updates tested
- [ ] 24-hour stability test passed
