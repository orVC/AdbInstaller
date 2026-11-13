# ADB 安装器 · AdbInstaller

电视 / 机顶盒上的 APK 安装器：**用手机给电视装应用**。

首次把它自己装到电视上，仍然需要一次外部手段（U 盘 / 小米电视助手 / 电脑 adb，见下）；装好之后，**以后再装任何应用都不再需要电脑和 U 盘**。

**中文** | [English](#english)

> 本仓库只发布安装包（Release），不提供源码 / This repo only publishes the APK in Releases, no source code.

## 下载

到 [Releases](https://github.com/orVC/AdbInstaller/releases/latest) 下载最新的 `adb-install-vX.Y.apk`（当前 v1.8）。

手机和电视装**同一个文件**即可，功能完全一样。

## 前提：第一次怎么把它装到电视上（避不开的一步）

这里必须说实话：**本应用不能自己把自己装上去**。它能解决的是"之后每一次装应用"，而不是它自己的首次安装——第一次仍然需要一次外部手段。

小米电视上常见途径（按省事程度排序）：

1. **U 盘（最省事）**：把 APK 拷到 U 盘 → 插到电视 → 电视会提示检测到 U 盘 → 用自带的「U 盘安装 / 媒体中心」打开安装。若提示未知来源，到 **设置 → 应用 → 特殊应用权限 → 安装未知应用** 里允许。
2. **小米电视助手（官方手机 App）**：手机装「小米电视助手」，与电视同一 WiFi，连接后可直接把手机里的 APK 推送到电视安装。
3. **电视自带的浏览器 / 文件管理器**：部分型号带有浏览器或文件管理，可直接下载并打开 APK 安装。
4. **电脑 + adb**：电脑与电视同一网段，电视开启「开发者选项 → USB 调试」（部分机型需先用 USB 线连一次并执行 `adb tcpip 5555`），然后：
   ```
   adb connect <电视IP>:5555
   adb install adb-install-vX.Y.apk
   ```
5. **电视上已有的第三方市场 / 文件管理器**：如当贝市场、小白文件管理器等，可从中安装本 APK。

装好这一次之后，后续所有 APK 都可以用本应用完成：手机扫码连电视直接装，或者电视上自己选文件装，不再需要 U 盘和电脑。

## 手机给电视装（主流用法，推荐）

前提：手机和电视连**同一个 WiFi / 路由器**（同一网段，路由器没开「AP 隔离」）。

1. **电视**：打开「开发者选项 → USB 调试 / 网络调试」（小米电视：设置 → 关于 → 连点版本号开启开发者选项），并先装上本应用（第一次可用 U 盘装，或让任意一台设备的 adb 帮你装进去）；
2. **手机**：也装同一个 APK；
3. 手机打开应用 →「搜索设备」→ 点「扫描局域网设备」（约 3–10 秒）；
4. 列表里出现电视的 `IP:5555`（可对照电视「推送文件」页显示的 IP 确认是哪台）→ **点它**；
5. **电视上**会弹「允许 USB 调试吗」→ 勾选**始终允许** → 确定；若手机仍提示未授权，再点一次那台设备（应用会自动断开重连握手）；
6. 手机切到「文件目录」→ 进入存放 APK 的目录（如 `Download`）→ 选中 APK 按确定；
7. 手机上日志出现 `Performing Streamed Install` → `Success`，电视上就已经装好了。

> 手机端**不需要**点「获取本机 ADB 权限」，因为它连的是电视，不是自己。

## 电视自己装（没有第二台设备时）

1. 把 APK 放进电视：插 U 盘，或用「推送文件」页扫码上传到 `/sdcard/AdbInstaller`；
2. 打开应用 →「设置」→ 点「获取本机 ADB 权限」；
3. 电视弹「允许 USB 调试吗」时勾选**始终允许** → 确定，**然后再点一次按钮**；
4. 状态变绿 → 切到「文件目录」→ 选中 APK 安装。

## 四个页签

| 页签 | 作用 |
| --- | --- |
| 设置 | 目标地址 + 获取本机 ADB 权限（内置 ARM32 adb，连 `127.0.0.1:5555`） |
| 搜索设备 | 扫描局域网 `1–254` 的 `5555` 端口，点一下即连接该设备 —— 手机给电视装就靠它 |
| 文件目录 | 内置文件浏览器（不依赖系统 DocumentsUI），选中 APK 直接 adb 安装 |
| 推送文件 | 手机扫码/浏览器上传到 `/sdcard/AdbInstaller`（默认端口 56789，被占用自动顺延） |

## 常见问题

- **手机搜到了电视，点连接后仍显示未授权**：电视上必须点「允许 USB 调试吗」里的**始终允许**（部分小米电视弹窗在屏幕左上角，容易漏看），然后再在手机上点一次该设备。
- **扫描不到电视**：确认电视已开「网络调试」、手机和电视在同一网段；电视走网线 / 手机走 4G 都不行。
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

An APK installer that runs on the TV / TV box itself: **install apps onto your TV from your phone.**

Getting this app onto the TV the first time still requires one external step (USB drive / Xiaomi TV Assistant / a PC with adb — see below). After that, installing any further APK no longer needs a PC or a USB drive.

> This repository only publishes the APK in [Releases](../../releases); source code is not published.

## Download

Get the latest `adb-install-vX.Y.apk` (currently v1.8) from [Releases](https://github.com/orVC/AdbInstaller/releases/latest).

Install the **same file** on both the phone and the TV — identical functionality.

## Prerequisite: getting this app onto the TV the first time

To be straightforward: **this app cannot install itself.** What it solves is *every* install after the first one — the very first install still needs one external path.

Common options on a Xiaomi TV (roughly from easiest):

1. **USB drive (easiest)**: copy the APK to a USB stick → plug it into the TV → the TV prompts about the drive → open the built-in *USB install / Media Center* and install it. If it complains about unknown sources, allow it under **Settings → Apps → Special app access → Install unknown apps**.
2. **Xiaomi TV Assistant (official phone app)**: install "小米电视助手" on the phone, connect over the same Wi-Fi, and push the APK straight to the TV.
3. **The TV's built-in browser / file manager**: some models ship one; download and open the APK there.
4. **PC with adb**: TV and PC on the same subnet, enable *Developer options → USB debugging* on the TV (some models need a one-time USB connection plus `adb tcpip 5555`), then:
   ```
   adb connect <TV_IP>:5555
   adb install adb-install-vX.Y.apk
   ```
5. **A third-party store / file manager already installed** (e.g. 当贝市场, 小白文件管理器): install this APK from there.

Once it is installed, every later APK can be handled by this app: scan-and-install from the phone, or pick the file on the TV itself — no USB drive or PC needed.

## Install to your TV from a phone (recommended)

Requirements: phone and TV on the **same Wi-Fi / router** (same subnet, no "AP isolation").

1. **TV**: enable *Developer options → USB debugging / Network debugging* (Xiaomi TV: Settings → About → tap the build number repeatedly), and install this app on it first (via USB drive, or push it with any `adb`);
2. **Phone**: install the same APK;
3. Open the app → **搜索设备 (Scan devices)** → tap **扫描局域网设备 (Scan LAN)** — takes ~3–10 s;
4. Tap the TV's `IP:5555` in the list (compare with the IP shown on the TV's *推送文件 / Push files* tab);
5. On the **TV**, accept **"Allow USB debugging"** with *Always allow* → OK; if the phone still reports *unauthorized*, tap the device once more (the app reconnects automatically);
6. On the phone switch to **文件目录 (Files)** → open the folder containing your APK (e.g. `Download`) → select it;
7. The phone logs `Performing Streamed Install` → `Success` — the app is now installed on the TV.

> No need to tap *获取本机 ADB 权限 (Get local ADB permission)* on the phone — it connects to the TV, not to itself.

## Install on the TV itself (no second device)

1. Get the APK onto the TV: USB drive, or use the *推送文件 / Push files* tab and scan the QR from a phone (uploads to `/sdcard/AdbInstaller`);
2. Open the app → **设置 (Settings)** → tap **获取本机 ADB 权限**;
3. Accept **"Allow USB debugging"** with *Always allow* → OK, then **tap the button once more**;
4. Status turns green → switch to **文件目录** → select the APK to install.

## The four tabs

| Tab | What it does |
| --- | --- |
| 设置 (Settings) | Target address + get local ADB permission (bundled ARM32 `adb`, connects to `127.0.0.1:5555`) |
| 搜索设备 (Scan) | Scans ports `5555` across `1–254` of your subnet; tap a device to connect — this is how phone-to-TV install works |
| 文件目录 (Files) | Built-in file browser (does **not** rely on the system DocumentsUI); select an APK to `adb install` it |
| 推送文件 (Push) | Phone scans a QR / opens a URL to upload files to `/sdcard/AdbInstaller` (default port 56789, auto-increments if busy) |

## FAQ

- **Phone found the TV but still shows "unauthorized"**: you must accept *"Allow USB debugging"* with **Always allow** on the TV (on some Xiaomi TVs the dialog appears in the top-left corner). Then tap the device again on the phone.
- **Scan finds nothing**: make sure network debugging is enabled on the TV and both devices are on the same subnet. A wired TV plus a phone on mobile data will not work.
- **"No app can perform this action" when picking a file**: older builds relied on the system file picker; the built-in browser removes that dependency.
- **"Cannot execute bundled adb"**: the bundled `adb` is ARM32; a 64-bit-only user space needs an aarch64 build.
- **QR page does not open on the phone**: again, different subnets or "AP isolation" on the router.
- **No directories listed on Android 13/14**: make sure the storage permission is granted (the app targets SDK 28 and uses the legacy permission model).

## Notes

- Requires Android 5.1+ (minSdk 21, targetSdk 28).
- The app does not request root and uploads nothing to any server; all network traffic stays on your LAN.
- Only install APKs you trust. Use at your own risk.
