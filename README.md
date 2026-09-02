# pcb-dfm-dft — Four-Layer Network Board

<p align="center">

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-0ea5e9?style=flat-square)](https://aditya0si.github.io/pcb-dfm-dft/)
  <img src="https://img.shields.io/badge/PCB-4%20Layer%20%7C%20100%C3%9780mm-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/DFM-0%20errors-success?style=flat-square" />
  <img src="https://img.shields.io/badge/DFT-54%20test%20points%20%7C%20100%25-orange?style=flat-square" />
  <img src="https://img.shields.io/badge/IPC-Class%202-lightgrey?style=flat-square" />
  <img src="https://img.shields.io/badge/status-fabrication%20ready-informational?style=flat-square" />
</p>

<p align="center"><i>A production-minded 4-layer board — designed for manufacturing, not just the schematic.</i></p>

---

### Why this exists
Most student boards stop at "it works on bench." This one is built for NPI: panelized, alternate-sourced, 100% probeable, and DFM-clean — the kind of board you'd hand to a CM and get 88 boards/panel back with >98% yield.

### Snapshot
| Spec | Detail |
|------|--------|
| **MCU + PHY + Memory** | STM32H743 + LAN8742A (RMII 50MHz) + 32MB SDRAM + QSPI |
| **Size / Stackup** | 100×80mm, 4L (Signal-GND-Power-Signal), Isola 370HR, ENIG |
| **Nets / Components** | 487 nets, 67 components |
| **DFM** | JLCPCB DFM **0 errors**, 5/5mil, 0.3/0.6mm via, 3 fiducials, 2×2 panel |
| **DFT** | **54 test points** (1.0mm), JTAG 20-pin + SWD, boundary scan, ICT nails bottom-only |
| **BoM** | **$38.42**, every line has 2 alternates (LCSC Basic), 0 extended parts, $8.84 saved via second-sourcing |
| **Compliance** | IPC-A-610, IPC-2221 Class 2, FCC Part 15 pre-scan, RoHS |

### Architecture
```
12V → [TPS54360 3.3V] → [TLV62569 1.8V] → [TPS54821 1.0V]
            ↓                ↓                 ↓
       STM32H743 ←→ LAN8742A (RJ45 MagJack) ←→ SDRAM / QSPI
            ↓
       JTAG/SWD/UART + 54 test points + boundary scan chain
```

> RMII baseline, but layout is **RGMII-ready** — 3W spacing, solid GND, stitching vias every 10mm. Swap PHY to 88E1512 for 1G.

### Interactive Preview
→ Open **`viewer.html`** for an interactive DFM/DFT explorer (hover test points, panel view, BoM cost breakdown). No install — just double-click.

Or try it inline below if your README renderer supports it:
[viewer.html](./viewer.html) · [docs/BRINGUP.md](./docs/BRINGUP.md) · [hardware/BoM.csv](./hardware/BoM.csv)

### Bring-up story (one debug that matters)
Intermittent link flap at 70°C — root cause was underdamped 1.0V buck (phase margin 32°, ESR too low). Correlated load-step scope capture with Bode peaking. Fixed with 1Ω ESR + 22µF, PM → 62°, yield +2.1%. Details in `docs/BRINGUP.md`.

### Quick start
```bash
# KiCad 7 (or Allegro via ODB++ export)
open hardware/pcb_4layer.kicad_pcb
# DRC → 0 errors → upload gerbers.zip to JLCPCB → DFM 0
```

### Files
`hardware/` — schematic.pdf (8 sheets), gerbers.zip, assembly.pdf, BoM.csv  
`docs/` — DFM_DFT_Strategy.md, BRINGUP.md, Design_Review_Checklist.md (112 pts)

---
*Built as part of a hardware NPI portfolio — SI/PI (si-pi-lab), PDN/thermal (pdn-thermal-lab), and ATE (ate-fixture-lab) build on this board.*
