# Android Smooth Dimming

通用 Android 自动亮度渐变平滑调节模块 / Universal Android Automatic Brightness Ramp Adjustment Module

- **作者 / Author:** @MOU_AJMD (酷安 `@C_AJMD_W`)
- **版本 / Version:** v2.0
- **模块 ID / Module ID:** `android_smooth_dimming`

---

## 简介 / Introduction

### 中文

**Android Smooth Dimming**（原名 *OP13 Smooth Dimming*）是一个面向全机型通用的 KernelSU / Magisk 模块，旨在接管并优化系统自动亮度升高与降低时的渐变过渡过程，消除亮度变化时明显的阶梯感，使屏幕背光过渡更加平滑自然。

本模块**不修改**系统原生的自动亮度算法、光线传感器数据或亮度映射曲线，仅接管底层亮度渐变速率与最大动画时长参数。

### English

**Android Smooth Dimming** (formerly *OP13 Smooth Dimming*) is a universal KernelSU / Magisk module designed to smoothly override automatic screen brightness increase and decrease transitions, eliminating noticeable brightness steps for a natural dimming experience.

This module **does not modify** native auto-brightness algorithms, ambient light sensor data, or brightness mapping curves—it solely adjusts brightness ramp rates and maximum transition durations.

---

## 功能特性 / Features

### 中文

- **双向渐变接管**：同时接管屏幕亮度**升高**与**降低**的渐变速率及最大动画时长。
- **纯净无侵入**：完全保留原厂自动亮度算法、传感器采样与亮度映射曲线。
- **全机型动态适配**：不硬编码设备型号或固定 `display_id` 文件名，自动扫描各分区中的 `DisplayDeviceConfig` 配置文件。
- **多后端智能仲裁**：优先采用无损 `Bind Mount` 覆盖 XML 配置（兼容 RRO / Framework 探测），防止多后端重复修改冲突。
- **Fail-Closed 安全隔离**：无法确认系统兼容性时自动安全退出，绝不强行修改系统原始分区。
- **支持自定义与一键停用**：支持通过 `/data/adb/smooth_dimming/config.prop` 微调参数或设置 `enabled=0` 安全停用。

### English

- **Bidirectional Ramp Control**: Overrides ramp rates and maximum transition durations for both brightness **increase** and **decrease**.
- **Non-Intrusive Design**: Leaves native auto-brightness algorithms, light sensor data, and brightness mapping curves untouched.
- **Universal Dynamic Discovery**: No hardcoded device models or `display_id` filenames; automatically scans `DisplayDeviceConfig` XML files across system partitions.
- **Adaptive Multi-Backend Arbitration**: Uses non-destructive `Bind Mount` for XML configs (with RRO / Framework probing) while preventing duplicate backend conflicts.
- **Fail-Closed Safety**: Safely exits without modifying system partitions if compatibility cannot be verified on unsupported ROMs.
- **Customizable & Reversible**: Fine-tune parameters or set `enabled=0` in `/data/adb/smooth_dimming/config.prop` to safely disable anytime.

---

## 修改参数 / Configuration

目标文件 / Target File:  
动态扫描系统分区的 `DisplayDeviceConfig` 配置文件（如 `displayconfig/*.xml`，无需固定 `display_id`）  
Dynamically discovered `DisplayDeviceConfig` XML files across system partitions (no hardcoded `display_id`)

用户配置文件 / User Config Path:  
`/data/adb/smooth_dimming/config.prop`

| 参数 / Parameter | 默认值 / Default Value | 说明 / Description |
| :--- | :--- | :--- |
| `screenBrightnessRampFastDecrease` | `0.05` | 快速降低亮度时的渐变速率 / Fast decrease ramp rate |
| `screenBrightnessRampSlowDecrease` | `0.02` | 缓慢降低亮度时的渐变速率 / Slow decrease ramp rate |
| `screenBrightnessRampFastIncrease` | `0.05` | 快速升高亮度时的渐变速率 / Fast increase ramp rate |
| `screenBrightnessRampSlowIncrease` | `0.02` | 缓慢升高亮度时的渐变速率 / Slow increase ramp rate |
| `screenBrightnessRampIncreaseMaxMillis` | `5000` | 亮度升高动画的最大时长 (ms) / Max increase duration (ms) |
| `screenBrightnessRampDecreaseMaxMillis` | `5000` | 亮度降低动画的最大时长 (ms) / Max decrease duration (ms) |

---

## 兼容性 / Compatibility

- **设备 / Device:** 全机型通用（不硬编码设备型号） / Universal (No hardcoded device models)
- **系统 / ROM:** 支持标准 `DisplayDeviceConfig` 的类原生及定制 Android ROM（如 LineageOS、Evolution X、ColorOS 等） / AOSP & custom ROMs using standard `DisplayDeviceConfig`
- **Root:** Magisk / KernelSU
- **Android:** Android 10+ (API 29+)

> 部分将调光逻辑硬编码于厂商私有 Framework 或底层 HAL 的特殊 ROM 可能无法通过静态 XML 后端接管，模块会自动执行 Fail-Closed 安全退出并输出诊断日志。  
> *Some vendor ROMs with hardcoded ramp logic in proprietary Frameworks or HALs may not be supported by the static XML backend; the module will safely exit (Fail-Closed) and log the reason.*

---

## 安装方法 / Installation

### 中文

1. 从 GitHub Releases 下载最新版本的 `Android_Smooth_Dimming_v2.0.zip`。
2. 打开 **KernelSU** 或 **Magisk** 管理器。
3. 进入模块页面，选择「从本地安装」。
4. 选择下载的 ZIP 压缩包并等待环境探测完成。
5. 安装完成后重启手机生效。

### English

1. Download the latest `Android_Smooth_Dimming_v2.0.zip` from GitHub Releases.
2. Open **KernelSU** or **Magisk** Manager.
3. Go to the Modules tab and select "Install from storage".
4. Choose the downloaded ZIP file and wait for the environment check to finish.
5. Reboot your device after installation.

---

## 工作原理 / How It Works

### 中文

1. 开机早期阶段（`post-fs-data`）自动检测 Android 版本、Root 环境及各分区的 `DisplayDeviceConfig` XML 文件。
2. 将命中的原始 XML 复制至模块数据目录并更新六项亮度渐变参数（保留原有自动亮度曲线与传感器配置）。
3. 继承原文件 SELinux 上下文，通过 `Bind Mount` 无损挂载覆盖原路径。
4. 若当前设备未检测到有效接管入口，安全退出并同步状态至管理器描述栏。

### English

1. During early boot (`post-fs-data`), the module probes the Android version, Root environment, and `DisplayDeviceConfig` XML files across partitions.
2. Matched XML files are copied to the module data directory and patched with the six target ramp parameters while preserving native curves.
3. Original SELinux contexts are preserved, and patched files are applied non-destructively via `Bind Mount`.
4. If no valid target is found, the module safely exits (Fail-Closed) and reports the status in the manager description.

---

## 卸载方法 / Uninstallation

### 中文

在 KernelSU 或 Magisk 管理器中禁用或卸载本模块，然后重启手机即可恢复原始系统配置，卸载脚本会自动清理 `/data/adb/smooth_dimming` 下的所有缓存与日志。

### English

Disable or uninstall the module in KernelSU or Magisk Manager, then reboot your device to restore stock behavior. The uninstall script automatically removes all cached files and logs under `/data/adb/smooth_dimming`.

---

## 注意事项 / Notes

### 中文

- 本模块通过 `Bind Mount` 应用配置，可能被部分系统完整性检测或游戏反作弊系统识别。
- 不保证所有游戏或反作弊系统均兼容。
- 本模块不修改游戏进程、游戏内存或游戏逻辑。
- 请根据自身设备环境决定是否安装。

### English

- This module uses a `Bind Mount` to apply configurations, which may be detected by system integrity checks or game anti-cheat systems.
- Compatibility with all games and anti-cheat systems is not guaranteed.
- This module does not modify game processes, game memory, or game logic.
- Please evaluate your device environment before installation.

---

## 许可证 / License

MIT License.  
See [LICENSE](LICENSE) for details.

---

- **Maintainer:** MOU_AJMD (酷安 `@C_AJMD_W`)
- **Project:** Android Smooth Dimming
