# Парсер статистики ДТП с официального сайта ГИБДД stat.gibdd.ru

Инструмент для иследователей, который позволяет скачивать актуальные подробные данные о ДТП с официального [сайта](http://stat.gibdd.ru/) статистики ДТП ГИБДД. 

Парсер работает из командной строки. Данные загружаются в формате `json`, в разбивке `один файл = один регион`. Папка с данными (по умолчанию `dtpdata`) создается рядом со скриптом. 

Парсер поддерживает докачку (т.к. не всегда удается скачать такой объем данных с первого раза), поэтому при повторном запуске пропускает уже скачанные регионы. Парсер пишет лог-файл с названием `parselog.log` со всей статистикой. По умолчанию данные дописываются и не обрезаются. Если нужен "чистый" лог, необходимо удалить этот файл.

Также всю статистику ДТП можно взять [здесь](https://github.com/Shorstko/GibddStat/tree/master/data). Guthub не поддерживает файлы размером больше 25 Мб, поэтому выгрузки за 2015-2017 пришлось разделить на два архива. В архивах с названием *\*part2* находится файл лога (по нему можно посмотреть, для каких муниципальных образований (районы города и пр.) на карте ДТП ГИБДД отсутствуют данные).
![](https://github.com/Shorstko/GibddStat/blob/master/image/title.png)

## Запуск парсера

Работает из командной строки. Базовый синтаксис: 
>   GibddStatParser.py [--year] [--month] [--regcode] [--dir] [--updatecodes] [--help]

`--year` год, за который скачивается статистика. если год не указан, скачивается статистика за текущий год

`--month` месяц, за который скачивается статистика. если не указан, скачивается статистика за все прошедшие месяцы года

`--regcode` ОКАТО-код региона. номера кодов регионов можно посмотреть в справочнике кодов регионов [regions.json](https://github.com/Shorstko/GibddStat/blob/master/regions.json) (параметр `id`)

`--dir` папка, в которую скачивается статистика. по умолчанию `dtpdata`

`--updatecodes` обновить ОКАТО-коды регионов с сайта stat.gibdd.ru. по умолчанию обновление отключено (`--updatecodes n`)

**Примеры вызова парсера**

`GibddStatParser.py --year 2018` : скачать все ДТП за 2018 год. для текущего года скачиваются все месяцы минус текущий. для прошедших лет скачиваются 12 месяцев

`GibddStatParser.py --year 2018 --month 4` : скачать все ДТП за апрель 2018 года

`GibddStatParser.py --year 2018 --regcode 45` : скачать все ДТП по Москве за 2018 год. 45 - ОКАТО-код г.Москва

`GibddStatParser.py --year 2018 --updatecodes y` : скачать все ДТП за 2018 год и обновить справочник кодов регионов

## Проверка результатов

Для этого служит скрипт *read_dtp_data.py*. Синтаксис скрипта:
>  	read_dtp_data.py [--filename]

`--filename` имя файла в формате .json, содержащего карточки ДТП по какому-либо региону. необходимо указывать полный путь к файлу

**Пример**

`read_dtp_data.py --filename "c:\45 г. Москва 1-4.2018.json"` : просмотреть статистику ДТП по г.Москве за январь-апрель 2018 года (результат на картинке)

![просмотреть статистику ДТП по г.Москве за январь-апрель 2018 года](https://github.com/Shorstko/GibddStat/blob/master/image/test.png)

## Вызов парсера из командной строки Windows

Для непрограммистов будет достаточно установить [Python 3.6.5](https://www.python.org/downloads/release/python-365/) и вызывать скрипт из командной строки Windows. Например:

>   c:\PythonVenv\vePython36\Scripts\python.exe c:\GibddStatParser.py --year 2018

## Технические параметры

Код парсера написан на Python 3.6.5.

Зависимости: `requests`, `json`, `datetime`, `codecs`, `os`, `re`, `sys`, `argparse`.
