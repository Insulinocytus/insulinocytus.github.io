---
title: Windows Terminalのセットアップガイド
date: 2025-06-04 20:26:00 +0900
categories: [ツール]
tags: [効率化]
pin: false
media_subpath: /../assets/media/windows-terminal-best-practice
mermaid: true
---

自分の経験でまとめたWindows Terminalを気持ちよく使う方法

## 前提

- Windows11
  - Windows10もできるが、手順が微妙に違ったりする場合もある
- Scoopインストール済み

## Msys2

Linuxに近い感覚のShell環境ですので、WindowsのためにわざわざPowerShellを勉強する必要もないし、他のLinux/Macで作成したShellそのまま使える

```shell
scoop install msys2
```

環境変数の追加  
これ追加しないとMsys2内でWindowsのPATH環境変数は認識されない

```text
MSYS2_PATH_TYPE    inherit
```

## Nerd Font

普段Rider使ってますので[JetBrains Nerd Font Mono](https://www.programmingfonts.org/#jetbrainsmono)で

```shell
scoop install nerd-fonts/JetBrainsMono-NF-Mono
```

あとでWindows Terminalのフォント設定でも使います

## Window Terminal Profile

自分用の設定です、JSONだとこんな感じ、ご参考まで

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

## HOME

Scoop経由でMsys2インストールすると、デフォルトのHOMEはScoop以下のどっかのフォルダになるのでHOMEも環境変数で指定する

```text
HOME    C:\Users\<UserName>
```

## 他のWindows Terminal設定

最小化後に通知領域にアイコン表示してウィンドウは非表示になる  
※あとでスタートアップ用のバッチ作成時に役立つ

```json
"minimizeToNotificationArea": true
```

## tmux

tmuxをインストール

```shell
pacman -S tmux
```

## Oh My Posh (Optional)

[Oh My Posh](https://ohmyposh.dev/)でBashの外見をファンシーにする

```shell
scoop install oh-my-posh
```

Bash起動時にOh My Posh初期化する

```.bashrc
eval "$(oh-my-posh init bash --config https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/refs/heads/main/themes/powerlevel10k_classic.omp.json)"
```

※個人的にはpowerlevel10k_classicが好きですので上記の例ではpowerlevel10k_classicを使います

## PCスタートアップ後に自動的にQuakeモードのWindows Terminalを起動する (Optional)

- Win + R で`shell:startup`を起動する
- `Terminal.bat`という名前ファイルを作成
- 以下の内容を`Terminal.bat`に入れる

```powershell
@echo off

chcp 65001

start "Msys2" /MIN "wt.exe" ^
    -w _quake ^
    --profile Msys2

exit
```

`start "Msys2" /MIN`で最小化状態で起動されるが、`"minimizeToNotificationArea": true`になっているので通知領域に最小化される  
※Windows Terminal 1.22ではなぜか最小化がバグるらしいので、一旦1.21に固定している
