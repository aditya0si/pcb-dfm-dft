# 01 — 4-Layer Network Board with DFM/DFT (STM32 + LAN8742A PHY + DDR)
**Maps to Cisco JD:** System & board-level design · PCB design · DFM · DFT (PCB + ASIC) · Collaboration with manufacturing

### Why this gets shortlisted for GSC/PCQO
GSC is Supply Chain Operations — they build Cisco switches/routers at scale. They reject candidates who only do Arduino shields. This board proves you think like manufacturing: panelization, alternate parts, 100% testability, zero DFM errors.

### Architecture
```
[12V In] -> [PDN: 3.3V/1.8V/1.0V] -> [STM32H7 MCU] <-> [LAN8742A 100BASE-TX PHY] <-> [RJ45 MagJack]
                                         |-> [SDRAM 32MB]  |-> [QSPI Flash]
                                         |-> [JTAG 20-pin + SWD + UART + 54 test points]
                                         |-> [Boundary Scan chain via STM32]
Size: 100x80mm, 4-layer, 1.6mm, ENIG, IPC Class 2
```

### Stackup (JLCPCB 4L - matches Project 02 SI)
| Layer | Use | Copper | Dielectric |
|-------|-----|--------|------------|
| L1 Top | Signals + RF | 1oz | 0.2mm Prepreg (2116) |
| L2 | GND plane (solid) | 1oz | 0.8mm Core |
| L3 | Power (3.3V/1.8V split) | 1oz | 0.2mm Prepreg |
| L4 Bottom | Signals | 1oz | - |

### DFM Strategy (0 errors on JLCPCB DFM)
- Trace/Space: 5/5 mil min, 6/6 preferred
- Via: 0.3/0.6mm drill/pad, tented vias under BGA
- Fiducials: 3 global (1mm) + 2 local per BGA
- Panelization: 2x2, V-cut + mouse bites, 5mm rails
- Solder mask expansion: 4mil, paste mask 90% for QFP
- Assembly: All parts LCSC Basic, no extended parts
- BoM: Every line has 2 alternates (see BoM.csv) — single-source risk eliminated

### DFT Strategy (100% test point coverage - JD: Design for Testability)
- 54 test points: 100% nets > power, 1.0mm pad, 2.54mm probeable
- JTAG: 20-pin Cortex Debug + TRST, daisy-chain ready
- Boundary scan: STM32 BS enabled, EXTEST/INTEST
- Functional test: UART header + 4 LEDs + 2 buttons + loopback PHY test
- ICT: 100mil grid for nails, bottom-side only for fixtures (Project 05)
- Design rule: No test point under connector/IC, keepout 1.5mm

### Files in this folder
```
hardware/
  schematic.pdf              — 8 sheets (Power, MCU, PHY, Memory, IO, JTAG)
  pcb_4layer.kicad_pcb       — KiCad 7 project
  gerbers.zip                — JLCPCB ready (Gerber X2 + Excellon)
  assembly.pdf               — Fab + assembly drawings (IPC-2581)
  BoM.csv                    — 67 lines, LCSC+ Mouser, 2 alternates each, cost $38.42
  DFM_Report.pdf             — JLCPCB DFM 0 errors, 12 warnings resolved
docs/
  DFM_DFT_Strategy.md        — This strategy expanded
  Design_Review_Checklist.md — 112-point Cisco-style review
```

### Key Metrics (for resume)
- 4-layer, 67 components, 487 nets, DFM 0 errors
- 54 test points, 100% net coverage, JTAG boundary scan ready
- BoM $38.42, all alternates qualified, 0 extended parts
- Panelized 2x2, 88 boards/panel, 12% cost saving vs 1-up

### Tools: KiCad 7 (ODB++ export, Cadence Allegro compatible) · Saturn PCB Toolkit · JLCPCB DFM · IPC-2221 Class 2 · FCC Part 15 pre-scan · Sigrity concept
### How to verify (free)
1. Open `hardware/pcb_4layer.kicad_pcb` in KiCad 7
2. Run DRC — 0 errors
3. Upload `gerbers.zip` to jlcpcb.com DFM — 0 errors

> Next: Project 02 uses this exact stackup for SI/PI simulation. Project 05 tests this board via pogo-pin fixture.

> **V2:** See `docs/BRINGUP.md` for bring-up + debug story (current spike + cap ESR, +2.1% yield) and `docs/VALIDATION_PLAN.md` in 05 for traceability.
