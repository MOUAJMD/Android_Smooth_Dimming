OP13 Smooth Dimming

OnePlus 13 自动亮度平滑调节模块 / Automatic Brightness Ramp Adjustment Module

作者 / Author: @MOU_AJMD

版本 / Version: 1.1

模块 ID / Module ID: "op13_ramp_fix"

---

简介 / Introduction

中文

OP13 Smooth Dimming 是一个适用于一加 13 的 KernelSU / Magisk 模块，旨在优化系统自动亮度降低时的过渡效果，减少亮度变化时明显的阶梯感，使屏幕亮度变化更加平滑自然。

本模块通过调整系统 Display Configuration XML 中的亮度渐变参数实现。

已在一加 13 上完成实际测试。

English

OP13 Smooth Dimming is a KernelSU / Magisk module for the OnePlus 13, designed to improve automatic screen brightness transitions.

It adjusts the brightness ramp parameters in the system Display Configuration XML to reduce noticeable brightness steps and provide smoother, more natural dimming.

Successfully tested on the OnePlus 13.

---

功能特性 / Features

- 优化自动亮度降低时的过渡效果。

- 减少屏幕亮度变化时的阶梯感。

- 自动查找目标 Display Configuration XML。

- 不直接修改原始系统 XML 文件。

- 通过 Bind Mount 在开机时应用修改后的配置。

- 支持通过 KernelSU / Magisk 管理器安装和卸载。

- Improve automatic brightness decrease transitions.

- Reduce noticeable brightness steps.

- Automatically locate the target Display Configuration XML.

- Does not directly modify the original system XML.

- Applies the modified configuration using a bind mount at boot.

- Supports installation and removal through KernelSU / Magisk managers.

---

修改参数 / Configuration

目标文件 / Target file:

"display_id_4630946756802996883.xml"

参数 / Parameter| 数值 / Value
"screenBrightnessRampSlowDecrease"| "0.02"
"screenBrightnessRampFastDecrease"| "0.05"
"screenBrightnessRampDecreaseMaxMillis"| "5000"

---

兼容性 / Compatibility

- 设备 / Device: OnePlus 13
- 系统 / ROM: Evolution X
- Root: KernelSU / Magisk
- Android: Android-based ROMs

其他设备及 ROM 的兼容性尚未确认。

Compatibility with other devices and ROMs has not been confirmed.

---

安装方法 / Installation

1. 从 GitHub Releases 下载最新 ZIP。

2. 打开 KernelSU 或 Magisk 管理器。

3. 选择从本地安装模块。

4. 选择下载的 ZIP 文件。

5. 安装完成后重启手机。

6. Download the latest ZIP from GitHub Releases.

7. Open KernelSU or Magisk Manager.

8. Select installation from local storage.

9. Choose the downloaded ZIP file.

10. Reboot your device after installation.

---

工作原理 / How It Works

中文

1. 自动搜索系统中的目标 XML 文件。
2. 将原始 XML 复制到模块目录。
3. 修改指定的亮度渐变参数。
4. 在开机阶段通过 Bind Mount 将修改后的 XML 挂载到原始文件路径。

整个过程不会直接修改原始系统分区中的 XML 文件。

English

1. Search for the target XML configuration file.
2. Copy the original XML into the module directory.
3. Modify the specified brightness ramp parameters.
4. Apply the modified XML to the original file path using a bind mount during boot.

The original system XML file is not directly modified.

---

卸载方法 / Uninstallation

中文

在 KernelSU 或 Magisk 管理器中禁用或卸载本模块，然后重启手机即可恢复原始配置。

English

Disable or uninstall the module through KernelSU or Magisk Manager, then reboot to restore the original configuration.

---

注意事项 / Notes

中文

- 本模块通过 Bind Mount 应用配置，可能被系统完整性检测或游戏反作弊系统识别。
- 不保证所有游戏或反作弊系统均兼容。
- 本模块不修改游戏进程、游戏内存或游戏逻辑。
- 请根据自身设备环境决定是否安装。

English

- This module uses a bind mount to apply the modified configuration, which may be detected by system integrity checks or game anti-cheat systems.
- Compatibility with all games and anti-cheat systems is not guaranteed.
- This module does not modify game processes, game memory, or game logic.
- Please evaluate your device environment before installation.

---

许可证 / License

MIT License.

See "LICENSE" (LICENSE) for details.

---

Maintainer: MOU_AJMD

Project: OP13 Smooth Dimming
