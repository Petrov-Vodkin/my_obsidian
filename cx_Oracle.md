2021-10-20 10:36
[doc-en](https://cx-oracle.readthedocs.io/en/latest/user_guide/connection_handling.html)
# cx_Oracle
```py
# Installing the cx_Oracle module
python3 -m pip install cx_Oracle --upgrade
```
You can connect to Oracle Database using `cx_Oracle` in two ways: standalone and pooled connections.

The standalone connections are useful when the application has a single user session to the Oracle database while the collection pooling is critical for performance when the application often connects and disconnects from the database.

Before diving into each method, let’s create a module `config.py` to store the Oracle database’s configuration:
```py
`username = 'OT'
password = '<password>'
dsn = 'localhost/pdborcl'
port = 1512
encoding = 'UTF-8'` 
```
In this module, the `dsn` has two parts the server (`localhost`) and the pluggable database (`pdborcl`)
If the Oracle Database runs on the `example.com`, you use the following `dsn`:
`dsn = 'example.com/pdborcl'` 

### Creating standalone connections
To create a standalone connection, you use the `cx_Oracle.connect()` method or `cx_Oracle.Connection()`.
The following `connect.py` shows how to create a new connection to Oracle Database:
```py
import cx_Oracle
import config

connection = None
try:
    connection = cx_Oracle.connect(
        config.username,
        config.password,
        config.dsn,
        encoding=config.encoding)

    # show the version of the Oracle Database
    print(connection.version)
except cx_Oracle.Error as error:
    print(error)
finally:
    # release the connection
    if connection:
        connection.close()
```

### [Попов](https://www.opennet.ru/base/dev/python_dba.txt.html)
Далее создадим экземпляр класса connect, именно этот объект и
   обеспечивает взаимодействие с сервером Oracle
```py
try:
	my_connection=cx_Oracle.connect('system/manager@test_db')
except cx_Oracle.DatabaseError,info:
	print "Logon  Error:",info
	exit(0)
# Теперь создаем курсор и выполняем запрос:
my_cursor=my_connection.cursor()
try:
	my_cursor.execute("""
	SELECT OWNER,SEGMENT_TYPE,TABLESPACE_NAME,SUM(BLOCKS)SIZE_BLOCKS,
	COUNT(*) SIZE_EXTENTS FROM DBA_EXTENTS
	GROUP BY OWNER,SEGMENT_TYPE,TABLESPACE_NAME
	""")
except cx_Oracle.DatabaseError,info:
	print "SQL Error:",info
	exit(0)
```
   Динамическая типизация и поддержка сложных структур данных позволяет
   легко обработать результаты SQL запроса. В результате вызова
   my_cursor.fetchall() возращается список записей,каждая из которых
   явлеяется кортежем(неизменяемым списком) полей разного типа. Если вы
   знакомы с PL/SQL, то тогда цикл по списку записей будет очевидным:
```py
for record in my_cursor.fetchall():
		<обработка записи record,
		record содержит кортеж из полей текущей записи>
```
   Существует еще более любопытная возможность "разобрать" текущую запись
   по полям:
```
for OWNER,SEGMENT_TYPE,TABLESPACE_NAME,SIZE_BLOCKS,SIZE_EXTENTS in my_cursor.fe
tchall():
		<теперь можно обращаться к переменным, содержащим значения
		одноименных полей курсора>
```
   Печатаем результат на stdout. Замечу здесь, что my_cursor.description
   возвращает описание столбцов запроса в виде списка кортежей. Для
   каждого столбца возвращаются следующие данные:(name, type_code,
   display_size, internal_size, precision, scale, null_ok). Далее следует
   форматированный вывод на stdout( почти как printf в языке C).
```py
print
print 'Database:',my_connection.tnsentry
print
print "Used space by owner, object type, tablespace "
print "----------------------------------------------------------------------------------"
title_mask=('%-16s','%-16s','%-16s','%-8s','%-8s')
i=0
for column_description in my_cursor.description:
    print title_mask[i]%column_description[0],
    i=1+i
print ''
print "----------------------------------------------------------------------------------"
row_mask='%-16s %-16s %-16s %8.0f %8.0f '
for record in my_cursor.fetchall():
    print row_mask%record
```
   В результате мы увидим что-то вроде:
```py
Database: testdb
Used space by owner, object type, tablespace
--------------------------------------------------------------------------------
OWNER            SEGMENT_TYPE     TABLESPACE_NAME  SIZE_BLOCKS SIZE_EXTENTS
--------------------------------------------------------------------------------
ADU2             INDEX            USERS                 784       25
ADU2             TABLE            USERS                 512       24
ADUGKS           INDEX            DEVELOP_DATA          984      123
ADUGKS           TABLE            DEVELOP_DATA          664       83
ADUGPA           INDEX            USERS                 784       25
ADUGPA           TABLE            USERS                 496       23
AGNKS_SG         INDEX            USERS                 352       22
AGNKS_SG         TABLE            USERS                 240       15
```  


Часть II. Запросы с параметрами.
--------------------------------

   Согласно спецификации Python Database API 2.0, для выполнения запросов
   с параметрами каждый модуль должен реализовывать переменную
   paramstyle, которая определяет каким образом будут передаваться
   параметры запросов. Текущая версия cx_Oracle(3.0) поддерживает режим
   'named', то есть в модуле установлена переменная
   cx_Oracle.paramstyle='named' и можно создавать конструкции в запросах
   в виде:
```py
select *  from all_users where USERNAME LIKE :S
```
   При этом связывание параметров запроса со значениями можно можно
   выполнить двумя способами:
     * Именованный параметeр метода execute:
```PY
cursor2.execute("select *  from all_users where USERNAME LIKE :S ",S='S%')
```
     * Словарь {':переменная':значение,...}
```PY
cursor2.execute("select *  from all_users where USERNAME LIKE :S ",{':S':'S%'})
```

Часть III. Анонимные блоки PL/SQL
---------------------------------

Несмотря на существавания стандартов на язык SQL, реальные потребности
администратора часто требуют использования нестандартных средств
сервера(более того, написать приложение, работающее с сервером на
стандартном SQL возможно только для весьма тривиальных приложений ),
для Oracle таким нестандартным, но очень удобным механизмом является
возможность исполнения анонимных блоков PL/SQL. Модуль сx_Oracle
реализует этот механизм, который естественно не описан в спецификации
Python Database API 2.0 .

Чтобы связать переменные блока PL/SQL c переменными языка PYTHON, в
модуле сx_Oracle реализован класс var.Эекземпляр можно создать
следующим образом:

     var=my_cursor.var(cx_Oracle.DATETIME)

Конструктор my_cursor.var(...) в качестве параметра требует указать
тип создаваемой переменной. Варианты: * `BINARY` * `DATETIME`* `FIXEDCHAR` * `LONGBINARY` * `LONGSTRING` * `NUMBER` * `ROWID` * `STRING`

```py
var=my_cursor.var(cx_Oracle.DATETIME)
try:
    my_cursor.execute("""begin
    SELECT SYSDATE INTO :p_Value from dual;
    end;""",p_Value = var)

except cx_Oracle.DatabaseError,info:
    print "SQL Error:",info
    exit(0)
```
Очевидно, что теперь var содержит текущее время сервера.Доступ к
значениям переменной выполняется с помощью метода var.getvalue():
```py
CDATE=var.getvalue()
print 'Date: %02u/%02u/%4u'%(CDATE.day,CDATE.month,CDATE.year)
print 'Time: %02u:%02u:%02u'%(CDATE.hour,CDATE.minute,CDATE.second)
```
Этот пример демонстрирует также и форматирование даты и времени для
экземпляра var. В результате напечатается нечто вроде:
```
     Date: 12/05/2003
     Time: 16:42:54
```
Всвязи с тем, что работа с типами времени и даты внутри сервера Oracle
реализованы особенным образом ( независимо от ОС), модуль сx_Oracle
реализует следующие функции для для преобразования значений дат и
времени :
  * Date( year, month, day)
  * DateFromTicks( ticks)
  * Time( hour, minute, second)
  * TimeFromTicks( ticks)
  * Timestamp( year, month, day, hour, minute, second)
  * TimestampFromTicks( ticks)
```py
     var=my_cursor.var(cx_Oracle.DATETIME)
     var.setvalue(0,cx_Oracle.Date( 2002, 02,12))
     CDATE=var.getvalue()
     print 'Date: %02u/%02u/%4u'%(CDATE.day,CDATE.month,CDATE.year)
     print 'Time: %02u:%02u:%02u'%(CDATE.hour,CDATE.minute,CDATE.second)
```
Результат будет следующий:
```
     Date: 12/02/2002
     Time: 00:00:00
```
Ссылки
[http://computronix.com/utilities.shtml](http://computronix.com/utilities.shtml)
[http://www.python.org/topics/database/DatabaseAPI-2.0.html](http://www.python.org/topics/database/DatabaseAPI-2.0.html)

Листинги

Листинг 1.
Выполняем простой запрос
_________________________________________________________________
```py
"""
cx_Oracle demo
simple query
"""
__AUTHOR__='POPOV O.'
__COPYRIGHT__='POPOV O. 2002 Samara, Russia'
from sys import exit
try:
    import cx_Oracle
except ImportError,info:
    print "Import Error:",info
    sys.exit()

if cx_Oracle.version<'3.0':
    print "Very old version of cx_Oracle :",cx_Oracle.version
    sys.exit()
try:
    my_connection=cx_Oracle.connect('system/gasdba@sqlmt')
except cx_Oracle.DatabaseError,info:
    print "Logon  Error:",info
    exit(0)

my_cursor=my_connection.cursor()
try:
    my_cursor.execute("""
    SELECT OWNER,SEGMENT_TYPE,TABLESPACE_NAME,SUM(BLOCKS)SIZE_BLOCKS,
    COUNT(*) SIZE_EXTENTS FROM DBA_EXTENTS
    GROUP BY OWNER,SEGMENT_TYPE,TABLESPACE_NAME
    """)
except cx_Oracle.DatabaseError,info:
    print "SQL Error:",info
    exit(0)

print
print 'Database:',my_connection.tnsentry
print
print "Used space by owner, object type, tablespace "
print "-----------------------------------------------------------"
title_mask=('%-16s','%-16s','%-16s','%-8s','%-8s')
i=0
for column_description in my_cursor.description:
    print title_mask[i]%column_description[0],
    i=1+i
print ''
print "------------------------------------------------------------"
row_mask='%-16s %-16s %-16s %8.0f %8.0f '
for record in my_cursor.fetchall():
        print row_mask%record
for column_description in my_cursor.description:
    print column_description
```
Листинг 2.
Запрос с параметрами
_________________________________________________________________
```py
"""
cx_Oracle demo
query with parameters
"""
__AUTHOR__='POPOV O.'
__COPYRIGHT__='POPOV O. 2002 Samara, Russia'
from sys import exit
try:
    import cx_Oracle
except ImportError,info:
    print "Import Error:",info
    sys.exit()

if cx_Oracle.version<'3.0':
    print "Very old version of cx_Oracle :",cx_Oracle.version
    sys.exit()
try:
    my_connection=cx_Oracle.connect('system/manager@test_db')
except cx_Oracle.DatabaseError,info:
    print "Logon  Error:",info
    exit(0)

my_cursor=my_connection.cursor()
try:
    my_cursor.execute("""
    SELECT OWNER,SEGMENT_TYPE,TABLESPACE_NAME,SUM(BLOCKS)SIZE_BLOCKS,
    COUNT(*) SIZE_EXTENTS FROM DBA_EXTENTS
        WHERE OWNER LIKE :S
    GROUP BY OWNER,SEGMENT_TYPE,TABLESPACE_NAME
    """,S='SYS%')
except cx_Oracle.DatabaseError,info:
    print "SQL Error:",info
    exit(0)

print
print 'Database:',my_connection.tnsentry
print
print "Used space by owner, object type, tablespace "
print "-----------------------------------------------------------"
title_mask=('%-16s','%-16s','%-16s','%-8s','%-8s')
i=0
for column_description in my_cursor.description:
    print title_mask[i]%column_description[0],
    i=1+i
print ''
print "------------------------------------------------------------"
row_mask='%-16s %-16s %-16s %8.0f %8.0f '
for record in my_cursor.fetchall():
        print row_mask%record
```
___________
#### Links
https://www.oracletutorial.com/python-oracle/connecting-to-oracle-database-in-python/
https://www.oracle.com/database/technologies/appdev/python/quickstartpythononprem.html
https://www.foxinfotech.in/2018/09/cx_oracle-python-tutorials.html
https://dbaportal.eu/sidekicks/sidekick-cx_oracle-code-paterns/#part1

[[SQL]] [[Библиотека Python]]