2021-04-05 15:33
#tag
# Установка клиента OpenSSH 
Клиент OpenSSH входит в состав **Features on Demand** Windows 10 (как и [RSAT](https://winitpro.ru/index.php/2018/10/04/ustanovka-rsat-v-windows-10-iz-powershell/))
Проверьте, что SSH клиент установлен:
`Get-WindowsCapability -Online | ? Name -like 'OpenSSH.Client*'`
!['OpenSSH.Client установка в windows 10](https://winitpro.ru/wp-content/uploads/2020/01/openssh-client-ustanovka-v-windows-10.png)
Выше клиент OpenSSH установлен (статус: **State: Installed**).
Если клиент отсутствует (**State: Not Present**), 3 варианта установки:
1. PowerShell: `Add-WindowsCapability -Online -Name OpenSSH.Client*`
2. DISM: `dism /Online /Add-Capability /CapabilityName:OpenSSH.Client~~~~0.0.1.0`
3. Параметры -> Приложения -> Дополнительные возможности -> Добавить компонент. Найдите в списке **Клиент OpenSSH** и нажмите кнопку **Установить**.

### [SSH аутентификации по ключам в Windows](https://winitpro.ru/index.php/2019/11/13/autentifikaciya-po-ssh-klyucham-v-windows/)
`ssh root@192.168.1.92 -i "C:\Users\username\.ssh\id_rsa"`
```powershell
ssh-keygen.exe	# создания, администрирования и преобразования ключей проверки подлинности SSH;
ssh-agent.exe	# хранения закрытых ключей, которые используются в процессах проверки подлинности с открытым ключом;
ssh-add.exe		# добавления закрытых ключей в список ключей, разрешенных к использованию на сервере;
```
**_Добавить ваш закрытый ключ в SSH-Agent_**
```shell
set-service ssh-agent StartupType ‘Automatic’	# включить службу ssh-agent
Start-Service ssh-agent 				#  автозапуск
ssh-add "C:\Users\username\.ssh\id_rsa" # add закрытый ключ в базу ssh-agent
```
Теперь вы можете подключиться к серверу по SSH без указания пути к RSA ключу, он будет использоваться автоматически. 
***`Несколько полезных аргументов SSH`:***
   `-C` – сжимать трафик между клиентом и сервером (полезно на медленных и нестабильных подключениях);
   `-v` – вывод подробной информации обо всех действия клиента ssh;
   `-R`/`-L` – можно использовать для [проброса портов через SSH туннель](https://winitpro.ru/index.php/2019/10/29/windows-ssh-tunneling/).
### SCP: копирование файлов из/в Windows через SSH
**`scp.exe`** входит в состав пакета клиента SSH
`scp.exe "E:\ISO\CentOS-8.1.1911-x86_64.iso" root@192.168.1.202:/home`  
![scp.exe копирование файлов через ssh](https://winitpro.ru/wp-content/uploads/2020/01/scp-exe-kopirovanie-fajlov-cherez-ssh.png)
```powershell 
scp -r E:\ISO\ root@192.168.1.202:/home	# рекурсивно скопировать все содержимое каталога
scp.exe root@192.168.1.202:/home/CentOS-8.1.1911-x86_64.iso e:\tmp	# скопировать файл с удаленного сервера 
````
#### Запуск и настройка OpenSSH Server
```powershell
Get-Service sshd				# проверьте состояние сервера sshd
Start-Service sshd				# Start sshd service
Set-Service -Name sshd -StartupType 'Automatic'	# автозапуск sshd
Get-NetFirewallRule -Name *ssh*	# убедиться, что порт 22, порт SSH, открыт в брандмауэр
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
Stop-Service sshd			# Остановить сервер sshd
```
### Подключение к windows
```powershell
net user 								# список пользователей

ssh DefaultAccount@host					# 
Administrator default pass p@ssw0rd		# 

net user Administrator [new password]	# смена пароля учетной записи
```
_____________
### Links
[[Windows]] | [[SSH]]
[link Microsoft](https://docs.microsoft.com/ru-ru/windows-server/administration/openssh/openssh_install_firstuse#start-and-configure-openssh-server)
[link to article](https://winitpro.ru/index.php/2020/01/22/vstroennyj-ssh-klient-windows/)
https://itigic.com/ru/ssh-in-windows-10-activate-server-and-connect-as-client/