# 内置组件与许可

客户端左下角“第三方许可证”可查看组件许可和完整通知。

- **Node.js v24.22.1**：采用 MIT 许可及所附第三方许可。[官方项目](https://nodejs.org/)。
- **PearlFortune v2.3.9**：来自[官方 Windows 发布](https://github.com/pearlfortune/pearl-miner/releases/tag/v2.3.9)，主程序未修改。原包未附带独立许可文件。
- **Pearl Oyster v1.5.5**：来自[Pearl 官方发布](https://github.com/pearl-research-labs/pearl/releases/tag/v1.5.5)，采用 ISC 许可。
- **Microsoft WebView2 SDK 1.0.4314.35**：来自微软官方 NuGet 包，遵守所附 Microsoft 软件许可条款。
- **Microsoft Edge WebView2**：使用系统 Evergreen 组件；缺少时可由用户选择运行微软官方安装程序。Microsoft Defender SmartScreen 默认启用，相关数据处理遵循[微软隐私声明](https://privacy.microsoft.com/privacystatement)和[Edge 隐私白皮书](https://learn.microsoft.com/microsoft-edge/privacy-whitepaper/)。完整许可和隐私通知随应用保留。
- **7-Zip Extra 26.06 x64**：来自[官方发行包](https://github.com/ip7z/7zip/releases/tag/26.06)，主要采用 GNU LGPL，部分代码采用 BSD 2/3 条款许可。完整许可随应用保留，[对应源代码](https://github.com/ip7z/7zip/tree/26.06)公开可用。
- **PearlBox 桌面与显卡辅助组件**：由本项目构建。核心偏移与功耗控制调用用户已安装的 NVIDIA NVML / NVAPI 驱动接口，不分发 NVIDIA 驱动或驱动 DLL。功耗策略的接口布局参考 [NvAPIWrapper](https://github.com/falahati/NvAPIWrapper)，本应用未内置或链接该库；相关辅助组件为独立实现。

本应用不内置 SRBMiner-MULTI。可用版本由服务器发布，下载后才会安装。
