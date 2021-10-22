2021-02-05 13:36
#fake 
# [Mimesis](https://ru.bmstu.wiki/Mimesis)
```bash
pip install mimesis # генерация фиктивных данных
```
Наиболее распространенным типом данных в приложениях являются личные данные пользователей, такие как имя, фамилия, информация о кредитной карте и т. д. Для генерации таких данных существует специальный класс-провайдер — `Person`:
```python
from mimesis import Person
from mimesis.enums import Gender
from collections import OrderedDict
from pprint import pprint

def get\_card(gender):
   person \= Person('ru')
   return OrderedDict(id\=person.identifier(mask\='##-##/##'),
                      full\_name\=person.full\_name(gender),
                      age\=person.age(minimum\=18, maximum\=66),
                      occupation\=person.occupation(),
                      telephone\=person.telephone(mask\='', placeholder\='#'),
                      email\=person.email(domains\=('yandex.ru', gmail.com')),
                      username\=person.username(template\='UU\_d'),
                      password\=person.password(length\=8, hashed\=True))

pprint(get\_card(Gender.FEMALE))
OrderedDict(\[('id', '62-76/68'),
       ('full\_name', 'Наталья Краснова'),
       ('age', 35),
       ('occupation', 'Кинолог'),
       ('telephone', '+7-(930)-658-55-35'),
       ('email', 'alligators1947@gmail.com'),
       ('username', 'Index1932'),
       ('password', '69507085011b9806a5e4ef26dd13e034'\])

print(get\_card(Gender.MALE))
OrderedDict(\[('id', '57-07/90'),
      ('full\_name', 'Арнольд Платонов'),
      ('age', 43),
      ('occupation', 'Судебный пристав'),
      ('telephone', '+7-(968)-704-33-85'),
      ('email', 'darrin1897@yandex.ru'),
      ('username', 'Stork1992'),
      ('password', '1219adb0a8061adef29c4ee4c8845986'\])
```
_____________
#### Links
[[PyTest]] [[Python]]