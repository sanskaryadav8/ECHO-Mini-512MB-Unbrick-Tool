# ECHO Series Firmware Recovery Tool — Repacked by YADAV (512MB Echo Mini Fixed)

> **Credits & Acknowledgments:**
> - **Original tool & repository base:** **blackvllad** (`ECHO-Firmware-Tool`), originally compiled from FiiO Support tools and documentation.
> - **512MB Echo Mini fix, driver `.inf` fix & `echo_mini_v400_flash.img` repack:** **YADAV**.

---

## What Was Fixed in This Repack

If you tried unbricking a **512MB FiiO / Snowsky Echo Mini** with the original package, you likely hit three issues that are all fixed here:

1. **Broken Driver `.inf` Hashes (`Error = 0xE000024B`)**
   - In the original repository, `.inf` files were saved with Unix `LF` line endings instead of Windows `CRLF`, breaking their cryptographic hash against `rockusb.cat`.
   - **Fixed:** All `.inf` files have been restored to Windows `CRLF`, and `.gitattributes` preserves `CRLF` so `DriverInstall.exe` installs cleanly.
2. **Wrong Bootloader in 6-Key `firmware.img` (`RetCode(-3)`)**
   - `Firmware/Echo MIni 8gb/firmware.img` is for the **8GB** version (`HIFIEC`) and bundles an older `BOOT Ver: 4.08` (2016) loader that fails flash detection on newer 512MB Samsung NAND chips (`INFO:CS(1) (480MB) (SAMSUNG)`).
3. **Black Screen / "ECHO NANO" Bug in Original 512MB Recovery Image**
   - The original file in `Firmware/Echo Mini 512mb` (`NANO_F512M_HW_V1.0_SW_V1.5.0_HIFIDAC43131_3KEY_20260714_1725.img`) had the working `BOOT Ver: 4.16` loader, **but** its firmware payload was actually for the **3-key Echo Nano** — causing a 5-second black screen on boot, mounting as `ECHO NANO`, and ignoring `MINIV400.IMG`.
   - **Fixed (`echo_mini_v400_flash.img`):** Built by packing FiiO's working **`BOOT Ver: 4.16`** Samsung NAND loader with FiiO's genuine **512MB 6-key Echo Mini V4.0.0 (`MINIV400.IMG`)** payload.

---

## Firmware Files Included

> **WARNING:** Double-check your device's internal storage size (**512MB vs 8GB**) before flashing. Do **NOT** flash 512MB firmware on an 8GB model or vice versa.

| Device | Firmware Path |
| :--- | :--- |
| **Echo Mini (512MB, 6-Key)** *(Fixed V4.0.0)* | `Firmware/Echo Mini 512mb/echo_mini_v400_flash.img` |
| **Echo Mini (8GB, 6-Key)** | `Firmware/Echo MIni 8gb/firmware.img` |
| **ECHO** | `Firmware/ECHO/HT9099(ECHO)BT53_HW_V1.7_SW_V1.2.0_HIFIDAC43198_6KEY_20260204_1130.img` |
| **Echo Nano (3-Key)** | `Firmware/Echo Nano/NANO_F512M_HW_V1.0_SW_V1.4.0_HIFIDAC43131_3KEY_20260623_1024.img` |

---

## Step-by-Step Recovery Guide (Windows 10 / 11)

### 1. Temporarily Disable Driver Signature Enforcement (Windows 11)
Windows 11 blocks older cross-signed `rockusb.sys` drivers (`0xc0e90002`) unless temporarily disabled for the session:
1. Hold **Shift** and click **Restart** in the Start Menu.
2. Go to **Troubleshoot** → **Advanced options** → **Startup Settings** → **Restart**.
3. After rebooting, press **`7`** (*Disable driver signature enforcement*).

### 2. Install the Rockusb Driver
1. Open `DriverAssistant_v5.14/DriverInstall.exe` as **Administrator**.
2. Click **Install Driver** and wait for the success prompt.

### 3. Enter MaskRom Mode
1. Open `RKDevelopTool.exe` as **Administrator**.
2. Connect your Echo Mini via USB:
   - **If the bottom bar says `Found One MSC Device`:** Click **`Switch`** once so it changes to **`Found One MASKROM Device`**.
   - **If it does not appear (black screen / dead):** Unplug the player, press and hold **all front buttons except Power** (e.g., **Menu + Play + Vol-**), plug in the USB cable while holding, and release after 3 seconds. The bottom bar should say **`Found One MASKROM Device`**.

### 4. Flash the Firmware
1. Click **`...`** next to **FW Path** at the top of `RKDevelopTool.exe`.
2. Select `Firmware/Echo Mini 512mb/echo_mini_v400_flash.img` (for the 512MB Echo Mini).
3. Click **`Erase ID Block`**.
4. Click **`Upgrade`** and wait until the log reports **`Download Firmware Success`**.

### 5. Boot Up
Unplug the USB cable and hold the **Power** button for 3–5 seconds. Your Echo Mini will boot straight into **V4.0.0**!
