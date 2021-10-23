2021-06-18 13:26
#tag `установщик командной строки`
# Scoop 

V2 [установка + работа](https://itisgood.ru/2019/03/11/kak-ustanovit-prilozhenija-iz-komandnoj-stroki-windows/)
Make sure [PowerShell 5](https://aka.ms/wmf5download) (or later, include [PowerShell Core](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6)) and [.NET Framework 4.5](https://www.microsoft.com/net/download) (or later) are installed. Then run:
``` powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
# or shorter
iwr -useb get.scoop.sh | iex
```
**Note:** if you get an error you might need to change the execution policy (i.e. enable Powershell) with
``` powershell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```
```shell
scoop install aria2		# загрузка нескольких подключений(многопоток)
scoop install unxutils	# все\почти утилиты linux 
```
_____________
#### Links


2del: scoop install 7zip git?