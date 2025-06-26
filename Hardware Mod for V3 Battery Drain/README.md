# Detailed Documentation: AMNVOLT Mini SI4732 V3 Battery Drain Hardware Mod

This document presents a detailed analysis, rationale, and procedure for a hardware modification to the AMNVOLT Mini SI4732 V3 receiver. The goal is to eliminate unwanted battery drain observed while the device is powered off, which stems from a design issue in the Hi-Z antenna input buffer.

---

## 🧠 Background & Root Cause

The V3 version of the AMNVOLT Mini introduces a Hi-Z front-end input circuit intended to improve reception with active antennas. However, this feature introduces a flaw: the Hi-Z buffer stage receives power directly from the battery (VBAT), and **remains powered even when the main power switch is off**.

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

🔄 Update: Post-Mod Battery Drain Test (20h & 50h)

After charging the modified V3 device with a verified, known-good charger (same used with the V2), the battery voltage reached 4.17V according to the device’s internal display.

    20 hours later, the battery read 4.16V, indicating a very minimal drop of 0.01V.
    50 hours later, the battery still reads 4.16V, indicating that the first measurement was before the battery voltage was stabilized (it was measured after charging).

    This suggests that the mod is fully effective in eliminating the primary source of passive drain (i.e. Hi-Z buffer powered from VBAT).

    Measurements were based on the device’s internal ADC; external multimeter readings may follow later for confirmation.

This is a significant improvement compared to pre-mod behavior, where voltage dropped almost nothing in 50 hours.

More measurements (e.g., directly across the battery terminals) are planned.

---

## 🔁 Reversibility & Notes

- The mod does **not damage the PCB** or remove any parts.
- If needed, the transistor can be fully restored to its original function.
- The Kapton tape provides sufficient isolation.

This modification is a clean, effective way to correct a flaw in the AMNVOLT Mini V3 design, and it aligns the behavior of the device with what users expect from a powered-off state.

---

Additional photos, diagrams, and trace mapping may be added to this folder in future updates.

