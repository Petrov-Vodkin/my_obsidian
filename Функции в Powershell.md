2021-06-24 14:50
#tag [link](https://fixmypc.ru/post/sozdanie-funktsii-powershell-i-komandy-s-vyzovom-i-peredachei-parametrov/)
# Функции в Powershell
### Создание
```powershell
function Get-DayLog {
    Get-EventLog -LogName Application -Newest 50 | where TimeWritten -ge (Get-Date).AddHours(-14)
}
```
Любая функция обязательно должна состоять из трех вещей:
-   function - объявляет и говорит что после нее будет название;
-   имя функции - название, по которому мы будем ее вызывать. В нашем случае имя Get-DayLog;
-   скобки - обозначают начало и конец выражения.
После написания функции она вызывается по имени:
```powershell
Get-DayLog
```
[![функции windows powershell](https://fixmypc.ru/media/uploads/2019/10/10/2.jpg "функции windows powershell")](https://fixmypc.ru/media/uploads/2019/10/10/2.jpg)

### Именование (рекомендованное)
Функции в Powershell всегда именуются по следующему признаку. Первое слово это глаголы типа:
-   Get - получить;
-   Set - изменить;
-   Install - установить;
-   New - создать.
Второе имя - это существительное, как в случае выше DayLog(дневной лог). У Microsoft есть утвержденный список глаголов, который доступен по [ссылке](https://docs.microsoft.com/en-us/powershell/developer/cmdlet/approved-verbs-for-windows-powershell-commands) на английском языке.
## Передача параметров, аргументов[](https://windowsnotes.ru/powershell-2/peredacha-parametrov-v-powershell/)
[Обработка аргументов функций](https://webistore.ru/administrirovaniye-windows/obrabotka-argumentov-funkcij-v-windows-powershell/)
```powershell
			"Пердача массива любой длинны(len)"
# Все переменные, с которыми работает функция, автоматически сохраняются в переменной $Args.
# в $Args содержится массив, элементами которого являются параметры функции, указанные при её запуске
Function Primer{"Столицей России были $Args"}
```
```powershell
# Самый простой вариант передачи данных — использовать встроенную переменную $args, которая имеет тип одномерный массив (hashtable). Для этого создадим скрипт с именем service.ps1 вот такого содержания:
				'1'
Get-Service -Name $args[0] -ComputerName $args[1]
# Теперь выполним его, указав в качестве аргументов сервис печати (spooler) и имя компьютера SRV1:
.\name_script.ps1 spooler SRV1
				'2'
Param (  
[string]$service,  
[string]$computer  
)
Get-Service -ServiceName $service -ComputerName $computer
# Если блок param имеется в сценарии или функции, PowerShell сам считывает его и разделяет знаками табуляции. Поскольку имена параметров явно указаны, то их можно вводить в любом порядке, например так:
.\\name_script.ps1 -Service spooler -Computer SRV1
				
				'3'
function Get-DayLog ($param1,$param2) {
    Get-EventLog -LogName Application -Newest $param1 | where TimeWritten -ge (Get-Date).AddHours($param2)
}
# Теперь, для вызова функции, требуется передавать 2 ОБЯЗАТЕЛЬНЫХ параметра:
Get-DayLog -param1 50 -param2 -14
```

### Установка значений по умолчанию
```powershell
function Get-DayLog ($param1=50,$param2) {
     Get-EventLog -LogName Application -Newest $param1 | where TimeWritten -ge (Get-Date).AddHours($param2)
}
Get-DayLog -param2 -7
```

[![powershell параметры функции](https://fixmypc.ru/media/uploads/2019/10/10/5.jpg "powershell параметры функции")](https://fixmypc.ru/media/uploads/2019/10/10/5.jpg)

Мы должны всегда указывать ключ param2, что добавляет немного работы. Что бы это исправить достаточно поменять их местами:

```powershell
function Get-DayLog ($param2,$param1=50) {
     Get-EventLog -LogName Application -Newest $param1 | where TimeWritten -ge (Get-Date).AddHours($param2)
}
Get-DayLog -7 1
Get-DayLog -7
```

[![powershell функции](https://fixmypc.ru/media/uploads/2019/10/10/7.jpg "powershell функции")](https://fixmypc.ru/media/uploads/2019/10/10/7.jpg)

Как видно на примере, если мы не указываем ключи param1 и param2 важна последовательность, так как именно в такой последовательности они будут присвоены переменным внутри функций.

### Возвращение значений

В отличие от других языков, если мы присвоим переменной $result результат функции Get-DayLog, то она будет содержать значения:

```powershell
$result = Get-DayLog -7 1
```

Это будет работать до тех пор, пока мы не решим изменить функцию присвоив переменные:

```powershell
function Get-DayLog ($param2,$param1=50) {
     $events = Get-EventLog -LogName Application -Newest $param1 | where TimeWritten -ge (Get-Date).AddHours($param2)
}
$result = Get-DayLog -7
$result
$events 
```

[![powershell функции и процедуры](https://fixmypc.ru/media/uploads/2019/10/10/10.jpg "powershell функции и процедуры")](https://fixmypc.ru/media/uploads/2019/10/10/10.jpg)

Мы не можем получить результат используя переменную $result, так как функция не выводит информацию, а хранит ее в переменной $events. Вызывая $events мы тоже не получаем информацию, так как тут работает понятие "область видимости переменных".

Так как функции это отдельная часть программы вся логика и ее переменные не должны касаться другой ее части. Область видимости переменных подразумевает это же. Все переменные, созданные внутри функции остаются в ней же. Эту ситуацию можно исправить с помощью return:

```powershell
function Get-DayLog ($param2,$param1=50) {
     $events = Get-EventLog -LogName Application -Newest $param1 | where TimeWritten -ge (Get-Date).AddHours($param2)
return $events
}
$result = Get-DayLog -7
$result
```

[![включения функции ввода команд в powershell](https://fixmypc.ru/media/uploads/2019/10/10/11.jpg "включения функции ввода команд в powershell")](https://fixmypc.ru/media/uploads/2019/10/10/11.jpg)

Я бы рекомендовал всегда возвращать значения через return, а не использовать вывод используя команды типа Write-Output внутри функции. Использование return останавливает работу функции и возвращает значение, а это значит что его не стоит ставить по середине логики если так не планировалось.

Возвращаемых значений может быть несколько. Для примера создадим функцию, которая будет считать зарплату и налог:

```powershell
function Get-Salary ($Zarplata) {
$nalog = $Zarplata * 0.13
$zarplata_bez_nds = $Zarplata - $nalog
return $nalog,$zarplata_bez_nds
}
Get-Salary -Zarplata 100000
```

[![powershell процедуры](https://fixmypc.ru/media/uploads/2019/10/10/12.jpg "powershell процедуры")](https://fixmypc.ru/media/uploads/2019/10/10/12.jpg)

Я вернул оба значения разделив их запятой. По умолчанию всегда возвращается массив. [Массивы в Powershell](https://fixmypc.ru/post/rabota-v-powershell-s-massivami-i-listami-na-primerakh/ "Массивы в Powershell") это набор не именованных параметров. Более подробно  о них мы уже писали.

В случае с массивами, что бы добавит надпись о зарплате, и налоге нужно использовать индексы:

```powershell
$result = Get-Salary -Zarplata 100000

$result.GetType()
Write-Host "это зарплата" $result[1]
Write-Host "это налог" $result[0]
```

[![создание скрипта powershell](https://fixmypc.ru/media/uploads/2019/10/10/13.jpg "создание скрипта powershell")](https://fixmypc.ru/media/uploads/2019/10/10/13.jpg)

Возвращаться может любой тип данных. Например мы можем использовать другой тип данных хэш таблицы, которые в отличие от массивов именованные:

```powershell
function Get-Salary ($Zarplata) {
$nalog = $Zarplata * 0.13
$zarplata_bez_nds = $Zarplata - $nalog
return @{"Налог"=$nalog;"Зарплата"=$zarplata_bez_nds;}
}
Get-Salary -Zarplata 100000
```

[![командный файл powershell](https://fixmypc.ru/media/uploads/2019/10/10/14.jpg "командный файл powershell")](https://fixmypc.ru/media/uploads/2019/10/10/14.jpg)

Подробно о [хэш таблицах в Powershell](https://fixmypc.ru/post/ispolzovanie-khesh-tablits-v-powershell-hashtable-i-sozdanie-na-primerakh/ "хэш таблицах в Powershell") вы можете почитать в предыдущих статьях. Далее так же будет несколько примеров с ними.

Вы можете возвращать любой тип данных и в любом формате и последовательности. Каждый из них имеет своё преимущество.

### Область видимости переменных

Все переменные объявленные до момента вызова функции могут быть ей использованы:

```powershell
$Zarplata = 100000
function Get-Salary {
$nalog = $Zarplata * $nalog
$zarplata_bez_nds = $Zarplata - $nalog
return @{"Налог"=$nalog;"Зарплата"=$zarplata_bez_nds;}
}
$nalog = 0.20
Get-Salary
```

[![powershell написать скрипт](https://fixmypc.ru/media/uploads/2019/10/10/15.jpg "powershell написать скрипт")](https://fixmypc.ru/media/uploads/2019/10/10/15.jpg)

Такой подход не запрещает переопределить переменную внутри функции дав ей другое значение:

```powershell
$Zarplata = 100000
function Get-Salary {
$Zarplata = 200000
$nalog = $Zarplata * $nalog
$zarplata_bez_nds = $Zarplata - $nalog
return @{"Налог"=$nalog;"Зарплата"=$zarplata_bez_nds;}
}
$nalog = 0.20
Get-Salary
$Zarplata
```

[![написание скрипта powershell](https://fixmypc.ru/media/uploads/2019/10/10/16.jpg "написание скрипта powershell")](https://fixmypc.ru/media/uploads/2019/10/10/16.jpg)

Как уже писалось выше, значения внутри функции не доступны вне нее и у нас есть все возможности что бы этого не потребовалось. Тем не менее есть способ объявить внутри функции переменную, которая будет доступна вне нее.

Такие переменные называются глобальными. Объявляются приставкой $global:

```powershell
$Zarplata = 100000
function Get-Salary {

$global:Zarplata = 200000
$nalog = $Zarplata * $nalog
$zarplata_bez_nds = $Zarplata - $nalog
return @{"Налог"=$nalog;"Зарплата"=$zarplata_bez_nds;}
}
$nalog = 0.20
Get-Salary
$Zarplata
```

[![powershell передать параметры](https://fixmypc.ru/media/uploads/2019/10/10/17.jpg "powershell передать параметры")](https://fixmypc.ru/media/uploads/2019/10/10/17.jpg)

Как вы видите, в отличие от предыдущего примера, переменная $zarplata изменила значение. Использование глобальных переменных является нежелательным так как может привести к ошибкам. Ваш скрипт может быть импортируемым модулем и об этой переменной может никто не знать, тем не менее она будет в области видимости.

## Дополнительные возможности работы с параметрами

### Строгие типы данных

Powershell автоматически преобразует типы данных. В отличие от других языков результат этого выражения будет число 3, а не "111":

```powershell
3 * "1"
```

Такой подход может привести к ошибке. Мы можем исправить это объявляя типы:

```powershell
function Get-Size ([int]$Num){
    $size = 18 * $Num
    return $size
}
Get-Size 5
Get-Size "str"
```

[![powershell передача параметров](https://fixmypc.ru/media/uploads/2019/10/10/18.jpg "powershell передача параметров")](https://fixmypc.ru/media/uploads/2019/10/10/18.jpg)

То есть объявляя типы данных мы либо получим ошибку избежав неверного преобразования. Если бы мы передавали такую строку "1", то у нас корректно выполнилось преобразование в число.

Таких типов данных в Powershell  всего 13:

-   \[string\] - строка;
-   \[char\] - 16-битовая строка Unicode;
-   \[byte\] - 8 битовый символ;
-   \[int\] - целое 32 битовое число;
-   \[long\] - целое 64 битовое число;
-   \[bool\] - булево значение True/False;
-   \[decimal\] - 128 битовое число с плавающей точкой;
-   \[single\] - 32 битовое число с плавающей точкой;
-   \[double\] - 64 битовое число с плавающей точкой;
-   \[DateTime\] - тип данных даты и времени;
-   \[xml\] - объект xml;
-   \[array\] - массив;
-   \[hashtable\] - хэш таблица.

Примеры работы с некоторыми типами данных вы увидите далее.

### $args

В языках программирования есть понятие позиционного параметра. Это такие параметры, которые могут передаваться без имен:

```powershell
function Get-Args {
    Write-Host "Пример с arg: " + $args[0] -BackgroundColor Red -ForegroundColor Black
    Write-Host "Пример с arg: " + $args[1] -BackgroundColor Black -ForegroundColor Red
}

Get-Args "Первый" "Второй"
```

### [![функции windows powershell](https://fixmypc.ru/media/uploads/2019/10/11/25.jpg "функции windows powershell")](https://fixmypc.ru/media/uploads/2019/10/11/25.jpg)

Обратите внимание, что $args является массивом и значение получаются по индексу. Я не ставлю запятую при вызове функции так как в этом случае у меня был бы массив двойной вложенности.

### Обязательные параметры Mandatory

Попробуем выполнить следующий пример, который должен вернуть дату изменения файла:

```powershell
function Get-ItemCreationTime ($item){
    Get-Item -Path $item | select LastWriteTime
}

Get-ItemCreationTime "C:\Windows\explorer.exe"
Get-ItemCreationTime
```

[![powershell вызов функции](https://fixmypc.ru/media/uploads/2019/10/11/23.jpg "powershell вызов функции")](https://fixmypc.ru/media/uploads/2019/10/11/23.jpg)

Первый вызов функции прошел успешно, так как мы передали параметры. Во втором случае мы не передаем значения, а значит переменная $item равна $null (неопределенному/неизвестному значению). Во многих языках, в таких случаях, у нас появилась бы ошибка еще в момент вызова функции Get-ItemCreationTime, а не во время выполнения Get-Item.

Представьте что до получения даты изменения файла будут еще какие-то действия, например удаление и создание файлов, которые могут привести к поломке компьютера или программы. Что бы этого избежать можно объявить этот параметр обязательным:

```powershell
function Get-ItemCreationTime ([parameter(Mandatory=$true)]$item){
    Get-Item -Path $item | select LastWriteTime
}

Get-ItemCreationTime "C:\Windows\explorer.exe"
Get-ItemCreationTime
```

[![powershell параметры функции](https://fixmypc.ru/media/uploads/2019/10/11/24.jpg "powershell параметры функции")](https://fixmypc.ru/media/uploads/2019/10/11/24.jpg)

Атрибут Mandatory обязывает указывать значение. Если оно будет проигнорировано, то мы получим ошибку еще до момента выполнения функции.

### Param()

Вы могли видеть функции, которые имеют значение Param(). Это значение так же объявляет параметры. На предыдущем примере это значение использовалось бы так:

```powershell
function Get-ItemCreationTime {

param (
[parameter(Mandatory=$true)]$item
)

    Get-Item -Path $item | select LastWriteTime
}

Get-ItemCreationTime "C:\Windows\explorer.exe"
Get-ItemCreationTime
```

Microsoft Рекомендует использовать именно такой синтаксис написания функции, но не обязывает его использовать. Такой синтаксис говорит, что это не просто функция, а командлет.

Создадим скрипт, в котором будет происходить умножение, где добавим несколько обязательных параметров используя синтаксис с Param:

```powershell
function Get-PlusPlus {
param (
[parameter(Mandatory=$true, Position=0)]
[int]
$item1,
        [parameter(Position=1)]
        [int]
        $item2,
        [parameter(Position=2)]
        [string]
        $item3
        )

    $summ = $item1 + $item2
    Write-Output $item3 $summ
}

Get-PlusPlus 2 5 "Summ"
```

[![powershell функции](https://fixmypc.ru/media/uploads/2019/10/12/28.jpg "powershell функции")](https://fixmypc.ru/media/uploads/2019/10/12/28.jpg)

Position говорит под каким номером передается значение.

Одно из преимуществ работы с param() в том, что у нас становятся доступны ключи типа -Confirm и -Verbose. 

### CmdletBinding()

Использование этого атрибута позволяет расширять возможность по созданию командлетов. Microsoft пишет, что использование CmdletBinding или Parameter расширяет возможность функций в Powershell, но по моему опыту не всегда все срабатывает и нужно ставить оба атрибута.

На примере ниже я установил ограничение на длину строк с 1 по 13 символов с помощью ValidateLength(1,13). Position=1 говорит об индексе элемента в массиве:

```powershell
function Get-LenStr {
    [CmdletBinding()]
param (        
[parameter(Mandatory=$true,
                        Position=1
                        )]        
        [ValidateLength(1,13)]
[string]
$len1,
[parameter(Mandatory=$true, 
                        Position=0
                        )]
[string]
$len2
        )
    Write-Host $len2 $len1
}

Get-LenStr "Это строка 1" "Это строка 2"
```

Таких дополнительных аргументов для команд достаточно много. Для примера еще несколько атрибутов, которые можно добавить в блок parameter:

-   HelpMessage = "Текст"  - подсказка по использованию переменной. Это пояснение можно увидеть при запросе справки через Get-Help;
-   ParameterSetName="Computer" - указывает к какой переменной относятся параметры;

Отдельные блоки типа \[ValidateLength\]:

-   \[Alias('t')\] - устанавливает алиас для этого параметра в виде буквы t;
-   \[PSDefaultValue(Test='Test')\] - устанавливает значение по умолчанию переменной Test;
-   \[string\[\]\] - такое использование говорит, что значение принимает массив строк
-   \[AllowNull()\] - позволяет обязательным параметрам быть $null
-   \[AllowEmptyString()\] - позволяет обязательным параметрам быть пустой строкой
-   \[AllowEmptyCollection()\] - обязательный параметр с пустым массивом
-   \[ValidateCount(1,5)\] - минимальное и максимальное количество значений.
-   \[ValidatePattern("\[0-9\]")\] - проверка на шаблон используя регулярного выражения

Больше примеров и аргументов на [сайте Microsoft](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-6).

## Использование массивов

### Передача массивов в виде параметров

В предыдущих статьях было множество примеров по работе с массивами и хэш таблицами. Их же, как и любой другой тип данных, мы можем передавать в функцию. Для примера все команды Powershell, которые имеют ключ ComputerName, могут выполняться удаленно. Большинство таких команд могут принимать в виде значений массивы, то есть нам не обязательно передавать поочередно имена компьютеров.

Функция будет принимать массив с именами компьютеров и возвращать все остановленные сервисы. Я так же объявлю этот тип строгим, для наглядности, хотя и без этого в любом случае сработает:

```powershell
function Get-ServiceStopped ([array]$Computers){
    $services = Get-Service -ComputerName $Computers | where Status -eq Stopped
    return $services
}

Get-ServiceStopped '127.0.0.1','localhost'
```

[![powershell функции и процедуры](https://fixmypc.ru/media/uploads/2019/10/10/19.jpg "powershell функции и процедуры")](https://fixmypc.ru/media/uploads/2019/10/10/19.jpg)

Массивы так же работают по индексам, что позволяет передавать больше параметров. Такой способ не релевантный, но может когда-то пригодиться.

```powershell
function Get-ServiceStopped ([array]$Computers){
    $services = Get-Service -ComputerName $Computers[0,-2] | where Status -eq $Computers[-1]
    return $services
}

Get-ServiceStopped '127.0.0.1','localhost','Stopped'
```

### Хэш таблицы

Параметры хэш таблиц и могут передаваться не просто в функцию, а как параметры командлетов. Добавим в нашу функцию запуск сервиса, если он остановлен:

```powershell
function Get-ServiceStopped ([hashtable]$Params){
    $services = Get-Service @Params | where Status -eq Stopped
    $services = Start-Service $services
    return $services
}

Get-ServiceStopped @{Name='WinRM';ComputerName=@('127.0.0.1','localhost')}
```

Знак @ в команде объявляет, что данные хэш таблицы будут использоваться как параметры команды. Важно, чтобы их имена  соответствовали настоящим параметрам.

## Условия

Нет никаких ограничений на использования условий. Это бывает достаточно удобно, когда функция должна вернуть разные значения.

### IF

Ниже приведен пример, где в зависимости от скорости загрузки основной части сайта будет возвращен разный ответ. Если скорость ответа меньше 76 миллисекунды нормальная, в случае если более долгого ответа вернется другой результат:

```powershell
function Get-SiteResponse {
    
    $start_time = Get-Date
    
    $request = Invoke-WebRequest -Uri "https://fixmypc.ru"
    
    $end_time = Get-Date
    
    $result =  $end_time - $start_time
    
    if ($result.Milliseconds -lt 76) {
        return "Скорость ответа нормальная " + $result.Milliseconds}
    else{
        return "Сайт отвечает долго " + $result.Milliseconds }
    
}

Get-SiteResponse
```

[![включения функции ввода команд в powershell](https://fixmypc.ru/media/uploads/2019/10/11/20.jpg "включения функции ввода команд в powershell")](https://fixmypc.ru/media/uploads/2019/10/11/20.jpg)

### Switch

Мы уже говорили про [Powershell Switch](https://fixmypc.ru/post/rabota-s-protsessami-powershell-start-process-i-upravlenie-imi-na-primerakh/ "Powershell Switch") в предыдущих статьях. Если коротко, то это более удобные условия. Используя предыдущий пример, но со Switch, это будет выглядеть так:

```powershell
function Get-SiteResponse {
    
    $start_time = Get-Date
    
    $request = Invoke-WebRequest -Uri "https://fixmypc.ru"
    
    $end_time = Get-Date
    
    $result =  $end_time - $start_time
    
    switch($result.Milliseconds) {
        {$PSItem -le 76} {
            return "Скорость ответа нормальная " + $result.Milliseconds}
        default {
            return "Сайт отвечает долго " + $result.Milliseconds }
    }
}

Get-SiteResponse
```

[![powershell процедуры](https://fixmypc.ru/media/uploads/2019/10/11/21.jpg "powershell процедуры")](https://fixmypc.ru/media/uploads/2019/10/11/21.jpg)

Другой пример Switch это вызов функции в зависимости от переданных параметров. На примере ниже я вызываю функцию, в которой находится Switch. В эту функцию я передаю имя компьютера, которое проверяется на упоминание указанных фраз и вызывает соответствующую функцию. Каждая функция, которая устанавливает обновления, возвращает значение в Switch, а затем происходит return внутри нее:

```powershell
function Install-SQLUpdates {
return "Установка обновлений на SQL сервер прошла успешно"
}

function Install-ADUpdates {
return "Установка обновлений на сервер AD прошла успешно"
}

function Install-FileServerUpdates {
return "Установка обновлений на файловый сервер прошла успешно"
}

function Make-Switch ($computer) {
$result = switch($computer){
{$computer -like "SQL*"} {Install-SqlUpdates}
{$computer -like "AD*"} {Install-ADUpdates}
{$computer -like "FileServer*"} {Install-FileServerUpdates}
default {"Такого типа компьютеров нет"}
}
return $result
}

Make-Switch "AD1"
```

[![создание скрипта powershell](https://fixmypc.ru/media/uploads/2019/10/11/22.jpg "создание скрипта powershell")](https://fixmypc.ru/media/uploads/2019/10/11/22.jpg)

Со switch так же удобно передавать булевы значения. В примере ниже если указан ключ -On сервис включится, а если его нет выключится:

```powershell
function Switch-ServiceTest ([switch]$on) {
    if ($on) {Write-Output "Сервис включен"}
    else {"Сервис выключен"}
}


Switch-ServiceTest -On
Switch-ServiceTest
```

[![командный файл powershell](https://fixmypc.ru/media/uploads/2019/10/11/26.jpg "командный файл powershell")](https://fixmypc.ru/media/uploads/2019/10/11/26.jpg)

## Передача через конвейер или Pipeline

Вы наверняка работали через команды Powershell, которые позволяли использовать конвейер следующим образом:

```powershell
Get-Process -Name *TestProc* | Stop-Process
```

Если мы захотим использовать подход описанный выше, создав новые команды в виде функций, то конвейер не будет работать:

```powershell
function Get-SomeNum {
    
    $num = Get-Random -Minimum 5 -Maximum 10
    return $num
}

function Plus-SomeNum ($num) {
    Write-Host "Прибавление числа " $num 
    $num += $num
    return $num
}

Get-SomeNum
Plus-SomeNum 5
Get-SomeNum | Plus-SomeNum
```

[![powershell написать скрипт](https://fixmypc.ru/media/uploads/2019/10/12/35.jpg "powershell написать скрипт")](https://fixmypc.ru/media/uploads/2019/10/12/35.jpg)

Выполнив следующую команду мы сможем увидеть, что значения, которые могут приниматься через конвейер помечаются специальным атрибутом:

```powershell
Get-Help Stop-Process -Parameter Name
```

[![](https://fixmypc.ru/media/uploads/2019/10/12/36.jpg)](https://fixmypc.ru/media/uploads/2019/10/12/36.jpg)

Таких атрибутов всего два:

-   **ValueFromPipelineByPropertyName** - получение значения из конвейера по имени;
-   **ValueFromPipeline** - получение через конвейер только значения .

Кроме этого, внутри нашей функции, мы должны добавить специальный блок Process. Наш скрипт в итоге будет выглядеть так:

```powershell
function Get-SomeNum { 
        $num = Get-Random -Minimum 5 -Maximum 10
        return $num
}

function Plus-SomeNum {
    [cmdletbinding()]
    Param (
            [parameter(ValueFromPipeline=$True)]
            [int]
            $num
        )
    process {  
    Write-Host "Прибавление числа " $num 
    $num += $num
    return $num
    }
}

1..5 | Plus-SomeNum
Get-SomeNum | Plus-SomeNum
```

[![написание скрипта powershell](https://fixmypc.ru/media/uploads/2019/10/12/37.jpg "написание скрипта powershell")](https://fixmypc.ru/media/uploads/2019/10/12/37.jpg)

\[cmdletbinding()\] - атрибут расширения функции, который добавляет некоторые возможности в функции позволяя им работать как команду.

Если бы мы не указали блок Process функция бы вернула только последней результат из массива 1..5:

[![powershell передать параметры](https://fixmypc.ru/media/uploads/2019/10/12/39.jpg "powershell передать параметры")](https://fixmypc.ru/media/uploads/2019/10/12/39.jpg)

Если наши команды будут иметь критический характер, такой как удаление, или через конвейер может передаваться несколько значений, то стоит использовать атрибут ValueFromPipelineByPropertyName. Таким образом мы исключим попадания через конвейер случайного значения. На примере ниже я изменил

```powershell
function Get-SomeNum { 
    $num = Get-Random -Minimum 5 -Maximum 10
    $object = [pscustomobject]@{num=$num}
    return $object
}

function Plus-SomeNum {
    [cmdletbinding()]
    Param (
            [parameter(ValueFromPipelineByPropertyName=$True)]
            [int]
            $num
        )
    process {  
    Write-Host "Прибавление числа " $num 
    $num += $num
    return $num
    }
}

Get-SomeNum | Plus-SomeNum
[pscustomobject]@{num=5} | Plus-SomeNum
[pscustomobject]@{bad=5} | Plus-SomeNum
```

[![powershell передача параметров](https://fixmypc.ru/media/uploads/2019/10/12/40.jpg "powershell передача параметров")](https://fixmypc.ru/media/uploads/2019/10/12/40.jpg)

Как уже писалось ValueFromPipelineByPropertyName принимает только именованные параметры и в случае с именем "bad" мы получаем ошибку:

-   Не удается привязать объект ввода к любым параметрам команды, так как команда не принимает входные данные конвейера
-   The input object cannot be bound to any parameters for the command either because the command does not take pipeline input or the input and its properties do not match any of the parameters that take pipeline input.

Причем передавать именованные параметры через хэш таблицы мы не можем, только через pscustomobject.

Вы можете указывать сразу два атрибута таким образом:

```powershell
[parameter(ValueFromPipelineByPropertyName,ValueFromPipeline)]
```

Это позволит использовать и значение с именем, если оно указано либо без него. Это не спасет вас от ситуации, если вы передаете параметр с другим именем:

[![функции windows powershell](https://fixmypc.ru/media/uploads/2019/10/12/41.jpg "функции windows powershell")](https://fixmypc.ru/media/uploads/2019/10/12/41.jpg)

### Передача через конвейер нескольких значений

Для примера рассмотрим ситуацию, где нам нужно передать через конвейер два значения. Если Get-SomeNum будет возвращать массив, то через конвейер у нас будет проходить каждое число по отдельности. Это еще один повод использовать именованные параметры:

```powershell
function Get-SomeNum { 
    $number1 = Get-Random -Minimum 5 -Maximum 10
    $number2 = Get-Random -Minimum 1 -Maximum 5
    $object = [pscustomobject]@{num1=$number1;num2=$number2}
    return $object
}

function Plus-SomeNum {
    [cmdletbinding()]
    Param (
            [parameter(ValueFromPipelineByPropertyName=$true,
                        ValueFromPipeline=$true,
                        Mandatory=$true)]
            [int]
            $num1,
            [parameter(ValueFromPipelineByPropertyName=$true,
                        ValueFromPipeline=$true,
                        Mandatory=$true)]
            [int]
            $num2
        )
    begin {$num1 += $num1
           $num2 = $num2 * $num2}
    process {  
    return @{"Результат сложения"=$num1; "Результат умножения"=$num2}
    }
}

Get-SomeNum | Plus-SomeNum
```

[![powershell вызов функции](https://fixmypc.ru/media/uploads/2019/10/12/42.jpg "powershell вызов функции")](https://fixmypc.ru/media/uploads/2019/10/12/42.jpg)

## Комментарии, описание и synopsis 

При вызове справки на любой командлет мы получим такую информацию:

[![](https://fixmypc.ru/media/uploads/2019/10/12/43.jpg)](https://fixmypc.ru/media/uploads/2019/10/12/43.jpg)

Описание функции, так же как и ее именование относится к рекомендованным действиям. Что бы это сделать нужно после объявления функции заполнить соответствующий блок. Я заполнил этот блок для одного из примеров:

```powershell
function Get-SomeNum { 
  <
  .SYNOPSIS
  (короткое описание) Получение случайного числа
  .DESCRIPTION
  (полное описание) Получение случайного числа от 1 до 3
  .EXAMPLE
  (пример) Get-Random
  .EXAMPLE
  (второй)
  .PARAMETER num
  (описание параметра) Параметр ни на что не влияет
  .PARAMETER num2
  (описание второго)
  
    [CmdletBinding()]
    param (
           [int]
           $num
    )
    $num = Get-Random -min 1 -Max 3
    return $num
}

Get-SomeNum
```

[![powershell параметры функции](https://fixmypc.ru/media/uploads/2019/10/12/44.jpg "powershell параметры функции")](https://fixmypc.ru/media/uploads/2019/10/12/44.jpg)

Некоторые виды описаний, например Examples, можно использовать несколько раз.

## Сохранение, загрузка и импорт

Скорее всего нашу функцию или готовый командлет мы захотим использовать и далее. В зависимости от ситуации мы должны сохранять и загружать его разными способами.

### Импорт на множество компьютеров

Если это командлет, который будет использоваться на множестве компьютеров или вы его планируете использовать короткое время, то скрипт можно сохранить в файл с расширением ".ps1". Загрузка такой функции будет выполняться так:

```powershell
Import-Module C:\funct.ps1 -Force
```

После выполнения этой команды мы сможем использовать нашу функцию.

Минус такого способа в том, что нужно будет делать каждый раз после закрытия консоли (сессии).

Такой подход хорошо подходит в удаленных сценариях, когда на компьютерах пользователей нужно сделать какую-то работу.

### Загрузка как модуля
Если вы планируете использовать функцию на своем компьютере, то вы можете загрузить эту команду как модуль. Вы можете использовать и на других компьютерах, но я считаю это плохим вариантом.
Первое что нужно сделать это получить пути окружения Powershell:
```powershell
$env:PSModulePath
```
Выберете один из путей, где лежат модули или перейдите по следующему:
```powershell
C:\Users\%username%\Documents\WindowsPowerShell\Modules
```
В указанной папке Modules вам нужно создать папку и файл с одинаковыми именами. Файлу добавляете расширение ".psm1" и помещаете в него свой скрипт.
В моём случае путь выглядит так:
```powershell
C:\Users\%username%\Documents\WindowsPowerShell\Modules\Test\Test.psm1
```
После этого закройте все окна Powershell и откройте заново. Модуль будет импортироваться автоматически. Проверти что он загружен можно с помощью команды:
```powershell
Get-Module -ListAvailable -Name "*Часть имени папки*"
```
...
_____________
#### Links
