2021-03-24 20:21
#sstp #vpn
# SSTP 
### Win10 
#### `RASDIAL`: start VPN  из командной строки
```powershell
Get-VpnConnection					# получить добавленные vpn
rasdial name_vpn name_user pass		# connect 
rasdial name_vpn /disconnect 		# disconnect
rasdial								# статус подключений
						'Автостарт VPN'
sc create autoVPN start= auto binPath= "rasdial vpn\_office winitpro\_admin $ecretnaRFr@z@" DisplayName= "AutoVPN" depend= lanmanworkstation obj= "NT AUTHORITY\\LocalService"
# Чтобы служба запускалась уже после запуска всех системных служб, поставим ее в зависимость от службы **lanmanworkstation**.  В консоли **services.msc** должна появиться новая служба **autoVPN**, если она отсутствует, проверьте правильность введенной команды. Учтите, что это псевдо-служба, и она не будет отображаться в процессах, отрабатывая один раз при запуске системы.
sc delete autoVPN # удалить из автозапуска
```

```powershell
Add-VpnConnection
   [-Name] <String>
   [-ServerAddress] <String>
   [-RememberCredential]
   [-SplitTunneling]
   [-Force]
   [-PassThru]
   [-ServerList <CimInstance[]>]
   [-DnsSuffix <String>]
   [-IdleDisconnectSeconds <UInt32>]
   [-PlugInApplicationID] <String>
   -CustomConfiguration <XmlDocument>
   [-CimSession <CimSession[]>]
   [-ThrottleLimit <Int32>]
   [-AsJob]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
   ```
```powershell
Add-VpnConnection -Name "Имя подключения L2TP" -ServerAddress "IP адрес VPN сервера" -TunnelType L2tp -EncryptionLevel Maximum -AuthenticationMethod Eap -SplitTunneling -RememberCredential -AllUserConnection
					'Example'
		Name                  : keenHome
		ServerAddress         : greentee.keenetic.pro
		AllUserConnection     : False
		Guid                  : {F58A2A7A-D032-4CE3-B398-C5819B8F8C60}
		TunnelType            : Sstp
		AuthenticationMethod  : {MsChapv2}
		EncryptionLevel       : Optional
		L2tpIPsecAuth         :
		UseWinlogonCredential : False
		EapConfigXmlStream    :
		ConnectionStatus      : Disconnected
		RememberCredential    : True
		SplitTunneling        : True
		DnsSuffix             :
		IdleDisconnectSeconds : 0
```
ключ -`AllUserConnection` можно убрать, если подключение создаётся только для одного пользователя, и компьютер не в домене.  
Логин пароль, а также PSK ключ для L2TP записываю руками.
_____________
### Links
[[Сеть]] | [[Windows]]
[Microsoft](https://docs.microsoft.com/en-us/powershell/module/vpnclient/?view=windowsserver2019-ps)
[Как устроен VPN через SSTP](https://habr.com/ru/post/196134/)
[[#RASDIAL start VPN из командной строки]] [](https://winitpro.ru/index.php/2012/11/26/avtozapusk-vpn-v-windows/)