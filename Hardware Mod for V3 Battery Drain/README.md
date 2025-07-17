# Detailed Documentation: AMNVOLT Mini SI4732 V3 Battery Drain Hardware Mod and Noise on FET Mod

This document presents a detailed analysis, rationale, and procedure for a hardware modification to the AMNVOLT Mini SI4732 V3 receiver. The goal is to eliminate unwanted battery drain observed while the device is powered off, which stems from a design issue in the Hi-Z antenna input buffer. Also (see update #3) after the mod (Update 1 & 2) noise problem detected and fixed.

Please note that everything written here are for the Amnvolt V3. The V3S has all these fixed.

---

## 🧠 Background & Root Cause

The V3 version of the AMNVOLT Mini ATS introduces a Hi-Z front-end input circuit intended to improve reception with active antennas. However, this feature introduces a flaw: the Hi-Z buffer stage receives power directly from the battery (VBAT), and **remains powered even when the main power switch is off**.

At the heart of this problem lies a transistor (labeled `K51G`) which switches or buffers power for the Hi-Z stage. Its **drain pin is connected directly to VBAT** (bottom right pin when looking the PCB with the SMA connector up/right), meaning the transistor (and by extension, the buffer circuit) continuously draws current, discharging the battery even when the device is powered off by the hardware switch.

This behavior **is not present in the older V2 model**, which does not include the Hi-Z input circuit.

---

## 🔬 Controlled Comparison: V2 vs V3

To validate this behavior, a comparison test was conducted:

- Both V2 and V3 units were fully charged to approximately 4.21V.
- After being powered off and left unused for 24 hours:
  - **V2 unit** retained full voltage: **4.21 V** (no drain).
  - **V3 unit** dropped to: **3.99 V** (significant drain).

This confirmed that the V3’s Hi-Z stage remains active even when the device is off.

---

## 🧪 Diagnostic Measurements

Measurements were made with a multimeter to track the source of the battery drain:

- The top-side capacitor near the Hi-Z section measured **4.1 V on one side**, even with the device off.
- The other side of the capacitor showed **0 V**, meaning it was downstream of a component.
- Continuity checks revealed:
  - The capacitor was directly connected to the **battery + terminal**.
  - The **drain pin of the K51G transistor** also had direct continuity to VBAT.
* Note: Some users make a connection between two capacitors and cut a trace. I measure continuity on these two capacitors, at least on my version. So take a look if you're trying this approach before cutting the PCB trace.

These results demonstrated that:
- **VBAT flowed into the Hi-Z stage at all times**, not controlled by the power switch.
- **No simple capacitor mod** would solve this, as power came from the upstream VBAT net.

---

## 🛠️ Selected Fix: Drain Pin Lift + Controlled Jumper

Several modding options were considered:

- ❌ Cutting traces → irreversible and risk-prone
- ❌ Disabling Hi-Z entirely → undesired loss of feature
- ✅ Lifting the transistor drain pin and rerouting it to a switched power source → non-destructive, reversible

### Mod Procedure

1. **Locate the `K51G` transistor** near the Hi-Z circuit.
2. **Heat and partially lift the drain pin** (typically lower-right pin). I didn't use hot gun air, as I was afraid of the 3d-printed enclosre. So, I took the more difficult way.
3. **Isolate the pad** beneath it using a square of **Kapton tape**.
4. **Confirm disconnection**: no more continuity between the pad and battery +.
5. **Identify a point on the board** that only becomes powered when the power switch is ON. (There are many points that you can get + voltage. I took the easiest way, to connect it to the ON/OFF switch.)
6. **Solder a thin jumper wire** from that ON-only source to the lifted drain pin.
7. **Check continuity** and functionality: device must boot and receive normally.

Photos of each step were taken and are available in this folder.

---

## 📉 Outcome

- Measured **0 V at the Hi-Z buffer drain pad** when device is off.
- Verified that the transistor now receives power **only when the device is switched on**.
- Battery holds charge across multiple days without unexpected loss.
- Device performance (e.g. FT8 decoding) unaffected.

---

## 🔄 Update `: Post-Mod Battery Drain Test (20h)

After charging the modified V3 device with a verified, known-good charger (same used with the V2), the battery voltage reached 4.17V according to the device’s internal display.

    20 hours later, the battery read 4.16V, indicating a very minimal drop of 0.01V.
    
    This suggests that the mod is fully effective in eliminating the primary source of passive drain (i.e. Hi-Z buffer powered from VBAT).

    Measurements were based on the device’s internal ADC; external multimeter readings may follow later for confirmation.

This is a significant improvement compared to pre-mod behavior, where voltage dropped 0.01V in 20 hours.

---

## 🔄 Update 2: 50h Standby Test — No Drain

A follow-up check was performed after **50 hours powered off**. The device still reported **4.16 V**, confirming that:

- The previous 0.01 V drop was likely a post-charge stabilization effect.
- No actual leakage current is present.
- The battery retains full voltage even after 2+ days of standby.

This supports the conclusion that the mod **fully eliminates the parasitic drain** that existed in stock V3 units.

---

## 🔁 Reversibility & Notes

- The mod does **not damage the PCB** or remove any parts.
- If needed, the transistor can be fully restored to its original function.
- The Kapton tape provides sufficient isolation.

This modification is a clean, effective way to correct a flaw in the AMNVOLT Mini V3 design, and it aligns the behavior of the device with what users expect from a powered-off state.

---

Additional photos, diagrams, and trace mapping may be added to this folder in future updates.

## 🔁 Update 3: Noise-Informed Refinement — Supply From SI4732 Pin 10

After the successful battery drain fix (see previous mod), the K51G transistor responsible for powering the Hi-Z buffer was being supplied directly from the battery-side switch, using a clean path without cutting PCB traces.

However, Peter Neufeld finds that there are many birdies and spurious signals comming from not clean power line. They are all well documented on his blog: https://peterneufeld.wordpress.com/2025/06/13/si4732a-minirx-modifications

During follow-up testing with an oscilloscope (Siglent SDS1202X-E), small but noticeable ripple and noise were observed on the supply rail — even when drawing power directly from the battery. The FFT revealed energy peaks around 250 kHz, likely due to the boost converter’s switching noise or internal interference, still present even when I turned off (via software/long press on the knob) of the display.

## 🛠️ Mod Update

To address this, the supply line to the Hi-Z buffer’s FET (K51G) was moved from the battery switch to Pin 10 of the SI4732, which provides a regulated and clean 3.3 V rail (Vcc_Radio).
This pin is active only when the radio is ON (so no battery drain).

It is near the buffer and free from switching noise.
The rerouting was easy: simply de-solder the wire from the previous supply line and reattach it to SI4732 pin 10 — no board trace cutting required. So, the previous mod was a useful one, no cuts of traces were needed. 

## 🔬 Verified with Oscilloscope

Measurements before and after the change showed:
Ripple at ~250 kHz was clearly visible before.
After rerouting to Pin 10, the rail was flat and noise-free.

When the radio was OFF, 0 V confirmed isolation and no drain.

FFT showed no more harmonics or interference peaks in the 0–500 kHz range.

## ✅ Result

The Hi-Z buffer is now powered cleanly and only when the radio is ON.The overall noise floor is improved, reception is slightly cleaner, and the mod remains fully reversible and minimal.
