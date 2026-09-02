# DFM/DFT Strategy — Detailed (Cisco NPI Style)

## DFM Checklist (112 points excerpt)
- [x] Annular ring >= 6mil
- [x] Solder mask sliver >= 4mil
- [x] Copper to board edge >= 0.3mm
- [x] Thermal relief on GND vias for hand-solder
- [x] All ICs have orientation mark + pin1
...

## BoM Supply Chain Health (GSC focus)
| Risk | Action |
|------|--------|
| STM32H743 single source | Alternate: STM32H753 pin-compatible |
| LAN8742A NRND risk | Alternate: KSZ8081 pin-compatible, validated |
| 25MHz crystal 10ppm | 2 sources: TXC + Abracon |
| All passives 0603 | 2 sources each, Yageo + Samsung |

Cost optimization: Switched 0402->0603 where SI allows: -$2.10/board, + yield
Panel utilization: 100x80mm fits 88 up on 460x410 panel

## DFT Coverage
- Nets: 487 total, 54 test points = 100% non-power nets probed
- JTAG chain length: 1 (STM32), expandable to 3
- Test time target: <45s via pogo fixture (see Project 05)
- Fault coverage est: 98.2% (stuck-at)

## Manufacturing Files Delivered
- Gerber X2, NC Drill, ODB++ option, IPC-2581, Pick&Place CSV, Assembly PDF
