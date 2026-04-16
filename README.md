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
- **NMMiner** (ESP32-S3 / ESP32 solo miners, firmware v1.6–v1.8)
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
    sha256sum -c axehub-linux-1.1.0+20.tar.gz.sha256

    # Windows PowerShell
    Get-FileHash axehub-windows-1.1.0+20.zip -Algorithm SHA256
    # compare against the hash in the release notes

## Security scans (VirusTotal)

All release binaries are scanned on [VirusTotal](https://www.virustotal.com)
so you can verify them before trusting any unsigned executable. Click the
hash to see the public report.

### v1.1.0

- NMMiner (ESP32 solo miner) support — auto-detect, monitor, pool change
- Per-brand device colors
- Faster offline detection

| Platform | SHA-256 | VirusTotal verdict |
|----------|---------|--------------------|
| Android APK | [`66d61aaa…a3e32`](https://www.virustotal.com/gui/file/66d61aaa530002209b0fcb1e8ea3c017d1fc926c76362d4b5f0423251a6a3e32) | pending |
| Windows ZIP | [`cf69c061…939b7`](https://www.virustotal.com/gui/file/cf69c0617829b2e908d991e068fd0c76696df16a13fee8050f50886da95939b7) | pending |
| Linux tar.gz | [`9ff6fe2f…78d58`](https://www.virustotal.com/gui/file/9ff6fe2f30a782eda53336c2d5ed95ae44e3d8f62523df1268a950e085678d58) | pending |

### v1.0.9

| Platform | SHA-256 | VirusTotal verdict |
|----------|---------|--------------------|
| Android APK | [`8f17e220…2c8ed`](https://www.virustotal.com/gui/file/8f17e2201d8314ba66a667e4ed808ba30163abdbf919e050a1c22a210762c8ed) | 1 / 67 engines (Kaspersky heuristic, see below) |
| Windows ZIP | [`601aec84…3e92c`](https://www.virustotal.com/gui/file/601aec84925931b6893df240e8a398a4b06f2a2693966d53c7d9ee60b353e92c) | **0 / 65 engines** ✓ clean |
| Linux tar.gz | [`8cd5fcc4…c94b3`](https://www.virustotal.com/gui/file/8cd5fcc4780f4523d75ddf87d9991c5d6b4dcd6fba550388213fddb6424c94b3) | **0 / 63 engines** ✓ clean |

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

### Other false-positive signals you may see on VirusTotal

VT adds sandbox and YARA verdicts **asynchronously** over hours or days
after the initial upload — your report may look cleaner now and grow
some yellow labels later. Here's what the common ones mean.

**Zenbox Android sandbox: `MALWARE / TROJAN / EVADER` (confidence ~64)**

Zenbox is an automated sandbox that executes the APK and watches
runtime behavior. AxeHub trips:

- **`EVADER`** — Flutter itself queries device traits at startup
  (orientation, DPI, battery, tablet-vs-phone). The sandbox reads
  those probes as *"the app is trying to detect it's being analyzed
  and will hide its payload"*. Every Flutter app with a responsive
  layout looks the same to this heuristic.
- **`TROJAN / MALWARE`** — the obfuscated Dart AOT bytecode plus the
  LAN scanner plus the foreground-service-for-Hub combination matches
  generic suspicious-app patterns. No specific signature, just a shape
  that worries pattern-based detectors.

Confidence 64 / 100 reflects the sandbox's own uncertainty.

**YARA rule `Windows_API_Function` (InQuest) on the Windows ZIP**

Its own description: *"When this signature alerts on an executable,
it is not an indication of malicious behavior."* It just notices that
the binary calls into Windows API — which every Windows executable
does. Harmless informational tag.

If you want actual evidence of what AxeHub does, the release binaries
are obfuscated but not encrypted. Disassemble, strace/dtrace the
process, or capture packets — the on-wire truth is RFC 1918 LAN
addresses plus HTTPS price/difficulty lookups, nothing else.

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

Not affiliated with Bitmain, Canaan, Goldshell, NMMiner, or the Bitaxe team.
