2021-06-14 15:18
#tag
# AVD Emulator
#### Run AVD Emulator without Android Studio
```shell
emulator.exe -list-avds			# установленные вирт устройства
# Дело в том, что привычный /tools/emulator объявлен устаревшим. Вместо него теперь используется /emulator/emulator. Вот так просто:
$ANDROID_HOME/emulator/emulator -avd 'name_device'	# use a specific android virtual device
# выполняет холодную загрузку эмулятора(с чистого листа), а не восстанавливают его состояния из снапшота
			-no-snapshot -avd 'name_device' 
			-no-snapshot-load -avd 'name_device' # + сохранение состояния
```
qemu-system-x86_64.exe
_____________
#### Links
[[Android Studio]]
[Скучный бложик тестировщика](https://myachinqa.blogspot.com/2018/03/android-panic-missing-emulator-engine.htm)
[[Профиль Powershell]]
[[Инструменты для тестировщика]]