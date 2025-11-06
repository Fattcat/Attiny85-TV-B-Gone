# 📺 TV-B-Gone for ATtiny85 (EU-Only Version)

> A tiny, battery-powered device that turns off TVs using IR signals.  
> This version includes **only European (EU) power-off codes**, is optimized for **ATtiny85**, and compiles using standard AVR-GCC toolchains on Linux.

---

## 📦 Required Components

| Component | Qty | Notes |
|----------|-----|-------|
| ATtiny85 | 1 | DIP-8 package preferred |
| Arduino Uno | 1 | Used temporarily as ISP programmer |
| IR LED | 1 | e.g., TSAL6200, TSAL7600 |
| Push button | 1 | Momentary tactile switch |
| Capacitor | 1 | **10 µF electrolytic**, 16 V+ |
| Transistor (recommended) | 1 | e.g., 2N2222, BC547, or PN2222A |
| Resistors | 2–3 | 100 Ω (IR LED), 1 kΩ (transistor base), 220 Ω (status LED, optional) |
| Power source | 2× AA batteries | 3 V (ideal), or 3.3–5 V external supply |

---

## 🔌 Wiring

### 1. Programming (ATtiny85 → Arduino Uno as ISP)

| ATtiny85 (physical pin) | Name | Arduino Uno |
|-------------------------|------|-------------|
| 1 | RESET | Pin 10 |
| 4 | GND | GND |
| 5 | **PB0 (IRLED)** | Pin 11 (MOSI) |
| 6 | PB1 (MISO) | Pin 12 |
| 7 | PB2 (SCK) | Pin 13 |
| 8 | VCC | **5 V** |

> ⚠️ **Critical**: Add a **10 µF capacitor** between **Uno’s RESET and GND** (– to GND, + to RESET).  
> Without it, the Uno resets when avrdude opens the serial port.

---

### 2. Standalone Operation

| ATtiny85 | Connection |
|---------|------------|
| Pin 1 (RESET) | → Push button → GND |
| Pin 5 (PB0) | → 100 Ω → **IR LED anode** → **IR LED cathode** → GND  
| *(better option)* | → 1 kΩ → transistor base (2N2222), emitter → GND, collector → IR LED cathode, IR LED anode → VCC |
| Pin 8 | VCC (3–5 V) |
| Pin 4 | GND |

> 💡 Optional status LED: Pin 7 (PB2) → 220 Ω → LED → GND

---

## 🐧 Setup (Linux: Kali/Ubuntu/Debian)

```bash
# 1. Install toolchain
sudo apt update && sudo apt install -y gcc-avr avr-libc avrdude make git

# 2. Clone firmware
git clone https://github.com/adafruit/TV-B-Gone-kit.git
cd TV-B-Gone-kit/firmware
