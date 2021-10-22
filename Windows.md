2021-04-05 15:05
#windows
# Windows
версия win - Win+R введите `winver`
[[PowerShell]] | [[Windows terminal]] | [[Cmder]] | [ConEmu](https://conemu.ru/)
[[Scoop]]
[[WSL]]
[[Winget]] 
[[Електропитание]]

[[OpenSSH в Windows 10]] | [[SSTP]] |
[open code, free брандмауэр](https://itigic.com/ru/best-open-source-firewall-to-protect-and-control-network-traffic/)

«Управления дисками»:  **`Win+R`** (Win — клавиша с эмблемой Windows) на клавиатуре, введите **`diskmgmt.msc`**
[[Форматирование носителей]]
[Восстановление флешки](https://flashboot.ru/)
[Восстановление и прошивка флешки по VID и PID](https://repairflash.ru/vosstanovlenie-fleshki-po-vid-i-pid.html)

```powershell
					"Утилиты windows"
net 		# 
    [ ACCOUNTS | COMPUTER | CONFIG | CONTINUE | FILE | GROUP | HELP |
      HELPMSG | LOCALGROUP | PAUSE | SESSION | SHARE | START |
      STATISTICS | STOP | TIME | USE | USER | VIEW ]
	  
rasdial		#  start VPN из командной строки

netsh 		# управлять брандмауэром Windows
netsh advfirewall firewall добавить имя правила = «Открыть порт 80» dir = in action = allow protocol = TCP localport = 80

winhex		# hex редактор
```

C:\Windows\System32\drivers\etc\hosts 
_____________
#### Links
