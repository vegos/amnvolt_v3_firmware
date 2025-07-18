
# Mini ATS V2 vs V3 (AMNVOLT) – Audio and SNR Evaluation

## Test Setup

A signal generator (OWON DGE1060) was used to transmit a 440Hz tone at 15000KHz to both Mini ATS receivers (V2 and V3). The output from each receiver was connected to a sound card, and the input gain was carefully adjusted so that both signals matched in amplitude.

After that, I recorded two sets of audio from each receiver:
1. **Noise Floor** – with no antenna connected at 15000KHz.
2. **Tone Input** – the 440Hz tone from the signal generator at the same frequency.

To eliminate noticeable 50Hz hum on the V2, the signal generator’s ground had to be connected to the line-in ground of the sound card. The V3 didn’t exhibit any hum.

---

## Audio Signal Comparison

Measured RMS audio levels:

- **V3 Noise RMS**: -28.27 dB  
- **V2 Noise RMS**: -31.93 dB

V3 appears to have a higher RMS noise level at the output. However, examining the spectrum and waveform data gives deeper insights.

### Frequency Spectra

- **[V2 Noise Spectrum](v2.png)**  
- **[V3 Noise Spectrum](v3.png)**  
- **[V2 Tone Spectrum](V2 Tone.png)**  
- **[V3 Tone Spectrum](V3 Tone.png)**

From the tone spectra, we observe that the V3 output has less low-frequency ripple and better harmonic suppression around 440Hz.

---

## Audio Waveforms

See the combined waveform view:  
**[Audio Overview](Audio Details.png)**

This illustrates clearly that the output of V3 has slightly more background noise but a better-defined tone with less visible distortion.

---

## Real-World SNR Performance

A practical comparison was conducted by decoding FT8 signals over time from both receivers.

**[SNR Graph – V2 vs V3](FT8 Signals Decoding with V2 and V3.png)**

- **Red plot**: V3 (before 15:30)  
- **Blue plot**: V2 (after 15:30)

### Observations

- V2 consistently achieved higher SNR values, including several positive peaks, indicating stronger signal decoding.
- V3 had more negative and lower SNR values overall, showing weaker reception under identical conditions.

The difference in SNR suggests that V2 has a better dynamic range and overall superior signal decoding performance.


---

## Conclusion

Based on both controlled audio measurements and real-world FT8 signal reception:

- **V2 exhibits a lower output noise level** compared to V3, as shown by RMS and frequency analysis in Audacity.
- It also delivers a **cleaner audio signal** with fewer harmonics and lower noise floor in the entire spectrum.
- In real RF conditions, V2 achieved **consistently higher SNR readings** and **better FT8 decoding success**.
- These results suggest that **V2 has better real-world dynamic range and sensitivity**, despite its simpler or older hardware revision.

In contrast, **V3 has a slightly higher noise floor and more distortion**, which may impact weak signal performance under certain conditions.

### Overall, V2 outperforms V3 both in laboratory noise tests and actual RF decoding scenarios.
