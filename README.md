# Heathkit SB-101 Restoration Plan

A restoration plan for the Heathkit SB-101 HF SSB/CW transceiver (1967–1970) and its HP-23 series or PS-23 power supply. It covers:

- which parts fail with age;
- the tube-type LMO (6CB6, 5.0–5.5 MHz);
- a phased restoration procedure;
- alignment and neutralization;
- a symptom-to-service-bulletin index;
- maintenance.

**Status:** Rev. 1, checked against the original Heath SB-101 assembly manual (595-840, ©1966). The check covers the specifications, the tube and diode complement, the parts lists, power supply connections, initial test, alignment, the troubleshooting chart and the circuit description. The SB-101 service bulletins (1967–1980) and Heath list field reports are included and credited. Conflicts between the manual and the field sources are listed in §11.

## Files

- [`sb101-restoration-plan.md`](sb101-restoration-plan.md) — source (pandoc markdown)
- [`sb101-restoration-plan.pdf`](sb101-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc sb101-restoration-plan.md -o sb101-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Power supply

The SB-101 needs 300 V B+. On an HP-23A, set the switch **right (350 VDC)**. On a PS-23, use the **350 VDC wiring**. Final bias is set with the SB-101's own BIAS ADJUST; the supply's bias control is inoperative. Leave pin 8 of the power plug empty, because in the SB-101 it is a spare relay contact. See [heathkit-hp23-ps23-restoration](https://github.com/usa4148/heathkit-hp23-ps23-restoration).

## Safety

The supply delivers about 700–850 V DC, and that charge stays on the filter capacitors after unplugging. Discharge them before working inside. For transmit work, use a 50 Ω dummy load, never light bulbs.

## Sources

- Heath SB-101 assembly manual: W5RKL (w5rkl.com/manuals)
- Heath SB-101 service bulletins, transcribed on the Heath mailing list (puck.nether.net)
- Heath list threads on SB-101 LMOs and drift
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*; K5BCQ SB-101/102 Repair Site; W2PA SB-101 rebuild notes
- groups.io: Heathkit-Radios, heathkit, HeathKit-HFAmps

See also: [heathkit-restoration (index)](https://github.com/usa4148/heathkit-restoration) · [heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration) · [heathkit-hw101-restoration](https://github.com/usa4148/heathkit-hw101-restoration) · [heathkit-sb102-restoration](https://github.com/usa4148/heathkit-sb102-restoration) · [heathkit-sb110-restoration](https://github.com/usa4148/heathkit-sb110-restoration) · [heathkit-sb220-restoration](https://github.com/usa4148/heathkit-sb220-restoration) · [heathkit-sb200-restoration](https://github.com/usa4148/heathkit-sb200-restoration) · [heathkit-hp23-ps23-restoration](https://github.com/usa4148/heathkit-hp23-ps23-restoration)
