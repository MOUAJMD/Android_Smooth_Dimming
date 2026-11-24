# Android Smooth Dimming

通用 Android 极致丝滑自动亮度渐变调节模块 / Universal Android Ultra-Smooth Automatic Brightness Ramp Module

- **作者 / Author:** MOUAJMD (酷安 `@C_AJMD_W`)
- **版本 / Version:** v2.5.2
- **模块 ID / Module ID:** `android_smooth_dimming`

---

## 简介 / Introduction

### 中文

**Android Smooth Dimming**（原名 *OP13 Smooth Dimming*）是一个面向全机型通用的 KernelSU / Magisk / APatch 模块。通过重构底层光线采样窗口、亮暗防抖周期与深度阻尼插值速率，将屏幕亮度变化打磨至**几乎完全无感**（甚至比原厂官方调教更加顺滑），彻底消除日常走动时的阶梯跳跃感、响应延迟以及无操作熄屏前的亮度突降。

本模块**不修改**原厂自动亮度样条映射曲线（Spline）或手动亮度学习记录，仅在内存层动态接管时间窗口与渐变速率参数。

### English

**Android Smooth Dimming** (formerly *OP13 Smooth Dimming*) is a universal KernelSU / Magisk / APatch module that refines ambient light sampling horizons, debounce intervals, and damped ramp rates. It delivers a **virtually imperceptible** brightness transition—even smoother than stock OEM tuning—eliminating step-like jumps, response lag, and sudden pre-sleep dimming (`POLICY_DIM`).

This module **does not modify** native brightness mapping curves (Splines) or manual brightness learning data; it solely overrides timing windows and ramp rate parameters in memory.

---

## 功能特性 / Features

### 中文

- **比官方更无感的阻尼过渡**：拉平高频采样与动画过渡的时间差，使相邻采样动画首尾无缝重叠，日常走动、明暗切换及低亮度微调均如水波般线性滑过。
- **消除熄屏前突降**：优化无操作超时进入预暗淡（`POLICY_DIM`）时的快速降亮速率，屏幕变暗柔和自然，不再刺眼或跳格。
- **跟手低延迟**：大幅缩短原厂冗长的亮暗防抖等待，光线变化响应更及时，同时完整保留原厂亮度曲线。
- **全机型深度兼容**：自动扫描 `/vendor`、`/odm`、`/product`、`/my_product`、`/system_ext` 等分区的 `DisplayDeviceConfig` XML；支持为省略速率标签的原厂配置（如 MIUI Android 13、Pixel / 类原生 ROM）自动补全注入。
- **轻量安全，即刷即用**：开机早期 `Bind Mount` 内存级挂载，继承原文件 SELinux 上下文，不改写物理分区、不影响 OTA、零后台常驻与额外待机功耗。
- **支持自定义与一键停用**：可通过 `/data/adb/smooth_dimming/config.prop` 微调参数，或设置 `enabled=0` 安全停用。

### English

- **Imperceptible Damped Transitions**: Aligns high-frequency sampling intervals with ramp animation durations so consecutive transitions overlap seamlessly without step-like pauses.
- **No More Pre-Sleep Sudden Drops**: Refines fast-decrease rates during inactivity pre-dimming (`POLICY_DIM`) for gentle, flicker-free dimming before screen-off.
- **Responsive & Low Latency**: Shortens sluggish stock brightening/darkening debounce delays while preserving native brightness curves and user learning data.
- **Broad OEM & AOSP Compatibility**: Dynamically scans `DisplayDeviceConfig` XMLs across `/vendor`, `/odm`, `/product`, `/my_product`, and `/system_ext`, automatically injecting missing ramp tags on ROMs like MIUI (Android 13) and Pixel/AOSP.
- **Safe & Lightweight**: Applied non-destructively via `Bind Mount` with original SELinux contexts preserved; zero partition modification, OTA-safe, and zero background battery drain.
- **Customizable & Reversible**: Fine-tune all 15 parameters or set `enabled=0` in `/data/adb/smooth_dimming/config.prop` anytime.

---

## 修改参数 / Configuration

目标文件 / Target File:  
动态扫描系统分区的 `DisplayDeviceConfig` 配置文件（如 `displayconfig/*.xml`，支持多屏及缺失标签自动补全）  
Dynamically discovered `DisplayDeviceConfig` XML files across system partitions (supports multi-display & missing tag auto-injection)

用户配置文件 / User Config Path:  
`/data/adb/smooth_dimming/config.prop`

| 参数 / Parameter | 默认值 / Default | 说明 / Description |
| :--- | :--- | :--- |
| `screenBrightnessRampFastDecrease` | `0.18` | 快速降低亮度（含熄屏前预暗淡）的渐变速率 / Fast decrease ramp rate (incl. `POLICY_DIM`) |
| `screenBrightnessRampSlowDecrease` | `0.038` | 日常缓慢降低亮度时的深度阻尼速率 / Slow decrease damped ramp rate |
| `screenBrightnessRampFastIncrease` | `0.38` | 快速升高亮度时的渐变速率 / Fast increase ramp rate |
| `screenBrightnessRampSlowIncrease` | `0.052` | 日常缓慢升高亮度时的深度阻尼速率 / Slow increase damped ramp rate |
| `screenBrightnessRampIncreaseMaxMillis` | `2600` | 亮度升高动画的最大时长 (ms) / Max increase duration (ms) |
| `screenBrightnessRampDecreaseMaxMillis` | `3500` | 亮度降低动画的最大时长 (ms) / Max decrease duration (ms) |
| `screenBrightnessRampSlowDecreaseThreshold` | `0.005` | 触发慢速降亮阻尼的亮度变化阈值 / Slow decrease threshold |
| `screenBrightnessRampSlowIncreaseThreshold` | `0.005` | 触发慢速提亮阻尼的亮度变化阈值 / Slow increase threshold |
| `lightSensorRateMillis` | `100` | 环境光传感器采样周期 (ms，10Hz) / Ambient light sensor sampling rate (ms) |
| `ambientLightHorizonShort` | `1000` | 短期环境光滑动平均窗口 (ms) / Short ambient light averaging horizon (ms) |
| `ambientLightHorizonLong` | `2000` | 长期环境光滑动平均窗口 (ms) / Long ambient light averaging horizon (ms) |
| `brighteningLightDebounceMillis` | `300` | 环境光变亮防抖延迟 (ms) / Brightening light debounce delay (ms) |
| `darkeningLightDebounceMillis` | `600` | 环境光变暗防抖延迟 (ms) / Darkening light debounce delay (ms) |
| `brightnessIncreaseDebounceMillis` | `300` | 屏幕提亮防抖延迟 (ms，Android 14+) / Brightness increase debounce delay (ms) |
| `brightnessDecreaseDebounceMillis` | `600` | 屏幕降亮防抖延迟 (ms，Android 14+) / Brightness decrease debounce delay (ms) |

---

## 兼容性 / Compatibility

- **设备 / Device:** 全机型通用（含折叠屏多屏自动适配） / Universal (including foldables / multi-display)
- **系统 / ROM:** HyperOS / MIUI、ColorOS / OxygenOS / RealmeUI、OriginOS、Pixel / 类原生（LineageOS 等） / Major OEM & AOSP-based ROMs
- **Root:** Magisk (v20.4+) / KernelSU / APatch
- **Android:** Android 12 ~ 16 (API 31 ~ 36)

> Android 11 及更早版本或将调光硬编码于私有 Framework 的特殊 ROM 无法通过静态 XML 后端接管，模块会自动执行安全旁路退出并记录日志至 `/data/adb/smooth_dimming/runtime.log`。  
> *Android 11 and earlier, or ROMs with hardcoded ramp logic in proprietary Frameworks, will safely bypass mounting and log diagnostics to `/data/adb/smooth_dimming/runtime.log`.*

---

## 安装方法 / Installation

### 中文

1. 从 GitHub Releases 下载最新版本的 `Android_Smooth_Dimming_v2.5.2.zip`。
2. 打开 **KernelSU**、**Magisk** 或 **APatch** 管理器。
3. 进入模块页面，选择「从本地安装」。
4. 选择下载的 ZIP 压缩包并等待配置扫描完成。
5. 安装完成后重启手机，模块描述栏显示 `[已激活]` 即生效。

### English

1. Download the latest `Android_Smooth_Dimming_v2.5.2.zip` from GitHub Releases.
2. Open **KernelSU**, **Magisk**, or **APatch** Manager.
3. Go to the Modules tab and select "Install from storage".
4. Choose the downloaded ZIP file and wait for the config scan to finish.
5. Reboot your device; `[已激活]` in the module description confirms active status.

---

## 工作原理 / How It Works

### 中文

1. 开机早期阶段（`post-fs-data`）自动扫描各系统分区的 `DisplayDeviceConfig` XML 文件。
2. 将命中的原始 XML 复制至 `/data/adb/smooth_dimming/patched`，更新或自动补全 15 项平滑调光参数（完整保留原厂亮度曲线）。
3. 校验 XML 完整性并继承原文件 SELinux 上下文，通过 `Bind Mount` 无损挂载覆盖原路径。
4. 开机完成后通过 `service.sh` 将挂载状态与命中配置数量同步至管理器描述栏。

### English

1. During early boot (`post-fs-data`), the module scans `DisplayDeviceConfig` XML files across system partitions.
2. Matched XML files are copied to `/data/adb/smooth_dimming/patched` and patched (or auto-injected) with all 15 smooth dimming parameters while preserving native curves.
3. XML integrity and original SELinux contexts are verified before applying non-destructive `Bind Mount` overlays.
4. After boot completion, `service.sh` updates the module description with active backend status and mounted file count.

---

## 卸载方法 / Uninstallation

### 中文

在 KernelSU、Magisk 或 APatch 管理器中禁用或卸载本模块，然后重启手机即可恢复原始系统配置，卸载脚本会自动清理 `/data/adb/smooth_dimming` 下的所有缓存与日志。

### English

Disable or uninstall the module in KernelSU, Magisk, or APatch Manager, then reboot your device to restore stock behavior. The uninstall script automatically removes all cached files and logs under `/data/adb/smooth_dimming`.

---

## 注意事项 / Notes

### 中文

- 本模块通过 `Bind Mount` 应用配置，可能被部分系统完整性检测或游戏反作弊系统识别。
- 不保证所有游戏或反作弊系统均兼容。
- 本模块不修改游戏进程、游戏内存或游戏逻辑，请根据自身设备环境决定是否安装。

### English

- This module uses a `Bind Mount` to apply configurations, which may be detected by system integrity checks or game anti-cheat systems.
- Compatibility with all games and anti-cheat systems is not guaranteed.
- This module does not modify game processes, game memory, or game logic; please evaluate your device environment before installation.

---

## 许可证 / License

MIT License.  
See [LICENSE](LICENSE) for details.

---

- **Maintainer:** MOUAJMD (酷安 `@C_AJMD_W`)
- **Project:** Android Smooth Dimming
