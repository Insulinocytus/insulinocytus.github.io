---
title: Windows Terminal Setup Guide
date: 2025-06-04 20:26:00 +0900
categories: [Tools]
tags: [效率, windows-terminal]
pin: false
media_subpath: /../assets/media/windows-terminal-best-practice
mermaid: true
---

> This article was created collaboratively by the author and AI. The author provided the core content, and AI helped organize, polish, and improve readability. This English version was also translated by AI from the Chinese version, so minor wording differences may exist.
{: .prompt-tip }

My personal Windows Terminal setup for a smooth workflow

## Prerequisites

- Windows 11 (Windows 10 also works, but steps may differ slightly)
- Scoop installed

## MSYS2

A Linux-like Shell environment. No need to learn PowerShell specifically; existing Linux/Mac scripts work directly.

```shell
scoop install msys2
```

Add this environment variable, otherwise MSYS2 won't recognize Windows PATH:

```text
MSYS2_PATH_TYPE    inherit
```

## Nerd Font

Recommended: [JetBrains Nerd Font Mono](https://www.programmingfonts.org/#jetbrainsmono):

```shell
scoop install nerd-fonts/JetBrainsMono-NF-Mono
```

This will be used in the Windows Terminal font settings later.

## Windows Terminal Profile

Sample configuration (JSON):

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

## HOME Directory

When MSYS2 is installed via Scoop, the default HOME points to a folder under Scoop. Set it manually:

```text
HOME    C:\Users\<UserName>
```

## Minimize to Notification Area

Minimizes to the notification area, hiding the window. Useful for setting up startup automation:

```json
"minimizeToNotificationArea": true
```

## tmux

```shell
pacman -S tmux
```

## Oh My Posh (Optional)

Beautify the Bash prompt:

```shell
scoop install oh-my-posh
```

Add initialization to `.bashrc`:

```bash
eval "$(oh-my-posh init bash --config https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/powerlevel10k_classic.omp.json)"
```

Personal preference: powerlevel10k_classic theme.

## Auto-start Quake Mode on Boot (Optional)

1. Win + R, enter `shell:startup` to open the startup folder
2. Create a new file `Terminal.bat` with the following:

```powershell
@echo off

chcp 65001

start "Msys2" /MIN "wt.exe" ^
    -w _quake ^
    --profile Msys2

exit
```

`/MIN` launches minimized, and `minimizeToNotificationArea: true` sends it to the notification area.

> Note: Windows Terminal 1.22 has a minimize bug. Consider staying on version 1.21 for now.
{: .prompt-warning }
