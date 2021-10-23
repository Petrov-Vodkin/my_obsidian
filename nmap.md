2021-10-20 09:12
[link](http://www.itword.net/page/nmap)
# nmap
### Несколько примеров использования nmap.

TCP-Сканирование.

Методом TCP connect nmap будет сканировать диапазон портов (1-65535) компьютера с IP-адресом, опция -sV служит для получения версий запущенных сервисов:

`$ nmap -sV ххх.ххх.ххх.ххх -p 1-65535`

Так же стоит обратить внимания что у нас в поле SERVICE и STATE.

В поле SERVICE - всегда отображается значение из файла /etc/services, соответствующее номеру порта. Это отнюдь не означает, что по данному порту будет [запущен](http://www.itword.net/page/kak-uznat-kakoj-distributiv-versija-linux-zapushhen-ustanovlen) тот сервис, который указан в поле SERVICE. [Можно](http://www.itword.net/page/delphi-punkt-v-kontekstnoe) запустить Web-сервер по 22 порту, а [сервер](http://www.itword.net/page/citrix-license-server) SSH - по 80, но nmap все будет писать, что 22 порт - это ssh, a 80 - это HTTP.

В поле STATE - В одном случае порт ssh открыт (open), другом - отфильтрован (filtered). Значение Filtered значит, что порт отклоняет (reject) или отбрасывает (drop) трафик. Это не говорит о том, [запущен](http://www.itword.net/page/kak-uznat-kakoj-distributiv-versija-linux-zapushhen-ustanovlen) ли на этом порту сервис или нет.

UDP-сканирование.

UDP-порты надо обязательно сканировать. При поиске уязвимостей UDP-сервисы обычно упускают из виду, но многие UDP-сервисы (echo, chargen, DNS - работает как по TCP, так и по UDP, а также RPC (Remote Procedure Call)) работают по протоколу UDP. Некоторые из них известны своим огромным списком эксплоитов, позволяющим [получить](http://www.itword.net/page/link-exchange) права root'a. UDP-сканирование делается с помощью опции -sU сканера nmap:

`$ nmap -sU xxx.xxx.xxx.xxx -p 1-65535`

Время сканирования UDP-портов довольно большое примерно 1 секунда на порт.Отчего так долго ? [Система](http://www.itword.net/page/dell-ubuntu-luchshe-windows) ограничила отправку ICMP-ответов: не более 1 в секунду. При UDP-сканировании нужно использовать опцию -Т. Она позволяет указать агрессивность сканирования. Есть 6 скоростей сканирования: Paranoid, Sneaky, Polite, Normal, Aggressive и Insane ( -T Polite). [Первая](http://www.itword.net/page/taler-tlr-pervaja-belorusskaja-kriptovaljuta) скорость самая медленная, последняя - самая быстрая.

Описания методов типов сканирования.

-sT - сканирование TCP портов в обычном режиме. Сканирование происходит на [основе](http://www.itword.net/page/antivirus-avg) функции connect() присутствующей во всех полноценных ОС. Если соединение с удалённым портом установлено, то данный порт открыт, иначе порт закрыт либо фильтруется.

-sS - использование метода TCP CYN. Это так называемое стелс сканирование. Nmap отправляет на удалённый порт SYN-пакет и ожидает ответа. В зависимости от ответа определяется состояние порта. При этом полноценное соединение не устанавливается. Благодаря этому определить факт сканирования очень сложно. Для запуска этого метода требуются рутовские привилегии на Вашей тачке.

-sF,-sX,-sN (scan FIN, scan Xmas, scan NULL) - эти совместные методы используется например если если не помогло -sS или -sT сканирование.

-sU - сканирование UDP портов. На удалённый порт отправляется UDP-пакет и ожидается ответ. Если ответ содержит ICMP-сообщение "порт недоступен" значит порт закрыт либо режется файерволом, иначе порт открыт. Для запуска опять же требуются рутовские привилегии на вашем компе.

-sО - похоже на -sU, только для IP портов.

-sR - использование RCP-сканированиея. Этот метод позволяет определить прогу обслуживающую RCP-порт и её версию. При этот если на удалённом серваке установлен файервол, Nmap его пробивает не оставляя логов.

-sP - ping-сканирование. Данный метод позволяет [узнать](http://www.itword.net/page/kak-uznat-versiju-microsoft-sql-server-i-sp) все [адреса](http://www.itword.net/page/dva-ip-adresa-na-kartochke) активных хостов в сети. Nmap отправляет на указанный IP ICMP-запрос, если в [сети](http://www.itword.net/page/vvedenie-v-virtualnye-chastnye-seti-vpn) есть активные хосты, они отправят нам ответ, тем [самым](http://www.itword.net/page/debian_linux_distribution_on_web_servers) указав на свою активность. Если Вы пингуете [сети](http://www.itword.net/page/vvedenie-v-virtualnye-chastnye-seti-vpn) [лучше](http://www.itword.net/page/dell-ubuntu-luchshe-windows) не указывать больше никаких методов сканирования.

Описания некоторых опций.

Они служат для тонкой [настройки](http://www.itword.net/page/web-server-apache) сканирования и задания дополнительных функций. Опции не обязательны, [работа](http://www.itword.net/page/rabota-s-istoriej-komand-history) сканера будет нормальной и без них. Но все они будут полезны в том или ином случае. Основные опции:

-O - так называемый режим "снятия отпечатков" TCP/IP для определения удалённой ОС (OS fingerprints). Работает это следующим образом: Nmap отправляет удалённой системе запросы и в зависимости от ответов ("отпечатков" стека) определяется ОС и её версия.

-p "диапазон" - сканирование определённого диапазона портов. Например: '-p 21, 22, 25, 80, 31337'. Это уменьшает время сканирования за [счёт](http://www.itword.net/page/gosuchrezhdenija-mjunhena-sekonomili-4-mln-€-za-schjot-perehoda-na-ubuntu-i-openofficeorg) уменьшения диапазона портов.

-F - сканирование стандартных портов записанных в [файл](http://www.itword.net/page/repair-resolvconf-v-ubuntu) services (1-1024). Это так называемое быстрое сканирование.

-P0 - отмена ping-опросов перед сканированием портов хоста. Полезна в тех случаях, если Вы сканируете [сети](http://www.itword.net/page/vvedenie-v-virtualnye-chastnye-seti-vpn) типа microsoft.com, так как в них ICMP-запрос режется файерволом.

-6 - сканирование [через](http://www.itword.net/page/samba-i-avtorizacija-cherez-active-directory) протокол IPv6. Работает значительно быстрее чем [через](http://www.itword.net/page/samba-i-avtorizacija-cherez-active-directory) IPv4.

-T "Paranoid|Sneaky|Polite|Normal|Aggressive|Insane" - [настройка](http://www.itword.net/page/ustanovka-i-nastrojka-servera-na-baze-debian) временных режимов. При "Paranoid" сканирование будет длиться очень долго, но тогда у Вас больше шансов остаться не обнаруженными скан-детекторами. И наоборот "Insane" используёте при сканировании быстрых либо слабо защищённых сетей.

-oN/-oM "logfile" - вывод результатов в logfile в нормальном (-oN) или машинном (-oM) виде.

-oS "logfile" - эта опция позволяет возобновить сканирование если оно было по каким-либо причинам прервано и результат записывался в [файл](http://www.itword.net/page/repair-resolvconf-v-ubuntu) (была включена опция -oN "logfile" или -oM "logfile"). Для продолжения работы нужно запустить Nmap с указанием только этой функции и файла в которой записывалось предыдущее сканирование ("logfile").

-D "host_1, host_2,...,host_n" - это очень [полезная](http://www.itword.net/page/nircmd-windows) функция. Она позволяет запутать удалённую [систему](http://www.itword.net/page/sluzhba-profilej-polzovatelej-prepjatstvuet-vhodu-v-sistemu) и [сделать](http://www.itword.net/page/zazemlenie-chto-eto-takoe-i-kak-ego-sdelat) видимость что её сканируют с [нескольких](http://www.itword.net/page/obzor-neskolkih-video-konvertorov-v-gnulinux) хостов ("host_1, host_2,...,host_n"), тем [самым](http://www.itword.net/page/debian_linux_distribution_on_web_servers) стараясь скрыть Ваш реальный адрес.

nmap примеры:

`nmap -A -T4 192.168.100.123` - [самый](http://www.itword.net/page/opera-12-wahoo) распространенный метод сканирования.

`nmap -sS -O -p 21, 25, 80 www.site.com` - сканируем www.site.com проверяем только 21, 25, 80 порты, используем определение удалённой ОС (метод OS fingerprints) и стелс сканирование.

`nmap -sT -F -P0 -oN scan.txt www.site.com` -сканируем www.site.com применяем обычное сканирование стандартных портов (1-1024), с отменой ping-опросов и заносим результат в [файл](http://www.itword.net/page/repair-resolvconf-v-ubuntu) scan.txt

`nmap -sU -D 143.121.84.12 132.154.156.6 localhost www.site.com` - сканируем www.site.com проводим сканирование UDP-портов, при этом маскируемся двумя хостами (третий наш).
_____________
#### Links
[[Сеть]]