<p align="center">
  <img src="assets/pearlbox-logo.png" width="96" height="96" alt="PearlBox miner cat logo">
</p>

<h1 align="center">PearlBox</h1>

<p align="center"><strong>Pearl mining starts with your choice.</strong></p>

<p align="center">
  <a href="https://pearlbox.app">Website</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ru.md">Русский</a>
</p>

PearlBox brings miners, pools, GPU tuning, updates and a local wallet into one Windows desktop app. Enter your own PRL payout address and worker name, choose a miner and pool, and start mining.

**Choose a miner and version → Select a compatible pool → Enter your address → Start mining**

This overview covers client **1.0.0**.

Import local plugins from Advanced → Plugins → ＋. The dashboard includes a GPU table and copyable logs; startup and tuning options are grouped under Unattended mode.

Linux x64 offers a GTK desktop and a browser UI for headless rigs, initially supporting NVIDIA PearlFortune CUDA 12/13. The plugin workshop on Windows and Linux offers a fleet controller, managed device agent and optional XMR CPU dual mining.

PearlBox opens in English. The first-run guide can be skipped and reopened from **Advanced → Quick guide**. On compatible pools, select **Receive USDT** beside the wallet address. A second address field shows the conversion fee and payout network; the regular pool fee stays beside the pool.

On Peryn, OneZeroMiner and MH-Miner can switch between PPS+ and PPLNS next to **Test latency**. The pool fee and the launch settings follow the selected payout mode.

## Highlights

| Feature | What you can do |
| --- | --- |
| **Prepare and start in one flow** | Automatically match miners with compatible pools. Missing versions are downloaded, verified and installed, without writing BAT files or startup arguments. |
| **Manage versions in one place** | Choose miner versions, read release notes and track download progress and speed. Update the current version or keep older miner versions separately. |
| **See what is happening** | View total hashrate on the main panel, with GPU status pinned above the logs. Accepted/rejected shares and connection status are also recognized. |
| **Tune GPUs by model** | Detect discrete GPUs and group matching models. Presets fill minimum/maximum core clocks, core offset, memory clock and power settings for manual review and application. |
| **Local wallet and encrypted backups** | Create a wallet beside the address field, export an encrypted backup and restore it locally. Wallet passwords and recovery words are handled on your computer. |

### Updates that preserve your choices

- **Client:** the update center offers the latest client release and preserves local wallet data and settings.
- **Miners:** new releases can replace the current version; older releases can be kept separately. The bundled PearlFortune is also updatable.
- **Downloads:** track progress and speed, retry interrupted connections and resume when supported by the server. Files are verified before installation.
- **Notifications:** equal or older versions do not trigger upgrade notices. Ignored releases remain available for manual updates.

## Quick start

1. **Download and open**  
   Get the Windows EXE from this repository's **Releases** page and run it. Confirm the Windows administrator prompt when launching.

2. **Choose and configure**  
   Select a miner, version and pool. Enter your own **PRL payout address** and worker name. If you need a wallet, use **Create wallet** beside the address field and save your recovery words and encrypted backup first.

3. **Start mining**  
   Click **Start mining**. Missing miner software is prepared automatically before launch. Watch hashrate and logs on the main panel; use **Stop mining** to end the session.

GPU tuning is optional and requires manual confirmation. Launch at login and automatic mining are off by default and can be enabled in the advanced panel. Automatic mining does not apply GPU tuning settings.

### Requirements

- Windows 10 / 11, 64-bit.
- Microsoft Edge WebView2 Runtime; follow the app's installation prompt if it is missing.
- Hardware and drivers supported by the selected miner. GPU tuning currently targets supported NVIDIA discrete GPUs; support for other vendors is primarily device detection.

The single EXE is the distribution format. Runtime components, miners and application data are prepared locally, so disk usage exceeds the download size.

## Miner adapters

| Software | Availability |
| --- | --- |
| [PearlFortune](https://github.com/pearlfortune/pearl-miner) | Bundled with the client; updatable through the server |
| [SRBMiner-MULTI](https://github.com/doktor83/SRBMiner-Multi) | Downloaded from server-published releases |
| [OneZeroMiner](https://github.com/OneZeroMiner/onezerominer) | Downloaded from server-published releases |

## Wallet

Wallet passwords, recovery words and private keys are handled locally and are not uploaded to the configuration server. Keep your own wallet backups. For transfers, restore and use the [official Pearl wallet](https://github.com/pearl-research-labs/pearl).

## Usage notes

- Clock, offset and power ranges depend on the GPU and driver. Preset hashrates are references; use stable measurements from your own machine.
- Missing or stale monitoring values are shown as “—”.
- Stop mining before installing/removing miners or updating the client. If you move the EXE, enable launch-at-login again to update its path.
- Windows and Linux clients currently mine PRL. USDT is an optional payout currency at participating pools.

## Components and licensing

See [Third-party notices](THIRD-PARTY-NOTICES.md), also available from the lower-left corner of the client.

This repository contains project information and release distribution materials, not the complete PearlBox source tree. Third-party software retains its own licenses. PearlBox is an independent tool; upstream software and pool names describe compatibility.
