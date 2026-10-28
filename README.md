<div align="center">

# CellularPro Unlocker

**装一个模块，把 CellularPro 里被会员挡住的地方打开。**

不改 APK，不重签名，不用手动敲命令，不绑定机型。

[![Release](https://img.shields.io/badge/release-v3.1-2ea043?style=flat-square)](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest)
[![Android](https://img.shields.io/badge/Android-8.0%2B-3ddc84?style=flat-square&logo=android&logoColor=white)]()
[![LSPosed](https://img.shields.io/badge/LSPosed-API%20102-fc8d62?style=flat-square)]()
[![Version](https://img.shields.io/badge/target-2.0.16.1-8b949e?style=flat-square)]()

</div>

---

## v3.1 更新了什么（性能修复，建议所有 v3.0 用户升级）

v3.0 里带了一套诊断用的重钩子，忘了在发布前拆掉，会拖死主线程：在 `FragmentLock` 和 `ge` 上挂了 169 个方法钩子，每次调用都走一遍调用栈；锁页一交互就开 12 秒窗口，期间每秒几十次的轮询全部走栈 + 拼串 + 写盘。

表现：**锁页待一会儿，RSRP / SINR 不刷新，整个界面卡住。**

v3.1 把这些全部拆掉：移除 169 个追踪钩子、移除 `View.setEnabled` 钩子、会员放行改成布尔标记、移除 logcat 镜像写入。功能一个没少。

---
## v3.0 更新了什么

- **锁网页控件全解锁**：锁 PCI / EARFCN / NR 小区 / NR 频点 / NR 频段 / LTE、WCDMA、GSM 频段，之前全是灰的点不动，现在都能点能填。
- **点锁网不再弹会员框**：跳过点击转发器，直接调用背后真正的方法。
- **局部放行，不闪退**：只在锁网页那几个方法的调用栈里临时放行会员判定，返回即恢复，App 自检看到的仍是真实值。
- **信令导出修好了**：反射爬不到列表时改用 `TextView` 文本缓存兜底，不会再提示「没有可导出的信令」。
- **彻底设备无关**：模块内没有任何硬编码地址、偏移或机型判断。

> **一句实话**：锁网控件解锁了、会员框也不弹了，但锁命令是否真的下发到基带，取决于 App 自己的原生层。实测在这台机器上 App 只发出查询、不发写锁命令，服务小区 PCI 不会变。那部分代码在 App 的 VMP 虚拟机里，模块够不到。

---

## 安装

1. 下载 [最新 Release](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest) 里的 APK
2. 安装后打开 **LSPosed** → 模块 → 启用 **CellularPro Unlocker**
3. 作用域只勾 **CellularPro**
4. 强行停止 CellularPro，再重新打开

**要求**：已 root + LSPosed（API 102）+ CellularPro 2.0.16.1（`make.more.r2d2.play.cellular_pro`）

---

## 兼容性

| 项目 | 说明 |
|---|---|
| App 版本 | CellularPro 2.0.16.1 |
| 系统 | Android 8.0 及以上 |
| 框架 | LSPosed（libxposed API 102） |
| 架构 | arm64-v8a |

---

完整说明、常见问题、历史版本见主仓库：[**ldxm666/CellularPro-Unlocker**](https://github.com/ldxm666/CellularPro-Unlocker)

## 卸载

LSPosed 里停用模块 → 卸载 APK → 强行停止 CellularPro。没有残留文件，也不改 App 本身。

## License

MIT