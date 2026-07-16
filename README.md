# OpenWrt for LITE-ON WPX8324 (IPQ50xx) 🚀

This repository provides an automated GitHub Actions workflow and custom firmware for the **LITE-ON WPX8324-BT** router, powered by the Qualcomm IPQ50xx architecture.

[🇮🇩 Baca versi Bahasa Indonesia di bawah](#-versi-bahasa-indonesia)

---

## 🎖️ Acknowledgments & Credits
A massive shoutout and special thanks to **[sib0ndt](https://github.com/sib0ndt)** for the initial OpenWrt port, Device Tree (DTS) configuration, caldata extraction scripts, and the core installation methods. This repository builds upon their outstanding foundation.

## 📋 Device Specifications
| Feature | Description |
| :--- | :--- |
| **Manufacturer** | LITE-ON Technology Corporation |
| **Model** | WPX8324 |
| **Device Type** | Indoor Access Point / Mesh AP |
| **Wi-Fi Standard** | IEEE 802.11ax (Wi-Fi 6) |
| **Wireless Class** | AX3000 |
| **2.4 GHz** | 2×2 MIMO |
| **5 GHz** | 2×2 MIMO |

**Hardware Details:** [FCC ID PPQ-WPX8324 Internal Photos](https://fccid.io/PPQ-WPX8324/Internal-Photos/Internal-Photos-6344601)

## ✨ Custom Build Features
This specific build (`v25.12.5`) includes:
* **USB Modem & Tethering Ready:** Pre-baked with `kmod-usb-net`, `qmi-wwan`, `cdc-ncm`, `rndis`, and `modemmanager`.
* **Anti-Kernel Panic:** The `kmod-qrtr` module (which causes bootloops on devices without internal modems) has been safely stripped from the build.
* **Auto Wi-Fi Config:** Wi-Fi is enabled by default with the SSID `WPX8324-BT`.

---

## 🛠️ Installation Instructions

### 1. Test Boot Without Flashing (Safe Mode)
The `initramfs` image can be booted from RAM first to test OpenWrt without modifying the NAND flash.
From the bootloader (U-Boot), load the `initramfs` image using TFTP:
```bash
tftpboot 0x44000000 openwrt_qualcommax_ipq50xx_wpx8324_ap_mp03_5_c1_initramfs.itb
```
Boot the loaded image as required by your bootloader.
After OpenWrt has started, access LuCI (192.168.1.1) and verify that the device is working correctly, including Ethernet, wireless, LEDs, and other required functionality.

If the system is working as expected, install the sysupgrade image directly through LuCI:
* In LuCI, navigate to: **System -> Backup / Flash Firmware**
* Upload the `openwrt_..._squashfs.bin` image and proceed with the firmware upgrade.

### 2. Advanced Installation (TFTP UBI Flash)
Load the UBI image through TFTP:
```bash
tftpboot 0x44000000 openwrt_25_12_5_qualcommax_ipq50xx_wpx8324_ap_mp03_5_c1_squashfs.ubi
```
Initialize NAND and select the NAND device:
```bash
nand init
nand device 0
```
Erase the target NAND area:
```bash
nand erase 0x80000 0x3e00000
```
Write the UBI image to NAND:
```bash
nand write 0x44000000 0x80000 0x1860000
```
Reboot using the default boot command:
```bash
run bootcmd
```
*Note: After OpenWrt has successfully booted, it is recommended to upgrade to the `squashfs.bin` sysupgrade image through LuCI for proper dual-bank layout.*

### 3. Manual Kernel Boot
The kernel UBI volume can be loaded into RAM and booted manually from the bootloader.
Display the UBI volume information, read, and boot:
```bash
ubi info l
ubi read 0x44000000 kernel
bootm 0x44000000
```

### 4. Untethered Boot / Bootloader Access
Power-cycle the device and immediately press `Esc` or `Enter` repeatedly to enter the bootloader menu.
At the bootloader prompt, run:
```bash
reset
```
This allows the device to continue booting normally without remaining attached to the serial terminal.

<br><br>

---
---

# 🇮🇩 Versi Bahasa Indonesia

Repositori ini menyediakan skrip otomatisasi GitHub Actions dan *custom firmware* untuk *router* **LITE-ON WPX8324-BT** yang berbasis arsitektur Qualcomm IPQ50xx.

## 🎖️ Kredit & Ucapan Terima Kasih
Apresiasi terbesar dan terima kasih khusus kepada **[sib0ndt](https://github.com/sib0ndt)** atas proses *porting* awal OpenWrt, konfigurasi *Device Tree* (DTS), skrip ekstraksi *caldata*, serta metode instalasi inti. Repositori ini dibangun di atas fondasi luar biasa yang telah beliau buat.

## 📋 Spesifikasi Perangkat
| Fitur | Keterangan |
| :--- | :--- |
| **Manufaktur** | LITE-ON Technology Corporation |
| **Model** | WPX8324 |
| **Tipe Perangkat** | Indoor Access Point / Mesh AP |
| **Standar Wi-Fi** | IEEE 802.11ax (Wi-Fi 6) |
| **Kelas Wireless** | AX3000 |
| **2.4 GHz** | 2×2 MIMO |
| **5 GHz** | 2×2 MIMO |

**Detail Hardware:** [FCC ID PPQ-WPX8324 Internal Photos](https://fccid.io/PPQ-WPX8324/Internal-Photos/Internal-Photos-6344601)

## ✨ Custom Build Features
*Build* spesifik ini (`v25.12.5`) dilengkapi dengan:
* **Siap USB Modem & Tethering:** Modul komplit (kmod) seperti `qmi-wwan`, `cdc-ncm`, `rndis`, dan `modemmanager` sudah tertanam permanen di dalam *firmware*.
* **Anti-Kernel Panic:** Modul `kmod-qrtr` (penyebab *bootloop* pada *router* tanpa modem internal) telah dihapus dari sistem agar *router* stabil.
* **Wi-Fi Otomatis:** Wi-Fi langsung memancar pada *boot* pertama dengan SSID `WPX8324-BT`.

---

## 🛠️ Panduan Instalasi

### 1. Uji Coba Booting Tanpa Flashing (Aman)
File `initramfs` dapat di- *booting* langsung melalui RAM terlebih dahulu untuk menguji OpenWrt tanpa mengubah atau menghapus isi memori NAND.
Dari *bootloader* (U-Boot), muat file `initramfs` menggunakan TFTP:
```bash
tftpboot 0x44000000 openwrt_qualcommax_ipq50xx_wpx8324_ap_mp03_5_c1_initramfs.itb
```
*Booting* file yang sudah dimuat tersebut sesuai instruksi *bootloader*.
Setelah OpenWrt menyala, akses LuCI (192.168.1.1) dan pastikan perangkat berfungsi normal (LAN, Wi-Fi, LED, dll).

Jika sistem berjalan lancar, instal *firmware* permanen (`sysupgrade`) langsung melalui LuCI:
* Di menu LuCI, buka: **System -> Backup / Flash Firmware**
* Upload file `openwrt_..._squashfs.bin` lalu lanjutkan proses *upgrade*.

### 2. Instalasi Tingkat Lanjut (TFTP UBI Flash)
Muat file UBI menggunakan TFTP:
```bash
tftpboot 0x44000000 openwrt_25_12_5_qualcommax_ipq50xx_wpx8324_ap_mp03_5_c1_squashfs.ubi
```
Inisialisasi dan pilih perangkat NAND:
```bash
nand init
nand device 0
```
Hapus area NAND yang ditargetkan:
```bash
nand erase 0x80000 0x3e00000
```
Tulis (Flash) file UBI ke dalam NAND:
```bash
nand write 0x44000000 0x80000 0x1860000
```
Nyalakan ulang (Reboot) menggunakan perintah bawaan:
```bash
run bootcmd
```
*Catatan: Setelah OpenWrt sukses melakukan booting, sangat disarankan untuk melakukan flash ulang menggunakan file `squashfs.bin` melalui menu LuCI agar struktur partisi ganda (Dual-Bank) tertata sempurna.*

### 3. Booting Kernel Manual
Volume kernel pada UBI dapat dimuat ke RAM dan di- *booting* secara manual melalui *bootloader*.
Tampilkan informasi volume UBI, baca, lalu *booting*:
```bash
ubi info l
ubi read 0x44000000 kernel
bootm 0x44000000
```

### 4. Booting Tanpa Kabel Serial (Untethered Boot)
Nyalakan perangkat dan segera tekan tombol `Esc` atau `Enter` berkali-kali untuk masuk ke menu *bootloader*.
Pada terminal *bootloader*, ketik:
```bash
reset
```
Perintah ini memungkinkan perangkat untuk melanjutkan proses *booting* secara normal meskipun Anda mencabut kabel antarmuka serial (TTL) dari perangkat.
