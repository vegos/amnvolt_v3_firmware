# MiniATS V3S (AMNVOLT) — ESP32 + SI4732 Multi‑Band Radio Receiver

Hands‑on photos, specs, and notes for the **MiniATS V3S** portable receiver.  
This page collects what’s observable from the unit in hand plus the manufacturer’s stated features.

> TL;DR: Latest (Jun‑2025) AMNVOLT Mini with **injection‑molded ABS shell**, **ESP32-S3-WROOM-1** microcontroller, **built‑in Hi‑Z** input network, **headphone amplifier**, and an optimized power circuit that **fixes the battery drain issue when powered-off**.


---

## 📸 Gallery

- **Front (powered off)**
  
  ![Front view](./images/1.jpg)


- **Back**
  
  ![Back view](./images/2.jpg)
  *(I put this sticker on to tell this radio apart from the others)*


- **MW/AM in operation**
  
  ![AM screen](./images/3.jpg)


- **FM in operation**
- 
  ![FM screen](./images/4.jpg)


- **Main PCB (top, close‑up)**
 
  ![PCB close](./images/5.jpg)


- **PCB with Li‑Po & speaker**

  ![PCB + battery](./images/6.jpg)


- **Front sub‑board with ESP32/SI4732 silkscreen**
  
  ![ESP32‑SI4732 sub‑board](./images/7.jpg)


- **Battery & speaker detail**  
  ![Battery & speaker](./images/8.jpg)


---

## ⭐ Highlights
- **June 2025 / upgraded version.**
- **Built‑in Hi‑Z circuit** to improve weak‑signal handling on long‑wire/Hi‑Z antennas (The same with V3, a very simple High-Z circuit with a JFET).
- **Built‑in headphone amplifier** — noticeably louder in headphones than previous non‑amp versions (V1 actually, V2 and later has headphone amp).
- **Injection‑molded ABS enclosure** (stronger and cleaner than prior 3D‑printed shells). That's a really good upgrade.
- **JFET power circuit optimized** — no more “battery drain after shutdown” issue.
- **1.9‑inch IPS color display** (170×320) with adjustable backlight and on‑screen battery level.
- **800 mAh Li‑Po battery**; manufacturer quotes **10+ hours** typical listening.
- **Firmware upgradable** (ESP32 platform).


---

## 📡 Bands & Modes
- **Modes:** AM, **LSB**, **USB**, **FM (WFM)**  
- **Coverage:** LW/MW/SW (**150 kHz–30 MHz**) and **VHF FM 64–108 MHz**  


---

## 🛠️ Hardware (observed on this unit)
- **MCU/Module:** *ESP32‑S3-WROOM‑1* (ESPRESSIF)
    - ESP32-S3 series of SoCs embedded, Xtensa® dual-core 32-bit LX7 microprocessor (with single precision FPU), up to 240 MHz
      • 384 KB ROM
      • 512 KB SRAM
      • 16 KB SRAM in RTC
      • Up to 16 MB PSRAM
    - Wi-Fi
      • 802.11b/g/n
      • Bit rate: 802.11n up to 150 Mbps
      • A-MPDU and A-MSDU aggregation
      • 0.4 μs guard interval support
      • Center frequency range of operating channel: 2412 ~ 2484 MHz
    - Bluetooth
      • Bluetooth LE: Bluetooth 5, Bluetooth mesh
      • Speed: 125 Kbps, 500 Kbps, 1 Mbps, 2 Mbps
      • Advertising extensions
      • Multiple advertisement sets
      • Channel selection algorithm #2
      • Internal co-existence mechanism between Wi-Fi and Bluetooth to share the same antenna
- **RF IC:** Silicon Labs **SI4732** DSP receiver
- **Audio:** Integrated **headphone amplifier**; internal **speaker (≈1 W class)**  
- **Display:** **1.9″ IPS**, 170×320
- **Battery:** **Li‑Po 3.7 V 800 mAh** (cell marked **603040**, 2.96 Wh)
- **Connectors:** **SMA‑K (female, inner pin)** antenna, **3.5 mm stereo** headphone jack, **USB‑C** for charging/firmware
- **Enclosure:** **ABS injection‑molded**
- **On‑board:** JST battery connector, tactile buttons (BOOT/RESET), piezo/mini speaker


---

## 📦 File Layout
```
repo-root/
├─ README.md
└─ images/
   ├─ 1.jpg        # front
   ├─ 2.jpg        # back
   ├─ 3.jpg        # AM screen
   ├─ 4.jpg        # FM screen
   ├─ 5.jpg        # PCB close
   ├─ 6.jpg        # PCB + battery
   ├─ 7.jpg        # ESP32‑SI4732 sub‑board
   └─ 8.jpg        # battery & speaker detail
```


---

## ⚠️ Notes & Tips
- Use a **standard 5 V USB‑C charger** (no QC/PD fast‑charge).  
- As with any **Li‑Po** battery, avoid over‑discharge and store at moderate charge if unused for long periods. LiPo batteries (single cell) must be between 3.7V to 3.85V for storage.


---

## 📐 Measurements

* **V3S power supply noise measurements** → [Power Supply Noise](./Power_Supply_Noise/README.md)  
  Includes oscilloscope tests on the JFET Source (pin 2), SI4732 VDD_RF (pin10), and power switch output.  
  Covers both **DC bias voltages** and **AC ripple/noise analysis**, identifying the cleanest supply rail for the Hi-Z input stage.


---

## 📐 JFET Noise Reduction Mod

* **V3S power supply noise reduction modification** → [JFET_Power_Supply_Mod](./Amnvolt_V3S/JFET_Power_Supply_Mod/README.md)
  Based on Peter Neufeld’s modification → [Si4732A MiniRX Modifications](https://peterneufeld.wordpress.com/2025/06/13/si4732a-minirx-modifications/)
  The JFET (K51G) is now powered from SI4732 Pin 10 through a 22Ω resistor.
  Added local decoupling capacitors (100nF + 10µF) from the JFET drain (pin 2) to ground.
  Includes DC voltage drop measurements across the resistor and oscilloscope noise analysis (files ./JFET_Power_Supply_Mod/images/SDS00014.png–21.png).
  Comparison between old supply rail, SI4732 pin 10, and the new filtered JFET feed.


---

## License
Unless stated otherwise, text and photos © the original author(s). If you reuse content, please provide attribution.
