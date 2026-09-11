# 软件安装包格式与 Arch 使用速查

> 目的：下载软件时知道该选哪种格式；在 Arch Linux 上知道怎么装、怎么找、怎么卸。
> 适用系统：Arch Linux（含 Manjaro / EndeavourOS 等 Arch 系）
> 最后更新：2026-09-11

---

## 目录

[TOC]

---

## 一、主流操作系统与对应安装包格式

| 操作系统 | 家族 | 常见安装包格式 | 说明 |
|---|---|---|---|
| **Windows** | — | `.exe`、`.msi`、`.msix` / `.appx` | `.exe` 最常见（NSIS / Inno Setup 打包）；`.msi` 是微软标准安装器；`.msix/.appx` 是商店格式 |
| **macOS** | Unix (Darwin) | `.dmg`、`.pkg`、`.app`、`.ipa` | `.dmg` 镜像拖进 Applications 即装；`.pkg` 是系统级安装包；`.app` 经常直接放在 `.zip` 里；`.ipa` 是 iOS 的，Mac 上不能装 |
| **Android** | Linux 内核 | `.apk`、`.aab`、`.xapk` | `.aab` 是商店提交格式（不能直接装）；`.apk` 可直接侧载 |
| **iOS** | Unix (Darwin) | `.ipa` | 基本只能通过 App Store 安装 |
| **Ubuntu / Debian** | Debian 系 | `.deb`、`.snap`、Flatpak、`.AppImage` | `.deb` 最经典；Ubuntu 官方主推 `.snap`；Flatpak 和 AppImage 是跨发行版通用格式 |
| **Linux Mint** | Debian 系 | `.deb`、`.flatpak`、`.AppImage` | 同上，Mint 也支持 Flatpak |
| **Fedora** | RPM 系 | `.rpm`、Flatpak、`.AppImage` | `.rpm` 是主力 |
| **RHEL / CentOS / Rocky / Alma** | RPM 系 | `.rpm` | 服务器/企业场景，基本只认 `.rpm` |
| **openSUSE** | RPM 系 | `.rpm` | |
| **Arch / Manjaro / EndeavourOS** | Arch 系 | 无独立安装包（pacman / AUR） | 软件从仓库装，没有 `.deb` / `.rpm` 这种东西，见第四节 |
| **Alpine** | 独立 Linux | `.apk` | ⚠️ 和安卓 `.apk` 名字一样但**完全不是一回事** |
| **ChromeOS** | Linux 内核 | `.deb`（Linux 容器内）、安卓 `.apk` | 桌面 Linux 容器里装 `.deb` |
| **FreeBSD / OpenBSD** | BSD | `.txz`（pkg 仓库） | 服务器为主，不常见 |

---

## 二、跨发行版通用 Linux 格式

这类格式"不分发行版"，几乎所有 Linux 都能用：

| 格式 | 特点 | 什么时候选它 |
|---|---|---|
| **`.AppImage`** | 单文件、免安装，下载后 `chmod +x` 双击就能跑 | 最省心，推荐优先选 |
| **Flatpak**（`.flatpak` 或 flathub） | 沙盒运行，需要先装 Flatpak 运行时 | 有商店/自动更新需求时 |
| **Snap**（`.snap`） | Ubuntu 主推，启动略慢 | 只在 Ubuntu 系推荐 |
| **`.tar.gz` / 源码** | 压缩包解压即用，或源码编译 | 其他格式都没有时的兜底 |

---

## 三、看到文件名快速判断

| 后缀 | 适用系统 | 说明 |
|---|---|---|
| `.deb` | **仅 Debian / Ubuntu / Mint** 系 | 其他发行版不能直接装 |
| `.rpm` | **仅 Fedora / RHEL / openSUSE** 系 | 其他发行版不能直接装 |
| `.AppImage` | 所有 Linux | 通用，直接下 |
| `.sh` / `.run` / `.bin` | 所有 Linux | 通用安装脚本 / 自解压程序（⚠️ 不是 Windows 的 `.exe`） |
| `.tar.gz` / `.zip` | 多数系统 | 解压即用，多数通用 |
| `.exe` / `.msi` | 仅 Windows | |
| `.dmg` / `.pkg` | 仅 macOS | |
| `.apk` | 仅 Android | |
| `.ipa` | 仅 iOS | |

> ⚠️ **`.bin` 的坑**：Linux 下 `.bin` 通常指可执行安装程序或固件（显卡驱动、VMware 等）；Windows 下 `.bin` 多半是**光盘镜像**（配 `.cue` 用），别搞混。

---

## 四、Arch 上能安装的包格式（按推荐顺序）

| 优先级 | 格式/来源 | 怎么装 | 说明 |
|---|---|---|---|
| ⭐ 1 | **pacman 官方仓库**（`.pkg.tar.zst`） | `sudo pacman -S 包名` | 官方维护、最稳，首选 |
| ⭐ 2 | **AUR**（PKGBUILD 源码或 -bin 预编译） | `yay -S 包名` | 社区维护，几乎什么都有 |
| 3 | **本地/远程 .pkg.tar.zst** | `sudo pacman -U 文件` 或 `sudo pacman -U URL` | 别人构建好的 Arch 包 |
| 4 | **AppImage**（单文件） | `chmod +x xxx.AppImage && ./xxx.AppImage` | 免安装、通用，双击即用 |
| 5 | **Flatpak**（`.flatpak` / `.flatpakref`） | `flatpak install 文件` / 从 Flathub 装 | 需先装 `flatpak`，沙盒运行 |
| 6 | **Snap**（`.snap`） | `snap install 文件` | 需先装 `snapd`，Ubuntu 主推，Arch 上不常用 |
| 7 | **源码**（`.tar.gz` 等） | 解压后 `makepkg -si`（若有 PKGBUILD）或手动 `./configure && make && sudo make install` | 兜底方案；**不推荐直接 make install**，难卸载 |
| 8 | **通用脚本**（`.sh` / `.run` / `.bin`） | `chmod +x 文件 && sudo ./文件` | 官方安装器（如部分显卡驱动/IDE），可接受 |
| ❌ | **`.deb` / `.rpm`** | 理论上可转换，**不建议** | 会污染依赖，出问题难排查，先找 AUR |

> 补充：Arch 官方包格式是 `.pkg.tar.zst`（新版）/ `.pkg.tar.xz`（旧版），这是 Arch 专属的，网上其他格式都不是给 pacman 直接用的。

---

## 五、pacman 常用命令（官方仓库）

### 查找 / 查看
```bash
pacman -Ss 关键词        # 搜索软件源里的包
pacman -Si 包名          # 查看远程包详细信息（依赖、大小、描述）
pacman -Q 包名           # 查看本地是否已安装
pacman -Qi 包名          # 查看已安装包的详细信息
pacman -Qe               # 列出所有“显式安装”的包（你自己装的）
pacman -Ql 包名          # 列出该包安装后包含哪些文件
pacman -Qo /路径/文件    # 查询某个文件属于哪个包
pacman -Qtdq             # 列出孤儿包（不再被依赖的残留）
pacman -F 文件名         # 在文件数据库里搜：哪个包提供这个文件
```

### 安装 / 更新
```bash
sudo pacman -S 包名                  # 安装（可多个，空格分隔）
sudo pacman -Syu                     # 全系统升级（日常更新就这个）
sudo pacman -U 本地包.pkg.tar.zst    # 安装本地包文件
sudo pacman -U http://.../pkg.zst    # 安装远程包
```

> ⚠️ **不要单独执行 `sudo pacman -Sy`（只同步数据库不升级）再去装包**，容易造成“部分升级”破坏系统。要么 `-Syu` 一起，要么先完整升级再装。

### 卸载
```bash
sudo pacman -R 包名        # 删除包（保留配置文件）
sudo pacman -Rs 包名       # 删除包 + 不再被需要的依赖（推荐日常用）
sudo pacman -Rns 包名      # 删除包 + 依赖 + 配置文件（最彻底）
sudo pacman -Rdd 包名      # 强制删除（忽略依赖检查，慎用）
```

### 清理
```bash
sudo pacman -Sc            # 清理旧版本包缓存（保留当前版本）
sudo pacman -Scc           # 清空整个包缓存
sudo pacman -Rns $(pacman -Qtdq)   # 一键清理孤儿包（推荐定期做）
paccache -r                # 更精细清理：只留最近版本（来自 pacman-contrib）
```

---

## 六、yay 常用命令（AUR + 官方仓库一体）

yay 是 pacman 的包装器，**pacman 的命令 yay 基本都能用**，另外自动覆盖 AUR：

```bash
yay -Ss 关键词        # 同时搜官方仓库 + AUR
yay -S 包名           # 安装（自动判断官方/AUR，AUR 会询问 PKGBUILD）
yay -Syu              # 升级官方包 + AUR 包（日常更新推荐直接 `yay`）
yay -Si 包名          # 查看包信息
yay -Rns 包名         # 卸载（同 pacman）
yay -Qi 包名          # 已安装包信息
yay -Qe               # 列出显式安装的包
yay -Qu               # 查看有哪些更新
yay -G 包名           # 只把 PKGBUILD 下载到当前目录（不安装，方便检查源码）
yay --editmenu        # 安装时弹出 PKGBUILD 编辑界面（想审查源码时用）
```

### AUR 使用要点
- AUR 包是**社区用户维护**的，装之前最好 `yay -G 包名` 看下 PKGBUILD 和评论区反馈；
- 同一软件常见后缀：`xxx`（源码编译）、`xxx-bin`（官方预编译二进制，装得快）、`xxx-git`（开发版）；
- 装 AUR 需要 `base-devel`（make/gcc 等），一般 Arch 装机时已包含；
- 首次用 yay 会提示选择 AUR 助手（继续用 yay 即可）。

---

## 七、实际场景速查

| 想做什么 | 命令 |
|---|---|
| 找软件 | `yay -Ss 关键词` |
| 装软件 | 官方：`sudo pacman -S 包名`；找不到再用：`yay -S 包名` |
| 更新系统（含 AUR） | `yay` 或 `yay -Syu` |
| 卸载软件（推荐） | `sudo pacman -Rns 包名` |
| 卸载并删配置 | `sudo pacman -Rns 包名`（-n 即删配置） |
| 定期清理 | `sudo pacman -Sc` + `sudo pacman -Rns $(pacman -Qtdq)` |
| 下载了 .pkg.tar.zst | `sudo pacman -U ./文件.pkg.tar.zst` |
| 下载了 .AppImage | `chmod +x ./文件.AppImage && ./文件.AppImage` |
| 下载了 .deb / .rpm | **跳过**，去 `yay -Ss 包名` 找 Arch 版 |
| 查文件属于哪个包 | `pacman -Qo /usr/bin/xxx` |

---

## 八、Arch 使用注意事项

1. **别用 `sudo pacman -Sy` 单独同步数据库**后立即装包 → 部分升级风险；
2. 升级前如果看到大版本更新，先看一眼 Arch 新闻（archlinux.org）和中文社区（archlinuxcn）；
3. 长期不更新再升级，先 `sudo pacman -S archlinux-keyring` 刷新密钥；
4. 从官网下载的 `.deb`/`.rpm` 在 Arch 上**基本都不适用**，先搜 AUR；
5. 多用 `pacman -Qtdq` 清理孤儿，保持系统干净。

> **一句话总结**：在 Arch 上，**先去 pacman / AUR 搜，搜不到就下 AppImage**；看到 `.deb` / `.rpm` 直接跳过即可。
