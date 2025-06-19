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

## 🔗 Information About flashing

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
---

## 🔗 Related Firmware Project

The factory firmware (v1.01) appears to be based on or derived from [G8PTN's ATS_MINI project](https://github.com/G8PTN/ATS_MINI), which provides a minimal implementation of the ATS receiver using ESP32-S3 and the SI4732.

> 📌 G8PTN confirms that some units with the Hi-Z front-end exhibit **battery drain even when powered off**, due to a design issue where the power to the JFET buffer is not properly switched.  
> See: [this discussion with annotated image](https://github.com/G8PTN/ATS_MINI/discussions/106) for more details.

This hardware modification involves cutting a factory trace and rerouting it through a switch-controlled line to avoid leaving the front-end MOSFET permanently powered.

---
---

## ⚠️ Known Hardware Issue: Battery Drain on V3 Units

Several users have confirmed that **AMNVOLT Mini V3** units with the **Hi-Z input circuit** and headphone amplifier may experience significant **battery drain**, even when turned off.

### 🧠 Cause
The issue is related to a design flaw where a power trace feeds voltage directly to the **JFET buffer** or associated front-end circuitry without being properly switched off. This keeps a transistor partially powered at all times.

### 🔧 Community Workaround
To fix this, users have applied a **hardware modification** by:

- Scraping a specific via and soldering a **0-ohm resistor** to bypass or reroute the trace to a switched voltage point.
- Avoiding the method of lifting transistor legs, which may harm RF performance or bypass critical filtering.

### 📎 References
- 📷 [Community discussion and visual mod example (Facebook group)](https://www.facebook.com/groups/629443686140117/permalink/723494063401745)
- 🛠️ [G8PTN analysis and trace reroute suggestion](https://github.com/G8PTN/ATS_MINI/issues/37#issuecomment-2934519034)

> This issue does **not affect earlier V2 units**, which seem to have proper power gating for the front-end.

---
