# Домашнее задание к занятию "6.2. SQL"

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.

## Ответ
```commandline
version: "2.4"

volumes:
  pg_data: {}
  pg_backup: {}

services:
  postgesql:
    image: postgres:12
    container_name: postgresql
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - pg_data:/var/lib/postgresql/data/
      - pg_backup:/var/backups/pg_backup
    restart: always
```

## Задача 2

В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
- создайте пользователя test-simple-user  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,
- описание таблиц (describe)
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
- список пользователей с правами над таблицами test_db

## Ответ
```commandline
postgres=# CREATE USER "test-admin-user"; CREATE DATABASE test_db;
CREATE ROLE
CREATE DATABASE


CREATE TABLE orders
(
id SERIAL PRIMARY KEY,
name VARCHAR(20),
price INT
);

test_db=# CREATE TABLE clients
(
  id SERIAL PRIMARY KEY,
  lastname VARCHAR(20),
  country VARCHAR(20),
  booking INT,
  FOREIGN KEY (booking) REFERENCES orders (id)
);

create country_idx on clients (country);

postgres=# GRANT ALL ON orders, clients TO "test-admin-user";
postgres=# CREATE USER "test-simple-user";
CREATE ROLE

postgres=# GRANT SELECT, INSERT, UPDATE, DELETE ON orders, clients TO "test-simple-user";
GRANT
```
приведите итоговый список БД после выполнения пунктов выше
```commandline
postgres=# \l
                                     List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |       Access privileges
-----------+----------+----------+------------+------------+--------------------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres                   +
           |          |          |            |            | postgres=CTc/postgres
 test_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =Tc/postgres                  +
           |          |          |            |            | postgres=CTc/postgres         +
           |          |          |            |            | "test-admin-user"=CTc/postgres+
           |          |          |            |            | "test-simple-user"=c/postgres
(4 rows)
```
описание таблиц (describe)
```commandline
postgres=# \d clients
                                    Table "public.clients"
  Column  |         Type          | Collation | Nullable |               Default
----------+-----------------------+-----------+----------+-------------------------------------
 id       | integer               |           | not null | nextval('clients_id_seq'::regclass)
 lastname | character varying(20) |           |          |
 country  | character varying(20) |           |          |
 booking  | integer               |           |          |
Indexes:
    "clients_pkey" PRIMARY KEY, btree (id)
    "country_idx" btree (country)
Foreign-key constraints:
    "clients_booking_fkey" FOREIGN KEY (booking) REFERENCES orders(id)
    
postgres=# \d orders
                                   Table "public.orders"
 Column |         Type          | Collation | Nullable |              Default
--------+-----------------------+-----------+----------+------------------------------------
 id     | integer               |           | not null | nextval('orders_id_seq'::regclass)
 name   | character varying(20) |           |          |
 price  | integer               |           |          |
Indexes:
    "orders_pkey" PRIMARY KEY, btree (id)
Referenced by:
    TABLE "clients" CONSTRAINT "clients_booking_fkey" FOREIGN KEY (booking) REFERENCES orders(id)    
```
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
```commandline
# SELECT * FROM information_schema.table_privileges WHERE table_name = 'clients' OR table_name = 'orders';
```
список пользователей с правами над таблицами test_db
```commandline
 grantor  |     grantee      | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy
----------+------------------+---------------+--------------+------------+----------------+--------------+----------------
 postgres | postgres         | test_db       | public       | orders     | INSERT         | YES          | NO
 postgres | postgres         | test_db       | public       | orders     | SELECT         | YES          | YES
 postgres | postgres         | test_db       | public       | orders     | UPDATE         | YES          | NO
 postgres | postgres         | test_db       | public       | orders     | DELETE         | YES          | NO
 postgres | postgres         | test_db       | public       | orders     | TRUNCATE       | YES          | NO
 postgres | postgres         | test_db       | public       | orders     | REFERENCES     | YES          | NO
 postgres | postgres         | test_db       | public       | orders     | TRIGGER        | YES          | NO
 postgres | test-simple-user | test_db       | public       | orders     | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | orders     | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | INSERT         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | SELECT         | NO           | YES
 postgres | test-admin-user  | test_db       | public       | orders     | UPDATE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | TRUNCATE       | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | REFERENCES     | NO           | NO
 postgres | test-admin-user  | test_db       | public       | orders     | TRIGGER        | NO           | NO
 postgres | postgres         | test_db       | public       | clients    | INSERT         | YES          | NO
 postgres | postgres         | test_db       | public       | clients    | SELECT         | YES          | YES
 postgres | postgres         | test_db       | public       | clients    | UPDATE         | YES          | NO
 postgres | postgres         | test_db       | public       | clients    | DELETE         | YES          | NO
 postgres | postgres         | test_db       | public       | clients    | TRUNCATE       | YES          | NO
 postgres | postgres         | test_db       | public       | clients    | REFERENCES     | YES          | NO
 postgres | postgres         | test_db       | public       | clients    | TRIGGER        | YES          | NO
 postgres | test-simple-user | test_db       | public       | clients    | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | clients    | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | INSERT         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | SELECT         | NO           | YES
 postgres | test-admin-user  | test_db       | public       | clients    | UPDATE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | DELETE         | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | TRUNCATE       | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | REFERENCES     | NO           | NO
 postgres | test-admin-user  | test_db       | public       | clients    | TRIGGER        | NO           | NO
(36 rows)
```

## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL синтаксис:
- вычислите количество записей для каждой таблицы 
- приведите в ответе:
    - запросы 
    - результаты их выполнения.


## Ответ

```commandline
postgres=# \connect test_db

test_db=# INSERT INTO orders (name, price) VALUES
('Шоколад', 10),
('Принтер', 3000),
('Книга', 500),
('Монитор', 7000),
('Гитара', 4000);
INSERT 0 5

test_db=# INSERT INTO clients (lastname, country) VALUES
('Иванов Иван Иванович', 'USA'),
('Петров Петр Петрович', 'Canada'),
('Иоганн Себастьян Бах', 'Japan'),
('Ронни Джеймс Дио', 'Russia'),
('Ritchie Blackmore', 'Russia');
INSERT 0 5

test_db=# SELECT COUNT(*) FROM orders;
 count
-------
     5
(1 row)

test_db=# SELECT COUNT(*) FROM clients;
 count
-------
     5
(1 row)
```

## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
Подсказка - используйте директиву `UPDATE`.

## Ответ

```commandline
UPDATE clients SET "booking" = 3 WHERE lastname = 'Иванов Иван Иванович';
UPDATE clients SET "booking" = 4 WHERE lastname = 'Петров Петр Петрович';
UPDATE clients SET "booking" = 5 WHERE lastname = 'Иоганн Себастьян Бах';

SELECT * FROM clients WHERE booking IS NOT NULL;
 id |       lastname       | country | booking
----+----------------------+---------+---------
  1 | Иванов Иван Иванович | USA     |       3
  2 | Петров Петр Петрович | Canada  |       4
  3 | Иоганн Себастьян Бах | Japan   |       5
(3 rows)
```

## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

## Ответ

```commandline
EXPLAIN SELECT * FROM clients WHERE booking IS NOT NULL;
                         QUERY PLAN
------------------------------------------------------------
 Seq Scan on clients  (cost=0.00..15.30 rows=527 width=124)
   Filter: (booking IS NOT NULL)
(2 rows)
```
EXPLAIN — показать план выполнения оператора
cost - стоимость выполнения оператора, которая показывает, сколько, по мнению планировщика, будет выполняться этот оператор
        Фактически выводятся два числа: стоимость запуска до выдачи первой строки и общая стоимость выдачи всех строк.

rows и width - число строк и ширина каждой строки. 

## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

## Ответ

```commandline
docker exec postgresql pg_basebackup -h localhost -U postgres -p 5432 -w -D /var/backups/pg_backup/datebase.backup -Ft

docker-compose down

root@vagrant:/devops-netology/src# rm -R /var/lib/docker/volumes/src_pg_data/_data/*
root@vagrant:/devops-netology/src# cd /var/lib/docker/volumes/src_pg_backup/_data/datebase.backup
root@vagrant:/var/lib/docker/volumes/src_pg_backup/_data/datebase.backup# cp base.tar ../../../src_pg_data/_data/
root@vagrant:/var/lib/docker/volumes/src_pg_backup/_data/datebase.backup# cp pg_wal.tar ../../../src_pg_data/_data/
root@vagrant:/var/lib/docker/volumes/src_pg_backup/_data/datebase.backup# cd ../../../src_pg_data/_data/
root@vagrant:/var/lib/docker/volumes/src_pg_data/_data# touch recovery.signal
root@vagrant:/var/lib/docker/volumes/src_pg_data/_data# tar -xf base.tar
root@vagrant:/var/lib/docker/volumes/src_pg_data/_data# tar -xf pg_wal.tar
root@vagrant:/var/lib/docker/volumes/src_pg_data/_data#  echo restore_command = \'cp /var/lib/postgresql/data/%f %p\' >> postgresql.conf
root@vagrant:/var/lib/docker/volumes/src_pg_data/_data# cd /devops-netology/src/
root@vagrant:/devops-netology/src# docker-compose up -d
[+] Running 1/1
 ⠿ Container postgresql  Started    
vagrant@vagrant:/devops-netology/src$ docker exec -it postgresql psql -U postgres

postgres=# \dl
      Large objects
 ID | Owner | Description
----+-------+-------------
(0 rows)

postgres=# \d
               List of relations
 Schema |      Name      |   Type   |  Owner
--------+----------------+----------+----------
 public | clients        | table    | postgres
 public | clients_id_seq | sequence | postgres
 public | orders         | table    | postgres
 public | orders_id_seq  | sequence | postgres
(4 rows)

postgres=# \dt
          List of relations
 Schema |  Name   | Type  |  Owner
--------+---------+-------+----------
 public | clients | table | postgres
 public | orders  | table | postgres
(2 rows)

postgres=# \d clients
                                       Table "public.clients"
      Column       |       Type        | Collation | Nullable |               Default
-------------------+-------------------+-----------+----------+-------------------------------------
 id                | integer           |           | not null | nextval('clients_id_seq'::regclass)
 фамилия           | character varying |           |          |
 страна проживания | character varying |           |          |
 заказ             | integer           |           |          |
Indexes:
    "clients_pkey" PRIMARY KEY, btree (id)
Foreign-key constraints:
    "clients_заказ_fkey" FOREIGN KEY ("заказ") REFERENCES orders(id)

postgres=# \d orders
                                    Table "public.orders"
    Column    |       Type        | Collation | Nullable |              Default
--------------+-------------------+-----------+----------+------------------------------------
 id           | integer           |           | not null | nextval('orders_id_seq'::regclass)
 наименование | character varying |           |          |
 цена         | integer           |           |          |
Indexes:
    "orders_pkey" PRIMARY KEY, btree (id)
Referenced by:
    TABLE "clients" CONSTRAINT "clients_заказ_fkey" FOREIGN KEY ("заказ") REFERENCES orders(id)

postgres=#
```