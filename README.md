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
├── u_codec : top_codec
│   ├── u_i2s : i2s
│   │   ├── u_timing_gen : timing_gen
│   │   └── u_rx : rx
│   └── u_i2c : i2c
│       ├── u_i2c_reg_cfg : i2c_reg_cfg
│       └── u_i2c_dri : i2c_dri
│
├── u_fft : top_fft
│   ├── u_frame_packer : frame_packer
│   ├── u_fft_wrapper : fft_wrapper
│   │   └── u_xfft : xfft_0 (Xilinx FFT IP)
│   ├── u_complex_to_mag : complex_to_mag
│   ├── u_band_accum : band_accum
│   └── u_band_buffer : band_buffer
│
└── u_led : top_led
    ├── u_spectrum_to_led : spectrum_to_led
    └── u_ws2812_dri : ws2812_dri
