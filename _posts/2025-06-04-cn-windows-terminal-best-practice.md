---
title: Windows Terminal 配置指南
date: 2025-06-04 20:26:00 +0900
categories: [Tools]
tags: [efficiency, windows-terminal, chinese]
pin: false
media_subpath: /../assets/media/windows-terminal-best-practice
mermaid: true
---

> 本文由作者提供核心内容与原始表达，并与 AI 协作完成。AI 主要负责将偏口语化、碎片化的内容整理为更清晰的书面表达，并辅助润色结构与可读性。
{: .prompt-tip }

版本：中文（当前） | [English](/posts/en-windows-terminal-best-practice/) | [日本語](/posts/jp-windows-terminal-best-practice/)

我的 Windows Terminal 使用配置

## 前提

- Windows 11（Windows 10 也可，但步骤略有差异）
- 已安装 Scoop

## MSYS2

Linux 风格的 Shell 环境，不需要专门学习 PowerShell，Linux/Mac 上的脚本直接可用。

```shell
scoop install msys2
```

添加环境变量，否则 MSYS2 内无法识别 Windows 的 PATH：

```text
MSYS2_PATH_TYPE    inherit
```

## Nerd Font

推荐 [JetBrains Nerd Font Mono](https://www.programmingfonts.org/#jetbrainsmono)：

```shell
scoop install nerd-fonts/JetBrainsMono-NF-Mono
```

后续在 Windows Terminal 的字体设置中使用。

## Windows Terminal Profile

参考配置（JSON）：

```json
{
    "altGrAliasing": true,
    "antialiasingMode": "cleartype",
    "closeOnExit": "automatic",
    "colorScheme": "Campbell",
    "commandline": "**********/msys2_shell.cmd -defterm -here -no-start -msys2 -shell bash",
    "cursorShape": "bar",
    "elevate": false,
    "font":
    {
        "face": "JetBrainsMonoNL Nerd Font Mono",
        "size": 12
    },
    "guid": "**********",
    "hidden": false,
    "historySize": 9001,
    "icon": "**********/msys2.ico",
    "name": "MSYS2",
    "padding": "8, 8, 8, 8",
    "snapOnInput": true,
    "startingDirectory": "**********",
    "useAcrylic": false
}
```

## HOME 目录

通过 Scoop 安装 MSYS2 后，默认 HOME 指向 Scoop 下的某个目录，需手动指定：

```text
HOME    C:\Users\<UserName>
```

## 通知区域最小化

最小化后隐藏到通知区域，窗口消失。对后续设置开机启动很有用：

```json
"minimizeToNotificationArea": true
```

## tmux

```shell
pacman -S tmux
```

## Oh My Posh（可选）

让 Bash 提示符变得美观：

```shell
scoop install oh-my-posh
```

在 `.bashrc` 中添加初始化：

```bash
eval "$(oh-my-posh init bash --config https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/powerlevel10k_classic.omp.json)"
```

个人偏好 powerlevel10k_classic 主题。

## 开机自动启动 Quake 模式（可选）

1. Win + R 输入 `shell:startup` 打开启动文件夹
2. 新建 `Terminal.bat`，内容如下：

```powershell
@echo off

chcp 65001

start "Msys2" /MIN "wt.exe" ^
    -w _quake ^
    --profile Msys2

exit
```

`/MIN` 最小化启动，配合 `minimizeToNotificationArea: true` 隐藏到通知区域。

> 注意：Windows Terminal 1.22 存在最小化 bug，建议暂时固定在 1.21 版本。
{: .prompt-warning }
