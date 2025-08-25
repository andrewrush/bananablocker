# 🍌 BananaBlocker

A Pi-hole-like ad blocker that runs on ESP32 microcontrollers. Block ads and trackers at the DNS level for your entire network!

## Features

- **DNS-based Ad Blocking**: Intercepts DNS requests and blocks known ad servers.
- **Web-based Admin Panel**: Beautiful, responsive interface for management.
- **Initial Setup Wizard**: Captive portal for easy WiFi configuration.
- **Statistics Dashboard**: Track blocked vs allowed requests.
- **Custom Block Lists**: Add/remove domains via web interface.
- **Factory Reset**: Hardware button reset (4-second press on BOOT button).
- **Persistent Storage**: Settings saved to SPIFFS (flash file system).

## Hardware Requirements

- **ESP32**: Any ESP32 module with CP2102 USB-to-serial chip (e.g., ESP32 Dev Module, NodeMCU-32S).
- **Power Supply**: 3.3V or USB power.
- **BOOT Button**: Built-in BOOT button (GPIO0) for factory reset and bootloader mode.

## Quick Start

### 1. Install Dependencies

1. **Install Arduino IDE**:
   - Download and install the latest Arduino IDE (2.x or 1.8.x) from [arduino.cc](https://www.arduino.cc/en/software).
2. **Install ESP32 Arduino Core**:
   - Open Arduino IDE, go to **File > Preferences**.
   - Add to "Additional Boards Manager URLs": `https://raw.githubusercontent.com/espressif/arduino-esp32/master/package_esp32_index.json`.
   - Go to **Tools > Board > Boards Manager**, search for "esp32," and install the **ESP32 by Espressif Systems** package (version 2.x or higher).
3. **Install Drivers**:
   - Download and install the drivers e.g., **CP210x USB to UART Bridge VCP Drivers** from [Silicon Labs](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers). Check your device description for available compatible drivers.
   - Verify the ESP32 is detected in Device Manager (Windows) or `/dev/tty*` (Linux/macOS) as a COM port (e.g., COM18).

### 2. Upload the Code

1. Open `BananaBlocker.ino` in Arduino IDE.
2. Select **Tools > Board > ESP32 Arduino > ESP32 Dev Module**.
3. Set:
   - **Upload Speed**: 921600 (or 115200 if issues occur).
   - **Partition Scheme**: Default 4MB with spiffs.
   - **CPU Frequency**: 240MHz.
   - **Flash Frequency**: 80MHz.
4. Connect your ESP32 via USB.
5. Select the correct port under **Tools > Port** (e.g., COM18 on Windows).
6. Click **Upload** to flash the sketch.
   - If prompted, press and hold the **BOOT** button on the ESP32 to enter bootloader mode, then release after the upload starts.
7. Open **Serial Monitor** (115200 baud) to verify startup.

### 3. Initial Setup

1. **First Boot**: Device creates a "BananaBlocker" WiFi hotspot.
2. **Connect**: Join the hotspot (SSID: `BananaBlocker`, password: `banana123`).
3. **Configure**: Open a browser and go to `http://192.168.4.1`. The setup page should load automatically (captive portal).
4. **WiFi Setup**: Enter your WiFi SSID, password (optional), and set an admin password.
5. **Complete**: The device restarts and connects to your WiFi network. Check the Serial Monitor for the assigned IP address.

### 4. Access Admin Panel

1. **Find IP**:
   - Check the Serial Monitor for the IP address (e.g., `192.168.1.x`).
   - Alternatively, use `http://bananablock.local` (mDNS) or check your router’s DHCP client list for “BananaBlocker.”
2. **Login**: Navigate to the device’s IP in a browser and enter the admin password.
3. **Dashboard**: View statistics, manage blocklists, and configure settings.

## Required Libraries
					  
						   
					  
					
				 
			 
   

All required libraries are included in the **ESP32 Arduino Core**:
- `WiFi` (built-in)
- `WebServer` (built-in)
- `ESPmDNS` (built-in)
- `DNSServer` (built-in)
- `WiFiUDP` (built-in)
- `EEPROM` (built-in)
- `SPIFFS` (built-in)

No additional library installation is needed.

## Default Configuration

- **AP SSID**: `BananaBlocker`
- **AP Password**: `banana123`
- **Web Port**: `80`
- **DNS Port**: `53`



## Admin Dashboard

Access via the device’s IP address after setup:

**Login Page:**
- Shows current statistics (blocked/allowed requests).
- Password-protected access.

**Dashboard:**
- Real-time statistics (blocked/allowed requests, number of blocked domains).
- Domain management interface.
- Toggle ad blocking on/off.
- System settings (including factory reset).

**Block List Management:**
- Add domains to block.
- Remove existing domains.
- View all blocked domains (200 pre-configured).

## DNS Configuration

To use BananaBlocker as your DNS server:

**Option 1: Router Configuration**
- Set your router’s primary DNS to the BananaBlocker’s IP address (found in Serial Monitor or dashboard).
- All devices on the network will use BananaBlocker for ad blocking.


**Option 2: Device Configuration**
- Set individual devices’ DNS to the BananaBlocker’s IP address.

## Factory Reset

**Software Reset:**
- Access admin panel → System Settings → Factory Reset.

**Hardware Reset:**
- Press and hold the **BOOT button** (GPIO0) for **4+ seconds**.
- The device will restart in AP mode (setup mode).

## Default Blocked Domains

														  

BananaBlocker comes pre-configured with 200 common ad servers, including:
- `doubleclick.net`
- `googlesyndication.com`
- `google-analytics.com`
- `googletagmanager.com`
- `facebook.com/tr`
- `ads.yahoo.com`
- `advertising.com`
					   
- `amazon-adsystem.com`
- `outbrain.com`
- `taboola.com`
- `scorecardresearch.com`
- `adsafeprotected.com`
- `moatads.com`
- *(Full list in `default_blocked_domains` array in the code)*

## Technical Details

### Memory Usage
- **Flash**: ~1.06MB (80% of 1.31MB) for the sketch and SPIFFS.
- **RAM**: ~55KB (16% of 327KB) for global variables, leaving ~272KB for local variables.
- **SPIFFS**: Stores configuration (`/config.dat`) and web assets.
- **EEPROM**: 512 bytes for configuration backup.

### Network Configuration
- **Station Mode**: Connects to your WiFi network.
- **AP Mode**: Creates a captive portal for setup (`192.168.4.1`).
- **mDNS**: Accessible via `bananablock.local` (if supported by your network).



## Troubleshooting

### Can't Connect to Setup Network
- Ensure the device is in AP mode (first boot or after reset).
- Verify SSID: `BananaBlocker`, password: `banana123`.
- Try forgetting and reconnecting to the network.

### Setup Page Won't Load
- Navigate directly to `http://192.168.4.1`.
- Clear browser cache or try a different browser.
- Check Serial Monitor for SPIFFS or AP initialization errors.

### WiFi Connection Fails
- Verify SSID and password in setup.
- Ensure a 2.4GHz network (ESP32 supports 2.4GHz only).
- Move the device closer to the router.



### DNS Not Working
- Confirm the device’s IP is set as the DNS server in router or device settings.
- Check Serial Monitor for DNS server startup messages.
- Restart router/devices after setting DNS.

### Factory Reset Not Working
- Hold the BOOT button for a full 4+ seconds.
- Check Serial Monitor for reset confirmation.
- Try software reset via the admin panel.

### Upload Issues
- If you see "No DFU capable USB device available":
  - Ensure CP210x drivers are installed.
  - Use a data-capable USB cable.
  - Manually enter bootloader mode: Hold BOOT, press/release EN (if available), release BOOT, then upload.
- Enable verbose output in **File > Preferences** to debug upload errors.

## API Endpoints

For developers and advanced users:

```   
GET  /                    - Main page (login or dashboard)
GET  /setup              - Setup page (AP mode only)
POST /setup              - Save WiFi configuration
POST /login              - Authenticate user
GET  /logout             - Logout user
GET  /api/stats          - Get statistics (JSON)
GET  /api/blocklist      - Get blocked domains (JSON)
POST /api/blocklist      - Add domain to blocklist
DELETE /api/blocklist    - Remove domain from blocklist
POST /api/toggle         - Toggle ad blocking
POST /api/reset          - Factory reset
```

## Development

### Customization

**Change Default Passwords:**
```cpp
const char* ap_ssid = "BananaBlocker";
const char* ap_password = "your_password";
strcpy(config.admin_password, "your_admin_password");
```   

**Add More Default Domains:**
```cpp
const char* default_blocked_domains[] = {
    "your.domain.com",
    // ... existing domains
};
``` 

**Modify UI Colors:**
Edit the CSS in the getSetupPage(), getLoginPage(), and getDashboardPage() functions.

### Building from Source

1. Clone or download the project.
2. Open BananaBlocker.ino in Arduino IDE.
3. Verify board settings:
   - Board: ESP32 Dev Module
   - Upload Speed: 921600
   - Partition Scheme: Default 4MB with spiffs
   - CPU Frequency: 240MHz
   - Flash Frequency: 80MHz
4. Upload to the ESP32.

## License

This project is open source. Feel free to modify and distribute.

## Support
											   
For issues and questions:
1. Check troubleshooting section
2. Verify hardware connections
3. Check serial monitor output
4. Reset to factory defaults

---

**Made with 🍌 for a better, ad-free internet!**
