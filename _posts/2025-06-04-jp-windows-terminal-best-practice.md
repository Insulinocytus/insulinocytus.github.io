---
title: Windows Terminal 設定ガイド
date: 2025-06-04 20:26:00 +0900
categories: [ツール]
tags: [效率, windows-terminal, japanese]
pin: false
media_subpath: /../assets/media/windows-terminal-best-practice
mermaid: true
---

> この記事は作者と AI の協働で作成されたものです。作者が中核となる内容を提供し、AI は構成整理、表現の推敲、読みやすさの向上を担当しました。また、この日本語版は中国語版をもとに AI で翻訳しているため、表現に若干の差異がある場合があります。
{: .prompt-tip }

私の Windows Terminal の実用設定

## 前提

- Windows 11（Windows 10 でも可。ただし手順は少し異なる）
- Scoop インストール済み

## MSYS2

Linux 風の Shell 環境。PowerShell をわざわざ覚えなくても、Linux / Mac のスクリプトをそのまま使いやすい。

```shell
scoop install msys2
```

MSYS2 から Windows の PATH を認識できるように、環境変数を追加する。

```text
MSYS2_PATH_TYPE    inherit
```

## Nerd Font

おすすめは [JetBrains Nerd Font Mono](https://www.programmingfonts.org/#jetbrainsmono)。

```shell
scoop install nerd-fonts/JetBrainsMono-NF-Mono
```

後で Windows Terminal のフォント設定に使う。

## Windows Terminal Profile

設定例（JSON）。

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

## HOME ディレクトリ

Scoop で MSYS2 を入れると、デフォルトの HOME が Scoop 配下のディレクトリを向く。手動で指定しておく。

```text
HOME    C:\Users\<UserName>
```

## 通知領域に最小化

最小化すると通知領域に隠れ、ウィンドウは表示されなくなる。起動時自動実行と組み合わせると便利。

```json
"minimizeToNotificationArea": true
```

## tmux

```shell
pacman -S tmux
```

## Oh My Posh（任意）

Bash のプロンプトを見やすくする。

```shell
scoop install oh-my-posh
```

`.bashrc` に初期化設定を追加する。

```bash
eval "$(oh-my-posh init bash --config https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/powerlevel10k_classic.omp.json)"
```

個人的には `powerlevel10k_classic` テーマが使いやすい。

## 起動時に Quake モードを自動起動（任意）

1. `Win + R` で `shell:startup` を開く
2. `Terminal.bat` を新規作成し、以下を記述する

```powershell
@echo off

chcp 65001

start "Msys2" /MIN "wt.exe" ^
    -w _quake ^
    --profile Msys2

exit
```

`/MIN` で最小化起動し、`minimizeToNotificationArea: true` と組み合わせると通知領域へ隠せる。

> 注意：Windows Terminal 1.22 には最小化関連の不具合があるため、当面は 1.21 に固定するのが無難。
{: .prompt-warning }
