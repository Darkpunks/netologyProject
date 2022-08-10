__________________________________________________________________________
Домашнее задание к занятию "6.2. SQL"
__________________________________________________________________________

Задача 1
__________________________________________________________________________
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose манифест.

ОТВЕТ: 
```
docker run -dt --name postgres1 -v /ivanpostgre/pgdata1:/var/lib/postgresql/data -v /ivanpostgre/backup1:/backup -e POSTGRES_PASSWORD=123 postgres:12
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.1.1.jpg">


__________________________________________________________________________
Задача 2
__________________________________________________________________________
В БД из задачи 1:

создайте пользователя test-admin-user и БД test_db
в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
создайте пользователя test-simple-user
предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db
Таблица orders:

id (serial primary key)
наименование (string)
цена (integer)
Таблица clients:

id (serial primary key)
фамилия (string)
страна проживания (string, index)
заказ (foreign key orders)
Приведите:


итоговый список БД после выполнения пунктов выше,

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.2.1.jpg">

описание таблиц (describe)

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.2.2.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.2.3.jpg">



SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.2.4.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5..2%20(2).jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5..2%20(1).jpg">

список пользователей с правами над таблицами test_db

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.2.5.jpg">


__________________________________________________________________________
Задача 3
__________________________________________________________________________
Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

Наименование	цена
Шоколад	40
Принтер	3000
Книга	500
Монитор	7000
Гитара	4000

Таблица clients

ФИО	Страна проживания
Иванов Иван Иванович	USA
Петров Петр Петрович	Canada
Иоганн Себастьян Бах	Japan
Ронни Джеймс Дио	Russia
Ritchie Blackmore	Russia
Используя SQL синтаксис:

вычислите количество записей для каждой таблицы
приведите в ответе:

запросы

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.3.1%20(2).jpg">

результаты их выполнения.

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.3.2.jpg">



__________________________________________________________________________
Задача 4
__________________________________________________________________________
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

ФИО	Заказ
Иванов Иван Иванович	Книга
Петров Петр Петрович	Монитор
Иоганн Себастьян Бах	Гитара
Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.

Подсказка - используйте директиву UPDATE.

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.3.3.jpg">

```
test_db=> UPDATE clients set  "заказ" = 3 where "фамилия" = 'Иванов Иван Иванович';
UPDATE clients set  "заказ" = 4 where "фамилия" = 'Петров Петр Петрович';
UPDATE clients set  "заказ" = 5 where "фамилия" = 'Иоганн Себастьян Бах';
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.4.1.jpg">


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.4.2.jpg">


__________________________________________________________________________
Задача 5
__________________________________________________________________________
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

ОТВЕТ:
```
EXPLAIN select * from clients where "заказ" is not null;
                         QUERY PLAN
------------------------------------------------------------
 Seq Scan on clients  (cost=0.00..13.00 rows=298 width=244)
   Filter: ("заказ" IS NOT NULL)
(2 rows)
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.5.1.jpg">


План запроса. Последовательное сканирование идет таблицы клиента с фильтром, где заказ не равен NULL.


__________________________________________________________________________
Задача 6
__________________________________________________________________________
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

ОТВЕТ:

```

pg_dumpall -U postgres > /backup/pg1.bak

[root@localhost ivanpostgre]# docker stop 85a7ebf8caa0
85a7ebf8caa0

[root@localhost ivanpostgre]# docker run -dt --name postgres2 -v /ivanpostgre/pgdata2:/var/lib/postgresql/data -v /ivanpostgre/backup1:/backup -e POSTGRES_PASSWORD=123 postgres:12
9038d2685a13f153cc20b386b1270899efc80a81d39233145026f8fc23eadd96

[root@localhost ivanpostgre]# docker exec -ti 9038d26 bash
root@9038d2685a13:/# psql -U postgre

root@9038d2685a13:/# psql -U postgres -f /backup/pg1.bak
```


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.6.1..jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.6.2.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.6.3.jpg">

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.2/5.6.4.jpg">
