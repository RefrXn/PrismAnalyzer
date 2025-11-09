[![License: CERN-OHL-W-2.0](https://img.shields.io/badge/License-CERN--OHL--W--2.0-blue.svg)](LICENSE)
[![FPGA](https://img.shields.io/badge/FPGA-Verilog--2001-green.svg)]()
[![Status](https://img.shields.io/badge/status-active-success.svg)]()

# PrismAnalyzer
FPGA implementation of a **32×8 WS2812B LED matrix spectrum visualizer**, written in **Verilog-2001**, using the **WM8731 audio codec** for input and a Xilinx FFT IP core for spectral analysis.

---

## 📘 Overview
PrismAnalyzer captures audio from the WM8731 codec via I²S, performs real-time FFT on the audio stream, and maps the frequency bands to a WS2812 LED matrix.  
The system is designed for real-time operation on FPGA hardware and is divided into three primary functional blocks:

- **Codec Subsystem (`top_codec`)** – Handles audio acquisition and codec configuration via I²S and I²C.  
- **FFT Subsystem (`top_fft`)** – Performs frame packing, FFT, magnitude conversion, and band accumulation.  
- **LED Subsystem (`top_led`)** – Converts spectral magnitudes to LED color patterns and drives WS2812 timing.

---

## ⚙️ Module Hierarchy
```text
top.v
├── top_codec.v
│   ├── i2s.v
│   │   ├── timing_gen.v
│   │   └── rx.v
│   └── i2c.v
│       ├── i2c_reg_cfg.v
│       └── i2c_dri.v
│
├── top_fft.v
│   ├── frame_packer.v
│   ├── fft_wrapper.v
│   │   └── xfft_0.xci   (Xilinx FFT IP)
│   ├── complex_to_mag.v
│   ├── band_accum.v
│   └── band_buffer.v
│
└── top_led.v
    ├── spectrum_to_led.v
    └── ws2812_dri.v
```

---

## 🔄 Data Flow
```text
Audio In (WM8731 / I²S)
        ↓
     top_codec
        ↓
     top_fft
        ↓
  FFT → Magnitude → Band Accumulation
        ↓
     top_led
        ↓
 Spectrum Mapping → WS2812 Timing
        ↓
 LED Matrix Visualization
