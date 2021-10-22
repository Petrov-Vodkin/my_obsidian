2021-06-15 16:33
#tag
# Appium-doctor
### install
[scoop](https://itisgood.ru/2019/03/11/kak-ustanovit-prilozhenija-iz-komandnoj-stroki-windows/) & nodejs(`scoop install nodejs`) & git clone https://github.com/appium/appium-doctor.git
```shell
# Install app-doc in nodejs 'in dir appium-doctor'
# При запуске npm ищет файл с именем `package.json`. В этом файле описан проект, его зависимости и прочая конфигурация. Ищется этот файл только в текущей директории, в которой выполнена команда.
npm install appium-doctor -g
```
### Usage
```shell
appium-doctor		# start
appium-doctor --help | -h
# Usage: appium-doctor.js [options, defaults: --ios --android]
# Options:
  --ios      # Check iOS setup                             [boolean]
  --android  # Check Android setup                         [boolean]
  --dev      # Check dev setup                             [boolean]
  --debug    # Show debug messages                         [boolean]
  --yes      # Always respond yes                          [boolean]
  --no       # Always respond no                           [boolean]
  --demo     # Run appium-doctor demo (for dev).           [boolean]
```
_____________
#### Links
[[Appium]]
https://github.com/appium/appium-doctor