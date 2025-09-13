# 📻 V3S JFET Power Supply Noise Investigation

This document summarizes the investigation of the **AMNVOLT MiniATS V3S** power supply for the JFET (K51G) input stage, comparing the original rail with a new feed from the **SI4732 Pin 10**.

Inspired by ideas from **[Peter Neufeld’s modifications](https://peterneufeld.wordpress.com/2025/06/13/si4732a-minirx-modifications/)**.

---

## 🔧 Mod Overview

- Original design powered the **JFET** from a noisy rail influenced by the OLED backlight PWM.  
- Initial trials included a **22 Ω series resistor** and **local decoupling (100 nF + 10 µF)**.  
- Final configuration shown here uses **direct feed only (no resistor, no capacitors)**.  
- Comparisons were made with the **screen ON** and **screen OFF**.

---

## 📐 DC Bias

- **SI pin 10:** 3.3207 V  
- With 22 Ω in series (initial test): **3.2194 V** at JFET side → ~101 mV drop (≈4.6 mA current).  

*(The series resistor was later removed for final tests.)*

---

## 📊 Time-Domain Noise (Oscilloscope)

**Settings:** AC coupling, BW limit 20 MHz, probe ×10.

### Old Line (original rail)
- RMS ≈ **18–20 mV**  
- Pk–Pk ≈ **70–80 mV**  
- Strong periodic disturbances (PWM-related).  

![Old line](../JFET_Power_Supply_Mod_Old/images/SDS00014.png)

---

### New Line (SI4732 pin 10)
- RMS ≈ **3–4 mV**  
- Pk–Pk ≈ **16–30 mV**  
- Much quieter baseline.  

![New line](./images/SDS00030.png)

---

### Screen OFF
- Noise reduces further with backlight off.  
- Confirms display PWM as main noise source.  

![Old line screen off](./images/SDS00031.png)  
![New line screen off](./images/SDS00032.png)

---

## 📉 FFT Analysis

**Settings:** FFT, Hanning window, span ≈ 0–200 kHz.  

### Old Line
- Clear tones around **10–20 kHz** and harmonics.  
- Higher noise floor.  

![FFT old line](./images/SDS00029.png)

---

### New Line (SI4732 pin 10)
- Noise floor reduced by ~10–15 dB.  
- PWM peaks suppressed; screen OFF → nearly flat spectrum.  

![FFT new line](./images/SDS00030.png)  
![FFT new line screen off](./images/SDS00032.png)

---

## 🗂 Archived Measurements (with R/C filter)

Earlier experiments included a **22Ω series resistor** and **local decoupling (100nF + 10µF)**.  
Older results are archived for reference in [`../JFET_Power_Supply_Mod_Old/`](../JFET_Power_Supply_Mod_Old/).

Example:  

![With 22 Ω + caps](../JFET_Power_Supply_Mod_Old/images//SDS00015.png)

---

## ✅ Conclusion

- Powering the JFET from **SI4732 pin 10** significantly lowers supply noise at the High-Z input stage:  
  - **Old rail:** ~18–20 mV RMS  
  - **New rail (direct):** ~3–4 mV RMS  
- FFT confirms suppression of PWM harmonics from the OLED display.  
- Even without RC filtering, SI4732 pin 10 provides a **much cleaner supply**.
- Next step is to **reinstall decoupling** capacitors (100nF & 10μF).

---

📂 All oscilloscope screenshots are in `./images` (final setup).  
Archived R/C filter tests are in `../JFET_Power_Supply_Mod_Old/`.
