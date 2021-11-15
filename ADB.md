2021-06-17 15:05
#tag
# ADB
### [[Отладка ADB]]
### Installation
| [Adb Kit](http://adbshell.com/downloads) | [драйверы ADB](http://adbdriver.com/downloads) | Добавить папку c adb в переменную Path
С ADB можно работать через [WiFi ADB](https://play.google.com/store/apps/details?id=com.ttxapps.wifiadb)
```powershell
adb devices			# список подключенных устройств
adb connect ip:port	# ответ: EX connected to 192.168.1.10:5555
						"install/uninstall apk"
adb install d:/downloads/имя_файла.apk # установка приложений без копирования их на смартфон
adb uninstall com.rovio.angrybirdsseasons # удалить, нужно название пакета
							'Shell & su'
adb shell	# запуск shell(терминал linux) на девайсе
su			# права root (после запуска shell т.е. в shell) знак $ сменится на # 
				"Управление приложениями pm(package manager)"
pm list packages		# список установленных приложений (названий пакетов)
					-s	# только системные приложения
					-3	# только сторонние
					-f	# пути установки пакетов
					-d	# отключенные приложения 
pm disable com.google.android.calendar	# отключить календарь
pm clear com.dropbox.android			# очистить данные
pm uninstall 							# uninstall
```
Для использования activity manager понадобятся более глубокие знания структуры Android + whatis [Avtivity](http://developer.android.com/intl/ru/reference/android/app/Activity.html) и [Intent](http://developer.android.com/intl/ru/tools/help/shell.html#IntentSpec).
```bash
				"Управление приложениями am(activity manager)"
am start -n com.android.browser/.BrowserActivity# запуск
am start -n com.android.settings/.Settings		# запуск
am kill com.android.browser # завершить работу приложения 
am kill-all					# убить все запущенные приложения — такой командой:
							# сделать звонок на нужный номер телефона
am start -a android.intent.action.CALL tel:123
							# открыть страницу в браузере:
am start -a android.intent.action.VIEW 'http:/xakep.ru'
							"Бэкап приложений"
adb backup [опции] <приложения>
			-f			# имя создаваемого файла и его расположение на компе. При отсутствии ключа будет создан файл backup.ab в текущем каталоге;
			-apk|-noapk	# включать ли в бэкап только данные приложения или сам .apk тоже (по умолчанию не включает);
			-obb|-noobb	# включать ли в бэкап расширения .obb для приложений (по умолчанию не включает);
			-all		# бэкап всех установленных приложений
			--			# перечень пакетов для бэкапа
			-shared|-noshared	# включать ли в бэкап содержимое приложения на SD-карте (по умолчанию не включает)
			-system|-nosystem	# включать ли в бэкап системные приложения (по умолчанию включает)
# создать бэкап всех несистемных прог, включая сами .apk, в определенное место
adb backup -f c:\android\backup.ab -apk -all -nosystem
adb restore c:\android\backup.ab	# восстановления бэкапа 
							'Создание скриншота'
adb shell screencap /sdcard/screen.png # Выполняется одной строчкой
adb pull /sdcard/screen.png		# выдернуть картинку из устройства 
adb pull /dev/graphics/fb0		# скриншот В recovery 
								'Запись видео'
adb shell screenrecord --size 1280x720 --bit-rate 6000000 --time-limit 20 --verbose /sdcard/video.mp4  #  запись видео с разрешением 1280 x 720 (defolt - нативное разрешение экрана устройства), с битрейтом 6 Мбит/с, длиной 20 с (defolt =  180 с), с показом логов в консоли
# Записанное видео будет находиться в /sdcard (файл video.mp4).
# необходимо преобразовать video файл fb0 в нормальное изображение с помощью FFmpeg, который нужно скачать и положить в папку с adb. Расширение необходимо ставить своего устройства:
ffmpeg -f rawvideo -pix_fmt rgb32 -s 1080x1920 -i fb0 fb0.png
```
_____________
#### Links
[[Droid]] | [[Инструменты для тестировщика]]
[50 команд ADB](https://xakep.ru/2016/05/12/android-adb/ "управлепие приложениями, бэкап, скрин && видео, логи")