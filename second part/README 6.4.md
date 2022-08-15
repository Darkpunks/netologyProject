__________________________________________________________________________
Домашнее задание к занятию "PostgreSQL"
__________________________________________________________________________

Задача 1
__________________________________________________________________________
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.
```
version: '3.1'

services:

  postgres:
    image: postgres:13
    container_name: postgres
    hostname: postgres
    volumes:
      - ./pgdata:/var/lib/postgresql/data
      - ./backup:/backup
    restart: always
    environment:
      POSTGRES_PASSWORD: 123

docker-compose up -d
docker exec -ti postgres bash
```

Подключитесь к БД PostgreSQL используя psql.
```
psql -U postgres
```
Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.


сделал.

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.4/6.4.1.jpg">

Найдите и приведите управляющие команды для:

вывода списка БД
```
\l[+]   [PATTERN]      list databases
```
подключения к БД
```
\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}
```
вывода списка таблиц
```
\dt[S+] [PATTERN]      list tables
```
вывода описания содержимого таблиц
```
\d[S+]                 list tables, views, and sequences
```

выхода из psql
```
\q                     quit psql
```
__________________________________________________________________________
Задача 2
__________________________________________________________________________

Используя psql создайте БД test_database.
```
postgres=# CREATE DATABASE test_database;
```
Изучите бэкап БД.
 
Изучил

Восстановите бэкап БД в test_database.
```
root@postgres:/# psql -U postgres -f /backup/test_dump.sql test_database
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.4/6.4.2-1.jpg">

Перейдите в управляющую консоль psql внутри контейнера.
```
root@postgres:/# psql -U postgres
psql (13.8 (Debian 13.8-1.pgdg110+1))
```
Подключитесь к восстановленной БД
```
postgres=# \c test_database
You are now connected to database "test_database" as user "postgres".
```
проведите операцию ANALYZE для сбора статистики по таблице.
```
test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
```
Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.
```
test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width
-----------
         4
        16
         4
(3 rows)
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.4/6.4.2-2.jpg">

__________________________________________________________________________
Задача 3
__________________________________________________________________________
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

ОТВЕТ: 

Избежать  "ручное"  разбиение при проектировании таблицы orders можно, но необходимо было определить тип на моменте проектирования и создания - partitioned table
```
begin;
    create table orders_part (
        id integer NOT NULL,
        title varchar(80) NOT NULL,
        price integer) partition by range(price);
    create table orders_part1 partition of orders_part for values from (0) to (499);
    create table orders_part2 partition of orders_part for values from (499) to (99999);
    insert into orders_part (id, title, price) select * from orders;
commit;
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.4/6.4.3.jpg">
__________________________________________________________________________
Задача 4
__________________________________________________________________________
Используя утилиту pg_dump создайте бекап БД test_database.
```
root@postgres:/# pg_dump -U postgres -d test_database >/backup/test_database_dump.sql
```
Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?
Для определения значения столбца title для таблиц test_database можно было бы использовать индекс, для обеспечения уникальности.
Например,
```
CREATE INDEX ON orders ((lower(title)));
```
