<p align="center">
  <img src="assets/pearlbox-logo.png" width="96" height="96" alt="PearlBox miner cat logo">
</p>

<h1 align="center">PearlBox</h1>

<p align="center"><strong>Take anyone easy to start Pearl</strong></p>

<p align="center">
  <a href="https://pearlbox.app">Website</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="README.zh-CN.md">简体中文</a> ·
  <a href="README.ru.md">Русский</a>
</p>

PearlBox brings mining software, pools, GPU tuning, updates and a local wallet into one Windows client. Choose a miner and a pool, enter your PRL address and worker name, and you’re ready to mine.

**Choose a miner and version → Pick a compatible pool → Enter your payout address → Start mining**

This guide covers client version **1.0.0**.

## Features

| Feature | What you can do |
| --- | --- |
| **Start mining in one click** | Choose a miner to see compatible pools. If the selected version is missing, PearlBox downloads, verifies and installs it. No batch files or command-line arguments to write. |
| **Manage miner versions** | Choose a version, read its release notes and see download progress and speed. Update your current miner or keep an older version alongside it. |
| **Monitor a session** | See total hashrate on the dashboard. GPU readings stay at the top of the log, and PearlBox recognizes accepted and rejected shares as well as connection status. |
| **Tune GPUs by model** | PearlBox detects discrete GPUs and groups them by model. A preset fills in minimum and maximum core clocks, core offset, memory clock and power limit. Review the values before applying them. |
| **Create and back up a local wallet** | Create a wallet next to the address field, export an encrypted backup and restore it locally. Your password and recovery phrase are handled locally. |
| **Plugin workshop** | Install optional features as plugins. Build your own monitoring plugin with the [plugin API](docs/WORKSHOP-API.en.md), or use plugins made by PearlBox. |

## Example

<img width="1300" height="808" alt="PearlBox in use" src="https://github.com/user-attachments/assets/5f48e492-3207-4e63-9edd-6aab9e7b1a1d" />

## Quick start

1. **Download and open PearlBox.** Download the Windows EXE from this repository’s **Releases** and run it. Approve the Windows administrator prompt at startup.
2. **Set up your worker.** Choose your miner, version and pool, then enter your **PRL payout address** and worker name. If you don’t have a wallet, select **Create wallet** next to the address field. Save the recovery phrase and an encrypted backup before mining.
3. **Start mining.** Select **Start mining**. PearlBox installs the miner first if needed, then starts the session. Check the hashrate and log on the dashboard. Select **Stop mining** to end the session.

GPU tuning is optional and requires manual confirmation before applying changes. **Open at sign-in** and **Auto-start mining** are off by default. You can enable them under **Advanced**. Enabling automatic mining alone does not apply overclock settings.

### System requirements

- Windows 10 or 11, 64-bit.
- Your GPU and driver must meet the requirements of the miner you choose. PearlBox was developed with NVIDIA drivers from the 610.x series.

## Wallet

Your wallet password, recovery phrase and private keys stay on your device and are never sent to the server. Keep your own backup. To send funds, restore your wallet in the [official Pearl wallet](https://github.com/pearl-research-labs/pearl).

## Usage notes

- Your GPU and driver determine the available clock, offset and power ranges. Preset hashrates are estimates; use the results your own hardware can sustain.
- A dash (—) means a monitoring value is unavailable or out of date.
- Stop mining before installing or removing a miner or updating the client. If you move the Windows EXE, re-enable **Open at sign-in** to update the startup path.
- Both the Windows and Linux clients mine PRL. Some pools offer USDT as an optional payout currency.

## Components and licenses

See [Third-party licenses](THIRD-PARTY-NOTICES.md) for component sources and licenses. You can also open the list from the bottom-left corner of the client.

This repository introduces the project and distributes releases. It does not currently include the full PearlBox source code. Third-party software is subject to its own licenses. PearlBox is an independent project; upstream software and pool names identify compatible products and services.
