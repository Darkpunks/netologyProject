__________________________________________________________________________
Домашнее задание к занятию "6.3. MySQL"
__________________________________________________________________________

Задача 1
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Ответ: 
```
version: '3.1'

services:

  mysql:
    image: mysql:8
    container_name: mysql
	hostname: mysql
    volumes:
      - ./mysql-conf:/etc/mysql/conf.d
      - ./mysql-data:/var/lib/mysql
      - ./mysql-backup:/backup
    
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123
```



Изучите бэкап БД и восстановитесь из него.


```
bash-4.4# mysql -uroot -p"123" test_db  < /backup/test_dump.sql
```

Перейдите в управляющую консоль mysql внутри контейнера.
```
bash-4.4# mysql -uroot -p"123"
```
Используя команду \h получите список управляющих команд.

Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.
```
mysql> \s
--------------
mysql  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:          12
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         8.0.30 MySQL Community Server - GPL
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    utf8mb4
Db     characterset:    utf8mb4
Client characterset:    latin1
Conn.  characterset:    latin1
UNIX socket:            /var/run/mysqld/mysqld.sock
Binary data as:         Hexadecimal
Uptime:                 24 min 55 sec

Threads: 2  Questions: 51  Slow queries: 0  Opens: 140  Flush tables: 3  Open tables: 58  Queries per second avg: 0.034
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(1).jpg">


Подключитесь к восстановленной БД и получите список таблиц из этой БД.
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)


mysql> USE test_db


mysql> SHOW TABLES;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(2).jpg">


Приведите в ответе количество записей с price > 300.




```
mysql> select * from orders where price > 300;
+----+----------------+-------+
| id | title          | price |
+----+----------------+-------+
|  2 | My little pony |   500 |
+----+----------------+-------+
1 row in set (0.00 sec)



mysql> select count(*) from orders where price > 300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)
```







В следующих заданиях мы будем продолжать работу с данным контейнером.

Задача 2
Создайте пользователя test в БД c паролем test-pass, используя:

плагин авторизации mysql_native_password
срок истечения пароля - 180 дней
количество попыток авторизации - 3
максимальное количество запросов в час - 100
аттрибуты пользователя:
Фамилия "Pretty"
Имя "James"

```
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY 'test-pass';
Query OK, 0 rows affected (0.01 sec)

mysql> ALTER USER 'test'@'localhost' ATTRIBUTE '{"fname":"James", "lname":"Pretty"}';
Query OK, 0 rows affected (0.00 sec)


mysql> ALTER USER 'test'@'localhost'
    -> IDENTIFIED BY 'test-pass'
    -> WITH
    -> MAX_QUERIES_PER_HOUR 100
    -> PASSWORD EXPIRE INTERVAL 180 DAY
    -> FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME 3;
Query OK, 0 rows affected (0.00 sec)
```


Предоставьте привелегии пользователю test на операции SELECT базы test_db.

```
mysql> GRANT Select ON test_db.orders TO 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.00 sec)

```

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.

```
mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
+------+-----------+---------------------------------------+
| USER | HOST      | ATTRIBUTE                             |
+------+-----------+---------------------------------------+
| test | localhost | {"fname": "James", "lname": "Pretty"} |
+------+-----------+---------------------------------------+
1 row in set (0.00 sec)
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(3).jpg">

Задача 3
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.

Какой запрос сколько занял времени. 



Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.

```
mysql> SELECT TABLE_NAME,ENGINE,ROW_FORMAT,TABLE_ROWS,DATA_LENGTH,INDEX_LENGTH FROM information_schema.TABLES WHERE table_name = 'orders' and  TABLE_SCHEMA = 'test_db' ORDER BY ENGINE asc;
+------------+--------+------------+------------+-------------+--------------+
| TABLE_NAME | ENGINE | ROW_FORMAT | TABLE_ROWS | DATA_LENGTH | INDEX_LENGTH |
+------------+--------+------------+------------+-------------+--------------+
| orders     | InnoDB | Dynamic    |          5 |       16384 |            0 |
+------------+--------+------------+------------+-------------+--------------+
1 row in set (0.01 sec)
```




<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(4).jpg">


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(5).jpg">


Используеся ENGINE - InnoDB

Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:
```
на MyISAM
на InnoDB

mysql> ALTER TABLE orders ENGINE = MyISAM;
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> ALTER TABLE orders ENGINE = InnoDB;
Query OK, 5 rows affected (0.02 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> show profiles;
+----------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Query_ID | Duration   | Query                                                                                                                                                                                |
+----------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|        1 | 0.00033950 | select count(*) from orders where price > 300                                                                                                                                        |
|        2 | 0.00279775 | SELECT TABLE_NAME,ENGINE,ROW_FORMAT,TABLE_ROWS,DATA_LENGTH,INDEX_LENGTH FROM information_schema.TABLES WHERE table_name = 'orders' and  TABLE_SCHEMA = 'test_db' ORDER BY ENGINE asc |
|        3 | 0.01382000 | ALTER TABLE orders ENGINE = MyISAM                                                                                                                                                   |
|        4 | 0.01724700 | ALTER TABLE orders ENGINE = InnoDB                                                                                                                                                   |
+----------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
4 rows in set, 1 warning (0.00 sec)

```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(6).jpg">

Переключение на MyISAM: 0.0138 секунд
Переключение на InnoDB: 0.0172 секунд


Задача 4


Изучите файл my.cnf в директории /etc/mysql.

Изучил. 

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(8).jpg">

Измените его согласно ТЗ (движок InnoDB):

Скорость IO важнее сохранности данных
Нужна компрессия таблиц для экономии места на диске
Размер буффера с незакомиченными транзакциями 1 Мб
Буффер кеширования 30% от ОЗУ
Размер файла логов операций 100 Мб
Приведите в ответе измененный файл my.cnf.

```
[ivan@localhost mysql-compose]$ free -mh
              total        used        free      shared  buff/cache   available
Mem:           3.7G        1.2G        147M         12M        2.3G        2.2G
Swap:          3.0G          0B        3.0G
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.3/6%20(7).jpg">

30% от 3,7 гб - округляем до 1 гб 


Создал файл netology.cnf вместо my.cnf в директории /etc/mysql/conf.d
```
#Set IO Speed
# 0 - скорость IO
# 1 - важнее сохранности данных
# 2 - универсальный параметр
innodb_flush_log_at_trx_commit = 0 

#Set compression
# Barracuda - компрессия таблиц для экономии места 
innodb_file_format=Barracuda

#Set buffer
innodb_log_buffer_size	= 1M

#Set Cache size
key_buffer_size = 1G

#Set log size
max_binlog_size	= 100M
```



