2021-02-01 13:06
#powershell
# PowerShell 
[Первые шаги](https://habr.com/ru/post/113913/)
### [[Alias]] | [[ScriptPowerShell]]
### Команды
```powershell
systeminfo	# systeminfo ))
findstr		# grep
rm -Force	# удалить всё в dir & files 
echo $PROFILE.AllUsersAllHosts # вывод dir файла
ls  *.txt 	# поиск всех файлов txt
	-Include        	# рекурсивно во вложенных папках 
	- `l` (link)
    - `d` (directory)
    - `a` (archive)
    - `r` (read-only)
    - `h` (hidden)
    - `s` (system).
						'запуск и остановка процессов'
# каталог с исполняемым файлом нужно добавить в path (или указывать полный путь)					
Start-Process -FilePath notepad # запуск notepad
Start-Process -FilePath ping -ArgumentList "-n 10 192.168.1.11" # передача аргументов
Stop-Process -Name notepad		# || (Get-Process -Name notepad).Kill()
Stop-Process -Name notepad.exe -Confirm # с подтверждением ДА\НЕТ


Get-PSReadLineKeyHandler		# terminal hotkeys
Get-Command -Noun you_commant	# все команды этого типа you_commant
								"Сеть"
Invoke-RestMethod -Uri https://ipinfo.io	# свой общедоступной IP-адрес
Get-NetTCPConnection |Select-Object -Property LocalPort, RemoteAddress, @{name='ProcessID';expression={(Get-Process -Id $_.OwningProcess). ID}}, @{name='ProcessName';expression={(Get-Process -Id $_.OwningProcess). Path}} | Format-Table -AutoSize	# список подключений TCP
Get-NetUDPEndpoint		 # Перечислите открытые порты UDP
								'VPN'
Get-VpnConnection                   # получить добавленные vpn
rasdial name_vpn name_user pass     # connect  
rasdial name_vpn /disconnect        # disconnect

								"Железо"
Stop-Computer		# poweroff
Restart-Computer	# reboot 

```
### Работа с пакетами
[[Scoop]] [[OneGet]]
### [[Профиль Powershell]]
`profile.ps1`файле можно хранить любой код powershell, который будет выполняться при каждом запуске powershell
```shell
$Profile | select *				# type of profile
test-path $Profile				# проверить, используется ли консолью профиль
$PROFILE | Format-List * -Force # список всех возможых профайлов
						"Создать/Редактировть профиль"
notepad++.exe $PROFILE.AllUsersAllHosts # создать\открыть для редактирования
								"Внешний вид"
# Изменения внешнего вида консоли производятся изменением значений
(Get-Host).UI.RawUI # список доступных изменений
 							"Конвейер | Запись в txt"
# получаем текстовый файл со списком запущенных процессов и необходимой информацией по ним
Get-Process|Select-Object ID,CPU,ProcessName|Out-File C:\TMP\out.txt
```
```shell
$PSVersionTable		# версия powershell
```
### Операции с файлами
```shell
								"Создать"
ni file_name # ==  New-Item -Path . -Name` `"testfile1.txt"` `-ItemType` `"file"` `-Value` `"Это текстовая строка в файле."
# Создание нескольких файлов:
ni file_name file_2 file3 # == New-Item -ItemType` `"file"` `-Path` `"c:\ps-test\test.txt"``,` `"c:\ps-test\Logs\test.log"
								"... в фаил"
echo your_text >> file_name # добавить your_text в file_name
echo your_text > file_name 	# заменить всё содержимое на your_text в file_name
								"Свойства файла"
Get-ItemProperty -Path C:\Folder1\file1.txt | SELECT *#все свойсва
# или через присвенме переменной 
doc = Get-ItemProperty -Path C:\Folder1\file1.txt
doc.GetType()
# меняем(переназначаем свойства) свойства 
$file.IsReadOnly = $true
```
### [[Функции в Powershell]]
 вызывов | передача параметров | возвращение значений | .........
<<<<<<< HEAD
 экранирование: `'строка дословно`a'`     в общем ё перед переменной
=======
 экранирование: `'строка дословно`a'`     `в общем ё перед переменной

#### [Unsplash и PowerShell для установки обоев рабочего стола](http://dimayakovlev.ru/notebook/unsplash-powershell-wallpaper/)
```powershell
# Получение текущего разрешения экрана по горизонтали и вертикали
$horRes = (Get-WmiObject -Class Win32_VideoController).CurrentHorizontalResolution
$vertRes = (Get-WmiObject -Class Win32_VideoController).CurrentVerticalResolution

# Путь к файлу изображения
$file = Join-Path -Path ([Environment]::GetFolderPath('MyPictures')) -ChildPath "UnsplashWallpaper.${horRes}x${vertRes}.jpg"

# URL для отправки запроса
$url = "https://source.unsplash.com/collection/1065396/${horRes}x${vertRes}"

# Загрузка файла изображения
Invoke-WebRequest -Uri $url -OutFile $file

# Установка обоев рабочего стола
Set-ItemProperty -Path 'HKCU:\Control Panel\Desktop\' -Name WallPaper -value $file
rundll32.exe user32.dll, UpdatePerUserSystemParameters 1, True
```

Сохраните код скрипта в файл _Get-UnsplashWallpaper.ps1_ и запустите. После завершения работы скрипта в качестве обоев будет установлено случайное изображение из [курируемой коллекции обоев](https://unsplash.com/wallpaper/1065396/desktop-wallpapers) для рабочего стола Unsplash.

>>>>>>> w_pc
### Help
```bash
Save-Help					# загрузка файлов справки из Интернета и сохранение их в общей папке.
Update-Help					#загрузка и установка файлов справки из Интернета или общей папки.
Get-Help Get-Process		# отображение справки по командлету Get-Process.
Get-Help Get-Process -Online# открытие справки по командлету Get-Process в Интернете.
```

### Салат
```powershell 
			"Перенос вывод на следующую строку == $OFS"
Function Primer{
$OFS="," 
"Столицей России были $Args"}
```
#### Links
[[Windows]]
[](https://techexpert.tips/ru/powershell-ru/)
[Microsoft about](https://docs.microsoft.com/ru-ru/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7)
[Управление процессами с помощью PowerShell](https://winitpro.ru/index.php/2020/10/26/upravlenie-processami-powershell/)