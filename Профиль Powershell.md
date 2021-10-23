2021-06-10 14:30
#profile
# Профиль Powershell
#### [Git автодополнение](https://github.com/dahlbyk/posh-git)
```powershell
			'Installing posh-git via PowerShellGet'
PowerShellGet\Install-Module posh-git -Scope CurrentUser -Force
OR
PowerShellGet\Update-Module posh\-git
						'Import'
Import-Module posh-git # можно добавить в профаил шелл(читай ниже)
```
####  My PROFILE.AllUsersAllHosts
```powershell
Import-Module posh-git

Set-Alias -Name dc -Value docker
Set-Alias -Name grep -Value findstr
Set-Alias -Name np -Value notepad++.exe
Set-Alias -Name gt -Value git
Set-Alias -Name acp -Value git_add_commit_push
Set-Alias -Name emul -Value C:\Users\Di13\AppData\Local\Android\Sdk\emulator\emulator.exe

Clear-Host

#function Color-Console {
# $Host.ui.rawui.backgroundcolor = "DarkMagenta"
#$Host.ui.rawui.foregroundcolor = "DarkYellow"
#}
#Color-Console
```
_____________
#### Links
[[PowerShell]]