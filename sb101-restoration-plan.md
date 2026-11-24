---
title: "Heathkit SB-101 Transceiver — Restoration Plan"
subtitle: "Rev. 1 — field sources and service bulletins, checked against the original Heath manual"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
linkcolor: black
urlcolor: black
citecolor: black
toccolor: black
toc: true
---

<!-- Build: pandoc sb101-restoration-plan.md -o sb101-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety (manual p. 92).** "Lethal voltages are present both above and below the chassis." The HP-23 series and PS-23 supplies deliver about **700–850 V** to the finals (the meter reads **+800 to +900 V** on HV at initial test), and that charge stays on the supply filter capacitors after unplugging. Connect a lead from a good earth ground to the **ground bolt on the rear of the chassis and to all test equipment** (p. 92). Discharge with a grounded bleeder stick before every hands-in session; never run with the final cage open; work one-handed when live. Use a **50 Ω non-reactive dummy load rated for 100 W, never light bulbs** (p. 93). **Never pull a tube or pilot lamp with power on.** The series-parallel filament string can take out other tubes (p. 112). The transmitter voltage chart warns the **fuse may blow if the transmitter runs full TUNE output for more than 1 minute** (Fig. 1-13B).

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath SB-101 Assembly Manual**, 595-840 (©1966, printed 2-9-68) | Owner's copy (scan); W5RKL (w5rkl.com/manuals) | **Primary reference.** Page numbers below are the manual's printed pages. |
| **SB-101 Service Bulletins** -1D … -5D (1967–68) and -1 … -28 (1970–80) | Heath list (puck.nether.net), Apr 1998 transcription | Factory fixes. Four bulletins (-11, -26, -27, -28) have no text in the transcription. |
| W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting* | w5rkl.com | Family field practice. |
| K5BCQ SB-101/102 repair site | qsl.net/k5bcq | Leakage limits, IF centering, dial mechanics. |
| W2PA SB-101 rebuild notes | sites.google.com/site/radiow2pa | LMO connector, bandswitch rotors, driver neutralizing. |
| Heath list threads on SB-101 LMOs, drift and restoration | puck.nether.net | Field experience (cited individually below). |
| Heathkit-Radios / heathkit / HeathKit-HFAmps (groups.io) | groups.io | Field experience. **Topic pages would not fetch for this revision**; listed as places to search. |
| **HP-23A AC Power Supply assembly manual** 595-924-01 (©1968) | Supplied by owner | §3.6.1, carried over from the SB-102 plan. |
| **PS-23 AC Power Supply manual** 595-2314 (©1979) | Supplied by owner | §3.6.2, carried over from the SB-102 plan. |
| Companion plan: **heathkit-hp23-ps23-restoration** | github.com/usa4148 | Full supply rebuild procedure. |

**Designator groups (p. 125):**

| Range | Board |
|---|---|
| 0–99 | Modulator |
| 100–199 | IF |
| 200–299 | Bandpass |
| 300–399 | Audio |
| 400–499 | RF-driver |
| 500–599 | Crystal |
| 600–699 | Heterodyne oscillator |
| 700–799 | Driver grid |
| 800–899 | Driver plate |
| 900–999 | Chassis |

Resistors are 1/2 W unless marked (p. 5). Voltage charts were taken with an 11 MΩ input VTVM; readings may vary ±10% (p. 112). The manual is a scan without a text layer; values from the small fold-out charts are marked *approx.* where the print is hard to read.

# 2. Transceiver at a glance

## 2.1 Factory specifications (pp. 121–124)

| Item | Spec |
|---|---|
| Coverage | 3.5–4.0, 7.0–7.3, 14.0–14.5, 21.0–21.5, 28.0–30.0 MHz (eight 500 kHz segments) |
| Sensitivity | **< 0.5 µV** for 10 dB (S+N)/N, SSB |
| Selectivity | SSB 2.1 kHz @ 6 dB / 5 kHz @ 60 dB (3.395 MHz filter). CW 400 Hz / 2.0 kHz with optional **SBA-301-2**. |
| Spurious | Image and IF rejection > 50 dB; internal spurs below 1 µV equivalent |
| Audio | 2 W, < 10% distortion; 8 Ω speaker and high-Z phones; 350–2450 Hz |
| Input / output | 180 W PEP SSB, 170 W CW (50% duty) input; **100 W out on 80–15 m, 80 W on 10 m** into 50 Ω |
| Output impedance | 50–75 Ω, < 2:1 SWR |
| Suppression | Carrier 50 dB; unwanted sideband 55 dB @ 1 kHz; feedthrough/mixer products 55 dB; harmonics 45 dB; 3rd-order IMD 30 dB |
| ALC | Heath **TALC** (Triple Action Level Control), ≥ 10 dB compression at 0.1 mA grid current |
| CW | Grid-block keying, VOX from a keyed 1000 Hz tone; sidetone to speaker or phones |
| Stability | < 100 Hz/hr after **20 min** warm-up; < 100 Hz for ±10% line |
| Dial | Visual within 200 Hz; **electrical within 400 Hz after calibration at the nearest 100 kHz point**; backlash < 50 Hz |
| **Power required** | **700–850 V @ 250 mA** (1% ripple); **300 V @ 150 mA** (0.05%); **−115 V @ 10 mA** (0.5%); **12 V AC/DC @ 4.76 A** |
| Size / weight | 14-7/8 × 6-5/8 × 13-3/8 in.; 17.5 lb |
| Built | 1967–1970; $370 kit, $540 wired (SBW-101) (RigPix) |

**Frequency scheme (Figs. 2-2, 2-21, pp. 125, 140):** LMO 5.0–5.5 MHz; first IF (bandpass filter T202) 8.395–8.895 MHz; second IF 3.395 MHz; heterodyne crystal = band + 8.895 MHz (12.395, 15.895, 22.895, 29.895, 36.895, 37.395, 37.895, 38.395 MHz). Carrier crystals: Y1 3396.4 kHz (USB), Y2 3393.6 kHz (LSB), Y3 3395.4 kHz (CW).

## 2.2 Tube complement (spec p. 123; V numbers from the circuit description)

| V | Tube | Function | Known issues (source) |
|----|----------|--------------------|----------------------------------------------|
| V1 | 6EA8 | Speech amp (A) / cathode follower (B) | **GE 6EA8 oscillates → relay chatter in VOX** (SB-101-19). Defective → no SSB (chart 28G). |
| V2 | 6AU6 | Isolation amp | No output → chart 27D. |
| V3 | 6AU6 | 1st IF amp (also the S-meter/ALC bridge) | **Brand-critical:** Sylvania, RCA and Westinghouse fail here; El-Menco, IEC, GE and Realistic work; never Westinghouse (SB-101-24). Leaky → S-meter drift (SB-101-22; chart 18E). |
| V4 | 6AU6 | 2nd IF amp | Same as V3. |
| V5 | 6EA8 | 1st transmit mixer (A) / crystal oscillator (B) | No LMO signal at pin 7 → chart 25C. |
| V6 | 6CB6 | 2nd transmit mixer | Chart 24D. |
| V7 | 6CL6 | Driver | Chart 22F. |
| V8, V9 | 6146 | Finals, class AB1, parallel | **Gassy → grid drive falls off** (chart 40D). |
| V10 | **6AU6** | Receiver RF amp | **Manual says 6AU6** (spec p. 123; Fig. 2-22). SB-101-14 names a **6HS6** (411-247) here; see §11. Leaky → poor AGC, fast S-meter decay, low sensitivity (SB-101-14). |
| V11 | **6AU6** | 1st receive mixer | Same leakage symptoms (SB-101-14, -22). Heterodyne frequency is measured at **pin 7** (p. 98). |
| V12 | 6EA8 | 2nd receive mixer (A) / relay amp (B) | **IEC 6EA8 at V12 → relays stay in after PTT** (> 10 V at pin 9 after 30–40 s; SB-101-13). Bad V12B → relays won't energize (chart 36A). |
| V13 | 6BN8 | AVC (A/B) / product detector (C) | Faulty → hum only (chart 13C). |
| V14 | 6GW8 | Audio amp (A) / audio power amp (B) | C304 is its B+ filter (chart 10D/E). |
| V15 | 6EA8 | CW tone osc (A) / tone amp (B) | 1000 Hz sidetone and CW VOX (p. 137). |
| V16 | 12AU7 | Carrier oscillators: A = USB (Y1); B = LSB (Y2) / CW (Y3) | Plate resistors R6/R7 33 kΩ (Fig. 2-7). Chart 30E, 31F. |
| V17 | 12AT7 | VOX amp (A) / 100 kHz calibrator (B) | V17A defective → VOX won't trip (chart 36C, p. 120). |
| V18 | OA2 | +150 V regulator | Bad → no regulated B+ (chart 5B). |
| V19 | 12AT7 | Heterodyne osc (A) / cathode follower (B) | **Wrong type → S-meter faults** (chart 18H). Repeated failures → R212 220 → 330 Ω (SB-101-2). |
| V20 | **6CB6** | **LMO oscillator (inside the LMO)** | **Replace with a 6CB6 of the same manufacturer**; a change shifts calibration (p. 112; chart 33D). |

**Twenty tubes** in all (p. 2). The parts list (p. 24) supplies only 19 because the LMO's 6CB6 comes installed in the LMO; it lists one 6CB6 and the spec list two.

## 2.3 Semiconductors (spec p. 123; parts lists pp. 4, 23)

The manual counts **16 diodes**: 6 germanium, 9 silicon, 1 zener (p. 123). There are no transistors.

| Ref | Type (Heath p/n) | Function | Notes |
|------|------------------|----------------------|------------------------------------|
| CR1–CR4 | 1N191 Ge (56-26-1) | Balanced modulator ring | Wrong type, reversed or failed → no SSB, CW/TUNE OK (chart 28C). SB-101-27 is a germanium diode change (text not available). |
| CR201 | 1N191 Ge | Calibrator harmonic generator | Fig. 2-28. |
| CR901 | 1N191 Ge | REL PWR sampling | Fig. 2-29. |
| D1, D2 | Silicon 750 mA 500 PIV (57-27) | Anti-trip rectifiers | Reversed → relays stay in (chart 37B). |
| D101 | 57-27 | AVC diode gate | **Defective → slow recovery from transmit** (chart 39). Leaky → same (SB-101-4). |
| D201 | 57-27 | VOX rectifier | Leaky → low VOX sensitivity, long delay (SB-101-4); wrong or reversed → no VOX (chart 36D). |
| **D202** | **15 V 1 W zener, 1N4166A** (56-25) | Holds relay amp V12B cut off in receive | Reversed or bad → relays won't trip (chart 36E). |
| D301 | 57-27 | Transmit mixer/driver cutoff bias | Fig. 2-14. |
| D902, D903 | 57-27 | **ALC voltage doubler** | Leaky → **ALC falls off after about an hour** (SB-101-4). |
| D904 | 57-27 | CW keying, cutoff bias to V5A/V6/V7 | p. 138. |
| D905 | 57-27 | RF-gain/AVC diode gate | Leaky → slow recovery and false meter readings (SB-101-4). |

## 2.4 LMO (pp. 112, 119, 132–133, 139)

- **LMO 110-40** (chassis parts list p. 25) is a sealed, factory-calibrated tube oscillator using a **6CB6 (V20)**. It tunes **5.0–5.5 MHz**, five turns of the main knob per segment; the dial is a **#100-450 dial drive** with three rubber belts (268-7), a nylon spiral follower and a zero-set pulley (p. 24).
- **B+:** regulated **+150 V** from the OA2 (V18), routed through **relay RL2 section 6 and the FREQ CONTROL switch** to either the LMO or crystal oscillator V5B, decoupled by C941 (.005 µF) (p. 133, 139). There is **no lamp, zener or transistor in the LMO supply**; those belong to the SB-102.
- **Output** goes through the FREQ CONTROL switch to V5A (transmit) and V12A (receive). **R928 (56 Ω)** terminates the line (Fig. 2-13).
  - LOCKED NORMAL: LMO to both mixers.
  - LOCKED AUX: crystal Y5 (V5B) to both.
  - UNLOCKED AUX: Y5 transmits, LMO receives.
- **Mode shift:** the LMO frequency moves with the MODE switch so the calibration stays put between USB and LSB. The **frequency shifter adjustment on the LMO** is set in alignment (p. 97). **R306 or R307 wrong value, or mode wafer 1F miswired → LMO won't shift** (chart 34).
- **Manual warnings (p. 112):** "At no time should the LMO be opened or the Warranty will be voided. The LMO was aligned using the tube supplied. Changing this tube may cause some slight difference in calibration. Replace the tube with one of the same brand if replacement is necessary."
- **Chart 33, no LMO injection at V5 or V12 cathodes:**
  - A. B+ (items 5A–5C);
  - B. FREQ CONTROL switch position or wiring;
  - C. violet, orange or white coax at the FREQ CONTROL switch, or the LMO coax;
  - D. **LMO tube V20 defective** (same-manufacturer replacement);
  - E. defective LMO unit: the manual says to return it to Heath.
- **[field]** The SB-301 receiver and SB-401 transmitter are reported to use the same LMO part number 110-40 (6CB6 in the SB-301 version, 6BZ6 in the SB-401 version; Heath list, W9DZ, 2009). Check the tube and calibration before swapping units.

## 2.5 Pilot lamps and supply

- **Lamps (parts list p. 24):** **three #44** (412-14). They are part of a series-parallel filament arrangement; wrong lamps unbalance it (chart 4). Panel lamps should light with equal brilliance (p. 92). *(The SB-102 uses #47/#1815 lamps; don't carry those over.)*
- **Filament voltages** on the voltage chart: brown leads 6.3 V, brown-white leads 12.6 V AC/DC (Fig. 1-13A note 9). Large brown-white harness wires open → B+ and bias OK but no filaments (chart 3).
- **Supply (pp. 90–91):** HP-23 (117 VAC) or HP-13 (12 VDC). The SB-102-era HP-23A and the later PS-23 are drop-in equivalents (§3.6).
  - **HP-23 series: the low-voltage DC circuit must be connected for +300 V output** (p. 91). On an HP-23A that is the AC OFF switch **right (350 VDC no-load)**; on a PS-23 it is the **350 VDC internal wiring** (§3.6).
  - **"With the above connections, the BIAS control in the HP-23 Power Supply is inoperative. Proper bias settings are accomplished with the BIAS control in the Transceiver"** (p. 91).
  - HP-13: set the supply's BIAS control fully clockwise (p. 91). The mobile section (p. 111) then sets 50 mA with the HP-13's bias control so the transceiver's BIAS ADJUST needn't change between fixed and mobile use; see §11.
- **11-pin plug, transceiver end (Fig. 1-5, p. 91):**

| Pin | Function |
|---|---|
| 1 | Bias −130 VDC |
| 2 | Ground (filament) |
| 3 | LV +300 VDC |
| 4 | HV +800 VDC |
| 5, 11 | N.O. relay (to a linear amplifier's relay or the HA-14) |
| 6 | Filament 12 VAC |
| 7 | Ground (B− and bias) |
| 8 | No connection in the supply cable |
| 9, 10 | SW (120 VAC): the FUNCTION switch's AC snap switch |

- **Pins 5, 8 and 11 are a spare SPDT relay contact set** for an external antenna relay or amplifier, rated **3 A at 117 VAC or 30 V DC** (pp. 103, 138). **Leave pin 8 unconnected in the supply cable.** A cable made up for another rig (for example an SB-102 cable with a 6–8 mobile jumper) would put filament voltage on the relay contacts.
- **The HP-23 is switched by the SB-101's FUNCTION switch** through pins 9–10. Don't jumper 9–10 in the SB-101 cable; chart 1D lists a defective AC snap switch as a cause of no power.

# 3. What fails, ranked

## 3.1 Capacitors

**All four electrolytics** (circuit-board parts list p. 4; none on the chassis list p. 23). The parts lists give no voltage ratings; the ratings below are safe replacements.

| Ref | Value (p/n) | Role (manual figure) | Replace with |
|---|---|---|---|
| **C304** | 20 µF tubular (25-135), audio board | Audio output stage B+ filter, V14 (Fig. 2-27, p. 143) | 22 µF 450 V. **Shorted or reversed → no audio** (chart 10D). |
| **C12** | 20 µF tubular (25-135), modulator board | Speech-amp B+ decoupling from +300 V via R14 4.7 kΩ, V1B (Fig. 2-6, p. 128) | 22 µF 450 V |
| C2 | 10 µF (25-115), modulator board | V1A cathode bypass (Fig. 2-6) | 10 µF 50 V |
| C212 | 10 µF (25-115), bandpass board | V17A VOX-amp cathode bypass (Fig. 2-3, p. 126) | 10 µF 50 V |

**Other capacitors:**

| Part | Likelihood | Symptom (manual ref) | Action |
|---|---|---|---|
| **.2 µF resin (27-34), 8 in the kit**: C213 VOX delay, C25 anti-trip, C106 product detector, C932 ALC filter and others | Medium–high | **C213 drift → VOX delay changes** (SB-101-3); poor anti-trip; ALC time constant | Replace with film, 0.22 µF. |
| .05 µF tubular (23-59; C928 audio feedback), 1 µF tubular (27-19) | Medium | Leakage | Film replacements. |
| C1, C3, C9 (speech amp) | Low–medium | Low VOX or audio (SB-101-3) | Measure. |
| C4–C8, C16, C17 (carrier oscillator) | Low–medium | No carrier on LSB/USB (chart 31D) | Measure. |
| C208, C223, C604 (heterodyne osc) | Low–medium | No heterodyne injection (chart 35D, 35G) | Measure. |
| C111, C402 | Low | No first-mixer output (chart 25F) | Measure. |
| **C925** final tuning capacitor | — | Instability if not isolated from its tuning shaft (SB-101-5) | Check the insulation. |
| C14 carrier-null capacitor; C18 12 pF | — | **C14 won't null with its plates fully meshed toward V2** | Move C18 to the other section of C14 (SB-101-18). |
| Disc C924 (.02 µF) on early units | — | Driver oscillates on 40 m | Removed by SB-101-1D (1967). |
| IF transformer internal micas | Low–medium | Corroded → value change | Measure (K5BCQ). |

Leakage limits (K5BCQ): < 0.01 µF ≤ 0.5 µA; 0.01–1 µF ≤ 1 µA (aged). Electrolytics: I ≤ kC + 0.3 mA, with k = 0.02 (101–250 V) or 0.035 (251–350 V).

## 3.2 Resistors

Carbon-composition resistors in this family drift high with age, worst in the 100 kΩ-and-up values, and some go open (W5RKL; the measured SB-102 survey in the companion plan found 100 kΩ parts reading up to 851 kΩ). Replace what is out by more than 10%, and pre-emptively replace the hot ones.

| Ref | Value (manual figure) | Failure / symptom (manual chart) | Fix |
|---|---|---|---|
| **R106** | **22 kΩ 1 W**, V3 screen (Fig. 2-24) | Heats and drifts → S-meter zero wander (chart 18D) | 22 kΩ 2 W metal film or wirewound. |
| **R107** | **100 kΩ** meter circuit (Fig. 2-24) | Meter won't zero / drifts (chart 18D) | **SB-101-1** (1970): 82 kΩ. **SB-101-9** (1976): 100 kΩ **1 W**. Check which is fitted; see §11. |
| R103, R105, R110, R115 | 100 kΩ, 47 Ω, 330 Ω, 150 Ω (Fig. 2-29) | S-meter won't zero or shifts (chart 18D) | Measure. |
| **R6, R7** | **33 kΩ**, V16 plates (Fig. 2-7) | No LSB/USB carrier (chart 31E, which also lists R8, R9, R11) | 33 kΩ 1 W. |
| **R212** | **220 Ω** (Fig. 2-15) | Repeated heterodyne-oscillator tube failures | **330 Ω 1/2 W** (SB-101-2). |
| R117 | 3300 Ω (Fig. 2-25, AVC) | Open → fast AVC decay | Replace. |
| R304, R305 | (OA2 regulator) | No, high or low +150 V (chart 5C, 6B) | Measure. |
| R306, R307 | (LMO mode shift) | LMO won't shift with mode (chart 34C) | Measure. |
| R18, R19, R23, R24, R937, R938 | — | No isolation-amp output (chart 27B) | Measure. |
| R915 | 3.3 MΩ (ALC, Fig. 2-18) | Open → ALC upset | Replace. |
| R916 | 10 kΩ, final grid (Fig. 2-18) | Grid-meter shunt and bias feed | Measure. |
| R920 | 100 Ω, final screen | Burns on a screen fault | Replace (also the screen-disconnect point for SB-101-6). |
| R919, R917, R911, R910 | 10 Ω, 6146 cathodes (Fig. 2-18) | Plate-current meter reads wrong | Measure. |
| 7 W wirewounds | 1000 Ω and 2500 Ω (3-15-7, 3-16-7; p. 3) | Run hot | Inspect the board around them for charring. |

Precision **R921** is the HV meter multiplier (1 MΩ 1% in Fig. 2-29; the chassis list supplies two **500 kΩ 1%** resistors, 2-76). Replace only with 1% parts.

Lift one lead to measure, because parallel paths mislead.

## 3.3 Tubes, by risk

1. **6AU6 at V3/V4:** brand matters (SB-101-24); leakage drifts the S-meter (SB-101-22).
2. **V10/V11 RF amp and mixer:** grid leakage (SB-101-14, -22).
3. **6146 pair:** gassy tubes reduce grid drive (chart 40D).
4. **6EA8 (V1, V5, V12, V15):** brand-sensitive at V1 (GE oscillates) and V12 (IEC hangs the relays).
5. **12AT7 (V17, V19).** V19 must be a 12AT7 (chart 18H).
6. **OA2 V18** and the **LMO's 6CB6 V20** (same brand only).

Use a tester with a **grid-leakage** test. Put each tube back in its original socket.

## 3.4 LMO

| Symptom | Cause | Fix (source) |
|---|---|---|
| **No LMO injection** | +150 V missing (OA2, RL2 section 6), FREQ CONTROL switch or coax, **V20 6CB6** (chart 33) | Check +150 V at the LMO; try a same-brand 6CB6. The manual's last resort is factory return. |
| **Drift / intermittent jumps** | **Dirty or corroded phono connector on the rear of the LMO** (SB-101-16) | Clean plug and jack with DeoxIT; re-solder the shield (W2PA). |
| **Erratic tuning, early LMOs** | Dirty tuning-capacitor wipers; a fiber washer between the cap frame and worm gear (SB-101-12) | Clean the wipers; replace the fiber washer with metal. Requires opening the LMO. |
| **Knob turns only about 3 of 5 turns** | Turn-limiting stop washers stuck together after long storage (Heath list, AE5P, 2007) | Spin the knob briskly into the false stop to free them, or disassemble and separate. |
| Drift that tracks mode changes or wanders during a QSO | LMO mode-shift voltage not stable (K9STH, Heath list 2005). 10–20 Hz is normal. | Check R306/R307 and the mode wafer. **[optional]** K9STH zener stabilization of the shift supply (moves the dial about 1 kHz). |
| **Dial off by about 200 kHz after a rebuild** | LMO reinstalled rotated (5.0 MHz at the wrong end of shaft travel) (Heath list, 2016) | Counter on the LMO output: it should tune 5.5 → 5.0 MHz as the dial reads low → high. Re-orient LMO and dial. |
| **Dial off between 100 kHz points** | Spec is within **400 Hz after calibration at the nearest 100 kHz point** (p. 122). Tube-type LMOs are generally well within 1 kHz across 500 kHz (Heath list, 2009). | Recalibrate at the nearest point; larger errors mean LMO service. |
| Zero setting changes band to band | **Calibrator not exactly on 100 kHz** (chart 41) | Re-zero the CAL XTAL trimmer against WWV. |
| Stiff tuning, slipping knob | Dial drive, three belts 268-7 | Clean; renew belts; light grease (the kit supplied silicone grease 352-13). |

**[field]** Restorers open these LMOs to clean and re-grease the drive and to add or renew a braid wiper on the rotor (Glen Zook's LMO rebuild notes are often cited on the Heath list). That voids nothing today, but it can shift calibration. Do it only for a mechanical fault, and keep the 6CB6 that came with it.

## 3.5 Mechanical, switches, relays

| Item | Symptom | Fix (source) |
|---|---|---|
| **Switch-wafer rotors 180° out** | Dead bands, no injection, no final output | Chart 21G (driver plate board), 35I (crystal/heterodyne boards); W2PA found several wrong in kit-built units. |
| **SSB/CW FILTER switch** | Left on CW without an SBA-301-2 → no RF, unintelligible receive. Dirty → low TX and RX. | Chart 14L, 23A, 26B; clean the slide switch (SB-101-25). |
| **FREQ CONTROL switch** | Wrong position → no LMO or no output | Use **LOCKED NORMAL** for normal operation (chart 23B, 25A, 33B). |
| **RL1, RL2** (4PDT plug-in, 69-35) | Chirp and slow recovery from carbon at the base of RL2 (SB-101-21); many chart entries (8, 10G/H, 19, 20, 21C) | Clean or replace. The manual calls them "sealed relays … troublefree" (p. 112); the bulletin says to remove the RL2 cover; see §11. |
| **Board mounting hardware and lockwashers** | Receiver birdies or oscillation; transmitter instability, especially 15/10 m | Lockwashers between board and chassis, tight (chart 16A, 38C; p. 112). **Steel comb brackets 204-2096** with the switch shields and driver boards soldered to them (SB-101-7, -8). |
| Loose rivets at rear phono sockets and final sockets | Grounding faults | Ground bus for the rear phono sockets; replace final-socket rivets with 6-32 hardware (SB-101-5D). |
| Early driver/mixer boards (85-131-1) and 26-109 capacitors | Improper tuning, 40 m trouble | Replaced by 85-131-2 and 27-122 (SB-101-3D, -4D). The kit manual already ships 85-131-2. |
| Slide-switch actuators, detent washers | Poor switch action | SB-101-2D parts. |
| Final/loading cap hardware, coil cover, tube-shield clips | Transmit instability | Chart 38B, G, H. |
| Preselector drive | Poor tracking, mainly 10 m | Drive belt, bearing lube, capacitor 26-122 (SB-101-17). |
| Loading cap turns with plate | Friction | Free and lubricate the shafts; grommet 73-3 on the loading shaft (SB-101-20). |
| Final knobs or shafts loose | No RF | Chart 21H. |

## 3.6 HP-23 power supply (do first)

The full rebuild is in the companion plan, [heathkit-hp23-ps23-restoration](https://github.com/usa4148/heathkit-hp23-ps23-restoration). In brief:

- Recap the high-voltage section (C1–C4 → 150 µF 500 V).
- Recap C5 (40 µF B+). An open C5 causes a CW tone and relay chatter (HW-101-73).
- Recap the bias caps C6/C7 (**20 µF in the HP-23A**, 40 µF in the PS-23).
- Replace the bleeders (100 kΩ 2 W) and diodes (1N4007).
- Fit a grounded cord.
- **+300 V B+ connection** (HP-23A switch right; PS-23 350 VDC wiring).
- Must meet the SB-101 spec: **700–850 V @ 250 mA, 300 V @ 150 mA, −115 V @ 10 mA, 12 V @ 4.76 A**.
- Reform or ramp on a variac over 5–10 min.

### 3.6.1 HP-23A supply (checked against the HP-23A manual, 595-924-01, ©1968)

Page numbers are the HP-23A manual's printed pages.

**How the HP-23A works:**

- **The front-panel AC OFF switch is also the B+ selector** (DPDT, 61-12). **Right = 350 VDC** (high tap), **left = 275 VDC** (low tap), **center = OFF** (p. 14). Plate and filament voltages switch together.
  - The field "300/OFF/250" labels are the **loaded** values of the same two taps.
  - With remote switching wired (pins 9–10 to the SB-101's FUNCTION switch), the panel switch still selects B+, and the SB-101 turns the supply on and off.
  - The switch is easy to bump into OFF, or through it to the other tap.
- **Two bias outputs** (pp. 13, 16, 17):
  - **fixed −130 V** (−100 V at 20 mA) on **pin 1**;
  - **adjustable −40 to −80 V** (1 mA max) on **pin 11**, set by BIAS ADJUST R9.

  When the cable is made up, its green wire goes to pin 1 *or* pin 11 (p. 12). The SB-101 takes the **fixed −130 V on pin 1** and sets final bias with its own BIAS ADJUST. **That is why the SB-101 manual calls the supply's BIAS control inoperative (p. 91): R9 only feeds pin 11.**
- **Filaments:** 12.6 VAC @ 5.5 A on **pin 6**, or 6.3 VAC @ 11 A on pin 8. The cable's red wire goes to one or the other (p. 12). **The SB-101 uses pin 6 (12.6 V).** Don't wire the 6.3 V option: pin 8 is a relay contact in the SB-101 (§2.5).
- **Primary protection is a circuit breaker** (65-17; the manual doesn't give its rating), not a fuse. Line: 120/240 VAC, 50/60 Hz, 350 W max (p. 16).
- The line cord (89-19) is listed without a conductor count. **[field]** Fit an 18/3 grounded cord.

**SB-101: switch RIGHT (350 VDC).** The SB-101 needs **300 V @ 150 mA** (spec p. 124), the HP-23A's high tap under load. Its initial test expects 800–900 V on HV (p. 92), matching the HP-23A's 820 V no-load figure. Make up the cable per **Fig. 1-5 (p. 91)**: green on pin 1, red on pin 6, nothing on pin 8, brown and blue to pins 9–10.

**Specifications (p. 16):**

| Output | No load | Loaded | Ripple / duty |
|----------------------|----------|----------------------------------|------------------------------------|
| HV (pin 4) | 820 V | 700 V @ 250 mA (62.5 µF effective) | < 1% @ 250 mA; continuous to 150 mA, 50% @ 300 mA |
| LV high tap, switch **right** (pin 3) | 350 V | 300 V @ 150 mA (with 100 mA on HV) | < 0.05%; continuous to 175 mA |
| LV low tap, switch **left** (pin 3) | 275 V | 250 V @ 100 mA (with 100 mA on HV) | < 0.05%; continuous to 175 mA |
| Fixed bias (pin 1) | −130 V | −100 V @ 20 mA | — |
| Adjustable bias (pin 11) | −40 to −80 V | 1 mA max | — |
| Filament (pin 6 / pin 8) | — | 12.6 VAC @ 5.5 A / 6.3 VAC @ 11 A | — |

**Supply-end socket pinout (schematic; cable wiring p. 12; Table 1 p. 13):**

| Pin | Cable wire | Function |
|----|-----------|--------------------------------------------------------------|
| 1 | Green (fixed option) | −130 V fixed bias |
| 2 | Black | Filament common |
| 3 | Orange | LV +350 V or +275 V (no load), per the switch |
| 4 | Yellow | HV +820 V (no load) |
| 5 | — | No connection |
| 6 | Red (12 V option) | 12.6 VAC filament |
| 7 | White | Ground |
| 8 | Red (6 V option) | 6.3 VAC filament |
| 9, 10 | Brown, blue | AC switch loop in the equipment (the SB-101 FUNCTION switch) |
| 11 | Green (adjustable option) | −40 to −80 V adjustable bias |

**Recap list, with the HP-23A manual's values (parts list p. 3; price list p. 19; schematic; circuit description p. 17):**

| Ref | Manual value (Heath p/n) | Function | Notes |
|------------|----------------------|------------------------------|----------------------------------------|
| C1, C2 | 125 µF tab-mount (25-34) | HV full-wave voltage doubler, from about 282 VAC | Replace together. Rating not in the parts list; **[field]** 500 V, or 150 µF 500 V. |
| C3, C4 | 125 µF tab-mount (25-34) | LV half-wave doubler; C4 is also the pi-filter input | Same as C1/C2. |
| C5 | 40 µF (25-36) | LV pi-filter output, with choke **L1 6 H** (46-22) | **[field]** 450 V. Open C5 → CW tone and relay chatter (HW-101-73). |
| C6, C7 | **20 µF** (25-19) | Bias filter, with R7 | **Negative supply: the + lead goes to ground.** **[field]** 160 V+. *(PS-23: 40 µF.)* |
| R1–R4 | 100 kΩ 2 W (1-24-2) | HV bleeders / equalizers | Drift high (field: ~150 kΩ). |
| R5 | 100 kΩ 2 W | LV bleeder | — |
| R7 | 1 kΩ 2 W (1-15-2) | Bias filter series | Chart: no bias → D7, R7, R8 or R9. |
| R8, R10 | 10 kΩ 1 W (1-9-1) | Limit the BIAS ADJUST range | — |
| R9 | 10 kΩ tab-mount control (10-57) | BIAS ADJUST (pin 11 only) | Not used by the SB-101. |
| D1–D7 | Silicon diode (57-27) | D1–D4 HV doubler; D5, D6 LV; D7 bias | **[field]** 1N4007. |
| Breaker | 65-17 | Primary | Opens on reset → short in the transformer, rectifier/filter or load. |

**Initial test (pp. 12–13)**, before the first power-on:

1. Ohmmeter on R × 1: find the lead polarity (about **10 Ω** between strip **J** lugs 3 (−) and 4 (+)). Put the correct lead on socket **pin 7**.
2. On R × 10k expect: **pin 1 ≥ 22 kΩ**; **pins 3 and 4 ≥ 75 kΩ** (rising as the caps charge); **pin 7 = 0 Ω**; **pin 11 = 10–20 kΩ**, varying with BIAS ADJUST; pins 2, 5, 6, 8, 9 and 10 infinite.

**Troubleshooting chart (p. 15):**

| Difficulty | Possible cause |
|---------------------------|------------------------------------------------------------|
| Completely dead | Breaker, switch, pin 9/10 wiring, **the SB-101's FUNCTION switch**, transformer |
| Breaker opens when reset | Short in transformer windings, rectifier/filter, or the equipment; overload |
| No HV | D1–D4; pin 4 wiring; transformer |
| No LV | D5/D6 open; L1 open; pin 3 wiring; switch; diode or cap polarity reversed |
| No bias | D7 open; R7, R8 or R9 open; pin 1/11 wiring |
| HV half with no load, lower under load | D1, D2, D3 or D4 open |
| LV half | D5 or D6 open |
| All outputs half | Primary wired for 240 V on a 120 V line |

**Caution (p. 13).** Let the bleeders discharge the caps; a **bleeder stick (resistor to ground)** is gentler than the manual's screwdriver blade. Stand on dry insulating material, keep one hand behind your back, and **never work on the supply with the line cord plugged in.** The manual's voltages are no-load readings with an 11 MΩ VTVM (p. 14).

### 3.6.2 PS-23 supply (checked against the PS-23 manual, 595-2314, ©1979)

**How the PS-23 differs from the HP-23/HP-23A:**

- **No front-panel B+ switch.** The LV tap is an internal jumper: **350 VDC wiring** (Detail 5B, p. 10) or **275 VDC wiring** (Detail 5C, p. 11).
- **No bias control.** Fixed **−130 V** no-load (−100 V at 20 mA); bias is set on the transceiver.
- **A 2.92 A circuit breaker** (65-17). Primary current goes through the breaker and through pins 9–10, the AC switch in the equipment (p. 21).
- **3-wire grounded line cord** standard (p. 13).
- Captive 8-wire cable with an 11-pin plug; the far-end connector comes with the equipment (p. 17).

**SB-101: LV tap.** Needs **300 V @ 150 mA**, so the PS-23 must have the **350 VDC wiring** (Detail 5B). HV 820 V no-load matches the SB-101's 800–900 V initial-test reading.

**Specifications (p. 20):**

| Output | No load | Loaded | Duty |
|---|---|---|---|
| HV | 820 V | 700 V @ 250 mA (< 1% ripple) | Continuous to 150 mA; 50% at 300 mA |
| LV high tap (350 VDC wiring) | 350 V | 300 V @ 150 mA (with 100 mA on HV) | Continuous to 175 mA |
| LV low tap (275 VDC wiring) | 275 V | 250 V @ 100 mA (with 100 mA on HV) | Continuous to 175 mA |
| Fixed bias | −130 V | −100 V @ 20 mA | — |
| Filament | — | 12.6 VAC @ 5.5 A | — |
| Line | 120/240 VAC, 50/60 Hz, 350 W | | |

**Output pinout (Table 1, p. 17):** 1 green −130 V; 2 black filament common; 3 orange LV; 4 yellow HV; 6 red 12.6 VAC; 7 white ground; 9, 10 brown/blue AC switch loop; **5, 8, 11 no connection**.

**Recap list (parts list p. 3; schematic; p. 21):**

| Ref | Manual value | Function | Notes |
|---|---|---|---|
| C1–C4 | 125 µF 500 V (25-265 can) | HV doubler (C1, C2); LV doubler / pi input (C3, C4) | Field practice: 150 µF 500 V, replace in pairs. |
| C5 | 40 µF 450 V (25-36) | LV pi-filter output (with L1, 6 H) | Open → CW tone and relay chatter. |
| C6, C7 | 40 µF 150 V (25-20) | Bias filter | **+ lead to the ground lug** (p. 12). |
| R1–R5 | 100 kΩ 2 W | HV equalizers / LV bleeder | Metal film 2 W. |
| R7 | 1000 Ω 2 W | Bias filter series | — |
| R8 | 27 kΩ 1 W | Bias bleeder | Field: uprate to 2 W. |
| D1–D7 | 1N2071 (57-27) | Rectifiers | 1N4007. |
| Breaker | 2.92 A (65-17) | Primary | — |

**Initial test (p. 15).** Use a VTVM or VOM (a solid-state ohmmeter won't turn the diodes on): **pin 1 ≥ 22 kΩ; pins 3 and 4 ≥ 75 kΩ**, rising; **pin 7 = 0 Ω**; all others infinite.

**Troubleshooting shortcuts (p. 19):** HV half → D1–D4 or C1/C2; LV half → D5/D6; all outputs half → primary on 240 V; dead → breaker, the 9–10 loop, or the equipment's switch.

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

1. Photograph everything. Note mods, wrong-value parts and tube brands. Note whether R107 is 82 kΩ or 100 kΩ, and what tube type is in V10/V11.
2. Pull and label the tubes; test with a grid-leakage tester. Confirm V19 is a 12AT7. Don't disturb the LMO's 6CB6 unless it tests bad.
3. Check wafer rotor orientation.
4. Inspect C304, C12, the 1 W, 2 W and 7 W resistors, RL1/RL2, belts, the LMO coax and its rear phono connector.
5. Run the **resistance chart, Fig. 1-14 (fold-out from p. 113)**: power plug disconnected, all controls and switches fully counterclockwise, measured from chassis to the point indicated. Typical points (*approx.*, from the fold-out):

| Point | Expected |
|---|---|
| V8/V9 grid | about 100 kΩ |
| V7 | 110 kΩ / 200 kΩ / 95 kΩ on the main pins |
| V10 grid | about 400 kΩ |
| V11 grid | about 1 MΩ |
| V3, V4 grids | about 1.5 MΩ |
| V13 | about 10 MΩ on the AVC pin |
| V19 | 33 kΩ, 4700 Ω, 10 kΩ, 220 Ω; should not change with the bandswitch |
| V17 | varies with FUNCTION switch: OFF 200 kΩ, PTT 100 kΩ, VOX 200 kΩ, CAL 100 kΩ |

## 4.2 Phase 1: Power supply

Recap, test unloaded on a variac and wire the cable per **Fig. 1-5 (p. 91)**. On an HP-23A, set the switch **right (350 VDC)** and run its socket resistance check (§3.6.1). On a PS-23, confirm the **350 VDC wiring** and run its check (§3.6.2). Leave pin 8 empty.

## 4.3 Phase 2: Clean

DeoxIT all rotary wafers, pots, the **FILTER** slide switch, the **FREQ CONTROL** and MODE switches, the rear phono jacks and the LMO connector. Keep solvents off the dial plastic.

## 4.4 Phase 3: Recap and resistor survey

1. Replace all **4 electrolytics** (§3.1).
2. Replace the eight .2 µF resin caps and the tubular paper caps with film.
3. Resistor survey board by board (audio → modulator → IF → bandpass → RF-driver → chassis), with priority on §3.2. Fit R212 = 330 Ω and R107 = 100 kΩ 1 W (both bulletin changes).
4. Replace the silicon diodes the bulletins name (D101, D201, D902, D903, D905) with 1N4007s, observing polarity.
5. Clean RL2's contacts; replace either relay if the contacts are pitted.

## 4.5 Phase 4: Mechanical and LMO

1. Belts and dial drive; free the LMO turn stops if travel is short.
2. Steel comb brackets and lockwashers (SB-101-7/-8) if the unit still has aluminum brackets.
3. Clean the LMO phono jack; check the LMO mounting is tight.
4. Ground bus at the rear phono sockets and solid final-socket hardware (SB-101-5D).
5. Check C925 is isolated from its shaft.

## 4.6 Phase 5: Initial test (manual p. 92)

1. Controls: FINAL (round) 80; FINAL (lever) over the 50 Ω mark; MODE LSB; RF GAIN fully CW; FILTER SSB; all others fully CCW. **BIAS ADJUST (top of chassis) fully CCW.**
2. Connect the 8 Ω speaker; **never run without a speaker or phones**, and the manual advises against headphones during testing.
3. FUNCTION → PTT. Panel lamps light with **equal brilliance**.
4. METER → HV: **+800 to +900 V**.
5. Check for overheating parts; confirm every filament glows.
6. METER → PLATE: **zero**.
7. AF GAIN up: receiver noise is heard (if not, check for a stuck PTT or closed key).
8. Check **+300 V** at bandpass board point 5 and **−111 V** bias at audio board point 4 (Fig. 1-2 locations).
9. Compare with the **receiver and transmitter DC voltage charts**, Figs. 1-13A/B (fold-out from p. 112; bias at 50 mA plate current). Key values: B+ about 300 V; V18 150 V; V5 cathode about −5 V on crystal, 0 V on the LMO (*approx.*; the fold-out print is small).

## 4.7 Phase 6: Alignment (manual pp. 93–97)

**Equipment:** 11 MΩ VTVM (or a 20 kΩ/V VOM); 50 Ω 100 W non-reactive dummy load (not light bulbs); a receiver for WWV (2.5/5/10/15 MHz) or a broadcast station on a 100 kHz multiple. A monitor scope is recommended for transmitter work.

**Setup:** ANTENNA switch (rear) to **COM**; dummy load on RF OUT; 8 Ω speaker; **CAL XTAL trimmer notch toward the 100 kHz crystal** (Fig. 1-1).

**Preset:**

| Control | Setting |
|---|---|
| DRIVER PRESELECTOR | 12 o'clock (3.7, 7.2, 14.2) |
| MIC/CW LEVEL | Fully CCW |
| MODE | LSB |
| BAND | 3.5 |
| Dial | 3.7 MHz (upper pointer at 2, circular dial at 0) |
| FUNCTION | PTT |
| FREQ CONTROL | LOCKED NORMAL |
| RF GAIN | Fully CW |
| FILTER | SSB |
| METER | ALC |
| AF GAIN | 9 o'clock |

**Do not transmit until directed** (p. 93).

**S-meter:** after a few minutes' warm-up, set **METER ZERO** (IF board, directly behind the FREQ CONTROL switch) for zero.

**Receiver:**

1. **Heterodyne oscillator:** VTVM on −DC between chassis and **TP** (bandpass board foil, screened side, near V19).
   - Target **−0.5 to −2 V** at every BAND position.
   - Adjust in the direction where the reading changes **slowly**.
   - Coils 3.5, 14 and 28.5 are on top of the RF-driver board (3.5 near V11); the others are adjusted from the bottom, marked on the shield cover.
2. FUNCTION CAL, BAND 3.5, tune around 3.7 MHz for the loudest signal. Confirm it is the calibrator: it stops in VOX and peaks with the DRIVER PRESELECTOR. Reset the preselector to 12 o'clock and remove the VTVM.
3. **IF:** peak **T201** for volume, then **T102** top and bottom slugs, then **T103** for S-meter. Repeat all three.
4. **Driver grid and plate coils**, in order **3.5 → 29 (29.2) → 21 (21.2) → 14 (14.2) → 7 (7.2)**, preselector at each marked position (inset, Fig. 1-2), for maximum S-meter. The meter moves slowly on these.

**Benchmark (p. 95):** calibrator **S9+20 dB at 3700 kHz, decreasing to S6 at 29.2 MHz.**

**Receiver signal-voltage chart (fold-out from p. 112)**, for signal-generator troubleshooting through .001 µF with a 50 Ω termination: 3.395 MHz at 20 mV at point 1 gives about 4 V RMS at the 8 Ω jack; **100 µV at 3.900 MHz at the antenna gives about S9**; calibrator about S9+20 dB with the preselector peaked.

**Transmitter** (pp. 95–96; coil cover on; dummy load; read "Reading the Meter," p. 108, first):

1. Connect a PTT microphone; optional scope across the dummy load.
2. **Neutralizing capacitor** (on the RF cage) at **½ meshed, slot vertical**.
3. Preset: preselector 12 o'clock; MIC/CW fully CCW; FINAL 80; lever over 50 Ω; LSB; BAND 3.5; 3.7 MHz; PTT; FILTER SSB; LOCKED NORMAL; METER PLATE.
4. **Bias:** press PTT and set **BIAS ADJUST for 50 mA** plate current. If it reads over 100 mA, hold PTT only a few seconds at a time until it's set.
5. RELATIVE POWER control mid-range; METER → REL PWR. MODE **TUNE**: advance MIC/CW until RF shows; peak the DRIVER PRESELECTOR, then the FINAL tune.
6. MIC/CW to about **¼ maximum output**; peak **T1**. No more than one turn should be needed.
7. MIC/CW and preselector for maximum; METER → **GRID: full scale.**
8. **Neutralization:** METER → PLATE and tune FINAL for minimum plate current; METER → REL PWR and note the FINAL setting for maximum. Move the neutralizing capacitor a small amount at a time until **minimum plate current and maximum relative power coincide**.
9. **Carrier null:** MIC/CW fully CCW; LSB; PTT. Adjust the **CARRIER NULL control**, then the **CARRIER NULL capacitor**, for minimum RF. Repeat until LSB and USB null about equally, down to **¼ V or less** on an RF voltmeter. If C14 runs out of range, see SB-101-18 (§3.1). **The 6.8 MHz trap coil is sealed; don't turn it.**
10. MODE TUNE, tune for maximum, and set RELATIVE POWER for a **6 to 8** meter reading.
11. **For each band from 3.5:** peak for maximum, set MIC/CW for **about 0.3 mA** grid current, and peak **that band's heterodyne oscillator coil** for maximum grid current, **on the "slow" side of the peak**. Maximum grid current should be **near or over full scale** on every band.
12. **Driver neutralization (21.2 MHz):** put the free end of the **driver neutralizing wire into hole W** on the RF-driver board (Fig. 1-1). Rock the preselector; if peaking is ragged, reposition or bend the wire for a smooth peak.
13. **Final neutralization again at 14.2 MHz.**
14. **Rezero the S-meter on 29.5**, then check zero on every band. If a band doesn't read zero, readjust **that band's heterodyne oscillator coil**.
15. **Crystal calibrator (pp. 96–97):** connect the external receiver's antenna wire to the SB-101's **REC ANT** jack; FUNCTION CAL; AF GAIN fully CCW; ANTENNA switch **REC**. Adjust the **CAL XTAL trimmer (bandpass board)** for zero beat with WWV; confirm by toggling VOX/CAL. **A 20 Hz error at 100 kHz becomes 740 Hz at 3.7 MHz.** Remove the wire and return the ANTENNA switch to COM.
16. **LMO shifter (p. 97):** 3.7 MHz (BAND 3.5, upper dial 2, circular dial 0); FUNCTION CAL; MODE USB; zero beat with the main dial and peak the preselector. Switch to LSB without touching the dial and set the **frequency shifter adjustment on the LMO** (Fig. 1-1) for exact zero beat. Recheck USB; repeat as needed.
17. **Dial calibration:** zero beat at 3.7 MHz and set the Zero Set dial. If the hairline is far off center, remove the knob, hold the LMO drive shaft with a screwdriver through the escutcheon hole, loosen the circular-dial setscrew and center 0, then retighten. **Tape the Allen wrench** so it can't short the pilot lamp socket. Check the circular dial turns freely and the nylon spiral follower is in its groove.

**Operation tune-up (p. 109):** TUNE should read **50 mA** at rest; set FINAL loading for REL PWR 50; advance MIC/CW and peak preselector, FINAL tune and loading. **Plate current about 250 mA** at maximum output. Don't hold full output for long; "the final amplifier tubes and/or the power supply may be damaged."

**Alternate neutralizing method (SB-101-6):** RF probe on the antenna jack, screen voltage removed (lift R920, 100 Ω), and adjust the neutralizer with an insulated tool for minimum feedthrough.

## 4.8 Phase 7: Optional improvements (reversible)

The family improvements used on the HW-101 and SB-102 (WB8JKR, K4JPN) apply to the SB-101 where the circuit matches; check each against this manual's schematic first:

- 10 MΩ meter-zero resistor.
- C119 500 pF → .001 µF; carrier-oscillator injection tweaks (C17, R6/R7).
- Sidetone fixes; multi-turn carrier-null pot.
- .01 µF filament bypass.
- **K9STH LMO mode-shift zener** (§3.4) if mode-dependent drift persists.
- **6HS6 at V10:** a 1970s receive-sensitivity change (Heath list, 1998; mod details not recovered). Only after confirming the socket wiring against your unit's schematic; the SB-101 already meets its 0.5 µV spec with a good 6AU6.

# 5. Troubleshooting quick-reference

## 5.1 Manual troubleshooting chart (pp. 113–120), selected

| # | Symptom | Key causes |
|---|---|---|
| 1–3 | No power / no B+ / no filaments | Supply switch, plug or cable wiring, **FUNCTION AC snap switch**, breakers; supply rectifiers; brown-white harness wires |
| 4 | Uneven filament or lamp voltages | Brown filament-balance leads wrong or missing |
| 5–7 | No / high / low regulated +150 V | RL2 wiring; **OA2 V18**; R304/R305; shorted bypass |
| 8 | No screen voltage at V10/V11 | RL2 lugs 3, 7, 11 |
| 10 | No audio | T301; **C304 shorted or reversed**; V14; RL2 leads |
| 13 | Hum only | RFC101 open; V13; no carrier injection; T102; V3/V4; FL1 coax |
| 14 | Noise, no signal | T201; no LMO at V12A; V3; V12A; no heterodyne at V11; V10/V11; RL1; T202; FL1; **FILTER in CW** |
| 16 | Receiver oscillates or birdies | Board hardware and lockwashers; antenna; high supply; low cutoff bias |
| 17 | Sideband reversed | Y1/Y2 or Y3 interchanged; FREQ CONTROL lugs 13/17; FILTER in CW |
| 18 | S-meter faults | R103/R105/R106/R107/R110/R115; V3/V4; AVC short; RL2 lugs 4/8/12; **V19 wrong type**; heterodyne coils |
| 19–20 | No V7 screen / bias won't shift | RL2 lugs; **BIAS ADJUST** |
| 21–27 | No RF at a stage | HV, L901, RL1 lugs 8/12, bias, finals, rotor 180°; RFC801, V7; V6 heterodyne; V5A, C111/C402, T202; T1, V2, R18/R19/R23/R24/R937/R938 |
| 23 | No RF output | FILTER in CW without a CW filter; FREQ CONTROL; antenna shorted; **no 800 V** |
| 28–32 | No SSB or no CW carrier | CR1–CR4; V1; MIC/CW LEVEL; V16; Y1/Y2/Y3; C4–C8, C16, C17; R6–R9, R11 |
| 33 | No LMO injection | +150 V; FREQ CONTROL; coax; **V20 6CB6 (same brand)**; LMO unit |
| 34 | LMO won't shift with mode | **R306/R307**; mode wafer 1F |
| 35 | No heterodyne injection | Y501–Y508; red coax; C208/C223/C604; **V19**; L601–L608; 150 V; rotors 180° |
| 36 (p. 119) | Relays won't energize in TUNE | V12B; RL1/RL2; mode wafer 2F |
| 36 (p. 120) | Relays won't energize on VOX | V17A; D201; **D202**; ANTI-TRIP too high; FUNCTION in CAL |
| 37 | Relays stay energized | D1/D2 reversed; PTT stuck; key closed |
| 38 | Transmitter unstable | Neutralizing; preselector hardware; board lockwashers; L801–L805; coil cover; shield clips |
| 39 | Slow receive recovery | **D101** |
| 40 | Grid drive falls off | Heat; bias; load; **gassy 6146s** |
| 41 | Zero shifts band to band | Calibrator not exactly 100 kHz |

## 5.2 Service bulletins (Heath list transcription, Apr 1998)

| Symptom | SB-101 bulletin |
|---|---|
| Driver oscillates on 40 m (early units) | -1D (remove C924) |
| Switch action; tuning; 40 m tuning | -2D, -3D, -4D (parts changes) |
| Grounding (phono sockets, final sockets) | -5D |
| Meter zero / S-meter drift | -1 (R107 → 82 kΩ), -9 (R107 → 100 kΩ 1 W), -22 (leaky V3/V4/V10/V11) |
| Heterodyne tube failures | -2 (R212 220 → 330 Ω) |
| Audio / VOX troubleshooting voltages | -3 |
| Diode leakage (ALC fade, slow recovery, VOX) | -4 (D902/D903, D101, D905, D201) |
| Instability checklist (25 items; LMO injection 1.0–1.5 V RF) | -5 |
| Alternate neutralizing | -6 |
| Self-oscillation, low drive on 15/10 m | -7, -8 (steel comb brackets, lockwashers) |
| Low sensitivity on 15/10 m | -10 (D907 reversed; see §11) |
| Erratic LMO tuning | -12 |
| Relays hang after PTT | -13 (IEC 6EA8 at V12) |
| Poor AGC, low sensitivity | -14 (leaky 6HS6 at V10/V11) |
| Calibrator spurs | -15 (Y201) |
| LMO drift / frequency jumps | -16 (LMO phono connector) |
| Preselector tracking | -17 |
| Carrier null out of range | -18 (move C18) |
| VOX relay chatter | -19 (GE 6EA8 at V1) |
| Loading cap drags | -20 |
| Chirp / slow recovery | -21 (RL2 carbon) |
| IF sensitivity | -23 (C101 wiring) |
| Low power, poor VOX | -24 (V3/V4 brand) |
| Low TX and RX | -25 (FILTER slide switch) |
| No text available | -11, -26 (low grid drive), -27 (germanium diodes), -28 (VOX oscillation) |

# 6. Operation notes (manual pp. 98–111)

- A 50–75 Ω non-reactive load (antenna, dummy load or adjusted linear) must be connected before transmitting (p. 108).
- **FREQ CONTROL:** LOCKED NORMAL (LMO for both); LOCKED AUX (crystal Y5 for both); UNLOCKED AUX (Y5 transmit, LMO receive). Because of the steep-sided bandpass filter T202, Y5 operation is limited to about 25 kHz outside each band, and the preselector limits the transmit/receive split to about 20 kHz on 80 m and 40 kHz on 40 m (p. 98).
- **Y5 crystal spec (p. 99):** fundamental, 0.01%, HC-6/U, 0.050 in. pins on 0.486 in. spacing, CL 32 pF, C0 ≤ 7 pF, Rs ≤ 25 Ω, 10 mW drive. f(Y5) = f(het) − f(carrier) − 3.3964 MHz (USB), − 3.3936 (LSB), − 3.3954 (CW net). **AUX TRIM** next to Y5 sets the exact frequency.
- **Heterodyne crystals may be up to 1500 Hz off nominal** (p. 98). Measure the actual frequency at **pin 7 of V11** with a counter through a small capacitor (LOCKED AUX, RF GAIN fully CCW, PTT, LSB). Using heterodyne crystals other than those supplied "is not recommended" (p. 99).
- **Meter scales (p. 108):** S-meter 0–60 dB (ALC position, receive); REL PWR 0–10; GRID 0–1 mA; HV 0–1000 V; PLATE 0–500 mA. The top 20 dB of the ALC scale is the maximum allowable modulation.
- **VOX (pp. 110–111):** VOX SENSITIVITY just past tripping; ANTI-TRIP just past the speaker-trip point; VOX DELAY to hold through pauses. The three interact slightly.
- **Linear amplifiers (p. 103):** the SB-200 and HA-14 provide ALC and antenna switching; no swamping pad needed. Others: use the spare relay contacts on pins 5, 8, 11 and, if the amp needs only 10 W drive, a 10 dB T-pad of non-inductive carbon resistors (p. 104).
- **Mobile (p. 111):** with an HP-13, set the bias with the HP-13's control at 50 mA while the engine runs at about 30 mph, so the transceiver's BIAS ADJUST stays at its fixed-station setting. Regulator ≤ 14.5 V (p. 90).

# 7. Tips and tricks

- Keep spare **#44** lamps, GE (or other known-good) 6AU6s for V3/V4, a 12AT7 for V19, a matched 6146 pair, and a non-GE 6EA8 for V1.
- **Never swap the LMO's 6CB6 for a different brand** unless you're prepared to recalibrate.
- Never pull tubes or lamps with power on (series-parallel filaments).
- Tape the Allen wrench during dial calibration.
- Measure resistors with one lead lifted.
- Reform any electrolytics you keep with a slow ramp.
- Leave pin 8 of the power plug empty.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | 20 min warm-up (the stability spec assumes it); zero-set on the nearest calibrator point. |
| 6 months | Exercise switches; METER ZERO check. |
| Yearly | Bias (50 mA), carrier null, grid drive; receive benchmark (S9+20 / S6); clean the LMO jack. |
| 2–3 years | Neutralization check (14.2 MHz); retighten board hardware; DeoxIT the FILTER slide switch. |
| 5 years | Tube test (V3/V4/V10/V11/V19, finals); belts; supply check; RL2 contacts. |
| ~15–20 years | Re-check electrolytics. |

# 9. Parts sources

- HW-101/SB-10x restoration kits (W8AOR and others): check the kit's list against the SB-101's **four** electrolytics; SB-102 kits include LMO-supply parts the SB-101 doesn't have.
- Mouser / Digi-Key; HP-23/PS-23 rebuild kits.
- Heathkit-Radios and heathkit (groups.io): LMOs (110-40), crystals, dials, belts, relays.

# 10. Sources

- Heath, *SB-101 Assembly Manual* 595-840 (©1966; printed 2-9-68), owner's scan; also at W5RKL ([w5rkl.com/manuals](https://w5rkl.com/manuals/)).
- Heath list, "Heathkit Service Bulletins - SB-101 (Complete to 1989)," Apr 1998: [puck.nether.net/pipermail/heath/1998-April/029133.html](https://puck.nether.net/pipermail/heath/1998-April/029133.html)
- Heath list, "SB-101 LMO" (W3FYD question; AE5P reply), Mar 2007: [022658](https://puck.nether.net/pipermail/heath/2007-March/022658.html), [022667](https://puck.nether.net/pipermail/heath/2007-March/022667.html)
- Heath list, "SB-101 frequency drift" (K9STH), Dec 2005: [020477](https://puck.nether.net/pipermail/heath/2005-December/020477.html)
- Heath list, "LMO rebuilding....did I mess it up?" (WA6OQQ, G3OOU), Mar 2016: [040196](https://puck.nether.net/pipermail/heath/2016-March/040196.html)
- Heath list, "SB-101 vs 102," Oct 2009: [025219](https://puck.nether.net/pipermail/heath/2009-October/025219.html)
- Heath list, "Heathkit LMO's TX vs RX tube" (W9DZ), Apr 2009: [024726](https://puck.nether.net/pipermail/heath/2009-April/024726.html)
- Heath list, "RF AMP MOD FOR THE SB-101" (KD5CML), Jun 1998: [029742](https://puck.nether.net/pipermail/heath/1998-June/029742.html)
- Heath list, "More SB-101 problems" (W4GBW), Oct 1999: [037158](https://puck.nether.net/pipermail/heath/1999-October/037158.html)
- RigPix, Heathkit SB-101: [rigpix.com/heathkit/sb101.htm](https://rigpix.com/heathkit/sb101.htm)
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*; K5BCQ SB-101/102 Repair Site; W2PA SB-101 rebuild notes.
- groups.io: [Heathkit-Radios](https://groups.io/g/Heathkit-Radios), [heathkit](https://groups.io/g/heathkit), [HeathKit-HFAmps](https://groups.io/g/HeathKit-HFAmps) (searched; pages would not fetch).
- AF6C *Heathkit of the Month*, SB-line overview ([w6ze.org/Heathkit](https://www.w6ze.org/Heathkit/Heathkit_Index.html); PDF would not fetch).
- Heath, *HP-23A Assembly Manual* 595-924-01 (©1968); *PS-23 Manual* 595-2314 (©1979).

# 11. Notes: what the manual confirmed, and conflicts

| Item | Manual | Field / bulletin | Resolution |
|---|---|---|---|
| **V10/V11 tube type** | **6AU6** RF amp and 6AU6 1st mixer (spec p. 123; Fig. 2-22) | SB-101-14 (1978) names a **6HS6** (411-247) at V10/V11; a 1998 list post recalls a 6HS6 sensitivity mod. The SB-102 uses a 6HS6 at V10. | Use whatever your unit's socket is wired for. Late production or a field change may have a 6HS6; don't swap types without checking the schematic. |
| **LMO** | Tube LMO 110-40, **6CB6 V20**, 5.0–5.5 MHz, +150 V from the OA2 via RL2 and FREQ CONTROL; never open; same-brand tube (pp. 112, 119, 132–133) | Field restorers do open and re-grease them; turn stops stick; connector corrosion (SB-101-16); wipers (SB-101-12) | Open only for a mechanical fault. No lamp/zener/transistor supply as in the SB-102. |
| **Pilot lamps** | **Three #44** (p. 24) | SB-102 sources give #47/#1815 | #44 for the SB-101. |
| **Electrolytics** | **Four**: C2 10 µF, C12 20 µF, C212 10 µF, C304 20 µF (pp. 4, 126, 128, 143) | — | No voltage ratings printed; replacement ratings are from circuit voltages. |
| **R107** | 100 kΩ (Fig. 2-24) | SB-101-1 (1970): 82 kΩ; SB-101-9 (1976): 100 kΩ 1 W | The later bulletin wins: 100 kΩ 1 W. |
| **Relays** | "Sealed relays … will be troublefree for years" (p. 112) | SB-101-21: remove the RL2 cover and clean carbon at the base | Field experience wins; clean or replace. |
| **D907** | **Not in the SB-101 diode set** (16 diodes, p. 123; parts lists) | SB-101-10 blames a reversed D907 for low 15/10 m sensitivity | The bulletin likely refers to later production or a field-added part. Check whether your unit has a D907. |
| **Chart numbering** | Symptom **36 appears twice** (p. 119 relays in TUNE; p. 120 relays on VOX) | — | Cited here as 36 (p. 119) and 36 (p. 120). |
| **Supply bias** | HP-23: supply's BIAS control inoperative, set on the transceiver (p. 91). HP-13: supply BIAS fully CW (p. 91) yet set to 50 mA in mobile use (p. 111). | — | Two separate manual instructions for the HP-13; follow p. 111 when mobile. Irrelevant for HP-23/23A/PS-23 users. |
| **Supply setting** | "+300 volts DC output as outlined in the HP-23 Manual" (p. 91) | — | HP-23A switch **right (350 VDC)**; PS-23 **350 VDC wiring**. Same as the HW-101 and SB-102. |
| **Pin 8** | Fig. 1-5 shows no connection; p. 103 lists pins 5, 8, 11 as spare relay contacts | SB-102 plan: SB-101 sockets need any pin-8 wire removed | Keep pin 8 empty in the supply cable. |
| **Initial test** | HV 800–900 V; +300 V at bandpass pt 5; −111 V at audio pt 4 (p. 92) | — | Confirmed. |
| **Not verified** | Fold-out schematic details beyond the circuit-description figures; small fold-out chart values (marked *approx.*) | W2PA, K5BCQ, W5RKL pages not re-fetched for this revision | Check against your copy before relying on them. |
