2021-02-01 14:41
#alias #powershell 
# Alias
[Set-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7)
```powershell
Set-Alias -Name calc -value calc.exe # алиас на запуск калькулятора.
```

```powershell
function AL01 {Test-Connection -Count 2 xakep.ru}
Set-Alias ping AL01
# При вызове нашего алиаса по имени ping мы сделаем два пинга до сервера xakep.ru. Чтобы удалить ненужный для нас алиас, существует команда

```


```powershell
Remove-Item alias:ping
```

`после закрытия оболочки PowerShell все созданные алиасы будут удалены.` Чтобы этого не происходило, их нужно сохранить в свой пользовательский профиль.
```powershell
$profile | Format-List -Force # Посмотреть профиля. 

# протестировать на наличие профиля в системе — командой
$profile | Format-List -Force | ForEach-Object (Test-Path _$)
```

Если в ответ возвращается False, то их просто нет. Создадим наш файл, к примеру по первому пути:

```powershell
C:\Windows\System32\WindowsPowerShell\v1.0\profile.ps1
```

и в него напишем наши алиасы:

```powershell
function AL01 {Test-Connection -Count 2 xakep.ru}
Set-Alias ping AL01
```

После сохранения и перезапуска пошик будет автоматически подгружать данный файл, и теперь настройки никуда не денутся. Стоит упомянуть и что по умолчанию в системе отключен запуск любых сценариев и, скорее всего, наш внешний файл будет забракован системой. Чтобы этого не произошло, давай посмотрим, что стоит в политике. Выполни в консоли PowerShell:

```powershell
Get-ExecutionPolicy
```

Скорее всего, система вернет значение Restricted, что как раз означает запрет выполнения сценариев. Чтобы это обойти, выполни из-под администратора:

```powershell
Set-ExecutionPolicy Unrestricted
```

После подтверждения сценарии будут запускаться без ошибок и наш файл тоже будет работать

#### Links
[[PowerShell]] 