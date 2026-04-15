# AxeHub — Releases

Official binary releases for **AxeHub**, a home ASIC miner manager.

One dashboard for everything from tiny Bitaxes to full S19 rigs. Auto-detects
mining hardware on your LAN, tracks hashrate/temperature/power, calculates
algorithm-aware profitability, and exposes optional remote access via a
built-in HTTPS hub.

## Supported hardware

- **Bitaxe** (AxeOS: Ultra / Supra / Gamma / Max / Hex / NerdQAxe / NerdOCTAXE)
- **Canaan Avalon Nano 3S**
- **Antminer S19 / S21 series** (stock cgminer, MaraFW, Braiins OS)
- **Goldshell Mini Doge** and other bfgminer devices
- **EBAZ4205 FPGA** — Xilinx Zynq-7010 SHA-256 miner (BC2 Edition)
- Generic CGMiner-family devices on port 4028

## Supported algorithms

- **SHA-256**: Bitcoin (BTC), BitcoinII (BC2), Bitcoin Cash (BCH)
- **Scrypt**: Litecoin (LTC), Dogecoin (DOGE)
- **kHeavyHash**: Kaspa (KAS) — price tracking, device support coming

## Download

Head over to the **[Releases page](https://github.com/dwespl/axehub-releases/releases/latest)**
for the latest builds.

Each release ships with three platforms:

| Platform | File | Install |
|----------|------|---------|
| Android | `axehub-android-X.Y.Z.apk` | Tap to install (enable "Install from unknown sources") |
| Windows | `axehub-windows-X.Y.Z.zip` | Extract and run `axehub.exe` |
| Linux | `axehub-linux-X.Y.Z.tar.gz` | Extract and run `./axehub` |

Android is also available on **[Google Play Store](https://play.google.com/store/apps/details?id=com.axehub.app)**.

## Verify downloads

Every binary has an accompanying `.sha256` file. Check the hash before running:

    # Linux / macOS
    sha256sum -c axehub-linux-1.0.7.tar.gz.sha256

    # Windows PowerShell
    Get-FileHash axehub-windows-1.0.7.zip -Algorithm SHA256
    # compare against the hash in the release notes

## Privacy policy

https://dwespl.github.io/axehub-legal/

## Feedback

Issues and feature requests: [axehub-releases/issues](https://github.com/dwespl/axehub-releases/issues)

This is a hobby project — replies come when they come.

Not affiliated with Bitmain, Canaan, Goldshell, or the Bitaxe team.
