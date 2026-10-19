# 猎刃 R15 黑苹果 EFI（ThundeRobot Hunter R15 / HM770 / i7-13650HX / RTX 4060）

OpenCore 引导的 macOS EFI，来自一台雷神猎刃 R15 笔记本（出厂 Windows + 黑苹果双系统）。
**当前状态：除睡眠外全部功能正常**（Wi-Fi、有线网、声卡、显卡加速、USB、触控板、键盘、亮度、双系统引导均正常）。

## 硬件配置

| 组件 | 型号 |
| --- | --- |
| 处理器 | 英特尔第十三代酷睿 i7-13650HX（14 核，Raptor Lake-HX） |
| 主板 | THUNDEROBOT V350SNX-HL（英特尔 HM770 芯片组） |
| 内存 | 三星 16GB DDR5 4800MHz |
| 显卡 | NVIDIA GeForce RTX 4060 Laptop GPU 8GB（蓝天 / Clevo，已屏蔽，仅做计算输出） |
| 核显 | 英特尔 Iris Xe / UHD（i7-13650HX 集成，macOS 下走核显加速） |
| 显示器 | 京东方 BOE0B40（15.3 英寸） |
| 磁盘 | 忆联 YMTC PC300 512GB + 梵想 Fanxiang S500Pro 2TB |
| 声卡 | Realtek High Definition Audio（另路 NVIDIA HD Audio，已屏蔽） |
| 网卡 | Intel Wi-Fi 6E AX210 160MHz + Realtek PCIe GbE 有线网卡 |

## 目录结构

```
EFI
├── BOOT            # BOOTx64.efi 引导入口
└── OC
    ├── ACPI        # 编译好的 SSDT / 重命名补丁对应的 AML
    ├── Drivers     # OpenRuntime / HfsPlus / OpenCanopy 等
    ├── Kexts       # 全部内核扩展（Lilu / VirtualSMC / WhateverGreen / AppleALC / AirportItlwm 等）
    ├── OpenCore.efi
    ├── config.plist  # OpenCore 主配置
    ├── Resources   # 引导界面资源（图标 / 字体 / 标签）
    └── Tools
```

## 已知问题

1. **睡眠不可用**：当前 BIOS 下 S3 休眠/唤醒链路未打通，睡眠后无法唤醒或直接断电。
   可尝试方向：BIOS 中切换睡眠状态（Modern Standby S0ix ↔ S3）、重新启用被清理的 S3 相关 SSDT。
2. **RTX 4060 已屏蔽**：NVIDIA 独显在 macOS 下无驱动，本 EFI 采用屏蔽方案，不抢核显输出。

## 使用注意

- `config.plist` 中的 SMBIOS（`SystemProductName = MacBookPro16,3`）为机型占位信息，
  直接套用同机型时建议重新生成三码（Serial / MLB / ROM）后再使用。
- Wi-Fi 依赖 `AirportItlwm`；若升级到更高版本 macOS 后无线网卡失效，
  需同步更换对应系统版本的 itlwm 构建，或改用 `itlwm + HeliPort` 方案。
- 本仓库内容按"原样分享"上传，未做任何删减。

## 版本

- Release v1.0：完整 EFI 打包压缩包，随附详细硬件说明。
