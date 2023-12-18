# Лабораторная работа №3

### Описание лабораторной работы

Задачей данной лабораторной работы является написание бенчмарка для измерения производительности при 4 запросах на 5 различных библиотеках на языке Python (psycopg2, SQLite, DuckDB, Pandas, SQLAlchemy).

### Среда разработки

Задача выполнялась на операционной системе MacOS Sonoma v.14.1.1.  Данный код можно запускать на unix/linux подобных операционных системах системах. На платформе Windows возможны ошибки.

Весь код был написан на языке Python с использованием Pycharm и dBeaver. Тесты проводились на платформе Apple Macbook Pro M1 Pro (16 gb / 512 gb).

Из-за ограничений GitHub на размеры загружаемых файлов дополнительные файлы были загружены на [Яндекс.Диск](https://disk.yandex.ru/client/disk/Database%20Course%20-%20Lab%203).

Для корректной работы кода нужно установить все необходимые библиотеки указанные в [requirements.txt](https://github.com/atayev2012/database_course_lab_3/blob/main/requirements.txt).

---
### Алгоритм работы

1. В файле settings.conf указываем нужные настройки:

```python
# csv file with data
csv= data/raw/nyc_yellow_big.csv

# postgres database connection info
POSTGRESQL_ENABLED= True
NAME= название баззы данных
HOST= хост
PORT= порт
USER= имя пользователя
PASSWORD= пароль

# Choose libraries to test
psycopg2= True
SQLite= True
DuckDB= True
Pandas= True
SQLAlchemy= True

# Choose test setting
# Printing out results will affect the execution speed
test_qty= 25
print_results= False
```
* csv файлы должны находится в каталоге /data/raw

2. Запускаем main.py:

* Автоматически данные с csv файла копируются в базу данных PostgreSQL и создается файл SQLite в каталоге /data/converted/.
* Далее запускаются тесты, генерируется график и текстовый файл со статистическими данными.
---
### Результаты тестов

Тесты запускались на двух csv файлах которые хранятся на диске ссылка на который была указана выше.

Представлены медианные значения по 25 запускам.

   - **Для файла nyc_yellow_tiny.csv**

     <img width="500" alt="nyc_yellow_tiny.png" src="https://github.com/atayev2012/database_course_lab_3/blob/main/output/img/nyc_yellow_tiny.png?raw=true">
   
   - **Для файла nyc_yellow_big.csv**

     <img width="500" alt="nyc_yellow_big.png" src="https://github.com/atayev2012/database_course_lab_3/blob/main/output/img/nyc_yellow_big.png?raw=true">
---
### Выводы
   - По графикам заметно что библиотека Pandas работает бытрее всего, но у неё есть свои минусы. Эта библиотека загружает всю базу данных в оперативную память, это длительный процесс и он не учитывался во время тестов.
   - Так же можно заметить что самой медленной библиотекой является SQLite. 
   - При простых запросах и маленьком количестве данных для обработки библиотеки psycopg2 и sqlalchemy показывают неплохие результаты. Но с увеличением сложности запросов и количества обрабатываемых данных библиотека DuckDB себя показала гораздо лучше.
   - При запусках тестов на большом количестве данных (с большим фалом данных) при усложнении условий запросы скорость библиотеки DuckDB почти не меняется. 

Получается что если есть доступен приличный объем памяти, то выгодней будет пользоваться Pandas. Но так как базы данных могут быть очень большими, приходится выбирать более оптимальный вариант. Для огромного количества данных логично быдет использовать psycopg2, так как она показала себя лучше всех, но если данных не так много и с памятью так же проблемы, то выгодно будетпоработать с DuckDB.