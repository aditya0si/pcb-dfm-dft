# Design Review Checklist — 112 Points (Cisco-style)
## Schematic
- [ ] All NC pins marked DNC
- [ ] Decoupling: 100nF per power pin + 4.7uF bulk per rail
- [ ] PHY: 49.9R series on TX, 100R diff on RX, 0.1uF AC coupling
## Layout
- [ ] Diff pairs length matched <5mil
- [ ] Crystal <12mm from MCU, guard ring
- [ ] No traces under crystal
- [ ] Stitching vias every 10mm on GND
## DFM/DFT
- [ ] 3 fiducials, 1mm copper + 2mm mask
- [ ] Test points 1mm, not under connectors
- [ ] JTAG TCK 10k pull-down, TDI/TMS pull-up
