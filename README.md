OV5640 Complete Register Reference
> **Maintained by [Camemake](https://camemake.eu)** — camera module specialists
This repository contains a comprehensive register reference for the OmniVision OV5640 — a 5-megapixel 1/4" CMOS image sensor with OmniBSI™ technology, widely used in embedded camera modules. The goal is to fill the gaps left by the official datasheet, particularly around ISP pipeline tuning, lens shading correction, AWB, and AEC configuration.
---
📁 Contents
File	Description
`OV5640_datasheet.pdf`	Official OmniVision OV5640 product specification (v2.03)
`OV5640_Register_Map.xlsx`	Complete register map — 520 registers extracted from the datasheet and cross-referenced with real-world tuning files
`OV5640-1B Product-Specification-CSP_Version-...`	CSP (Chip Scale Package) variant product spec
`ov5640_registers_ini_examples.rar`	Real-world register initialisation files from a MediaTek MT6573 platform (AE, AWB, ISP pipeline, PLine tables)
---
📊 Register Map — Sheet Guide
The Excel register map (`OV5640_Register_Map.xlsx`) is organised into 6 sheets:
1. Register Map (main sheet)
520 unique hardware registers, sorted by address and colour-coded by functional block:
Colour	Category
Blue	System Control, Pad/IO
Green	Clock / PLL
Yellow	AEC / AGC / AEC Control
Red tint	AWB Gain, Analog
Purple	ISP Control, Saturation, Effects
Amber	Lens Shading Correction (LSC)
Teal	MIPI / DVP Output
Each row contains: Address · Register Name · Category · Default Value · R/W · Description
Use the column auto-filter (row 1) to quickly browse by category, access type, or search descriptions.
2. AE Parameters
Extracted AEC/AGC tuning: metering mode, weighting matrix (central-weighted 5×5), saturation thresholds, face-AE settings, EV compensation table (±2 EV), and AF frame rate PLine.
3. AWB Parameters
White balance algorithm configuration for all 8 modes — Auto, Daylight, Cloudy, Shade, Twilight, Fluorescent, Warm Fluorescent, Incandescent. Includes XY color-space window bounds and light-source probability LUTs.
4. ISP Pipeline (MT6573)
Active index selections and raw register values for every ISP block: Shading, OB, Demosaic, Dead-Pixel correction, NR1, NR2, Edge Enhancement, Saturation, Contrast, Hue, CCM (3 color temperature sets), and Gamma.
5. PLL & Sensor Init
OV5640 PLL registers (`0x0300–0x030B`) with multiplier and pre-divider values as used on the MT6573 platform.
6. AE PLine Table (Preview)
Sample of the 132-entry AEC exposure program line for 60 Hz, showing the full exposure/gain trajectory from BV=+9.19 (bright sunlight, 320 µs) down to BV=−3.9 (maximum night mode, 196 ms + gain).
---
🔍 Key Insight — What the Datasheet Doesn't Tell You
The OV5640 datasheet covers the sensor's own register space (`0x3000–0x6000`). However, many parameters that people search for — lens shading correction coefficients, AWB algorithm tuning, CCM matrices, gamma curves, and noise reduction strength — are not OV5640 registers at all.
When paired with an application processor, these functions are handled entirely by the host ISP, with the OV5640 delivering raw Bayer data. The ISP tuning lives in the AP's own register/NVRAM space. The `.h` files in the ini examples archive document exactly these host-side parameters, which is why they are absent from the OmniVision datasheet.
This repository cross-references both sides so you have the full picture.
---
🛠 Usage Tips
Bring-up / init sequence: Start with the PLL registers (`0x3035`, `0x3036`, `0x3037`, `0x3108`), then apply windowing (`0x3800–0x3813`), then output format and MIPI/DVP lane config.
Mirror / Flip: Registers `0x3820` and `0x3821` — remember to re-check Bayer pattern phase after changing these.
AEC manual override: Set bit[0] of `0x3503` to disable AEC and write exposure directly to `0x3500–0x3502`.
AWB manual override: Clear bit[1] of `0x3406` and write R/G/B gains to `0x3400–0x3405`.
Test pattern: Set bit[7] of `0x503D` for a colour-bar output, useful for pipeline verification.
---
📄 License
Register map and documentation released under CC BY 4.0 — free to use, share, and adapt with attribution.
The OV5640 datasheet is the property of OmniVision Technologies and is reproduced here for reference only.
---
🔗 About Camemake
→ camemake.eu
