# ASUS H110M-E/M.2 — OpenCore macOS Sequoia EFI

Clean, production-ready **OpenCore 1.0.8** EFI built from scratch for the **ASUS H110M-E/M.2** motherboard running **macOS Sequoia 15.8.3** (Build `24H241`).

> **IMPORTANT — Generate Your Own SMBIOS:**
> All personal serials (`SystemSerialNumber`, `MLB`, `SystemUUID`, and `ROM`) have been sanitized. Before booting or signing into Apple Services (iCloud, iMessage, FaceTime), generate fresh **`iMac19,1`** serials using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).

## Hardware Specifications

| Component | Model / Details |
| :--- | :--- |
| **Motherboard** | ASUS H110M-E/M.2 (BIOS `3805`) |
| **CPU** | Intel Core i5-7400 @ 3.00 GHz (Kaby Lake) |
| **iGPU** | Intel HD Graphics 630 (1536 MB VRAM, Metal 3) |
| **Audio** | Realtek ALC887 (`alcid=1`) |
| **Ethernet** | Realtek RTL8111H Gigabit LAN |
| **Storage** | Kingston SA400S37240G (SATA SSD, TRIM enabled) + M.2 slot ready (`NVMeFix.kext`) |
| **SMBIOS** | `iMac19,1` |
| **Bootloader** | OpenCore `1.0.8` (OpenCanopy GUI — `Acidanthera\GoldenGate`) |
| **macOS Tested** | macOS Sequoia `15.8.3` (`24H241`) |

## Status

### Working
* **Graphics (QE/CI):** Full Metal 3 hardware acceleration on Intel HD Graphics 630 (1536 MB) via **HDMI / DVI-D**.
* **Audio:** Built-in line-out, line-in, and digital audio via `AppleALC.kext` (`alcid=1`).
* **Ethernet:** Gigabit (`1000baseT full-duplex`) via `RealtekRTL8111.kext`.
* **CPU Power Management:** Native XCPM (`machdep.xcpm.mode: 1`) via custom `SSDT-PLUG`.
* **USB Ports:** Custom mapped via `USBToolBox.kext` + `UTBMap.kext`.
* **Storage:** SATA SSD with TRIM (`sudo trimforce enable`) + `NVMeFix.kext` pre-installed for M.2 NVMe upgrades.
* **Boot Menu:** Graphical OpenCanopy picker (`PickerMode: External`, clean boot without `-v`).

### Notes & Limitations
* **VGA Output:** Not supported by macOS. Use **HDMI** or **DVI-D** (or an active HDMI-to-VGA adapter).
* **Sleep Mode:** Disabled for desktop stability (`sudo pmset -a sleep 0 displaysleep 15 tcpkeepalive 0`). Display sleep works normally.

## BIOS Configuration (ASUS BIOS 3805)

* **Advanced -> System Agent (SA) Configuration:** `VT-d`: Disabled | `Above 4G Decoding`: Enabled | `Primary Display`: CPU Graphics | `DVMT Pre-Allocated`: 128M (or 64M)
* **Advanced -> PCH Storage Configuration:** `SATA Mode Selection`: AHCI
* **Advanced -> Onboard Devices Configuration:** `Serial Port`: Disabled
* **Advanced -> USB Configuration:** `XHCI Hand-off`: Enabled
* **Boot:** `Fast Boot`: Disabled | `Launch CSM`: Disabled | `Secure Boot -> OS Type`: Other OS

## ACPI & Kexts

* **ACPI (Manual SSDTs):** `SSDT-PLUG`, `SSDT-EC`, `SSDT-HPET`
* **Kexts:** `Lilu`, `WhateverGreen`, `VirtualSMC`, `SMCProcessor`, `SMCSuperIO`, `AppleALC`, `RealtekRTL8111`, `RestrictEvents`, `CryptexFixup`, `USBToolBox`, `UTBMap`, `NVMeFix`

## Post-Install Commands

Enable TRIM on third-party SATA SSDs:

    sudo trimforce enable

Configure desktop power settings (disable system sleep, keep 15-min display sleep):

    sudo pmset -a hibernatemode 0 autopoweroff 0 standby 0 proximitywake 0 powernap 0 womp 0
    sudo pmset -a sleep 0 displaysleep 15 tcpkeepalive 0
