<div align="center">

# CellularPro Unlocker

**不改 APK · 不重签名 · 一条 `su` 都不用手敲**

把 CellularPro 里那道「会员」闸门直接掰开：会员判定恒为真，关于页里被藏起来的三个测试入口一并补全。

[![Release](https://img.shields.io/badge/release-v1.0-2ea043?style=flat-square)](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest)
[![Android](https://img.shields.io/badge/Android-8.0%2B-3ddc84?style=flat-square&logo=android&logoColor=white)]()
[![ABI](https://img.shields.io/badge/ABI-arm64--v8a-8957e5?style=flat-square)]()
[![KernelSU](https://img.shields.io/badge/KernelSU%20%2F%20Magisk-supported-4c8eda?style=flat-square)]()
[![LSPosed](https://img.shields.io/badge/LSPosed-API%20102-fc8d62?style=flat-square)]()
[![Version](https://img.shields.io/badge/target-2.0.16.1-8b949e?style=flat-square)]()

</div>

---

## 目录

- [一分钟上手](#一分钟上手)
- [两个模块，各管一段](#两个模块各管一段)
- [解锁了什么](#解锁了什么)
- [KernelSU / Magisk 模块（会员解锁）](#kernelsu--magisk-模块会员解锁)
- [LSPosed 模块（入口补全）](#lsposed-模块入口补全)
- [不用模块，只改一个 so](#不用模块只改一个-so)
- [恢复原状](#恢复原状)
- [FAQ](#faq)
- [兼容性](#兼容性)

---

## 一分钟上手

> 前提：手机已 root（KernelSU / KernelSU Next / Magisk），并且装好了 **CellularPro 2.0.16.1**（包名 `make.more.r2d2.play.cellular_pro`）。

**第 1 步 · 装会员解锁模块**

下载 [`CellularPro_df_KSU_v1.0.zip`](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest/download/CellularPro_df_KSU_v1.0.zip)

- KernelSU / KernelSU Next：管理器 → **模块** → 从本地安装 → 选中 zip
- Magisk：**模块** → 从本地安装 → 选中 zip

**第 2 步 · 重启手机**

模块在开机完成后自动打补丁，打完写一行日志：

```
/data/adb/cellularpro_df.log
```

**第 3 步 · 打开 CellularPro**

会员功能已经可用：锁频段、锁小区、PCI 锁定等实操页面不再拦你。

**第 4 步（可选）· 补全关于页入口**

想要那几个测试/文件入口，再装 LSPosed 模块：[`CellularPro-Unlocker.apk`](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest/download/CellularPro-Unlocker.apk)
LSPosed 管理器 → 模块 → 安装 → **勾选作用域 `CellularPro`** → 强行停止 App → 重新打开。

---

## 两个模块，各管一段

CellularPro 的会员判定不在 Java 层，而在 `libQualcommAdapter.so` 里的一个谓词函数 `A0B0.ge.df()Z`。所以：

| 模块 | 要解决的问题 | 手段 | 是否需要重启 |
|---|---|---|---|
| **KernelSU / Magisk 模块**<br>`cellularpro_df` | 会员判定恒假 → 功能被锁 | 直接改设备上那份 `libQualcommAdapter.so` 的 8 个字节 | 需要（装完重启） |
| **LSPosed 模块**<br>`io.github.ldxm666.cellularpro` | 关于页三个入口被藏起来 | 运行时 hook 点击方法，把目标 Activity 直接拉起来 | 不需要（强停 App 即可） |

两个模块互不依赖，也可以只用其中一个：

- **只要会员功能** → 只装 KSU 模块，够了。
- **只要入口** → 只装 LSPosed 模块，但会员那层闸门还是关着的。

**两者都不修改 APK 本体**：签名不变、Play 更新照常、不需要卸载重装、不需要重签名。

---

## 解锁了什么

### 会员功能（KSU 模块提供）

| 项目 | 状态 |
|---|---|
| 会员判定（普通会员 / 高级会员谓词） | 恒为 `true` |
| 锁网 / 锁频段（LTE、NR 频段勾选） | 解除限制 |
| 锁小区 / PCI 锁定 | 解除限制 |
| 信令、事件、室内各页的会员遮罩 | 不再拦截 |

### 关于页入口（LSPosed 模块提供）

| 入口 | 原本 | 装完后 |
|---|---|---|
| **业务测试** | 点击无反应 | 打开 `AddTaskActivity` |
| **文件管理** | 点击无反应 | 打开 `FileManageActivity` |
| **LOG 回放** | 点击无反应 | 打开 `FileSelectActivity` |

> 说明：信令的「详细解码 / 导出」依赖基带 diag 数据，本模块不伪造数据；云端 / VIP 服务器侧的校验也没有动。

---

## KernelSU / Magisk 模块（会员解锁）

**下载**：[`CellularPro_df_KSU_v1.0.zip`](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest/download/CellularPro_df_KSU_v1.0.zip) · 约 1.5 MB · 模块 id `cellularpro_df`

### 安装

```text
KernelSU 管理器 → 模块 → 从本地安装 → CellularPro_df_KSU_v1.0.zip → 重启
```

Magisk 用户路径相同（Magisk → 模块 → 从本地安装）。

### 打完补丁长什么样

```text
$ su -c tail -3 /data/adb/cellularpro_df.log
[2026-01-01 12:00:01] target: /data/app/.../lib/arm64/libQualcommAdapter.so
[2026-01-01 12:00:01] md5 a1a7e57e... == stock -> payload swap
[2026-01-01 12:00:01] done, verify md5 ab3c6655...
```

### 它到底改了什么

| 项目 | 值 |
|---|---|
| 目标文件 | `lib/arm64-v8a/libQualcommAdapter.so` |
| 偏移 | `0xF0ED4`（十进制 `986836`） |
| 原字节（锚点） | `FF 83 01 D1`（`sub sp, sp, #0x60`） |
| 写入字节 | `20 00 80 52 C0 03 5F D6` = `mov w0, #1 ; ret` |
| 效果 | `A0B0.ge.df()Z` 永远返回 `true` |
| 大小变化 | 无（就地改写 8 字节，不增不减） |

### 三个安全阀

1. **原版识别**：命中原版 md5 `a1a7e57e…` → 整文件替换成已补好的同名 so，一次到位。
2. **锚点校验**：md5 不认识时，先读偏移处的 4 字节锚点；不是 `FF 83 01 D1` 就 **直接放弃，一个字节都不写**（退出码 3）。App 换版本、so 重排，都不会被写坏。
3. **看门狗**：`service.sh` 每 5 分钟巡一次，应用升级把 so 覆盖回原版后，会自动重新打上。

### 文件清单

```text
/data/adb/modules/cellularpro_df/
├── module.prop     模块信息
├── config.sh       包名 / 偏移 / 锚点 md5 / 巡检间隔
├── patch.sh        补丁本体（可单独跑）
├── service.sh      开机自动执行 + 看门狗
├── restore.sh      恢复原版
├── payload/
│   ├── libQA.df.so 已补好的 so（原版 md5 命中时整文件使用）
│   └── df8.bin     那 8 个字节
├── backup/         首次修补前的原版备份
└── tools/
    └── CellularPro-Unlocker.apk   ← 顺便捎带的 LSPosed 模块，装不装随意
```

手动打一次：

```bash
su -c sh /data/adb/modules/cellularpro_df/patch.sh
```

---

## LSPosed 模块（入口补全）

**下载**：[`CellularPro-Unlocker.apk`](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest/download/CellularPro-Unlocker.apk) · 25 KB · 包名 `io.github.ldxm666.cellularpro`

### 安装

1. 安装 APK（普通安装即可）。
2. 打开 **LSPosed 管理器 → 模块**，启用 **CellularPro Unlocker**。
3. 进入模块详情，确认作用域勾选了 **CellularPro**（`make.more.r2d2.play.cellular_pro`）。
4. **强行停止** CellularPro，再打开。

### 生效检查

打开 App → 我的 / 关于页 → 点「业务测试」「文件管理」「LOG 回放」，应当直接进入对应页面。

模块自己会写一份日志，出问题先看它：

```text
/sdcard/Android/data/make.more.r2d2.play.cellular_pro/files/lsp_flags.log
```

正常长这样：

```text
unlock hooked task_test -> ...test.activity.AddTaskActivity
unlock hooked file_management -> ...activity.FileManageActivity
unlock hooked layout_play_back -> ...activity.FileSelectActivity
```

### 技术参数

| 项目 | 值 |
|---|---|
| 框架 | libxposed API 102（`minApiVersion=100` / `targetApiVersion=102`） |
| 作用域 | 静态作用域，`scope.list` 内置 `make.more.r2d2.play.cellular_pro` |
| Hook 点 | `AboutActivity.task_test` / `.file_management` / `.layout_play_back` |
| 依赖 | 无第三方库，25 KB 单 dex |

---

## 不用模块，只改一个 so

不想装模块？那就自己动手，两个命令的事。设备上那份 so 的路径：

```bash
su
SO=$(pm path make.more.r2d2.play.cellular_pro | sed 's/package://;s#/base.apk##')/lib/arm64/libQualcommAdapter.so
printf '\x20\x00\x80\x52\xC0\x03\x5F\xD6' | dd of="$SO" bs=1 seek=986836 conv=notrunc
```

改完 `am force-stop make.more.r2d2.play.cellular_pro`，重开 App 即生效。
改之前先把原文件拷出来一份，`restore` 的时候整文件覆盖回去最干净。

> 注意：这一改是**直接在应用目录里**动手，Play 商店更新 App 会把它覆盖回原版 —— 这也是为什么推荐用模块，模块会自己重打。

---

## 恢复原状

按你当初怎么改的，反向操作：

**KSU / Magisk 模块**

```bash
su -c sh /data/adb/modules/cellularpro_df/restore.sh   # 把备份的 so 盖回去
```

然后管理器里卸载模块、重启。想彻底干净：卸载 CellularPro 后重装一次。

**LSPosed 模块**

管理器里关掉模块 → 强行停止 App → 重新打开。或者直接卸载 APK。

**两条都做了** 就等于没来过。

---

## FAQ

**Q：装完打开还是老样子？**
A：先重启（KSU 模块要开机后才跑），再看 `/data/adb/cellularpro_df.log`。日志里出现 `done` 才算打上。

**Q：日志里写 `anchor mismatch`、退出码 3？**
A：说明你的 App 版本不是 2.0.16.1，so 里那段机器码变了。模块**一个字节都没写**，App 是完好的。换对应版本的模块，或按上面的手动方式自己确认偏移。

**Q：App 更新后会员又没了？**
A：正常，更新会覆盖 so。模块的看门狗 5 分钟内会重打；急着用就手动跑一次 `patch.sh`。

**Q：会不会掉签名、被商店判定篡改？**
A：不会。两个模块都不碰 `base.apk`，只改 `lib/` 下的 so 和运行时 hook，签名与原版完全一致。

**Q：只装 LSPosed 模块行不行？**
A：不行。会员判定在 so 里，LSPosed 模块只补入口。

**Q：会掉 root 隐藏 / 被检测吗？**
A：模块本身不碰检测逻辑。需要隐藏的话自行用 KernelSU 的 susfs / Zygisk 方案。

**Q：为什么只有 arm64-v8a？**
A：这个 App 只带 arm64 一份 so，x86 设备上它自己也跑不起来。

---

## 兼容性

| 项目 | 支持情况 |
|---|---|
| CellularPro | 2.0.16.1（110），`make.more.r2d2.play.cellular_pro` |
| ABI | arm64-v8a |
| Android | 8.0+（模块脚本 `minSdk 28` 逻辑） |
| Root 方案 | KernelSU、KernelSU Next、Magisk（含 susfs 分支） |
| Xposed | LSPosed（libxposed API 100–102） |
| 已验证设备 | Xiaomi 2210132C · Android 16 · KernelSU Next · LSPosed 1.9.x |

---

## 仓库结构

```text
CellularPro-Unlocker/
├── ksu/        KernelSU / Magisk 模块源码（module.prop、patch.sh、service.sh…）
└── module/     LSPosed 模块源码（Java + Manifest + xposed 元数据）
```

模块源码仅作审阅用途：里面的偏移、锚点、Hook 点都可逐行对照。

---

<div align="center">

**仅供个人学习与自有设备调试使用。** 喜欢这个 App 请支持正版。

</div>