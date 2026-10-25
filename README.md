# Geek-Uninstaller-for-Linux-
全程由deepseekv4.1制作
Geek Uninstaller for Linux 是把 Windows 免费软件 Geek Uninstaller 1.5.3.170（作者 Thomas Koen）的完整体验移植到 GNU/Linux 的独立实现，使用 Python 3.10 编写，按 MIT 协议发布。  它做的事情和原版一样：用一张排序的列表列出系统里安装的软件，用系统原生方式真正卸载它们；当卸载器损坏、或者卸载后留下残留时，用 强制移除 向导把残留清理干净。区别只在于 Linux 没有注册表和 MSI——这里换成包管理器、配置文件与系统状态。
 功能
统一列表：dpkg/APT（Debian、Ubuntu、Mint…）、RPM + dnf/zypper（Fedora、RHEL、openSUSE…）、pacman（Arch、Manjaro…）、snap、flatpak，以及不属于任何软件包的 .desktop 条目与 AppImage；三列：程序名称 / 大小 / 安装时间。
原生卸载：调用 apt-get remove、dnf remove、zypper remove、pacman -Rns、snap remove、flatpak uninstall；界面本身以普通用户运行，每个操作单独通过 pkexec 提权。
强制移除向导：分组展示“文件系统”与“配置和系统状态”（配置/缓存目录、.desktop、systemd、autostart、cron、udev、polkit、D-Bus…），勾选后删除；危险路径与符号链接逃逸会被拒绝。
搜索 / 排序 / 键盘操作：输入即筛选、ESC 清空；回车卸载、Shift+Del 强制移除、F5 刷新、Ctrl+S 导出、Ctrl+F 搜索等。
报表导出：Geek 风格 HTML，另支持 CSV / JSON / TXT。
43 种界面语言：与 Geek Uninstaller 1.5.3.170 完全相同的语言目录（简体中文、繁體中文、English、日本語、한국어、Deutsch、Français…），含阿拉伯语/希伯来语/波斯语的从右到左布局。
浅色 / 深色 / 跟随系统主题；配置遵循 XDG 规范；不联网、无遥测（仅在你主动打开网页或搜索时联网）。
无头 CLI：--list、--export、--details、--uninstall、--force-remove --dry-run，适合脚本、SSH 与 CI。
安装（Debian / Ubuntu）

bash


# 需要系统提供 Python 3.10+ 与 PySide6（python3-pyside6）
sudo apt install ./geek4linux_1.0.0-1_all.deb

geek4linux            # 图形界面
geek4linux --list     # 命令行
sudo apt remove geek4linux


技术实现
纯 Python，无 C 扩展；Qt 6 界面；包管理器抽象为 provider；对卸载与残留扫描设置了严格的安全约束（拒绝删除系统根目录、~、/usr、/etc 等，拒绝符号链接逃逸，删除前逐项确认）。仓库包含单元测试、Qt offscreen 无头 GUI 测试与 .deb 打包结构测试，CI 在 Ubuntu 上运行测试、构建 wheel 与 .deb。验证状态以 docs/verification/REPORT.md 为准：逻辑/CLI/GUI 已用 fixture 与无头方式验证，未在真实 Linux 桌面上做人工点击测试。

作者与致谢
原版程序：Geek Uninstaller，作者 Thomas Koen（2012–2025，Windows 免费软件，官网 https://geekuninstaller.com/）。“Geek Uninstaller”名称及原版程序版权归原作者所有。
本 Linux 移植版：由 〈你的名字 / GitHub 用户名〉 独立重写与维护（2026），使用 Python + Qt 6 实现，按 MIT 协议发布。原版仅作为行为参考与界面文案来源：移植版不含原版任何代码，也不在运行时调用原版程序。
界面语言：43 份语言目录恢复自原版资源，用于保持与原版一致的措辞；如权利人提出异议，会移除相关文本资源。
第三方组件：Qt / PySide6（LGPL/商业双许可，本项目按 LGPL 使用发行版提供的 PySide6）、Python 标准库、各发行版包管理器与 polkit。商标归各自所有者。
免责声明
中文

无隶属关系：本项目是独立的第三方移植项目，与 Geek Uninstaller 原作者 Thomas Koen、geekuninstaller.com 及其关联方没有任何隶属、授权、赞助或背书关系。原版名称与商标仅用于说明移植对象。
不含原版代码 / 二进制：本仓库不包含原版程序或其反编译代码；原版仅用于行为分析与界面文案参考。
按“现状”提供：本软件按 MIT 协议以 “AS IS” 提供，不提供任何明示或默示担保，包括但不限于适销性、特定用途适用性与不侵权担保。
删除不可撤销：卸载与强制移除会真实删除软件包、配置与残留文件，Linux 没有回收站，操作不可撤销。使用前请自行备份重要数据；因使用本软件导致的数据丢失、系统损坏、业务中断或其他任何损失，作者与贡献者不承担责任。
需要管理员权限：卸载/清理通过 pkexec 获取管理员权限执行系统包管理器命令；请确认你要卸载的目标与命令。提权被拒绝或失败时程序不会强制绕过系统策略。
第三方风险自负：软件包、仓库、snap/flatpak 源以及各发行版包管理器的行为由对应第三方负责；请遵守其许可与所在地区法律。
无官方支持：本项目不提供商业支持；Issue 与 PR 为社区自愿行为。
English (short)

This is an independent, unaffiliated port. Not endorsed by or connected to Thomas Koen or Geek Uninstaller. No original code or binaries are included. Provided "AS IS" under the MIT license, without warranty of any kind. Uninstall and force-removal permanently delete packages, configuration and leftover files — there is no recycle bin; back up your data and use at your own risk. The authors are not liable for data loss, system damage or any other damages. Elevation is performed through pkexec; trademark rights belong to their respective owners.

