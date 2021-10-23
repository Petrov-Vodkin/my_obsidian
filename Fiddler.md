2021-05-18 14:28
#tag
# Fiddler
[настройка RU](https://habr.com/ru/post/554562/)
[Тест безопастности с помощью Fiddler](https://www.software-testing.ru/library/testing/security/2853-but-i-m-not-a-security-tester-2)
### Using
[Composer](https://www.youtube.com/watch?v=rUXZvVmdb6o&t=541s) - Тестирование API во вкладке
[Auto Responder](https://www.youtube.com/watch?v=rUXZvVmdb6o&t=760s) - инструмент для подмены ответов (Only Fiddler Everywhere) 
[Breackpoint](https://www.youtube.com/watch?v=rUXZvVmdb6o&t=1100s) - пауза во время которой происходит Перехват, изменение и отправка запроса/ответа
|RULES|AUTOMATIC Breackpoint|Выбрать нужное|
TIMELINE - временной график выполнения запросов(выбрать нужные)

### Установка
[Download](https://www.telerik.com/download/fiddler) `select 4 dev & debug`
`Install cert in programm:` tools - option - HTTPS - Actions - Trust Root Cert
`Сделать прокси-сервис несистемным` Запустить **Fiddler 4** и отжать кнопку перехвата всех HTTP-запросов, чтобы не было надписи _**Capturing**_.
Открыть меню настроек из главного меню _и на вкладке «Connection» отжать кнопку **Act as system proxy on startup**_.[](https://loadtestweb.info/2017/03/02/fiddler-4-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B8-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81-https/)
### Настройка Fiddler на мобильном устройстве Android
`in Fiddler` tools - options - connections - `Allow remote computers to connect`
`Export Root Certificate to Desktop` - если вы планируете использовать Fiddler как прокси-сервер локальной сети, то на каждом устройстве пользователя необходимо установить сгенерированный выше сертификат. С помощью этой опции сохраняете сертификат на ваш рабочий стол.
`in Androyd` 
устанавливаем сертификат(запустив фаил) 
подключаемся к сети (в которой запущен Fiddler) через proxy к ip порт 8888

### Если пропал интернет после установки Fiddler
```shell
inetcpl.cpl # настройка инета
# подключение - настройка сети - (оставить ТОЛЬКО автоматическое определение сети)
```
_____________
#### Links
[[Инструменты для тестировщика]]
[Fiddler / Установка и настройка](https://www.youtube.com/watch?v=ILPJ653XXrY&t=2s)
[Документация Fiddler Classic](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)
[Документация Fiddler Everywhere](https://docs.telerik.com/fiddler-everywhere/introduction)