# AMNVOLT Mini ATS V3 – Original Firmware Backup

This repository contains a full backup of the original factory firmware preinstalled on the **AMNVOLT Mini SI4732 V3** pocket DSP radio, based on the **ESP32-S3** microcontroller and **SI4732** DSP chip.

## 📦 Firmware File

- **Filename:** `Mini ATS V3 - original-flash-1.01.bin`
- **Size:** 4 MB
- **Format:** Raw flash dump (using `esptool.py`)
- **Dumped with:**
  ```bash
  uvx --from esptool esptool.py --chip esp32s3 --port COM5 --baud 921600 read_flash 0x0 ALL "Mini ATS V3 - original-flash-1.01.bin"
  ```


> ⚠️ The firmware version does **not show any "About" screen**, but it is  version `1.01`.

## 📻 Device Overview

| Feature               | Value                             |
|-----------------------|-----------------------------------|
| Model                 | AMNVOLT Mini SI4732 V3            |
| MCU                   | ESP32-S3                          |
| DSP Receiver Chip     | SI4732                            |
| Display               | 1.9" IPS (170×320)                |
| Audio Output          | Headphone amplifier + speaker     |
| Input Impedance       | Hardware switchable (Low-Z / Hi-Z) |
| Supported Modes       | AM / FM / SSB (LSB & USB)         |
| Firmware Upgradable   | Yes (via UART and `esptool.py`)   |

## 🔧 Usage

This firmware can be flashed back to a compatible V3 unit using:

```bash
esptool.py --chip esp32s3 --baud 921600 write_flash 0x000000 "Mini ATS V3 - original-flash-1.01.bin"
```

or with other tools...

## Information About flashing

Take a look at this link:
https://esp32-si4732.github.io/ats-mini/flash.html
It's very well documented the flashing or backing up procedure!


## 📂 Roadmap

We plan to:

- [ ] Analyze and map memory partitions (`binwalk`, `uvx`)
- [ ] Extract SPIFFS or other resource sections (if available)
- [ ] Provide unpacked firmware components (e.g. `firmware.bin`, `spiffs.img`)
- [ ] Document firmware capabilities and limitations vs open source builds (e.g. [`ats-mini2`](https://github.com/jumbo5566/ats-mini2))

## 📜 License

This repository serves archival and educational purposes only.  
Firmware belongs to its respective OEM or vendor.
