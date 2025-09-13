# AMNVOLT Mini ATS V3 – Original Firmware Backup

This repository contains a full backup of the original factory firmware preinstalled on the **AMNVOLT Mini SI4732 V3** pocket DSP radio, based on the **ESP32-S3** microcontroller and **SI4732** DSP chip.


There are some mods for the V3 version. You can find them <a href="https://github.com/vegos/amnvolt_v3_firmware/tree/main/Hardware%20Mod%20for%20V3%20Battery%20Drain">here</a>.

Also an SNR/Audio evaluation <a href="https://github.com/vegos/amnvolt_v3_firmware/blob/main/V2_vs_V3/README.md">here</a>.


## 📦 Firmware File

- **Filename:** `Mini ATS V3 - original-flash-1.01.bin`
- **Size:** 4 MB
- **Format:** Raw flash dump
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


## 🔋 Experimental Verification (V2 vs V3 battery drain)

After fully charging at the same time and leaving both units powered off for 24 hours:

| Device | Voltage after 24h OFF | Notes |
|--------|-----------------------|-------|
| V2     | 4.21 V                | No drain observed — battery remains full |
| V3     | 3.99 V                | Battery drain confirmed — ~0.2 V drop overnight |

This confirms that the V3 unit suffers from a measurable standby drain even when powered off, consistent with the known issue described above.  
A hardware modification is advised if long-term standby retention is important.

I'm working on a proper design without cutting traces, by removing parts and reconnect them correctly.


## 🛠️ My Hardware Mod to Prevent Battery Drain

A hardware modification was performed to disconnect the drain pin of the transistor from the PCB (lift the drain pin of the transistor), insulate it using kapton tape and rewire it so it receives power only when the device is switched on.

Procedure

Identified the transistor responsible for drain (K51G) and verified continuity with the battery + terminal. 
Tried desoldering the component without hot air due to 3D printed enclosure constraints.
Lifted the drain pin enough to fit insulation under it (kapton tape).
Soldered a jumper wire from the drain pin to a nearby VCC pad that is active only when the power switch is on. So I get power (VCC) from the switch itself.
Verified the modification by checking that no current is drawn when the device is turned off.
Confirmed the device still works fully - receiving signals - after the modification.

More information here: https://github.com/vegos/amnvolt_v3_firmware/tree/main/Hardware_Mod_for_V3_Battery_Drain

Notes

RF performance remains unaffected.
This method was chosen over PCB trace cutting due to being less invasive and more easily reversible. It doesn't need to cut any traces etc.
Similar mods have been performed by other users, and documented online.

📷 Photos from before, during, and after the mod are available and a dedicated subfolder with detailed documentatio.

---

## 📊 Mini ATS V2 vs V3 (AMNVOLT) – Audio and SNR Evaluation

A detailed comparison between the Mini ATS V2 and V3 (AMNVOLT) receivers, focusing on audio noise levels, dynamic range, and FT8 decoding performance. Includes lab tests with signal generator, spectrum analysis, and real-world SNR tracking over time.

➡️ <a href="https://github.com/vegos/amnvolt_v3_firmware/blob/main/V2_vs_V3/README.md">Read the full evaluation</a>.
