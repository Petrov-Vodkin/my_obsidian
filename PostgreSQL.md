2021-07-22 14:09
[download](https://postgrespro.ru/windows) | [`Официальная документация по PostreSQL`](https://postgrespro.ru/docs/postgresql/13/index 'на русском языке') 
# PostgreSQL
[[psycopg2]] -py библиотека
Вся работа с PostgreSQL осуществляется под пользователем `postgres`.
`$sudo su postgres`
#### Основные команды PostgreSQL в интерактивном режиме:
```sql
				chcp 1251   # сменить кодировку для русской Windows 10
psql -h localhost -U user -d you_data_base # -U user(def: postgres) -d база_ _данных(на читую нужно создать - подкл без этого флага)
# psql -U postgres
					# список баз данных
\l ||  SELECT datname FROM pg_database; || psql -U postgres -c '\l'

\c || \connect	db	# подключиться к базе с именем db_name
\d table_name		# "описание" таблицы table_name (инф о table_name)
\du					# список пользователей
\dp || \z			# список таблиц, представлений, последовательностей, прав доступа к ним
\di	# индексы
\ds # последовательности
\dt # список таблиц
\dt+# список всех таблиц с описанием
\dt *s*	# список всех таблиц, содержащих s в имени
\dv # представления
\dS # системные таблицы
\d+ # описание таблицы
\o # пересылка результатов запроса в файл
\i # читать входящие данные из файла
\e # открывает текущее содержимое буфера запроса в редакторе (если иное не указано в окружении переменной EDITOR, то будет использоваться по умолчанию vi)
\d “table_name” # описание таблицы
\i		# запуск команды из внешнего файла, например \i /my/directory/my.sql
\pset	# команда настройки параметров форматирования
\echo	# выводит сообщение
\set	# устанавливает значение переменной среды. Без параметров выводит список текущих переменных (\unset # удаляет).
\? # справочник psql
\help # справочник SQL
\q (или Ctrl+D) # выход с программы
```
#### Работа с PostgreSQL из командной строки:
```sql
-c (или –command) # запуск команды SQL без выхода в интерактивный режим
-f file.sql # выполнение команд из файла file.sql
-l (или –list) # выводит список доступных баз данных
-U (или –username) # указываем имя пользователя (например postgres)
-W (или –password) # приглашение на ввод пароля
-d dbname # подключение к БД dbname
-h # имя хоста (сервера)
-s # пошаговый режим, то есть, нужно будет подтверждать все команды
–S # однострочный режим, то есть, переход на новую строку будет выполнять запрос (избавляет от ; в конце конструкции SQL)
-V # версия PostgreSQL без входа в интерактивный режим
'Примеры:'
psql -U postgres -d dbname -c «CREATE TABLE my(some_id serial PRIMARY KEY, some_text text);» # выполнение команды в базе dbname.
psql -d dbname -H -c «SELECT * FROM my» -o my.html # вывод результата запроса в html-файл.
```
#### Утилиты (программы) PosgreSQL:
```powershell
createdb и dropdb # создание и удаление базы данных (соответственно)
createuser и dropuser # создание и пользователя (соответственно)
pg_ctl # программа предназначенная для решения общих задач управления (запуск, останов, настройка параметров и т.д.)
postmaster # многопользовательский серверный модуль PostgreSQL (настройка уровней отладки, портов, каталогов данных)
initdb # создание новых кластеров PostgreSQL
initlocation # программа для создания каталогов для вторичного хранения баз данных
vacuumdb # физическое и аналитическое сопровождение БД
pg_dump # архивация и восстановление данных
pg_dumpall # резервное копирование всего кластера PostgreSQL
pg_restore # восстановление БД из архивов (.tar, .tar.gz)
 'Примеры создания резервных копий:'
# Создание бекапа базы mydb, в сжатом виде
pg_dump -h localhost -p 5440 -U someuser -F c -b -v -f mydb.backup mydb
# Создание бекапа базы mydb, в виде обычного текстового файла, включая команду для создания БД
pg_dump -h localhost -p 5432 -U someuser -C -F p -b -v -f mydb.backup mydb
# Создание бекапа базы mydb, в сжатом виде, с таблицами которые содержат в имени payments
pg_dump -h localhost -p 5432 -U someuser -F c -b -v -t *payments* -f payment_tables.backup mydb
# Дамп данных только одной, конкретной таблицы. Если нужно создать резервную копию нескольких таблиц, то имена этих таблиц перечисляются с помощью ключа -t для каждой таблицы.
pg_dump -a -t table_name -f file_name database_name
# Создание резервной копии с сжатием в gz
pg_dump -h localhost -O -F p -c -U postgres mydb | gzip -c > mydb.gz
```
#### Список наиболее часто используемых опций:
```powershell
-h host # хост, если не указан то используется localhost или значение из переменной окружения PGHOST.
-p port # порт, если не указан то используется 5432 или значение из переменной окружения PGPORT.
-u # пользователь, если не указан то используется текущий пользователь, также значение можно указать в переменной окружения PGUSER.
-a, —data-only # дамп только данных, по-умолчанию сохраняются данные и схема.
-b # включать в дамп большие объекты (blog’и).
-s, —schema-only # дамп только схемы.
-C, —create # добавляет команду для создания БД.
-c # добавляет команды для удаления (drop) объектов (таблиц, видов и т.д.).
-O # не добавлять команды для установки владельца объекта (таблиц, видов и т.д.).
-F, —format {c|t|p} # выходной формат дампа, custom, tar, или plain text.
-t, —table=TABLE # указываем определенную таблицу для дампа.
-v, —verbose # вывод подробной информации.
-D, —attribute-inserts # дамп используя команду INSERT с списком имен свойств.
"Бекап всех баз данных используя команду pg_dumpall."
pg_dumpall > all.sql
```
#### Восстановление таблиц из резервных копий (бэкапов):
```powershell
psql # восстановление бекапов, которые хранятся в обычном текстовом файле (plain text);  
pg_restore # восстановление сжатых бекапов (tar);
# Восстановление всего бекапа с игнорированием ошибок
psql -h localhost -U someuser -d dbname -f mydb.sql
# Восстановление всего бекапа с остановкой на первой ошибке
psql -h localhost -U someuser —set ON_ERROR_STOP=on -f mydb.sql
# Для восстановления из tar-арихива нам понадобиться сначала создать базу с помощью CREATE DATABASE mydb; (если при создании бекапа не была указана опция -C) и восстановить
pg_restore —dbname=mydb —jobs=4 —verbose mydb.backup
# Восстановление резервной копии БД, сжатой gz
gunzip mydb.gz
psql -U postgres -d mydb -f mydb
```
### [Install](https://winitpro.ru/index.php/2019/10/25/ustanovka-nastrojka-postgresql-v-windows/) in windows
#### Управления PostgreSQL через командную строку
```powershell
"Перед запуском СУБД"
chcp 1251	# смените кодировку для нормального отображения в русской Windows 10

cd C:\Program Files\PostgreSQL\11\bin # Перейдите в каталог bin
psql –V # Проверка установленной версии СУБД: 
```
**`postgresql.conf`**  C:\Program Files\PostgreSQL\11\data
#### Доступ к PostgreSQL по сети, правила файерволла 
разрешить сетевой доступ к вашему экземпляру PostgreSQL с других компьютеров
```PowerShell
'создать правила в файерволе. '
netsh advfirewall firewall add rule name="Postgre Port" dir=in action=allow protocol=TCP localport=5432
# rule name – имя правила &&   Localport – разрешенный порт
```
Либо вы можете [создать правило](https://winitpro.ru/index.php/2019/09/25/upravlenie-windows-firewall-powershell/), разрешающее TCP/IP доступ к экземпляру PostgreSQL на порту 5432 с помощью PowerShell:
```powershell
New-NetFirewallRule -Name 'POSTGRESQL-In-TCP' -DisplayName 'PostgreSQL (TCP-In)' -Direction Inbound -Enabled True -Protocol TCP -LocalPort 5432
```
После применения команды в брандмауэре Windows появится новое разрешающее правило для порта Postgres.

![правила бранжмауэра для доступа к PostgreSQL по сети](https://winitpro.ru/wp-content/uploads/2019/10/pravila-branzhmauera-dlya-dostupa-k-postgresql-po-se.png)

**Совет.** Для изменения порта в установленной PostgreSQL отредактируйте файл **postgresql.conf** по пути C:\Program Files\PostgreSQL\11\data.

Измените значение в пункте `port = 5432`. Перезапустите службу сервера postgresql-x64-11 после изменений. Можно [перезапустить службу с помощью PowerShell](https://winitpro.ru/index.php/2019/09/05/upravlenie-sluzhbami-windows-powershell/):

`Restart-Service -Name postgresql-x64-11`

![служба postgresql-x64-11](https://winitpro.ru/wp-content/uploads/2019/10/sluzhba-postgresql-x64-11.png)

Более подробно о настройке параметров в конфигурационном файле postgresql.conf с помощью тюнеров смотрите в [статье](https://winitpro.ru/index.php/2019/09/26/ustanovka-postgresql-db-centos/).

```sql
								"Example"
create database shop_test;  # ;;;;;;;_____;;;;;;;;;
\c shop_test;				# connect to shop_test
\d							# состояние бд
create table customer(
id serial primary key, name varchar(255), phone varchar(30), email varchar(255));

create table product(
id serial primary key, name varchar(255), description text, price integer);
'создать связанную таблицу ___references___'
create table product_photo(
id serial primary key, url varchar(255),
product_id integer references product(id)); # 'ссылка'\связь на\с табл product поле id

create table cart(
customer_id integer references customer(id), id serial primary key);

create table cart_product(
cart_id integer references cart(id), product_id integer references product(id));
'Заполнение таблиц(вставка данных)'
insert into customer(name, phone, email) values
('Василий', '02', 'vac@gmail.com'), ('Петр', '03', 'pert@gmail.com');

insert into product (name, description, price) values
('iPhone', 'cool_phone', 100000), ('Apple watch', 'крутые часы', 50000);

insert into product_photo (url, product_id) values ('iphone_photo', 1);
'Удаление FOREGN KEY'
alter table product_photo drop constraint product_photo_product_id_fkey;
```
_____________
#### Links
[Official](https://www.postgresql.org/) 
[Шпаргалка PSQL](https://www.oslogic.ru/knowledge/598/shpargalka-po-osnovnym-komandam-postgresql/)
[[SQL]]
[[Собес QA]]