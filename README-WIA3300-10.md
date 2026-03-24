# Skspruce WIA3300-10 适配指南

本仓库包含了针对 **Skspruce WIA3300-10** 路由器的 ImmortalWrt 24.10 完整适配代码。该设备硬件与斐讯 K2P 高度相似，但闪存容量更大（32MB）、内存更大（256MB）、带 USB 2.0 接口，端口顺序为 **LLLW**（从左至右：LAN1～LAN4、WAN）。

## 硬件规格
| 组件        | 型号/规格                     |
|------------|-----------------------------|
| CPU        | MediaTek MT7621AT (880MHz)   |
| 内存        | 256MB DDR3                   |
| 闪存        | 32MB SPI NOR (Winbond W25Q256) |
| 无线        | MT7615DN (2.4G/5G 双频单芯片) |
| 网络接口    | 4x Gigabit LAN, 1x Gigabit WAN (端口顺序 LLLW) |
| USB        | 1x USB 2.0                   |
| 复位键      | GPIO 18 (低电平有效)         |
| LED        | 电源（红/绿）、WAN、2.4G、5G |

## 适配特性
- **现代 DSA 架构**：采用双 GMAC 设计，GMAC0 负责 LAN 口，GMAC1 负责 WAN 口，实现理论 2Gbps 总带宽。
- **32MB 闪存分区布局**：完全占满 32MB 空间，Breed 兼容的 `firmware` 分区从 `0x50000` 开始，大小为 `0x1fb0000`。
- **MAC 地址自动修正**：通过 `/etc/rc.local` 启动脚本从 `factory` 分区读取 LAN MAC（偏移 `0xe000`），并为 2.4G 和 5G 分别生成 `LAN+2` 和 `LAN+3` 的 MAC 地址，解决双频芯片 MAC 自动递增失效的问题。
- **完整的 LED 配置**：电源灯心跳、WAN 灯网络活动、2.4G/5G 灯无线吞吐量指示。
- **USB 支持**：已集成 `kmod-usb3` 驱动，支持 USB 存储扩展。

## 已知问题
	Breed 对 LED 控制不完美：部分 LED 在启动初期可能状态异常，但系统启动后会由 OpenWrt 正确接管。

	5G MAC 需要脚本修正：因驱动原因，5G 无线默认会继承 2.4G 的 MAC，已通过 rc.local 脚本在启动时修正。


## 编译方法
1. 克隆本仓库并切换到 `wia3300-10-support` 分支：
   git clone https://github.com/uniessey/immortalwrt.git -b wia3300-10-support
   cd immortalwrt
2. 更新 feeds 并安装所需包：
	./scripts/feeds update -a
	./scripts/feeds install -a
3. 运行配置菜单，选择目标系统：
	make menuconfig
	*	Target System: MediaTek Ralink MIPS

	*	Subtarget: MT7621 based boards

	*	Target Profile: Skspruce WIA3300-10
4. 根据需要添加其他软件包（如 LuCI、USB 支持等），然后编译：
	首次编译使用：make -j1 V=s
	如果重新配置后重新编译：make -j$(nproc) V=s
5. 编译完成后，固件位于 bin/targets/ramips/mt7621/，文件名为 openwrt-ramips-mt7621-skspruce_wia3300-10-sysupgrade.bin。

	刷机说明
	推荐使用 Breed 引导程序（已适配 WIA3300-10 的版本）。

	刷机前请在 Breed 中正确设置 MAC 地址：

	LAN MAC：设备背面标签 MAC（例如 00:00:00:00:00:00）

	WAN MAC：标签 MAC +1（例如 00:00:00:00:00:01）

	RF1 (2.4G) WLAN MAC：标签 MAC +2（例如 00:00:00:00:00:02）

	RF2 (5G) WLAN MAC：标签 MAC +3（例如 00:00:00:00:00:03）

	在 Breed 中选择 闪存布局 → 公版(0x50000)，上传固件刷写。


	致谢
	感谢 ImmortalWrt 社区提供的优秀框架。

	感谢论坛开发者提供的 K2P 参考设计及 Breed 支持。

	许可证
	本适配代码遵循 GPL-2.0-or-later 协议，详见 SPDX 标识。

	如有问题，欢迎提交 Issue 或 Pull Request。
