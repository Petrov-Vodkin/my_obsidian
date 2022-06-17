2021-06-21 17:28
#tag
# API
`API` (Application Programming Interface - интерфейс программных приложений )   — это установка функций и правил позволяющая взаимодействовать между программным обеспечением, которое предоставляет API и другими программными компонентами. В Веб разработке, под API обычно подразумевают набор стандартных методов, свойств, событий и URL ссылок для взаимодействия с Веб контентом.

`API` - "контракт" общения\взаимодействия с системой `данные на входе`(`от кого`пользованеля\ситемы, тип запроса) => операции(`набор функций`) =>  `данные на выходе`(`кому` пользователю\системе)
**[[Стратегия тестирования REST API]]**: что именно вам нужно тестировать?
Тестируем `ЧЕРЕЗ` API, `ЧЕРЕЗ`GUI (не API, а через API)
-   GET — получение ресурса
-   POST — создание ресурса
-   PUT — обновление ресурса
-   DELETE — удаление ресурса

методы могут быть [безопасными](https://developer.mozilla.org/ru/docs/Glossary/safe), [идемпотентными](https://developer.mozilla.org/ru/docs/Glossary/Idempotent) или [кешируемыми](https://developer.mozilla.org/ru/docs/Glossary/cacheable).

[GET](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/GET) запрашивает представление ресурса. Запросы с использованием этого метода могут только извлекать данные.
[HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD) запрашивает ресурс так же, как и метод GET, но без тела ответа.
[POST](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/POST) используется для отправки сущностей к определённому ресурсу. Часто вызывает изменение состояния или какие-то побочные эффекты на сервере.
[PUT](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/PUT) заменяет все текущие представления ресурса данными запроса.
[DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE) удаляет указанный ресурс.
[CONNECT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT) устанавливает "туннель" к серверу, определённому по ресурсу.
[OPTIONS](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/OPTIONS) используется для описания параметров соединения с ресурсом.
[TRACE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE) выполняет вызов возвращаемого тестового сообщения с ресурса.
[PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH) используется для частичного изменения ресурса.

#### SOAP

#### [[REST API]]
Принципы REST API:
- **Отделения клиента от сервера**
- **Отсутствие записи состояния клиента**
- **Кэшируемость**
- **Единство интерфейса**
- **Многоуровневость системы**
- **Предоставление кода по запросу (Code on Demand).** Серверы могут отправлять клиенту код
- **Начало от нуля**

#### [[gRPC]]
??

#### [Спецификации](https://developer.mozilla.org/ru/docs/Web/HTTP/Methods#%D1%81%D0%BF%D0%B5%D1%86%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D0%B8 "Permalink to Спецификации")

_____________
#### Links
[[Swager]] | [[Postman]] | 