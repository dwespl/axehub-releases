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
    sha256sum -c axehub-linux-1.0.8+18.tar.gz.sha256

    # Windows PowerShell
    Get-FileHash axehub-windows-1.0.8+18.zip -Algorithm SHA256
    # compare against the hash in the release notes

## Security scans (VirusTotal)

All release binaries are scanned on [VirusTotal](https://www.virustotal.com)
so you can verify them before trusting any unsigned executable. Click the
hash to see the public report.

### v1.0.8

| Platform | SHA-256 |
|----------|---------|
| Android APK | [`c629af5d…2f566`](https://www.virustotal.com/gui/file/c629af5d20fa1f4062a815bc931a2220a1617e0c913c5f4a62c9934dbae2f566) |
| Windows ZIP | [`b2e11c46…82aad`](https://www.virustotal.com/gui/file/b2e11c46f8a75a92544ba237eaa710d2fb2f7c24647d7df9462e6ad4f4f82aad) |
| Linux tar.gz | [`fbfd50bd…4c616`](https://www.virustotal.com/gui/file/fbfd50bdd84ba8db8ad7d6d8744397537d2273191651cf122d2d39d71784c616) |

> Windows will still show a SmartScreen "Unknown publisher" warning — AxeHub
> is not yet code-signed with an EV certificate. Hash verification and the
> VirusTotal report are your primary trust signals for v1.0.x.

### Why does Kaspersky (and maybe others) flag the APK?

You may see **1 out of ~70 engines** flag the APK with a verdict like:

    Not-a-virus:HEUR:RiskTool.AndroidOS.Miner.b

The leading **`Not-a-virus:`** prefix is Kaspersky explicitly telling you
that **this is not malware**. `RiskTool` is their category for legitimate
utilities that *could* be misused — the same bucket that holds VPNs, remote
admin tools, SSH clients, and password recovery utilities.

Their heuristic triggers on the presence of mining-related strings
(`hashrate`, `stratum`, `pool`, etc.) in the APK. That signal is correct —
AxeHub talks to mining hardware — but the classification as `Miner.b` is
imprecise for this app:

- **Covert cryptominers** use your device's CPU/GPU to mine, typically
  without consent. That's what `Miner.*` usually flags.
- **AxeHub** does **not** mine on the device it runs on. It only queries
  stats (hashrate, temperature, power) and sends configuration commands
  over your LAN to *separate, dedicated* ASIC mining hardware that you
  physically own.

If you're uncomfortable, verify by reading the outbound connections on
first scan — all traffic goes to RFC 1918 private IPs (your LAN miners)
plus HTTPS to CoinGecko and mempool.space for price/difficulty lookups.
Nothing is uploaded anywhere else.

## Licensing

- **AxeHub** itself is **proprietary** — see [`LICENSE`](LICENSE).
  Personal, non-commercial use of official binaries is allowed; reverse
  engineering, redistribution, and rebranding are not.
- **Bundled open-source components** (120 Dart/Flutter packages +
  NotoSans font) ship under their original licenses — MIT, Apache 2.0,
  BSD 3-Clause, SIL OFL 1.1. Full text in
  [`THIRD_PARTY_LICENSES.txt`](THIRD_PARTY_LICENSES.txt).
- In-app view: **Settings → About → Open-source licenses** shows the
  same list via Flutter's `showLicensePage`.

## Privacy policy

https://dwespl.github.io/axehub-legal/

## Feedback

Issues and feature requests: [axehub-releases/issues](https://github.com/dwespl/axehub-releases/issues)

This is a hobby project — replies come when they come.

Not affiliated with Bitmain, Canaan, Goldshell, or the Bitaxe team.
