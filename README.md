# linux6.x-bnx2x-2.5g-patch

[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)
[![Kernel Support](https://img.shields.io/badge/Kernel-6.1%20%7C%206.6%20%7C%206.12-orange.svg)]()

### [English] Project Overview
This repository hosts the **2.5 Gbps HSGMII/SGMII mode enablement patch** for Broadcom BCM57810A (`bnx2x`) network interface cards, specifically adapted for **Linux Kernel 6.x (e.g., 6.1, 6.6, 6.12)**. 

**The Problem:** The legendary 2.5G patch originally maintained by *JAMESMTL* was designed for older Linux kernels (3.x to 5.x). Due to massive refactoring of `netdev` and `ethtool` structures in Linux Kernel 6.x, applying the legacy patch will cause compilation errors or instant `Kernel Panic` upon loading.
**The Solution:** This updated patch is extracted and backported from the Arch Linux AUR (`bnx2x-2500m-dkms`) community. It perfectly adapts to the modern 6.x kernel network stack API, allowing your 2.5G SFP PON Stick (猫棒) to sync flawlessly at **2500 Mbps**.

### [中文] 项目简介
本仓库维护了专门针对博通 Broadcom BCM57810A (`bnx2x`) 万兆网卡的 **2.5 Gbps HSGMII/SGMII 速率解锁补丁**，完美适配 **Linux 6.x 全系列新内核 (如 6.1 / 6.6 / 6.12)**。

**解决痛点：** 过去由 *JAMESMTL* 大佬维护的经典 2.5G 补丁仅支持 Linux 3.x - 5.x 老内核。在 Linux 进入 6.x 内核后，底层网络栈的 `netdev` 与 `ethtool` 架构发生了翻天覆地的重构，导致老补丁完全失效，强行编译会触发内核崩溃（Kernel Panic）。
**解决方案：** 本补丁提取并优化自 Arch Linux AUR (`bnx2x-2500m-dkms`) 极客社区。它完美修复了现代内核的 API 兼容性问题，让Openwrt直通万兆网卡插 2.5G 猫棒时，能够稳稳握手 **2500 Mbps** 全速。

---

## 🛠️ Hardware Supported / 支持硬件
* **NICs / 网卡:** Broadcom BCM57810A Family (e.g., Dell Y40PH, HP 530SFP+, etc.)
* **SFP Modules / 猫棒:** 2.5G SFP GPON/EPON/XG-PON Sticks (e.g., ODI, Huawei, Nokia, ODI DFP-34X-2C2)

---

## 🚀 Usage in OpenWrt / ImmortalWrt CI (GitHub Actions)
If you are using GitHub Actions to build your custom OpenWrt/ImmortalWrt firmware (e.g., P3TERX template), simply append the following code to your **`diy-part2.sh`** script:

```bash
# 1. Automatically locate the target kernel patches directory for x86 architecture
PATCH_DIR=$(ls -d target/linux/x86/patches-* | tail -n 1)

# 2. Fetch the 6.x compatible 2.5G patch from this repository
curl -L "https://raw.githubusercontent.com/zhinengzi/linux6.x-bnx2x-2.5g-patch/refs/heads/main/linux6.x-bnx2x-2.5g.patch" -o $PATCH_DIR/999-linux6.x-bnx2x-2.5g.patch

echo "===> [SUCCESS] Linux6.x-Bnx2x-2.5G Patch injected successfully! <==="
```

# **💻 Manual Compilation / 本地手动编译**
If you are compiling the Linux kernel or OpenWrt on a local Linux machine (Ubuntu/Debian):

```Bash
# 1. 下载补丁并保存到当前目录（建议在 OpenWrt 或 Linux 源码的根目录下执行）
curl -L "https://raw.githubusercontent.com/zhinengzi/linux6.x-bnx2x-2.5g-patch/refs/heads/main/linux6.x-bnx2x-2.5g.patch" -o linux6.x-bnx2x-2.5g.patch

# 2. 进入博通 bnx2x 驱动源码目录（请将下面的路径替换为你本地的实际绝对路径）
cd target/linux/x86/image/  # 如果是 OpenWrt 环境
# 或者内核环境: cd drivers/net/ethernet/broadcom/bnx2x/

# 3. 跨目录调用刚刚下载的补丁进行缝合
patch -p1 < ../../../../../../../linux6.x-bnx2x-2.5g.patch
```

# Required OpenWrt .config Options
Make sure the bnx2x driver is selected as a built-in module or external kmod package:

```text
CONFIG_PACKAGE_kmod-bnx2x=y
CONFIG_PACKAGE_pciutils=y
```

# **📊 Verification / 实机验证**
After booting into your system, run ethtool to verify the link speed:

'''Bash
ethtool ethX
'''

## **Expected Output:**
<img width="1405" height="880" alt="98c6d265f32412e9d67cd65a00dddca0" src="https://github.com/user-attachments/assets/c656dd8d-4584-4300-a8ea-ade1d93a18b0" />
<img width="675" height="710" alt="c159cfba2b82401060d434f18c5e646e" src="https://github.com/user-attachments/assets/66f4a20b-9152-4d8d-94a3-7c01ff11d374" />
<img width="556" height="440" alt="2bb6e6278ead276ea3c42555d3758eaa" src="https://github.com/user-attachments/assets/5349556a-fa4b-4157-af1c-7b84a652c139" />



# **🤝 Acknowledgments / 致谢**
JAMESMTL: For the original 2.5G warpcore logic that inspired the entire community.

Arch Linux AUR Community: For maintaining and updating the dkms patch code for 6.x kernels.

ImmortalWrt / OpenWrt Teams: For providing the greatest router operating system.

# **📄 License**
This patch modifies the upstream Linux kernel driver and is therefore distributed under the GPL-2.0 License.
