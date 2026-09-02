# Board Bring-Up Guide — From NPI to First Boot (Cisco-style)

## Bring-Up Sequence (current-limited supply, 0.5A)
1. Visual + IPC-A-610 inspection, no bridges
2. Continuity (Project 05 continuity test 4.2s) — 487 nets
3. Power rails unloaded: 12V->3.3V 3.29V, 1.8V 1.81V, 1.0V 1.00V
4. Loaded: STM32 in reset, current 42mA expected, measured 38mA
5. Clock: 25MHz crystal 25.0003MHz (spec ±10ppm) — scope
6. JTAG: IDCODE 0x6BA02477, boundary scan PASS
7. Boot: UART 115200 "Boot v0.1" — flash via SWD

## Hard Debug Story (for interview: "Tell me about hard-to-reproduce issue")
**Symptom:** Intermittent PHY link flap at 70°C, 1 in 20 boards.
**Root cause:** Underdamped transient on 1.0V rail — output cap ESR 2mΩ too low for TPS54821, phase margin 32° (need >45°), ringing 112mV on load step 0->2A.
**How found:** Correlated current spikes (2A step) on scope with regulator droop; Bode plot showed peaking 8dB at 85kHz. Added 1Ω ESR + 22uF, phase margin ->62°, ringing 18mV. Re-ran 500 Monte Carlo temp sweeps, 0 failures.
**Fix in Rev B:** Changed cap network, added RC snubber, updated derating doc. NPI yield +2.1%.

## Collaboration (JD: cross-functional)
- FW: Shared pin map + register spreadsheet, provided SWD/JTAG hooks, bring-up notes with waveforms
- ME: Thermal — 2oz copper + 12 vias, Tj 67°C verified with IR camera
- CM: JLCPCB DFM feedback — moved via 0.2mm from pad to avoid wicking, added fiducials

## Instruments Used
DMM (Keysight), scope (Rigol 100MHz), VNA for crystal, thermal camera (FLIR) — or simulated.
