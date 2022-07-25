__________________________________________________________________________
 3.4. Операционные системы, лекция 2"
__________________________________________________________________________
1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
поместите его в автозагрузку,
предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

ОТВЕТ:
Запустилось. 

HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.

TYPE go_gc_duration_seconds summary

go_gc_duration_seconds{quantile="0"} 0

go_gc_duration_seconds{quantile="0.25"} 0

go_gc_duration_seconds{quantile="0.5"} 0

go_gc_duration_seconds{quantile="0.75"} 0

go_gc_duration_seconds{quantile="1"} 0

go_gc_duration_seconds_sum 0

go_gc_duration_seconds_count 0

HELP go_goroutines Number of goroutines that currently exist.

TYPE go_goroutines gauge

go_goroutines 8

HELP go_info Information about the Go environment.

TYPE go_info gauge


и т.д.

Провериили запуски остановку процесса

[ivan@localhost node_exporter-1.3.1.linux-amd64]$ vi /etc/systemd/system/node_exporter.service
 
и задали данные для unit файла:

[Unit]
Description=Node Exporter
 
[Service]
ExecStart=/var/lib/node_exporter/node_exporter
EnvironmentFile=/etc/default/node_exporter
 
[Install]
WantedBy=default.target

создаем файл: 
[ivan@localhost node_exporter-1.3.1.linux-amd64]$ sudo vi /etc/systemd/system/node_exporter.service


запускаем: 
sudo systemctl start node_exporter

добавляем в автозагрузку: 

[ivan@localhost node_exporter-1.3.1.linux-amd64]$ sudo systemctl enable node_exporter


узнаем статус: 
[ivan@localhost node_exporter-1.3.1.linux-amd64]$ sudo systemctl status node_exporter

● node_exporter.service - Node Exporter
   Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-07-20 22:10:41 MSK; 17s ago
 Main PID: 30749 (node_exporter)
   CGroup: /system.slice/node_exporter.service
           └─30749 /var/lib/node_exporter/node_exporter

Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...zone
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...time
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...imex
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...eues
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...name
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...stat
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...=xfs
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:115 lev...=zfs
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=node_exporter.go:199 lev...9100
Jul 20 22:10:41 localhost.localdomain node_exporter[30749]: ts=2022-07-20T19:10:41.821Z caller=tls_config.go:195 level=...alse
Hint: Some lines were ellipsized, use -l to show in full.


2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

ОТВЕТ:
Отключили собственные метрики node exporter и включили статусы systemd сервисы 
идем во 2 файл и меняем его: sudo vi /etc/default/node_exporter

OPTIONS=--web.disable-exporter-metrics --collector.systemd

и меняем unit файл

[Unit]
Description=Node Exporter
 
[Service]
ExecStart=/var/lib/node_exporter/node_exporter $OPTIONS
EnvironmentFile=/etc/default/node_exporter
 
[Install]
WantedBy=default.target

перечитываем сервис файл sudo systemctl daemon-reload

[ivan@localhost node_exporter-1.3.1.linux-amd64]$ sudo systemctl restart node_exporter
[ivan@localhost node_exporter-1.3.1.linux-amd64]$ sudo systemctl status node_exporter
● node_exporter.service - Node Exporter
   Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-07-20 22:40:04 MSK; 3s ago
 Main PID: 41371 (node_exporter)
   CGroup: /system.slice/node_exporter.service
           └─41371 /var/lib/node_exporter/node_exporter --web.disable-exporter-metrics --collector.systemd



CPU, RAM, Disk включены по умолчанию:  
--collector.cpu  
--collector.diskstats
--collector.nvme 
--collector.meminfo 


3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

ОТВЕТ: Установил, заменил на 0.0.0.0, сделал проброс порта. Ответ на скриншоте по данным машины

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

ОТВЕТ:
sudo dmesg |grep virtualiz
[    0.000000] Booting paravirtualized kernel on VMware hypervisor
[    1.956774] systemd[1]: Detected virtualization vmware.


5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?

ОТВЕТ:
sudo sysctl -n fs.nr_open
1048576

Для пользователя задать больше этого числа нельзя. За исключение если поменять данные
Если задается кратное 1024, то получается =1024*1024. 

Если  хотим проверить максимуу :
cat /proc/sys/fs/file-max
9223372036854775807

Другой существующий лимит не позволит достичь такого числа:
ulimit -Sn
1024

6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

ОТВЕТ:
ps -e |grep sleep
   1910 pts/2    00:00:00 sleep
nsenter --target 1910 --pid --mount
ps
    PID TTY          TIME CMD
      2 pts/0    00:00:00 bash
     11 pts/0    00:00:00 ps

7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

ОТВЕТ:
Запустил.
таким образом это функция, которая параллельно пускает два своих экземпляра. Каждый пускает ещё по два и т.д. – получается словно «ветвление»
При отсутствии лимита на число процессов машина быстро исчерпывает физическую память и уходит в своп.

Нашел такой функционал:
[ 3099.973235] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope
[ 3103.171819] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-11.scope

Система опираясь на файлы предложенные выше, показывает что в пользовательской зоне ресурсов имеет определенное ограничение на новые ресурсы и как только происходит превышение – происходит блокировка ресурсов.

Изменить число процессов, которое можно создать в сессии:
ulimit -u 20 - число процессов будет ограниченно 20 для пользователя.
 
