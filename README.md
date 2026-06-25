# WiFi Handshake Capture Tool

<div align="center">
  
[![GPLv3 License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE.md)
[![Ethical Use](https://img.shields.io/badge/INTENDED_USE-Pentesting_Research-red)](https://github.com/Electro-Gamma/esp32-handshake-capture/blob/main/README.md#ethical-use)
[![Platform](https://img.shields.io/badge/Platform-ESP32-important)](https://github.com/Electro-Gamma/esp32-handshake-capture/blob/main/README.md#requirements)
[![Warning](https://img.shields.io/badge/WARNING-Legal_Restrictions-yellow)](https://github.com/Electro-Gamma/esp32-handshake-capture/blob/main/README.md#legal-disclaimer)


</div>

## ⚠️ Legal Disclaimer

This software is provided strictly for educational and authorized testing purposes only.  
Unauthorized use of this tool against networks you do not own or have written permission to test is illegal in most jurisdictions.  
The developers are not responsible for any misuse, damage, or legal consequences.

---

##  Features & My Improvements

This repository is a fork of the original project, a litle bit modified to fix critical bugs and add new management features.

### 🌟 What I Improved & Fixed:
* **SPIFFS Filename Limit Fix:** Resolved a critical bug where long SSIDs caused compilation/runtime crashes due to the SPIFFS 31-character filename limit. SSIDs are now safely truncated to 8 characters. (may be a bit annoying bcs you cant see the whole SSID in the .pcap file but still better than having a logic flaw in your code ig)
* **Web Routing & Sanitization Fix:** Fixed a `404 File Not Found` error during download by adding proper leading slash (`/`) sanitization to URL parameters. (I have no idea why the original code doesnt have this)
* **Naming Conflict Resolution:** Renamed the internal `Network` struct to `WifiNetwork` to prevent naming collisions with official ESP32 system libraries.
* **New Feature: Web-Based File Management:** Implemented a brand new `/delete` endpoint and updated the web UI with a list that allows downloading and deleting `.pcap` files directly from your browser.

## Features

- Captures WPA/WPA2 EAPOL 4-way handshakes
- Saves captured data in PCAP format
- Web interface to download handshakes
- Portable and lightweight with ESP32
- Passive monitoring (no injection required)
- Works with tools like Wireshark, Aircrack-ng, Hashcat (after conversion)

---

## Requirements

- ESP32 Development Board (e.g., DevKit v1)
- USB cable (Micro-USB or USB-C depending on your board)
- Arduino IDE or PlatformIO
- Serial Monitor (115200 baud)
- WiFi network you are authorized to test

---

## Flashing Instructions

### Option 1: Arduino IDE

1. Install ESP32 board support:
   - File → Preferences → Additional Board URLs:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
2. Open Boards Manager and install "ESP32" (3.3.10 is recommended for this fork)
3. Load `esp32-handshake-capture.ino`
4. Select:
   - Board: ESP32 Dev Module
   - Port: Your correct COM/tty port
5. Requirements & Compilation Fix

Before compiling the sketch in Arduino IDE, make sure you have the correct environment setup to avoid compilation errors.

### 1. ESP32 Arduino Core Version
This project is tested and optimized for the official **Espressif esp32**.
* **Recommended Version:** v3.3.10
* Older versions (like v2.x) might cause naming conflicts or lack support for specific 802.11 functions.

### 2. Multiple Definition Fix (`platform.txt`)
Due to duplicate symbol definitions between some internal Espressif Wi-Fi libraries, the compiler will likely fail with a `multiple definition of...` error. 

To fix this, you need to add the `-Wl,-zmuldefs` linker flag:

    1. Locate your ESP32 `platform.txt` file (usually found in `AppData/Local/Arduino15/packages/esp32/hardware/esp32/[version]/`).
    2. Open `platform.txt` in a text editor of your choice.
    3. Find the line starting with `compiler.c.elf.flags=` or `compiler.cpp.elf.flags=`.
    4. Append ` -Wl,-zmuldefs` to the end of that line.
    5. Save the file and restart Arduino IDE.
6. Click "Upload"

   

### Option 2: PlatformIO (VS Code)

```bash
git clone https://github.com/Electro-Gamma/esp32-handshake-capture.git
cd esp32-handshake-capture
pio run --target upload
```

---

## Running the Tool

1. Open a serial monitor at 115200 baud.
2. The ESP32 will scan for nearby WiFi networks.
3. Select the target network by its SSID/BSSID.
4. The handshake is captured passively.
5. ESP32 saves the handshake as a `.pcap` file.
6. Connect to the ESP32's web interface to download the capture.

---

## Output

Captured handshakes are stored in `.pcap` format and compatible with:
- Wireshark
- aircrack-ng
- hcxpcapngtool / hashcat

---

## Usage Policy

This tool is intended for:
- Security researchers
- Cybersecurity training labs
- Authorized penetration testers
- Educational demonstrations

You must:
- Use only on networks you own or have explicit permission to test
- Follow local, national, and international laws
- Accept full responsibility for all usage

---

## Legal Notice

Be aware of applicable laws that may govern or restrict the use of this tool, including but not limited to:
- Computer Fraud and Abuse Act (CFAA)
- General Data Protection Regulation (GDPR)
- Digital Millennium Copyright Act (DMCA)
- National and regional cybercrime laws

The authors are not liable for illegal or unethical use.

---

## License

This project is licensed under the **GNU General Public License v3.0 (GPLv3)**.  
See the [LICENSE](LICENSE.md) file for full terms.

---

## Contact

- GitHub: [Electro-Gamma](https://github.com/Electro-Gamma)

---

Stay ethical. Use responsibly.
