# VIJA synthesizer

Raspberry PICO digital synthesizer based on **Mutable Instruments Braids** macro oscillator 
in semi-modular format.  

🎥 [Watch demo](https://www.youtube.com/watch?v=sYbr-VH2LuQ)

---

## 🚀 Features

* **40+ Oscillator Engines:** Includes VA, FM, Additive, Wavetable, Physical Modeling and Drums.
* **4-Voice Polyphony:** Per-sample AR (Attack-Release) envelopes.
* **OLED Interface:** Real-time feedback with a menu system and a oscilloscope.
* **Intelligent Modulation:** CV input and modulation controls using midi.
* **Internal Filter:** Integrated State Variable Filter (SVF) with Low-Pass and Resonance.
* **Dual MIDI:** Support for both USB MIDI and classic UART MIDI.

---

## 🕹 Menu System & UI
The synthesizer operates in three primary display modes:
1.  **ENGINE SELECT:** Rotate the encoder to scroll through engines  
2.  **SETTINGS:** Click the encoder button to cycle through parameters
    * **VOLUME:** Global gain control
    * **A/R ENVELOPES:** Adjust the "snappiness" (Attack) or "fade" (Release) of notes
    * **FILTER:** Enable/Disable the Filter
    * **CV:** CV modulation mode
    * **MIDI:** Enable MIDI CC and disable CV
    * **MIDI CH:** Set the input channel (1-16)
    * **OSCILOSCOPE Toggle:** On / Off
    * **SAVE SETTINGS:** Long press button to save menu settings
    * **EXIT MENU**  
4.  **OSCILOSCOPE:** Automatically engages after 10 seconds to visualize the current waveform

### Filter Mode (Default)

- Timbre & Color (default)  
- CV1 & CV2 → Filter cutoff & resonance

### CV Modulation Mode

- Timbre & Color → Control modulation depth  
- CV1 & CV2 →  Modulation source

### MIDI Modulation Mode

- Timbre & Color (Soft takeover mode)

  Align coresponding MIDI CC value with Timbre or Color pot value to release or vice versa (screen indicator)
    
- CV1 & CV2 → Free for future functions
  
### All Modes OFF

- Timbre & Color (default)  
- CV1 → Engine selection
- CV2 → FM MOD

---

## 📟 MIDI Implementation (CC Chart)

VIJA responds to the following Control Change (CC) messages on the selected MIDI Channel:

| CC # | Parameter |
| :--- | :--- |
| **7** | Master Volume |
| **8** | Engine Select |
| **9** | Timbre |
| **10** | Color |
| **11** | Envelope Attack |
| **12** | Envelope Release |
| **15** | FM Modulation |
| **16** | Timbre Modulation Amount |
| **17** | Color Modulation Amount |
| **64** | Sustain (Hold notes) |
| **71** | Filter Resonance |
| **74** | Filter Cutoff |

---

## 💻 Software Setup

1.  **Arduino IDE:** Install the [Earle Philhower Pico Core](https://github.com/earlephilhower/arduino-pico)
2.  **Libraries:**

- arduinoMI project (ported Mutable Instruments libraries)
  - STMLIB  https://github.com/poetaster/STMLIB  
  - BRAIDS  https://github.com/poetaster/BRAIDS  

- I2S
 
- Adafruit TinyUSB

- Adafruit SSD1306

- LittleFS  & ArduinoJson for saving settings

3.  **Compilation Settings:**  
   * **RP2040:**
              - Optimize: Fast (-Ofast)    
              - CPU Speed: 240MHz (Overclock) depending on the sample rate  
              - Sample rate 32000 (4 voices) / 44100 (3 voices)  
   * **RP2350:**  
              - Optimize: Fast (-Ofast)  
              - Sample rate 48000
---

## ⚡ Schematic & Wiring

For this project I use RP2040 Zero model, so adjust GPIO numbers to your board.

### 1. Audio Output (I2S DAC)
Connect your **PCM5102** or similar I2S DAC:
* **VCC/VIN** -> Pico 3.3V (Pin 36)
* **GND** -> Pico GND
* **LCK (LRCK)** -> Pico GP11 (Pin 15) 
* **BCK (BCLK)** -> Pico GP10 (Pin 14)
* **DIN (DATA)** -> Pico GP9 (Pin 12)

### 2. Control & Display
* **OLED SDA** -> Pico GP0 (Pin 1)
* **OLED SCL** -> Pico GP1 (Pin 2)
* **Encoder CLK** -> Pico GP2 (Pin 4)
* **Encoder DT** -> Pico GP3 (Pin 5)
* **Encoder SW** -> Pico GP4 (Pin 6)

### 3. Potentiometers (ADC)
Connect the outer pins to 3.3V and GND, and the center wiper to:
* **Pot 1 (Timbre)** -> GP26 (ADC0)
* **Pot 2 (Color)** -> GP27 (ADC1)
* **Pot 3 (CV1)** -> GP28 (ADC2)
* **Pot 4 (CV2)** -> GP29 (ADC3)

### 4. MIDI Input (UART)
Connect your MIDI Jack via a 6N138 optocoupler circuit to **GP13 (Pin 17)**.

---

## 📜 License
* (c) 2025 Vadims Maksimovs - GPLv3
 
* **Core Libraries:** stmlib/braids - MIT License (Copyright 2020 Emilie Gillet)
* **Ported code:** stmlib/braids - MIT License (Copyright 2025 Mark Washeim)
  
---
##  Version history
* 2026-02-03 - v1.0.1
* 2026-02-02 - First release v1.0
