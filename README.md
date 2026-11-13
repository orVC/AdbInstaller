# ADB 安装器 · AdbInstaller

电视 / 机顶盒上的 APK 安装器：**全程用手机给电视装应用**，不用电脑、不用 U 盘。

**中文** | [English](#english)

> 本仓库只发布安装包（Release），不提供源码 / This repo only publishes the APK in Releases, no source code.

## 下载

到 [Releases](https://github.com/orVC/AdbInstaller/releases/latest) 下载最新的 `adb-install-vX.Y.apk`（当前 v1.13）。

手机和电视装**同一个文件**即可，功能完全一样。

## 第一次安装：电视开 ADB 权限就够，手机就能装

不需要电脑、不需要 U 盘，只要电视开启了 ADB 调试权限：

1. **电视**：设置 → 关于 → 连点「版本号」开启开发者选项 → 打开 **USB 调试 / 网络调试**；
2. **手机**：连**同一个 WiFi**，用任意支持 adb 的手机应用（如「甲壳虫ADB助手」、「小米电视助手」）连接电视的 `IP:5555`；
3. 在电视弹出的「允许 USB 调试吗」里勾选**始终允许** → 确定；
4. 用那个应用把 `adb-install-vX.Y.apk` 装到电视上。

> 电视 IP 可以这样查：设置 → 网络 → 当前 WiFi 的详情里看。装好之后就用不到上面那个工具了，后续都交给本应用。
>
> 实在不方便用手机时，把 APK 拷到 U 盘插到电视，用自带的「U 盘安装 / 媒体中心」也能装。

## 手机给电视装（主流用法，推荐）

前提：电视已开启 ADB 调试权限，手机和电视连**同一个 WiFi / 路由器**（同一网段，路由器没开「AP 隔离」）。

1. **手机**打开本应用 →「搜索设备」→ 点「扫描局域网设备」（约 3–10 秒）；
2. 列表里出现电视的 `IP:5555`（可对照电视「推送文件」页显示的 IP 确认是哪台）→ **点它**；
3. **电视上**会弹「允许 USB 调试吗」→ 勾选**始终允许** → 确定；若手机仍提示未授权，再点一次那台设备（应用会自动断开重连握手）；
4. 手机切到「文件目录」→ 进入存放 APK 的目录（如 `Download`）→ 选中 APK 按确定；
5. 手机上日志出现 `Performing Streamed Install` → `Success`，电视上就已经装好了。

> 手机端**不需要**点「获取本机 ADB 权限」，因为它连的是电视，不是自己。

**另一种传法**：电视「推送文件」页有二维码和地址，手机扫码或用浏览器打开即可上传 APK 到电视的 `/sdcard/AdbInstaller`，然后在电视「文件目录」里点它安装（适合电视上装了本应用、但又不想连 adb 的场景）。

## 更新

在「设置」页点「检查更新」：应用会经 `gh-proxy.org` 加速访问 GitHub 读取最新版本，有新版就自动下载并调起系统安装器（覆盖安装，设置保留）。加速失败会自动回退直连。

## 四个页签

| 页签 | 作用 |
| --- | --- |
| 设置 | 目标地址（`127.0.0.1:5555` = 本机，填电视 `IP:5555` = 连那台电视）+「获取 ADB 权限」+「检查更新」 |
| 搜索设备 | 扫描局域网 `1–254` 的 `5555` 端口，点一下即连接该设备 —— 手机给电视装就靠它 |
| 文件目录 | 内置文件浏览器（不依赖系统 DocumentsUI），选中 APK 直接 adb 安装 |
| 推送文件 | 手机扫码/浏览器上传到 `/sdcard/AdbInstaller`（默认端口 56789，被占用自动顺延） |

## 常见问题

- **扫描不到电视**：确认电视已开「USB 调试 / 网络调试」、手机和电视在同一网段；电视走网线、手机走 4G 都不行。
- **手机搜到了电视，点连接后仍显示未授权**：电视上必须点「允许 USB 调试吗」里的**始终允许**（部分小米电视弹窗在屏幕左上角，容易漏看），然后再在手机上点一次该设备。
- **点「选择文件」提示"没有可执行此操作的应用"**：老版本依赖系统文件选择器，现在已内置文件浏览器，不会再出现。
- **提示「无法执行内置 adb」**：内置 adb 是 ARM32；纯 64 位用户空间的设备需要换 aarch64 版。
- **手机扫码打不开**：同样是手机与电视不在同一网段，或路由器开了「AP 隔离」。
- **Android 13/14 列不出目录**：确认应用已获得存储权限（本应用 targetSdk 28，走旧权限模型）。

## 说明

- 系统要求：Android 5.1+（minSdk 21，targetSdk 28）。
- 应用不申请 root，也不向任何服务器上传数据；网络行为仅限你的局域网。
- 请只安装你信任的 APK；使用本工具造成的后果由使用者自行承担。

---

# English

An APK installer that runs on the TV / TV box itself: **install apps onto your TV entirely from your phone** — no PC, no USB drive.

> This repository only publishes the APK in [Releases](../../releases); source code is not published.

## Download

Get the latest `adb-install-vX.Y.apk` (currently v1.13) from [Releases](https://github.com/orVC/AdbInstaller/releases/latest).

Install the **same file** on both the phone and the TV — identical functionality.

## First install: just enable ADB on the TV, then install from the phone

No PC and no USB drive needed — the TV only has to allow ADB:

1. **TV**: Settings → About → tap *Build number* repeatedly to unlock Developer options → turn on **USB debugging / Network debugging**;
2. **Phone**: connect to the **same Wi-Fi** and use any Android app that speaks adb (e.g. 甲壳虫ADB助手, Xiaomi TV Assistant) to connect to the TV's `IP:5555`;
3. Accept **"Allow USB debugging"** on the TV with *Always allow* → OK;
4. Install `adb-install-vX.Y.apk` onto the TV from that app.

> Find the TV's IP under Settings → Network → current Wi-Fi details. Once installed, you no longer need that tool — this app takes over.
>
> If a phone is not at hand, copy the APK to a USB stick and use the TV's built-in *USB install / Media Center* instead.

## Install to your TV from a phone (recommended)

Requirements: ADB debugging enabled on the TV, and phone + TV on the **same Wi-Fi / router** (same subnet, no "AP isolation").

1. On the **phone** open this app → **搜索设备 (Scan)** → tap **扫描局域网设备 (Scan LAN)** — takes ~3–10 s;
2. Tap the TV's `IP:5555` in the list (compare with the IP shown on the TV's *推送文件 / Push files* tab);
3. On the **TV** accept **"Allow USB debugging"** with *Always allow* → OK; if the phone still reports *unauthorized*, tap the device once more (the app reconnects automatically);
4. On the phone switch to **文件目录 (Files)** → open the folder containing your APK (e.g. `Download`) → select it;
5. The phone logs `Performing Streamed Install` → `Success` — the app is now installed on the TV.

> No need to tap *获取本机 ADB 权限* on the phone — it connects to the TV, not to itself.

**Alternative transfer**: the TV's *推送文件 / Push files* tab shows a QR code and a URL; scan it (or open it in a browser) to upload APKs to `/sdcard/AdbInstaller` on the TV, then install from the TV's *文件目录* tab.

## Updating

Tap **检查更新 (Check for updates)** on the *Settings* tab. The app queries GitHub through the `gh-proxy.org` accelerator, downloads a newer APK if any, and opens the system installer (in-place upgrade, settings preserved). Direct connection is used as a fallback if the accelerator fails.

## The four tabs

| Tab | What it does |
| --- | --- |
| 设置 (Settings) | Target address (`127.0.0.1:5555` = this device, TV's `IP:5555` = that TV) + **Get ADB permission** + **Check for updates** |
| 搜索设备 (Scan) | Scans ports `5555` across `1–254` of your subnet; tap a device to connect — this is how phone-to-TV install works |
| 文件目录 (Files) | Built-in file browser (does **not** rely on the system DocumentsUI); select an APK to `adb install` it |
| 推送文件 (Push) | Phone scans a QR / opens a URL to upload files to `/sdcard/AdbInstaller` (default port 56789, auto-increments if busy) |

## FAQ

- **Scan finds nothing**: make sure USB/Network debugging is enabled on the TV and both devices are on the same subnet. A wired TV plus a phone on mobile data will not work.
- **Phone found the TV but still shows "unauthorized"**: you must accept *"Allow USB debugging"* with **Always allow** on the TV (on some Xiaomi TVs the dialog appears in the top-left corner). Then tap the device again on the phone.
- **"No app can perform this action" when picking a file**: older builds relied on the system file picker; the built-in browser removes that dependency.
- **"Cannot execute bundled adb"**: the bundled `adb` is ARM32; a 64-bit-only user space needs an aarch64 build.
- **QR page does not open on the phone**: again, different subnets or "AP isolation" on the router.
- **No directories listed on Android 13/14**: make sure the storage permission is granted (the app targets SDK 28 and uses the legacy permission model).

## Notes

- Requires Android 5.1+ (minSdk 21, targetSdk 28).
- The app does not request root and uploads nothing to any server; all network traffic stays on your LAN.
- Only install APKs you trust. Use at your own risk.
