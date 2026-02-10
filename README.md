<img src="https://avatars.githubusercontent.com/u/53193414?s=200&v=4" alt="logo" width="200" height="200" align="right">

# Project ImmortalWrt

ImmortalWrt is a fork of [OpenWrt](https://openwrt.org), with more packages ported, more devices supported, default optimized profiles and localization modifications for mainland China users.<br/>
Compared to upstream, we allow to use (non-upstreamable) modifications/hacks to provide better feature/performance/support.

Default login address: http://192.168.1.1 or http://immortalwrt.lan, username: __root__, password: _none_.

## Download
Built firmware images are available for many architectures and come with a package selection to be used as WiFi home router. To quickly find a factory image usable to migrate from a vendor stock firmware to ImmortalWrt, try the *Firmware Selector*.

- [ImmortalWrt Firmware Selector](https://firmware-selector.immortalwrt.org/)

If your device is supported, please follow the **Info** link to see install instructions or consult the support resources listed below.

## Development
To build your own firmware you need a GNU/Linux, BSD or macOS system (case sensitive filesystem required). Cygwin is unsupported because of the lack of a case sensitive file system.<br/>

  ### Requirements
  To build with this project, Debian 11 is preferred. And you need use the CPU based on AMD64 architecture, with at least 4GB RAM and 25 GB available disk space. Make sure the __Internet__ is accessible.

  The following tools are needed to compile ImmortalWrt, the package names vary between distributions.

  - Here is an example for Debian/Ubuntu users:<br/>
    - Method 1:
      <details>
        <summary>Setup dependencies via APT</summary>

        ```bash
        sudo apt update -y
        sudo apt full-upgrade -y
        sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
          bzip2 ccache clang cmake cpio curl device-tree-compiler ecj fastjar flex gawk gettext gcc-multilib \
          g++-multilib git gnutls-dev gperf haveged help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev \
          libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses-dev libpython3-dev \
          libreadline-dev libssl-dev libtool libyaml-dev libz-dev lld llvm lrzsz mkisofs msmtp nano \
          ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip python3-ply python3-docutils \
          python3-pyelftools qemu-utils re2c rsync scons squashfs-tools subversion swig texinfo uglifyjs \
          upx-ucl unzip vim wget xmlto xxd zlib1g-dev zstd
        ```
      </details>
    - Method 2:
      ```bash
      sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'
      ```

  Note:
  - Do everything as an unprivileged user, not root, without sudo.
  - Using CPUs based on other architectures should be fine to compile ImmortalWrt, but more hacks are needed - No warranty at all.
  - You must __not__ have spaces or non-ascii characters in PATH or in the work folders on the drive.
  - If you're using Windows Subsystem for Linux (or WSL), removing Windows folders from PATH is required, please see [Build system setup WSL](https://openwrt.org/docs/guide-developer/build-system/wsl) documentation.
  - Using macOS as the host build OS is __not__ recommended. No warranty at all. You can get tips from [Build system setup macOS](https://openwrt.org/docs/guide-developer/build-system/buildroot.exigence.macosx) documentation.
  - For more details, please see [Build system setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem) documentation.

  ### Quickstart
  1. Run `git clone -b <branch> --single-branch --filter=blob:none https://github.com/immortalwrt/immortalwrt` to clone the source code.
  2. Run `cd immortalwrt` to enter source directory.
  3. Run `./scripts/feeds update -a` to obtain all the latest package definitions defined in feeds.conf / feeds.conf.default
  4. Run `./scripts/feeds install -a` to install symlinks for all obtained packages into package/feeds/
  5. Run `make menuconfig` to select your preferred configuration for the toolchain, target system & firmware packages.
  6. Run `make` to build your firmware. This will download all sources, build the cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen applications for your target system.

  ### Related Repositories
  The main repository uses multiple sub-repositories to manage packages of different categories. All packages are installed via the OpenWrt package manager called opkg. If you're looking to develop the web interface or port packages to ImmortalWrt, please find the fitting repository below.
  - [LuCI Web Interface](https://github.com/immortalwrt/luci): Modern and modular interface to control the device via a web browser.
  - [ImmortalWrt Packages](https://github.com/immortalwrt/packages): Community repository of ported packages.
  - [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically focused on (mesh) routing.
  - [OpenWrt Video](https://github.com/openwrt/video): Packages specifically focused on display servers and clients (Xorg and Wayland).

## Support Information
For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)
  ### Documentation
  - [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
  - [User Guide](https://openwrt.org/docs/guide-user/start)
  - [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
  - [Technical Reference](https://openwrt.org/docs/techref/start)

  ### Support Community
  - Support Chat: group [@ctcgfw_openwrt_discuss](https://t.me/ctcgfw_openwrt_discuss) on [Telegram](https://telegram.org/).
  - Support Chat: group [#immortalwrt](https://matrix.to/#/#immortalwrt:matrix.org) on [Matrix](https://matrix.org/).

## License
ImmortalWrt is licensed under [GPL-2.0-only](https://spdx.org/licenses/GPL-2.0-only.html).

## Acknowledgements
<table>
  <tr>
    <td><a href="https://dlercloud.com/"><img src="https://user-images.githubusercontent.com/22235437/111103249-f9ec6e00-8588-11eb-9bfc-67cc55574555.png" width="183" height="52" border="0" alt="Dler Cloud"></a></td>
    <td><a href="https://www.jetbrains.com/"><img src="https://resources.jetbrains.com/storage/products/company/brand/logos/jb_square.png" width="120" height="120" border="0" alt="JetBrains Black Box Logo logo"></a></td>
    <td><a href="https://sourceforge.net/"><img src="https://sourceforge.net/sflogo.php?type=17&group_id=3663829" alt="SourceForge" width=200></a></td>
  </tr>
</table>


# ImmortalWrt 24.10 分支维护备忘录

## 基本信息
- **分支名称**: openwrt-24.10
- **上游仓库**: https://github.com/immortalwrt/immortalwrt
- **个人镜像**: git@github.com:uniessey/immortalwrt.git
- **适配设备**: WIA3300-10

## 常用 Git 操作

### 1. 查看分支状态
```bash
git branch -a # 查看所有本地和远程分支
git remote -v # 查看远程仓库地址
git log --oneline -10 # 查看精简提交历史

####2. 与上游仓库同步更新（关键）
# 获取上游所有最新改动
git fetch upstream

# 合并上游 openwrt-24.10 分支到当前分支
git merge upstream/openwrt-24.10

# 如果合并后出现冲突，手动解决冲突文件，然后：
git add <已解决冲突的文件>
git commit -m “merge: resolve conflicts”

###3. 推送更新到个人仓库
# 常规推送
git push

# 如果本地分支版本落后于远程，可能需要拉取变基后再推送
git pull --rebase origin openwrt-24.10
git push


######4. 减少重复输入 SSH 密码
# 启动 ssh-agent 并添加密钥（当前会话有效）
eval “$(ssh-agent -s)”
ssh-add ~/.ssh/id_ed25519

####其他提醒
重大修改前，创建新分支进行操作：git checkout -b feat/新功能名

定期执行 git fetch --all 了解所有远程的最新状态。

本分支主要用于 WIA3300-10 设备适配，合并其他功能时请注意测试。

####4、提交更改到本地仓库：
git add README.md
git commit -m “docs: add maintenance notes for openwrt-24.10 branch”

###5、推送到你的 GitHub 仓库（由于之前设置了跟踪，直接 push 即可）：

git push
