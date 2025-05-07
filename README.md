# 我使用的工具

## 系統工具

- [Windows Terminal](https://github.com/microsoft/terminal) Terminal
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/) Windows Subsystem for Linux
- [PowerShell](https://github.com/PowerShell/PowerShell) CLI
- [Oh-My-Posh](https://ohmyposh.dev/) CLI prompt
- [Starship](https://starship.rs/) CLI prompt
- [winget](https://github.com/microsoft/winget-cli) Windows 套件管理工具
- [Flow Launcher](https://www.flowlauncher.com/) Windows 搜尋文件、啟動 App 工具
- [everything](https://www.voidtools.com/) 搜尋文件工具
- [PowerToys](https://github.com/microsoft/PowerToys) Windows 官方增強工具，很多功能
- [mouseless](https://mouseless.click/) 💵 用鍵盤控制滑鼠

### PowerShell Profile

```powershell
oh-my-posh init pwsh --config ~/my.omp.json | Invoke-Expression

$env:VIRTUAL_ENV_DISABLE_PROMPT = 1

function sudo
{
    if ($args.Count -gt 0)
    {
        $lastIndex = $args.Count-1
        $programName = $args[0]
        if ($args.Count -gt 1)
        {
            $programArgs = $args[1 .. $lastIndex]
        }
        Start-Process $programName -Verb runAs -ArgumentList $programArgs
    }
    else
    {
        if ($env:WT_SESSION) {
        Start-Process "wt.exe" -Verb runAs
        }
        elseif ($PSVersionTable.PSEdition -eq 'Core')
        {
        Start-Process "$PSHOME\pwsh.exe" -Verb runAs
        }
        elseif ($PSVersionTable.PSEdition -eq 'Desktop')
        {
        Start-Process "$PSHOME\powershell.exe" -Verb runAs
        }
    }
}

# 設定一個 su 別名並執行 sudo 函式
Set-Alias -Name su -Value sudo

Set-Alias -Name ll -Value Get-ChildItem

function Recurse-Remove([string]$folder)
{
    # ask for confirmation before deleting
    $confirmation = Read-Host "Are you sure you want to delete $folder? (y/n)"
    if ($confirmation -ne 'y') {
        Write-Host "Operation cancelled."
        return
    }
    rm -Force -Recurse "$folder"
}

Set-Alias -Name rrm -Value Recurse-Remove

Import-Module -Name Microsoft.WinGet.CommandNotFound
```

### Oh-My-Posh Theme

```json
{
  "$schema": "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/schema.json",
  "blocks": [
    {
      "alignment": "left",
      "segments": [
        {
          "background": "#c386f1",
          "foreground": "#ffffff",
          "leading_diamond": "\ue0b6",
          "style": "diamond",
          "template": " {{ .UserName }} ",
          "trailing_diamond": "\ue0b0",
          "type": "session"
        },
        {
          "background": "#ff479c",
          "foreground": "#ffffff",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "folder_separator_icon": " \ue0b1 ",
            "home_icon": "~",
            "style": "folder"
          },
          "style": "powerline",
          "template": " \uea83  {{ .Path }} ",
          "type": "path"
        },
        {
          "background": "#fffb38",
          "background_templates": [
            "{{ if or (.Working.Changed) (.Staging.Changed) }}#FF9248{{ end }}",
            "{{ if and (gt .Ahead 0) (gt .Behind 0) }}#ff4500{{ end }}",
            "{{ if gt .Ahead 0 }}#B388FF{{ end }}",
            "{{ if gt .Behind 0 }}#B388FF{{ end }}"
          ],
          "foreground": "#193549",
          "leading_diamond": "\ue0b6",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "branch_max_length": 25,
            "fetch_stash_count": true,
            "fetch_status": true,
            "fetch_upstream_icon": true
          },
          "style": "powerline",
          "template": " {{ .UpstreamIcon }}{{ .HEAD }}{{if .BranchStatus }} {{ .BranchStatus }}{{ end }}{{ if .Working.Changed }} \uf044 {{ .Working.String }}{{ end }}{{ if and (.Working.Changed) (.Staging.Changed) }} |{{ end }}{{ if .Staging.Changed }} \uf046 {{ .Staging.String }}{{ end }}{{ if gt .StashCount 0 }} \ueb4b {{ .StashCount }}{{ end }} ",
          "trailing_diamond": "\ue0b4",
          "type": "git"
        },
        {
          "background": "#6CA35E",
          "foreground": "#ffffff",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "fetch_version": true
          },
          "style": "powerline",
          "template": " \ue718 {{ if .PackageManagerIcon }}{{ .PackageManagerIcon }} {{ end }}{{ .Full }} ",
          "type": "node"
        },
        {
          "background": "#8ED1F7",
          "foreground": "#111111",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "fetch_version": true
          },
          "style": "powerline",
          "template": " \ue626 {{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }} ",
          "type": "go"
        },
        {
          "background": "#4063D8",
          "foreground": "#111111",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "fetch_version": true
          },
          "style": "powerline",
          "template": " \ue624 {{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }} ",
          "type": "julia"
        },
        {
          "background": "#FFDE57",
          "foreground": "#111111",
          "powerline_symbol": "\ue0b0",
          "style": "powerline",
          "template": " \ue235 {{ if .Error }}{{ .Error }}{{ else }}{{ if .Venv }}(.Venv){{ end }}{{ end }}",
          "type": "python"
        },
        {
          "background": "#AE1401",
          "foreground": "#ffffff",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "display_mode": "files",
            "fetch_version": true
          },
          "style": "powerline",
          "template": " \ue791 {{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }} ",
          "type": "ruby"
        },
        {
          "background": "#FEAC19",
          "foreground": "#ffffff",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "display_mode": "files",
            "fetch_version": false
          },
          "style": "powerline",
          "template": " \uf0e7{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }} ",
          "type": "azfunc"
        },
        {
          "background_templates": [
            "{{if contains \"default\" .Profile}}#FFA400{{end}}",
            "{{if contains \"jan\" .Profile}}#f1184c{{end}}"
          ],
          "foreground": "#ffffff",
          "powerline_symbol": "\ue0b0",
          "properties": {
            "display_default": false
          },
          "style": "powerline",
          "template": " \ue7ad {{ .Profile }}{{ if .Region }}@{{ .Region }}{{ end }} ",
          "type": "aws"
        },
        {
          "background": "#ffff66",
          "foreground": "#111111",
          "powerline_symbol": "\ue0b0",
          "style": "powerline",
          "template": " \uf0ad ",
          "type": "root"
        },
        {
          "background": "#83769c",
          "foreground": "#ffffff",
          "properties": {
            "always_enabled": true
          },
          "style": "plain",
          "template": "<transparent>\ue0b0</> \ueba2 {{ .FormattedMs }}\u2800",
          "type": "executiontime"
        },
        {
          "background": "#00897b",
          "background_templates": ["{{ if gt .Code 0 }}#e91e63{{ end }}"],
          "foreground": "#ffffff",
          "properties": {
            "always_enabled": true
          },
          "style": "diamond",
          "template": "<parentBackground>\ue0b0</> \ue23a ",
          "trailing_diamond": "\ue0b4",
          "type": "status"
        }
      ],
      "type": "prompt"
    },
    {
      "segments": [
        {
          "background": "#0077c2",
          "foreground": "#ffffff",
          "style": "plain",
          "template": "<#0077c2,transparent>\ue0b6</> \uf489 {{ .Name }} <transparent,#0077c2>\ue0b2</>",
          "type": "shell"
        },
        {
          "background": "#1BD760",
          "foreground": "#111111",
          "invert_powerline": true,
          "powerline_symbol": "\ue0b2",
          "properties": {
            "paused_icon": "\uf04c ",
            "playing_icon": "\uf04b "
          },
          "style": "powerline",
          "template": " \uf167 {{ .Icon }}{{ if ne .Status \"stopped\" }}{{ .Artist }} - {{ .Track }}{{ end }} ",
          "type": "ytm"
        },
        {
          "background": "#f36943",
          "background_templates": [
            "{{if eq \"Charging\" .State.String}}#40c4ff{{end}}",
            "{{if eq \"Discharging\" .State.String}}#ff5722{{end}}",
            "{{if eq \"Full\" .State.String}}#4caf50{{end}}"
          ],
          "foreground": "#ffffff",
          "invert_powerline": true,
          "powerline_symbol": "\ue0b2",
          "properties": {
            "charged_icon": "\ue22f ",
            "charging_icon": "\ue234 ",
            "discharging_icon": "\ue231 "
          },
          "style": "powerline",
          "template": " {{ if not .Error }}{{ .Icon }}{{ .Percentage }}{{ end }}{{ .Error }}\uf295 ",
          "type": "battery"
        },
        {
          "background": "#2e9599",
          "foreground": "#111111",
          "invert_powerline": true,
          "leading_diamond": "\ue0b2",
          "style": "diamond",
          "template": " {{ .CurrentDate | date .Format }} ",
          "trailing_diamond": "\ue0b4",
          "type": "time"
        }
      ],
      "type": "rprompt"
    }
  ],
  "console_title_template": "{{ .Shell }} in {{ .Folder }}",
  "final_space": true,
  "version": 3
}
```

## 開發工具

- [Visual Studio Code](https://code.visualstudio.com/) 寫 code 用
- [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/) 寫 code 用
- [Fork](https://git-fork.com/) git GUI
- [Postman](https://www.postman.com/) API 測試
- [XPipe](https://github.com/xpipe-io/xpipe) SSH GUI
- [Beyond Compare](https://www.scootersoftware.com/) 💵 比對軟體
- [NETworkManager](https://github.com/BornToBeRoot/NETworkManager) 網路相關工具
- [DevTools](https://github.com/DevTools) 開發小工具集合
- [DBeaver](https://dbeaver.io/) 資料庫 GUI 工具
- [Proxyman](https://proxyman.io/) 💵 HTTP request/response debug tool
- [ImHex](https://github.com/WerWolv/ImHex) Hex editor
- [RunJS](https://runjs.app/) 💵 JavaScript playground
- [Docker](https://www.docker.com/) 容器工具
- [podman](https://podman.io/) 容器工具
- [nvm](https://github.com/nvm-sh/nvm) Node.js 版本管理工具
  - [nvm-windows](https://github.com/coreybutler/nvm-windows) Windows 版本
- [pnpm](https://pnpm.io/) JavaScript 套件管理工具

## 生活應用

- [Notion](https://www.notion.so/) 筆記工具
- [1Password](https://1password.com/) 💵 密碼工具
- [Flameshot](https://flameshot.org/) 截圖工具
- [ShareX](https://getsharex.com/) 截圖工具
- [DeepL](https://www.deepl.com/) 翻譯軟體
- [LINE](https://line.me/) 通訊軟體
- [Discord](https://discord.com/) 通訊軟體
- [LocalSend](https://github.com/localsend/localsend) 局域網檔案傳輸工具
- [Spotify](https://www.spotify.com/) 音樂串流平台
- [Steam](https://store.steampowered.com) 遊戲平台
- [Epic Games](https://www.epicgames.com) 遊戲平台
- [GOG](https://www.gog.com/) 遊戲平台
