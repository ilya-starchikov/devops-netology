# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя `psql`.

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД
- подключения к БД
- вывода списка таблиц
- вывода описания содержимого таблиц
- выхода из psql

## Ответ

```commandline
version: "3.1"

volumes:
  pg_data: {}
  pg_backup: {}

services:
  postgesql:
    image: postgres:13
    container_name: postgresql
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - pg_data:/var/lib/postgresql/data/
      - pg_backup:/var/backups/pg_backup
    restart: always
```
**Найдите и приведите** управляющие команды для:
- вывода списка БД
```commandline
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)
```
- подключения к БД
```commandline
\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}
                         connect to new database (currently "postgres")
```
- вывода списка таблиц
```commandline
\dt[S+] [PATTERN]      list tables
```
- вывода описания содержимого таблиц
```commandline
 \d[S+]                 list tables, views, and sequences
```
- выхода из psql
```commandline
\q                     quit psql
```

## Задача 2

Используя `psql` создайте БД `test_database`.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

Перейдите в управляющую консоль `psql` внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.

## Ответ

Используя `psql` создайте БД `test_database`.
```commandline
postgres=# CREATE DATABASE test_database;
CREATE DATABASE
```
Восстановите бэкап БД в `test_database`.
```commandline
psql -U postgres test_database < /var/backups/pg_backup/test_dump.sql
```
Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
```commandline
postgres=# \c test_database
You are now connected to database "test_database" as user "postgres".
test_database=#
test_database=# ANALYZE;
ANALYZE
test_database=#
```
Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.
```commandline
test_database=# SELECT attname, avg_width FROM pg_stats WHERE tablename = 'orders' ORDER BY attname DESC LIMIT 1;
 attname | avg_width
---------+-----------
 title   |        16
(1 row)
```

## Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

## Ответ

```commandline
BEGIN;
# установить изоляцию транзакций
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
# переименовать таблицу orders
ALTER TABLE orders RENAME TO orders_copy;
# создать таблицу orders
CREATE TABLE orders (
    id integer NOT NULL,
    title character varying(80) NOT NULL,
    price integer DEFAULT 0)
	PARTITION BY RANGE (price);
# создать таблицу 1
CREATE TABLE orders_1 PARTITION OF orders
    FOR VALUES FROM (500) TO (2147483647);
# создать таблицу 2
CREATE TABLE orders_2 PARTITION OF orders
    FOR VALUES FROM (0) TO (500);
# перенести из переименованной в orders
INSERT INTO orders (id, title, price) SELECT * FROM orders_copy;
# сменить владельца последовательности значений id
ALTER SEQUENCE orders_id_seq OWNED BY public.orders.id;
# Возобновить последовательность значений для id
ALTER TABLE ONLY orders ALTER COLUMN id SET DEFAULT nextval('public.orders_id_seq'::regclass);
# Удалить таблицу copy
DROP TABLE orders_copy;
COMMIT;

# Результат выполнения
test_database=# insert into orders (title, price) values ('orders 1', 300), ('orders 2', 600);
INSERT 0 2

test_database=# select * FROM orders;
  1 | War and peace        |   100
  3 | Adventure psql time  |   300
  4 | Server gravity falls |   300
  5 | Log gossips          |   123
  7 | Me and my bash-pet   |   499
  9 | test less 499        |   300
  2 | My little database   |   500
  6 | WAL never lies       |   900
  8 | Dbiezdmin            |   501
 10 | test more 500        |   600

test_database=# select * FROM orders_2;
  1 | War and peace        |   100
  3 | Adventure psql time  |   300
  4 | Server gravity falls |   300
  5 | Log gossips          |   123
  7 | Me and my bash-pet   |   499
  9 | test less 499        |   300

test_database=# select * FROM orders_1;
  2 | My little database |   500
  6 | WAL never lies     |   900
  8 | Dbiezdmin          |   501
 10 | test more 500      |   600
```
Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?
```commandline
Очень странный вопрос, если существующую можно разбить то заранее создать шардированную нет проблем. 
Не будет запросов с переносом данных.
```

## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

## Ответ

```commandline
pg_dump -U postgres --file /var/backups/pg_backup/test_database_dump.sql test_database
```
Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

Добавив параметр UNIQUE в описание столбца таблицы (в случае шадрированной таблицы соответственно в описание всех 3-х таблиц)
```commandline
CREATE TABLE public.orders (
    id integer DEFAULT NULL NOT NULL,
    title character varying(80) NOT NULL UNIQUE,
    price integer DEFAULT 0
);
CREATE TABLE public.orders_2 (
    id integer DEFAULT NULL NOT NULL,
    title character varying(80) NOT NULL UNIQUE,
    price integer DEFAULT 0
);
CREATE TABLE public.orders_1 (
    id integer DEFAULT NULL NOT NULL,
    title character varying(80) NOT NULL UNIQUE,
    price integer DEFAULT 0
);
```