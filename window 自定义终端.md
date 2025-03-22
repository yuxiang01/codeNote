win 11 自动补全 powershell 7 + oh-my-posh [参考](https://juejin.cn/post/7122718935849369608?from=search-suggest)

---

### 1. 安装powershell 7

[powershell 7 下载msi文件](https://github.com/PowerShell/PowerShell/releases/download/v7.5.0/PowerShell-7.5.0-win-arm64.msi) 安装的时候需要都勾选上

### 2. 安装字体

[下载Meslo字体](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/Meslo.zip) 其他 NF 也行，找到字体安装到电脑，在 powershell 中选择该字体

### 3. 安装 posh-git
```shell
Install-Module posh-git -Scope CurrentUser
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck
```

### 4. 安装 oh-my-posh, gsudo
```shell
winget install JanDeDobbeleer.OhMyPosh
winget install gsudo
```
打开 window terminal，配置
![[Pasted image 20250322165028.png]]
### 5. 编辑配置文件
打开配置文件
```shell
code $profile # 使用vscode打开

notepad.exe $profile # 使用记事本打开
```
编辑文件
```shell
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\montys.omp.json" | Invoke-Expression

Set-PSReadLineOption -PredictionSource History # 设置预测文本来源为历史记录
Set-PSReadLineKeyHandler -Key "Ctrl+Spacebar" -Function Complete
Set-PSReadLineKeyHandler -Key "Ctrl+d" -Function MenuComplete # 设置 Ctrl+d 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo # 设置 Ctrl+z 为撤销
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward # 设置向上键为后向搜索历史记录
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward # 设置向下键为前向搜索历史纪录
```
重启终端。若还看不到效果，输入 `. $profile` 立即配置文件生效
![[Pasted image 20250322165434.png]]
### 6. vscode 配置
打开 setting 搜索 `terminal.integrated.fontFamily` 修改值为对应字体即可
```json
"terminal.integrated.fontFamily": "MesloLGS Nerd Font"
```