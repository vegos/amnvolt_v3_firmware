# 9:1 Impedance Transformer (Unun) for MiniATS V3/V3S

## 📘 Overview
This project describes the construction of a **9:1 Unun** (Unbalanced-to-Unbalanced transformer) for use with receivers like the **MiniATS V3 / V3S**, which feature a **high input impedance (~450 Ω)**, while the antenna is 50 Ω.  
The transformer steps impedance from ~50 Ω to ~450 Ω, improving matching and SNR across the HF bands (1–30 MHz).

---

## ⚙️ Technical Specifications

| Parameter | Value |
|-----------|-------|
| Type | 9:1 Unun (step-up) |
| Core | Amidon FT114-43 |
| Material | Ferrite Mix 43 (µᵢ ≈ 800) |
| AL value | ~510 nH / turn² |
| Wire | 0.5 mm enameled copper wire (single-layer) |
| Operating Frequency | 1 – 30 MHz |
| Tap | 8 turns from ground |
| Total turns | 24 |
| Transformation ratio | 9 : 1 (≈ 450 Ω ↔ 50 Ω) |

---

## 🧭 Connections

The transformer has three connection points:

| Label | Description | Impedance | Connection |
|--------|--------------|------------|-------------|
| **A** | Start of the winding (common ground) | — | SMA ground / shield |
| **B8** | Tap at the 8th turn from the ground | ~50 Ω | Antenna input (low-Z side) |
| **C** | End of the winding (after 24 total turns) | ~450 Ω | Receiver input (high-Z side) |

- The winding starts at **A** and ends at **C**, with a **tap at the 8th turn (B8)** used for the 50 Ω connection.  
- The ground (**A**) is common for both connectors.  
- This configuration transforms **50 Ω ↔ 450 Ω** (impedance ratio 9:1).  

---

## 🧪 Verification with NanoVNA

1. **Setup**  
   - Port 1 (S11) → tap 8 (B8, 50 Ω side).  
   - A–C → load 470 Ω (24 turns, 450 Ω side).  

2. **Results**  
   - **Return Loss**: Based on the NanoVNA, RL sits typically between **−8 and −14 dB** from **≈3–20 MHz**, with the best region around **~7–15 MHz**. That corresponds roughly to **VSWR ≈ 1.5–2.3**. Below ~3 MHz and above ~25 MHz RL degrades toward **−6 dB** (VSWR ≈ 3:1), which is expected for a simple 9:1 on FT114‑43.  

---

## 🧱 Assembly Notes

- Single-layer winding, evenly spaced.  
- Tap at 8 turns from ground (A).  
- Cyanoacrylate glue on SMA pins for mechanical stability.  
- Hot glue inside the case to secure the core.  
- Ground (A) tied to SMA shells.  
- Optional: add a **common-mode choke** (7–10 turns of RG316 on FT240-43) at receiver input.  

---

## 🧰 Build Log

Below is the construction process of the 9:1 transformer, step by step.

### 1️⃣ Core Winding  
Initial winding of the FT114-43 ferrite toroid — 24 total turns with a tap at the 8th turn.

![Construction](./images/1.%20Construction.jpg)

---

### 2️⃣ Initial Tests  
First measurements and functional checks using the NanoVNA and test receivers.

![First Tests](./images/2.%20FirstTests.jpg)

---

### 3️⃣ SMA Connector Preparation  
SMA connectors mounted and soldered to the wire ends for the low-Z and high-Z sides.

![Almost Ready](./images/3.%20AlmostRead.jpg)

---

### 4️⃣ Enclosure Preparation  
Drilled aluminum enclosure with openings for the SMA connectors.

![The Case](./images/4.%20TheCase.jpg)

---

### 5️⃣ Placement in Enclosure  
The toroidal transformer placed and wired inside the box.

![Placed1](./images/5.%20Placed1.jpg)  
![Placed2](./images/6.%20Placed2.jpg)

---

### 6️⃣ Securing the Core  
Core fixed in place with hot glue for mechanical stability and vibration damping.

![Secured](./images/7.%20Secured.jpg)

---

### 7️⃣ Final Assembly  
The finished 9:1 transformer with clear labeling for 50 Ω and 450 Ω connections.

![Case1](./images/8.%20Case1.jpg)  
![Case2](./images/9.%20Case2.jpg)

---

✅ The build is complete and verified through NanoVNA measurements, confirming the expected 50 ↔ 450 Ω transformation across the HF range.

---

## 🔍 Usage

| Setup | Description |
|-------|-------------|
| **50 Ω Antenna → MiniATS V3/V3S** | Connect antenna to 50 Ω side (B8), receiver to 450 Ω side (C). The Unun raises impedance, improving matching. |
| **NanoVNA test** | 470 Ω dummy load at C → Port 1 at B8 → expect ~50 Ω matching. |
| **Receivers with ~7 kΩ input (MiniATS V1/V2)** | Tap at 2 turns from ground for 1:144 ratio. |

---

## 📦 Materials Recap
- 1 × FT114-43 ferrite toroid  
- 2m enameled copper wire 0.5 mm  
- 2 × SMA female connectors  
- Cyanoacrylate glue (pins)  
- Hot glue (core fixing)  
- 470 Ω non-inductive resistor (for testing)  
- Metal or plastic enclosure  

---

## 📈 Performance Summary

| Frequency | RL (dB) | VSWR | Notes |
|-----------|---------|------|-------|
| 3.5 MHz | −17 | 1.3 : 1 | Excellent |
| 7 MHz | −14 | 1.5 : 1 | Very good |
| 14 MHz | −10 | 1.8 : 1 | Good |
| 21 MHz | −8 | 2.2 : 1 | Acceptable |
| 28 MHz | −6 | 2.8 : 1 | Marginal |

*Note: Since this is for receiving, SWR values are less critical.*

---

## 🔄 Reverse Usage

This transformer can also be used in **reverse mode**:  
- For a **high-impedance (~450 Ω) antenna** (longwire, loop, etc.) connected to a **50 Ω receiver** (SDR dongle, scanner, transceiver).  
- Connect antenna to the **450 Ω port**, and receiver to the **50 Ω port**.  
- The 9:1 ratio works both ways, stepping impedance up or down depending on direction.  

---

## 📈 Receiver Input Characterization (No Transformer)

Measured directly using NanoVNA Port 1 (S11).  
No matching transformer was used — these values represent the native input impedance of each MiniATS receiver.

| Receiver | Freq (MHz) | Parallel R | Input Z (approx.) | Notes |
|-----------|-------------|-------------|------------------|--------|
| **MiniATS V1** | 6.88 | 8.9 kΩ | ≈ 9 kΩ | Early design, medium-high Z |
| **MiniATS V2** | 6.88 | 18.1 kΩ | ≈ 18 kΩ | Improved front-end coupling |
| **MiniATS V3** | 6.88 | 774 kΩ | ≈ 770 kΩ | Very high Z input |
| **MiniATS V3S** | 6.88 | 775 kΩ | ≈ 770 kΩ | Matches V3 behavior |

**Setup:**  
NanoVNA Port 1 → Receiver input (antenna port)  
No transformer.  

**Observation:**  
These measurements confirm the impedance evolution between MiniATS generations.  
The **9:1 Unun** provides proper matching for the **V3/V3S** (~50 → 450 Ω), while older versions (V1/V2) would require higher ratios (e.g. 1:144).

---

## 📊 Measurement Summary

Below are the NanoVNA measurements taken for comparison between the raw antenna, the transformer (Unun 9:1) connected to the antenna, and the transformer with a 50 Ω dummy load.

| Test | Description | Connection | Observations |
|------|--------------|-------------|---------------|
| **Antenna** | Direct measurement of the antenna’s impedance | NanoVNA Port 1 → Antenna | High impedance (~8–9 kΩ @ 6.88 MHz). |
| **Transformer** | Measurement with Unun 9:1 connected between antenna (50 Ω side) and NanoVNA (450 Ω side) | NanoVNA Port 1 → 450 Ω side → Unun 9:1 → Antenna (50 Ω) | Noticeable improvement in match — return loss improves to around −6 to −10 dB in HF band, confirming proper transformation. |
| **Transformer + 50 Ω Load** | Measurement of the Unun itself with a 50 Ω dummy load instead of an antenna | NanoVNA Port 1 → 450 Ω side → Unun 9:1 → Dummy Load 50 Ω | Clean response with return loss near −8 dB and low insertion loss (≈ 0.8 dB), showing efficient 9:1 transformation. |

These measurements demonstrate that the Unun 9:1 provides a solid impedance transformation across the HF range (1–30 MHz), improving the receiver match while maintaining low losses.

---

## 🛠 Author Notes
Built by Antonis Maglaras. Tests with NanoVNA-H4 (v4.3, running firmware v.1.2.44) for verification of 50 Ω ↔ 450 Ω matching.  
