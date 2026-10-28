<div align="center">

# CellularPro Unlocker

**装一个模块，把 CellularPro 里几个被会员挡住的地方打开。**

不改 APK，不重签名，不用手动敲命令。

[![Release](https://img.shields.io/badge/release-v2.0-2ea043?style=flat-square)](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest)
[![Android](https://img.shields.io/badge/Android-8.0%2B-3ddc84?style=flat-square&logo=android&logoColor=white)]()
[![LSPosed](https://img.shields.io/badge/LSPosed-API%20102-fc8d62?style=flat-square)]()
[![Version](https://img.shields.io/badge/target-2.0.16.1-8b949e?style=flat-square)]()

</div>

---

## v2.0 更新了什么

- **信令导出能用了**：点右上角导出，直接生成 CSV 文件，不再弹「抱歉！仅对高级会员开放」。
- **测试计划能进了**：点顶部任务按钮，直接打开测试计划页面。
- **不再闪退**：上一版会去强改会员判定，App 会自己退出；这一版把那段整个删掉了。
- **更稳**：冷启动、切页、锁网页勾选都试过，进程不重启。

> 会员判定本身写在 App 的原生库里，硬改会被自检直接杀掉进程。所以这一版不动它，改成把具体功能一个个接出来。

---

## 一分钟上手

**前提**：手机已 root，装了 **LSPosed**（API 102，LSPosed 1.9.x 以上），并且已经装了 **CellularPro 2.0.16.1**（包名 `make.more.r2d2.play.cellular_pro`）。

1. 下载 [`CellularPro_df_LSP_v2.0.apk`](https://github.com/ldxm666/CellularPro-Unlocker/releases/latest/download/CellularPro_df_LSP_v2.0.apk)
2. 安装，打开 **LSPosed** → 模块 → 启用 **CellularPro Unlocker**
3. 作用域只勾 **CellularPro**
4. 强行停止 CellularPro，再重新打开

打开 App 后模块就在工作了，不需要额外设置。

---

## 怎么用

### 导出信令

1. 打开 CellularPro，切到 **信令** 页
2. 点右上角 **导出** 图标
3. 文件生成在：

```
Android/data/make.more.r2d2.play.cellular_pro/files/CellularPro/export/signaling_<时间戳>.csv
```

4. UTF-8 带 BOM，Excel / WPS 直接双击打开，三列：**时间 / 制式 / 消息**

点导出后屏幕下方会提示导出了多少条，以及文件的完整路径。

### 测试计划

点顶部 **任务** 图标，直接进入测试计划页面，可以自己配任务、设次数。

---

## 关于会员

会员判定和会员状态显示在 App 的原生库里，Java 层改不了；强改会让 App 自检退出，所以这一版没有去动它。

结果是：**导出信令** 和 **测试计划入口** 现在可用；其余需要会员的功能（比如锁网下发）仍然走 App 原本的校验。

---

## 常见问题

**装完没反应？**
检查 LSPosed 里模块是否启用、作用域是否勾了 CellularPro，然后强行停止 App 再打开。

**导出点了没文件？**
先切到「信令」页停几秒，让列表有数据，再点导出。文件在 `Android/data/make.more.r2d2.play.cellular_pro/files/CellularPro/export/`。

**会掉会员状态吗？**
模块不写任何会员字段，只接管具体功能，App 的其他行为保持原样。

**升级 App 后失效？**
模块绑定的是 2.0.16.1，App 更新后可能需要适配。

---

## 卸载

LSPosed 里停用模块 → 卸载 APK → 强行停止 CellularPro。没有残留文件，也不改 App 本身。

---

## 兼容性

| 项目 | 说明 |
|---|---|
| App 版本 | CellularPro 2.0.16.1 |
| 包名 | `make.more.r2d2.play.cellular_pro` |
| 系统 | Android 8.0 及以上 |
| 框架 | LSPosed（libxposed API 102） |
| 架构 | arm64-v8a |

---

## 历史版本

- **v1.0**：KernelSU 打 so 补丁的方案。该方案会让 App 起不来（原生自检），**已废弃，不要再用**。
- **v2.0**：改为纯 LSPosed 模块，稳定可用。
