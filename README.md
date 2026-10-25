# Geek-Uninstaller-for-Linux-
Geek Uninstaller for Linux 是把 Windows 免费软件 Geek Uninstaller 1.5.3.170（作者 Thomas Koen）的完整体验移植到 GNU/Linux 的独立实现，使用 Python 3.10 编写，按 MIT 协议发布。  它做的事情和原版一样：用一张排序的列表列出系统里安装的软件，用系统原生方式真正卸载它们；当卸载器损坏、或者卸载后留下残留时，用 强制移除 向导把残留清理干净。区别只在于 Linux 没有注册表和 MSI——这里换成包管理器、配置文件与系统状态。
