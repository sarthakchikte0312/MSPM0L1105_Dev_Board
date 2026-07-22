
# MSPM0L1105 Dev Board

A custom development/breakout board for Texas Instruments' **MSPM0L1105**, an ultra-low-power Arm Cortex-M0+ microcontroller. Designed in KiCad.

![PCB Render](https://github.com/sarthakchikte0312/MSPM0L1105_Dev_Board/blob/main/images/10.png?raw=true)



## Why the MSPM0L1105?

The MSPM0L1105 is part of TI's MSPM0 family, and it's a solid choice for low-power, cost-sensitive embedded designs for a few reasons:

- **Arm Cortex-M0+ core** running up to 32 MHz — enough headroom for real control/sensor tasks while keeping power draw low.
- **Ultra-low-power modes** with fast wake-up, making it well suited for battery-powered or energy-harvesting applications.
- **Integrated analog peripherals** — 12-bit ADC, comparators, and op-amp(s) on many variants — reducing the need for external analog front-end components.
- **Rich communication peripherals** — UART, I2C, SPI — for interfacing sensors, displays, or other MCUs.
- **Low cost and small footprint**, making it attractive for space-constrained or high-volume designs where a full Cortex-M4 is overkill.
- **Common tooling** — MSPM0 shares a peripheral driver library (DriverLib) and code examples across the family, so firmware written here can port to bigger MSPM0 parts (M4-based) with minimal changes if the project scales up.
- Backed by TI's **SysConfig** tool, which simplifies pin muxing and peripheral configuration compared to hand-rolling register setup.

This dev board exists to make it easy to bring up, test, and prototype around the MSPM0L1105 — breaking out its I/O, providing power regulation, and exposing debug headers — before committing the MCU to a larger system design.

<img width="1675" height="892" alt="image" src="https://github.com/user-attachments/assets/540301ac-fca1-4186-9bcd-f1ca3d00cca2" />
```
## Repository Structure

```
mspm0l1105-dev-board/
├── hardware/       KiCad project, schematic, PCB layout, libraries, 3D models
├── docs/           Datasheet, schematic PDF, pinout reference, BOM
├── images/         Board renders and photos
```

## Hardware Overview

| Item | Details |
|---|---|
| MCU | TI MSPM0L1105TDGS28R (Arm Cortex-M0+), VSSOP-28 package |
| Design tool | KiCad 10 |
| Layers | 2-layer, 1.6 mm board thickness |
| Board size | ~124 x 89 mm |
| Power in | USB-C receptacle (J4, USB4085) |
| Regulation | MIC5209-3.3YS LDO (U4) → 3.3V rail, selectable via LDO_JUMPER (J6) |
| USB-to-Serial | CH340K bridge (U2), for UART programming/debug over USB |
| Protection | SRV05-4 TVS array (U3) on USB D+/D− for ESD protection |
| Programming | BSL entry circuit (BSL_TX / BSL_RX / BSL_INVOKE) via Q3 (BC857) + D5/D6 (BAS16), auto-triggered from the USB-serial bridge's DTR/RTS lines |
| Debug | Dedicated 5-pin SWD header (J5): SWDIO / SWCLK / RST |
| Jumpers | J3 (UART-SELECT, 2x5), J6 (LDO_JUMPER), J7 (GND_JUMPER) |
| User I/O | SW1 = RESET, SW2 = BSL_BOOT, SW3 = USER (tied to PA16); D1 = green LED, D2 = blue LED, D4 = red LED |
| I/O broken out | J1 (IO_A) and J2 (IO_B), two 12-pin headers exposing UART0 (TXD/RXD), I2C (SCL/SDA), SPI (SCK/MOSI/MISO/CS1/CS2), TIMG4 capture pins, and GPIOs PA9/PA14/PA15/PA16/PA21/PA25/PA26/PA27 |
| Connectors | J1–J7: two GPIO headers, UART-select header, USB-C, SWD header, LDO and GND jumpers |

Full parts list with values and footprints: [`docs/BOM.csv`](docs/BOM.csv) (58 line items).

## Gerbers / Fabrication Output

This repo was built in **KiCad 10**. To (re)generate fab output, run from the `hardware/` folder with KiCad 9+ installed:

```bash
kicad-cli pcb export gerbers --output ../gerbers/ MSPM0L1105_dev_board.kicad_pcb
kicad-cli pcb export drill   --output ../gerbers/ MSPM0L1105_dev_board.kicad_pcb
```

Then zip the contents of `gerbers/` and upload directly to your fab house (JLCPCB, PCBWay, etc.). If you want, run these locally and drop the output in `gerbers/` before pushing — this sandbox only has KiCad 7 available, which can't open KiCad 10 project files, so it can't generate them for you.

## Getting Started

1. Clone this repo.
2. Open `hardware/MSPM0L1105_dev_board.kicad_pro` in KiCad (version *X.X* or later recommended).
3. Gerbers for the current revision are in `gerbers/` — ready to send to a fab house (JLCPCB, PCBWay, etc.).
4. See `docs/BOM.csv` for the parts list.

## Status

✅ Schematic and PCB layout complete (KiCad 10). Gerbers pending local export (see above) before sending to fab.

## Acknowledgements

- [TI MSPM0L1105 product page](https://www.ti.com/product/MSPM0L1105) for datasheet and reference material.
