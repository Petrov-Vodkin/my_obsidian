2021-05-17 19:07
#tag
# Charles Proxy
### Установка / Настройка
[Download](https://www.charlesproxy.com) & Install
Help - SSL Proxying - install Charles Root `Certificate`(в хранилище: доверенные центры корневой сертификации)
`Просмотр https запросов`: выбрать запрос - Enable SSH Proxying
`Выбор всех ресурсов для SSL проксирования`: Proxy - SSL Proxy Settings(Ctr_Shift_L) - ADD - *
[Получить сертификат](http://www.charlesproxy.com/getssl/) Charles Proxy must be running
### Инструменты (Tools)
`Map Remote` - пренаправление From host To host
`Rewrite` -  подмена значений(URL, params,  headers.....) Match(что) Replace. `ЗАДАЙ Location`(URL)к чему применять, иначе ко всем запросам
`Block list` - add block host(список заблокированнх адресов)
`Allow list` - add host who NOT block(список одобренных адресов)
`Map Local` - (подмена ответа сервера в целом(изменение тела(body) запроса))
`Trotlling` - изменение скорости соединения
Proxy - `Breackpoint` - пауза во время которой происходит Перехват, изменение и отправка запроса/ответа
_____________
#### Links 
[[Инструменты для тестировщика]]