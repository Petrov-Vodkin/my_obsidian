2022-02-24 14:30
#tag
# protobuf
### [Основы](https://coderlessons.com/articles/programmirovanie/protokol-buffers-osnovnye-veshchi-kotorye-vy-dolzhny-znat)
#### Начальный файл прото

Давайте посмотрим на простой файл прото

`syntax =` `"proto3"``;`

`package` `xyz.itshark.blog.protobuf;`

`option java_package =` `"xyz.itshark.blog.protobuf.generated"``;`

`option java_multiple_files =` `true``;`

Здесь мы определяем синтаксис как proto3 – Буферы протокола версии 3.

Во второй строке мы говорим, что все сгенерированные классы и остальные должны быть частью пакета «xyz.itshark.blog.protobuf».

Следующие две строки добавляют определенное поведение в случае, если мы генерируем код Java. В случае кода Java мы будем использовать другой пакет «xyz.itshark.blog.protobuf.generated», также мы говорим, что мы хотим, чтобы было создано несколько файлов вместо одного класса, содержащего все классы, определенные в файле прото.

#### Первое сообщение

Давайте добавим это в наш файл прото

`message Example {`

     `int32 id =` `1``;`

     `string first_name =` `2``;`

     `string last_name =` `3``;`

`}`

Это простой пример сообщения в protobuf. Его имя – Пример, и оно состоит из трех полей: id, first_name и last_name. Если вы знаете C или C ++, это может выглядеть очень похоже на struct.

Первое поле имеет тип _int32_ и имя _идентификатора_ . Кроме того, в конце есть что-то странное: знак «=» и номер _1_ . Все поля имеют это, только разные номера. Эти числа являются «тегами» и используются для позиционирования значений внутри двоичного сообщения для более быстрой и простой сериализации и десериализации данных. Очень важно помнить, что если какое-то число используется в каком-либо сообщении, оно никогда не может быть изменено или использовано повторно. Если, например, через некоторое время мы решим, что не хотим иметь идентификатор в нашем сообщении, мы можем изменить определение сообщения, чтобы оно выглядело следующим образом

`message Example {`

     `string first_name =` `2``;`

     `string last_name =` `3``;`

     `Int32 new_id =` `4``;`

`}`

Тем не менее, мы не можем сделать что-то подобное

`message Example {`

     `string not_valid =` `1``;`

     `string first_name =` `2``;`

     `string last_name =` `3``;`

`}`

Так как он будет выдавать разные сообщения с одинаковыми тегами и завершит работу любого приложения, которое все еще использует старый файл proto.

Второе поле в нашем примере сообщения имеет тип _string_ и имеет имя _first_name_ . Если вы из мира Java, вы заметите, что это не верблюжий случай, а правильный способ именования данных в Java. Это стандартный способ именования полей в буферах протокола. Компилятор буферов протокола проанализирует файл прото и сгенерирует код с соответствующим синтаксисом для языка под рукой. Не забывайте, что protobuf не зависит от языка и используется в разных языках, которые имеют разную синтаксическую логику для именования классов, атрибутов и тому подобного.

Все поля в примере сообщения являются необязательными. Чтобы быть более точным, все поля в случае протокола версии 3 являются необязательными. В случае версии 2 необходимо было явно пометить поля как необязательные или как обязательные. Проблема с обязательными полями заключалась в обратной совместимости и том факте, что они никогда не могли быть удалены. Проанализировав множество вариантов использования, было решено, что обязательные поля приносят больше вреда, чем пользы в долгосрочной перспективе, и в версии 3 все поля стали необязательными по умолчанию.

#### Генерация исходного кода

Вам нужно скачать компилятор protobuf для вашей системы по этому [адресу https://github.com/google/protobuf/releases](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://github.com/google/protobuf/releases&usg=ALkJrhg8aVCN2muO7jSst5-Skgiq8jvPJw) , как только вы установите его в своей системе, просто запустите эту команду, и вы получите сгенерированный код Java, готовый для использования в вашем проект

`$ protoc example.proto --java_out=~\code\blog\`

#### Расширенное сообщение

Давайте посмотрим на немного более сложное сообщение сейчас

##### Список / Массивы

Вы можете спросить себя, как определить список или массив элементов внутри сообщения. Для этого нужно просто добавить _повтор_ перед именем поля

`message Advanced {`

 `repeated string text =` `4``;`

`}`

##### Enum

Многие языки программирования имеют перечисления, вы также можете определить их в буферах протокола. Для определения enum вам просто нужно добавить что-то вроде этого

`enum` `Status {`

`SUCCESS =` `0``;`

            `FAIL =` `1``;`

            `RANDOM =` `2``;`

`}`

После этого вы можете использовать его как любой другой тип

`message Advanced {`

 `repeated string text =` `4``;`

             `Status my_status =` `3``;`

`}`

##### Сообщение в сообщении

Вы можете добавлять сообщения также внутри других сообщений. Например, в нашем случае мы определили сообщение Example, поэтому мы можем добавить поле в сообщение Advanced типа Example

`message Advanced {`

 `repeated string text =` `4``;`

         `Status my_status =` `3``;`

         `Example message_example =` `5``;`

`}`

Вы можете определить сообщение внутри сообщения и таким образом заставить его использовать «личное», только внутри этого сообщения.

`message Example2 {`

`message Internal {`

       `string text =` `1``;`

        `}`

`Internal valid_message =` `1``;`

`}`

`Message Invalid {`

           `Internal can_not_be_used_here =` `1``;`

`}`

Первое сообщение из этого примера (Example2) является действительным сообщением, в то время как второе сообщение (Invalid) недопустимо, как указано его именем, так как Internal может использоваться только внутри Example2.

##### Полный файл прото

`syntax =` `"proto3"``;`

`package` `xyz.itshark.blog.protobuf;`

`option java_package =` `"xyz.itshark.blog.protobuf.generated"``;`

`option java_multiple_files =` `true``;`

`enum` `Status {`

    `SUCCESS =` `0``;`

    `FAIL =` `1``;`

    `RANDOM =` `2``;`

`}`

`message Example {`

    `int32 id =` `1``;`

    `string first_name =` `2``;`

    `string last_name =` `3``;`

`}`

`message Advanced {`

    `repeated string text =` `4``;`

    `Status my_status =` `3``;`

    `Example message_example =` `5``;`

`}`

`message Example2 {`

`message Internal {`

`string text =` `1``;`

        `}`

`Internal valid_message =` `1``;`

`}`

##### Использование буферов протокола в коде Java

Как только у нас есть готовый файл прото и мы сгенерируем код Java, следующим шагом будет использование его в сочетании с нашим кодом. Если вы использовали protoc для генерации кода, вы можете просто скопировать его в соответствующее место в вашем проекте. Как только это будет сделано, вы можете создать подобный код для создания экземпляра объекта Example.

`Example example = Example.newBuilder()`

                `.setId(``1``)`

                `.setFirstName(``"First"``)`

                `.setLastName(``"Last"``)`

                `.build();`

Как вы можете видеть, мы используем компоновщики для генерации экземпляров сообщений Protocol Buffers. Для генерации расширенного сообщения мы можем использовать такой код:

`Advanced advanced = Advanced.newBuilder()`

                `.setMyStatus(Status.RANDOM)`

                `.addText(``"some text"``)`

                `.addText(``"other text"``)`

                `.setMessageExample(example)`

                `.build();`

И это все, что нужно сделать.

##### Ресурсы

-   Пример полного кода из этого сообщения в блоге [https://github.com/vladimir-dejanovic/protocol-buffers-blog-entry](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://github.com/vladimir-dejanovic/protocol-buffers-blog-entry&usg=ALkJrhjO6-hoezNsD0SOTrlY7fYbJKIo1w)
-   Простой REST API с использованием буферов протокола [https://github.com/vladimir-dejanovic/springboot-protobuffer-jersey-sample](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://github.com/vladimir-dejanovic/springboot-protobuffer-jersey-sample&usg=ALkJrhje_2su301IfGWcge01y2aqNUHbKw)
-   Скалярные типы в буферах протокола [https://developers.google.com/protocol-buffers/docs/proto3#scalar](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://developers.google.com/protocol-buffers/docs/proto3&usg=ALkJrhj_cbJf4y-E7y1p_XHsT2VFFhbixg#scalar)


https://googleapis.dev/python/protobuf/latest/
```google.protobuf.json_format```
```python
from google.protobuf.json_format import MessageToJson
# MessageToJson
json_obj = MessageToJson(org)
```
```python
from google.protobuf.json_format import MessageToDict
#  You can also serialise the protobuf to a dictionary:
dict_obj = MessageToDict(org)
```

 
----------------------------------


#### Links
https://googleapis.dev/python/protobuf/latest/ API
[[gRPC]]