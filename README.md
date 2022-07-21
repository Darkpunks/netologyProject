<<<<<<< HEAD
1.1 Ivan Prosvetov введение в DevOps
=======
# **netologyProject**
1.1. Введение в DevOps - Prosvetov Ivan
______________
[netology.tf](https://github.com/Darkpunks/netologyProject/blob/main/terraform.png) 

[netology.bh](https://github.com/Darkpunks/netologyProject/blob/main/bash.png)

[netology.md](https://github.com/Darkpunks/netologyProject/blob/main/markdown.png)

[netology.yaml](https://github.com/Darkpunks/netologyProject/blob/main/yaml.png)

[netology.jsonnet](https://github.com/Darkpunks/netologyProject/blob/main/jsonnet.png)

______________________________________________________________________________
2. Описание жизненного цикла задачи

2.1.Определение требований 

Описание задачи, её постановка/требования/особенности.


2.2.Спецификации 

Создание Технического задания для проекта.

2.3.Проектирование 

Создание первых прототипов (Бета-версия), использование разных вариантов и способов.

2.4.Реализация 

Выпуски стабильный версий, раскатка на рабочем железе, обкатывание системы. 

2.5.Тестирование 

Отслеживание багов, повышение стабильности работы. 

2.6.Сопровождение 

Обновление программы, повышение стабильности программы. 

2.7.Развитие 

Добавление новых функций и улучшение работоспособности, по требованию заказчика

____________________________________________________________________

3.1 Системы контроля версий

 3.1.1 В файле .gitignore будут проигнорированы следующие файлы:

 3.1.1.2. Во всех вложенных Директориях - */.terraform/ :

3.1.1.3. .tfstate - В этот файл записывается вся инфраструктура, которую мы создали;


3.1.1.4. crash.log - это файл журнала с журналами отладки из сеанса, в который при сбое сохраняет терраформ;


3.1.1.5. .tfvars -  конфиг с значением переменных, часто является секретным, так как содержит данные пользователей (пароли\логины);


3.1.1.6. override.tf, override.tf.json - позволяет переобределить метод базового класса ;


3.1.1.7. .terraformrc, terraform.rc - конфигурационные файлы интерфейса командной строки, которые мы игнорируем;

4. Инструменты Git

4.1.Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.

Ответ:
 
команда git show aefea

commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545

Update CHANGELOG.md

4.2.Какому тегу соответствует коммит 85024d3?

Ответ

sudo git show 85024

commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)

4.3. Сколько родителей у коммита b8d720? Напишите их хеши

Ответ

sudo git show --pretty=format:' %P' b8d720

 56cd7859e05c36c06b56d013b55a252d0bb7e1589ea88f22fc6269854151c571162c5bcf958bee2b

4.4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

Ответ

sudo git log  v0.12.23..v0.12.24

commit 33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24)

Author: tf-release-bot <terraform@hashicorp.com>

Date:   Thu Mar 19 15:04:05 2020 +0000

    v0.12.24

commit b14b74c4939dcab573326f4e3ee2a62e23e12f89

Author: Chris Griggs <cgriggs@hashicorp.com>

Date:   Tue Mar 10 08:59:20 2020 -0700

    [Website] vmc provider links

commit 3f235065b9347a758efadc92295b540ee0a5e26e

Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>

Date:   Thu Mar 19 10:39:31 2020 -0400

    Update CHANGELOG.md

commit 6ae64e247b332925b872447e9ce869657281c2bf

Если в одну линию

sudo git log v0.12.23..v0.12.24 –oneline

33ff1c03b (tag: v0.12.24) v0.12.24

b14b74c49 [Website] vmc provider links

3f235065b Update CHANGELOG.md

6ae64e247 registry: Fix panic when server is unreachable

5c619ca1b website: Remove links to the getting started guide's old location

06275647e Update CHANGELOG.md

d5f9411f5 command: Fix bug when using terraform login on Windows

4b6d06cc5 Update CHANGELOG.md

dd01a3507 Update CHANGELOG.md

225466bc3 Cleanup after v0.12.23 release

4.5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...)
 (вместо троеточего перечислены аргументы). 

Ответ

sudo git log -S'func providerSource'

commit 5af1e6234ab6da412fb8637393c5a17a1b293663

Author: Martin Atkins <mart@degeneration.co.uk>

Date:   Tue Apr 21 16:28:59 2020 -0700

commit 8c928e83589d90a031f811fae52a81be7153e82f

Author: Martin Atkins <mart@degeneration.co.uk>

Date:   Thu Apr 2 18:04:39 2020 -0700

Если в одну линию - проверяем

sudo git log -S'func providerSource'  --oneline

5af1e6234 main: Honor explicit provider_installation CLI config when present

8c928e835 main: Consult local directories as potential mirrors of providers

4.6. Найдите все коммиты в которых была изменена функция globalPluginDirs.

Ответ

sudo git log -L :'func globalPluginDirs':plugins.go

commit 78b12205587fe839f10d946ea3fdc06719decb05

Author: Pam Selle <204372+pselle@users.noreply.github.com>


commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46

Author: James Bardin <j.bardin@gmail.com>


В одну линию провряем


git log -L :'func globalPluginDirs':plugins.go --oneline 

коммит 

78b122055 Remove config.go and update things using its aliases

52dbf9483 keep .terraform.d/plugins for discovery

4.7. Кто автор функции synchronizedWriters?

Ответ

sudo git log -S'func synchronizedWriters' --pretty=format:'%h - %an %ae'

Если я правильно понимаю: 2 автора

bdfea50cc - James Bardin j.bardin@gmail.com

5ac311e2a - Martin Atkins mart@degeneration.co.uk

______________________________________________________________________________

РАБОТА В ТЕРМИНАЛЕ (лекция 1)
______________________________________________________________________________

1. Установите средство виртуализации Oracle VirtualBox. 

Установил. Не знаю, надо команду прописывать? sudo apt install virtualbox

2.   Установите средство автоматизации Hashicorp Vagrant.

Установил. sudo apt-get install vagrant

3. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. 
     
классический терминал в ubuntu 20.04

4. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant.

Сделал. Ознакомился.

5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

По умолчанию 

Процессора 2шт

Оперативной 1024 мб

Видео 4мб

Жесткий диск 64 гб

6.  Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
в VagrantFile надо добавить в зависимости от того сколько нам надо, например, увеличить в 2 раза:
  
config.vm.provider "virtualbox" do |vb|
    
vb.memory = "2048"
    
vb.cpu = "4"
  
end
  
7. Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.

Сделано

8. Ознакомиться с разделами man bash, почитать о настройках самого bash: какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?

8.1. HISTFILESIZE – сколько всего может сохраниться строк в файле истории, 

по команде получилось 1155

8.2. HISTSIZE – количество команд для сохранения, 

по команде получилось 1164
       
что делает директива ignoreboth в bash?

•ignoreboth – это сокращение от значений «ignorespace» и «ignoredups». 

• ignorespace – строки, начинающиеся с пробела, не будут сохранены в истории.

• ignoredups – строки, соответствующие предыдущей записи истории, не будут сохранены. Другими словами, дубликаты игнорируются.
  
9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

{} - зарезервированные слова.

Применяется, когда надо ограничивать тело функции, циклах, операторах.

В командах выполняет подстановку элементов из списка, если упрощенно то цикличное выполнение команд с подстановкой например mkdir ./DIRPIV_{1..1000} - создаст каталоги с именами DIRPIV_1, DIRPIV_2 и т.д. до DIRPIV_1000

строка в баш 343
  
10. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

touch {000001..100000}.txt – все создалось

При команде touch {000001..300000}.txt пишет Argument list too long

При попытке выявить максимум (getconf ARG_MAX). Выдал число 2097152

11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]
  
Конструкция проверяет сам -d /tmp и меняет ее статус (0 или 1), а также проверяет наличие каталога /tmp

12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:
    bash is /tmp/PIVtemp/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash

    (прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

mkdir /tmp/PIVtemp/

cp /bin/bash /tmp/PIVtemp/

type -a bash

выдал 2 строки

bash is /usr/bin/bash

bash is /bin/bash

далее

export PATH=/tmp/PIVtemp/:$PATH

type -a bash

выдал 3 строчки

bash is /tmp/PIVtemp/bash

bash is /usr/bin/bash

bash is /bin/bash

13. Чем отличается планирование команд с помощью batch и at?

  
batch - запускает команды, когда уровни загрузки системы позволяют это делать

at  - запускает команды в заданное время.

14. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.
 
Сделано. Командой vagrant suspend
____________________________________________________
3.2. РАБОТА В ТЕРМИНАЛЕ (лекция 2)
____________________________________________________

3.2.1 Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

ОТВЕТ: внутренняя или встренная, команда оболочки. 
cd - вызывается из оболочки и выполняет непосредственно в самой оболочке. 

3.2.2 Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. 

ОТВЕТ:

cat tst_bash

if [[ -d /tmp ]];

abcefgh

abcdefgh

123

grep 123 tst_bash -c
1

grep 123 tst_bash |wc -l
1


3.2.3 Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?

ОТВЕТ:
процесс: systemd
           
3.2.4 Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

ОТВЕТ: команда ls -l \root 2>/dev/pts/1

3.2.5 Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

ОТВЕТ: 
cat tst_bash

if [[ -d /tmp ]];

abcefgh

abcdefgh

123

new line

123456

cat tst_bash_out

cat: tst_bash_out: No such file or directory 

vagrant@vagrant:~$ cat <tst_bash >tst_bash_out

vagrant@vagrant:~$ cat tst_bash_out

if [[ -d /tmp ]];

abcefgh

abcdefgh

123

new line

123456

3.2.6 Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?

ОТВЕТ: можем попробовать перенаправить поток на tty echo Hello! > /dev/tty3

3.2.7 Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

ОТВЕТ: 

3.2.8 Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

ОТВЕТ: можно попробовать bash 7>&1 1>>&2 2>>>&7

Рабочий пример 3>&1 ls -la /root ~/ 3>&2 2>&1 1>&3 | tee 1 + скрин

3.2.9 Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?

ОТВЕТ: Получим переменные окружения оболочки. Альтернативный вариант получения: env

3.2.10 Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.
 
ОТВЕТ: 
по первому адресу /proc//cmdline - мы получим строку с  параметрами запуска исполняемого файла
по вторму адресу процесса /proc//exe -  будет ссылка на путь к исполняемому файлу процесса.


3.2.11 Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.
 
ОТВЕТ: SSE версии v 4.2

3.2.12 При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. 
 
ОТВЕТ: 
ssh -t localhost 'tty'
vagrant@localhost's password: 
/dev/pts/2
Connection to localhost closed

3.2.13 Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.
 
ОТВЕТ: Команда reptyr ругается на права администратора. 
Ставим значение kernel.yama.ptrace_scope = 0
Теперь screen процесс должен работать и после закрытия терминала.

3.2.14 sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.
 
ОТВЕТ: Команда tee делает вывод и в файл, и дает права на запись.  
Команда echo выводит информацию на экран. Если запустить echo с рутом, то права на запись не получается получить. 
В случае с tee именно такие права и даются
_____________________________________________
3.7. Компьютерные сети, лекция 2
_____________________________________________
1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
ОТВЕТ:
Windows: 
Для ipv4
GetAdaptersInfo 
Для Ipv6
GetAdaptersAddresses
Можно получить список сетевых 
адаптеров GetIfTable или GetIfEntry или GetIfTable

Linux 
ip a |awk '/state UP/{print $2}'
eth0:

или 
ip -o a show | cut -d ' ' -f 2,7

или 

ip a |grep -i inet | awk '{print $7, $2}'

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

ОТВЕТ: Neighbor Discovery Protocol (NDP)  , 
пакет iproute2

3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

ОТВЕТ: vlan  и пакет iproite2
Пример конфига 
ip link add link eth0 name eth0.100 type vlan id 100
ip link set dev eth0.100 up
ip a add 192.168.100.2/24 dev eth0.100

4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.


ОТВЕТ:
Mode-0(balance-rr
Mode-1(active-backup) 
Mode-2(balance-xor) 
Mode-3(broadcast) 
Mode-4(802.3ad) 
Mode-5(balance-tlb) 
Mode-6(balance-alb) 
Б) 
[root@piv]# modprobe  bonding
[root@piv]# ip addr add 192.168.100.33/24 brd + dev bond0
[root@piv]# ip link set dev bond0 up
[root@piv]# ifenslave  bond0 eth2 eth3
master has no hw address assigned; getting one from slave!
The interface eth2 is up, shutting it down it to enslave it.
The interface eth3 is up, shutting it down it to enslave it.
[root@piv]# ifenslave  bond0 eth2 eth3
[root@piv]# cat /proc/net/bond0/info
Bonding Mode: load balancing (round-robin)
MII Status: up
MII Polling Interval (ms): 0
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth2
MII Status: up
Link Failure Count: 0

Slave Interface: eth3
MII Status: up
Link Failure Count: 0



5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

ОТВЕТ: 
8  адресов с маской подсети /29, минус броадкаст и адрес сети - в этой сети может быть 6 хостов
32 шт 29 посетей помещается в 24 подсети
примеры:
10.10.10.0/29
10.10.10.8/29 
10.10.10.16/29
10.10.10.24/29
10.10.10.32/29

6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.
ОТВЕТ: 192.0.0.0/26 на 62 хоста 

Можно взять из 100.64.0.0/10


In 2012, the IETF defined a Shared Address Space[4] for use in ISP CGN deployments and NAT devices that can handle the same addresses occurring both on inbound and outbound interfaces. ARIN returned space to the IANA as needed for this allocation and[5] "The allocated address block is 100.64.0.0/10".[4][6]



7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?
ОТВЕТ:
Для windows  
Посмотреть все ip:  arp –a
Удалить все ip: netsh interface ip delete arpcache
удалить один ip: arp -d 192.168.3.171
для линукса:
посмотреть все - ip n
удалить все ip n flush all
удалить один ip n del 192.168.0.1 dev eth0

 
 __________________________________________________________________________
 Домашнее задание к занятию "3.3. Операционные системы, лекция 1"
 __________________________________________________________________________
1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.

ОТВЕТ: 

Системный вызов — это механизм взаимодействия пользовательских программ с ядром Linux, а strace — мощный инструмент, для их отслеживания. 

Получается:  chdir("/tmp") 


2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например:
vagrant@vagrant: ~$ file /dev/tty
/dev/tty: character special (5/0)
vagrant@vagrant: ~$ file /dev/sda
/dev/sda: block special (8/0)
vagrant@vagrant: ~$ file /bin/bash
/bin/bash: ELF 64-bit LSB shared object, x86-64
Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.

ОТВЕТ:
Файл базы типов - /usr/share/misc/magic.mgc
Получается: 
openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3

происходит поиск файлов пользователей
stat("/home/piv/.magic.mgc", 0x7ffedefbea50) = -1 ENOENT (Нет такого файла или каталога)
stat("/home/piv/.magic", 0x7ffedefbea50) = -1 ENOENT (Нет такого файла или каталога)
stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3



3.  Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

ОТВЕТ:
c помощью lsof нашел pid процесса который пишет в удаленный файл. 

[ivan@localhost ~]$ lsof | grep deleted
nc        13049         ivan    1w      REG  253,0       105 50331728 /home/ivan/file (deleted)

в procfc нашел файловый дискриптор данного процесса который пишет в удаленный файл: 
[ivan@localhost ~]$ ll /proc/13049/fd

lrwx------. 1 ivan ivan 64 Jul 20 21:23 0 -> /dev/pts/1
l-wx------. 1 ivan ivan 64 Jul 20 21:23 1 -> /home/ivan/file (deleted)
lrwx------. 1 ivan ivan 64 Jul 20 21:23 2 -> /dev/pts/1
lrwx------. 1 ivan ivan 64 Jul 20 21:23 5 -> socket:[36294]

И с помощью echo обнулил дискриптор: 

[ivan@localhost ~]$ echo  >/proc/13049/fd/1

При проверке:
[ivan@localhost ~]$ lsof | grep deleted
nc        13049         ivan    1w      REG  253,0         1 50331728 /home/ivan/file (deleted)

4.  Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?


ОТВЕТ: 
Зомби-процессы -  процессы, которые завершили своё выполнение, но ещё присутствуют в списке процессов операционной системы, чтобы дать родительскому процессу считать код завершения.

Зомби-процесс существует до тех пор, пока родительский процесс не прочитает его статус с помощью системного вызова wait(), в результате чего запись в таблице процессов будет освобождена.

5. В iovisor BCC есть утилита opensnoop:
root@vagrant: ~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04

ОТВЕТ: 
root@vagrant: ~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
root@vagrant: ~# /usr/sbin/opensnoop-bpfcc
PID    COMM               FD ERR PATH
766    vminfo              6   0 /var/run/utmp
562    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
562    dbus-daemon        18   0 /usr/share/dbus-1/system-services
562    dbus-daemon        -1   2 /lib/dbus-1/system-services
562    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/


6 . Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

ОТВЕТ: 
системный вызов uname()

цитата из man: 
     Part of the utsname information is also accessible  via  /proc/sys/ker‐
       nel/{ostype, hostname, osrelease, version, domainname}.


7. Чем отличается последовательность команд через ; и через && в bash? Например:
root@vagrant: ~# test -d /tmp/some_dir; echo Hi
Hi
root@vagrant: ~# test -d /tmp/some_dir && echo Hi
root@vagrant: ~#
Есть ли смысл использовать в bash &&, если применить set -e?

ОТВЕТ: 

test -d /tmp/some_dir && echo Hi -  в первом примере оператор echo отработает свою команда если завершится test успешно 
set -e – если будет не нулевое значение исполняемых нами команд, то оператор set-e прерывает сессию. 
Если && использовать с set -e- то произойдет ошибка и команда прекратиться 

8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?

ОТВЕТ: 
Указав параметр -e скрипт немедленно завершит работу, если любая команда выйдет с ошибкой.
установить -o pipefail
Этот параметр предотвращает маскирование ошибок в конвейере. В случае сбоя какой-либо команды в конвейере этот код возврата будет использоваться как код возврата для всего конвейера. По умолчанию конвейер возвращает код последней команды, даже если она выполнена успешно.
set -u

Благодаря ему оболочка проверяет инициализацию переменных в скрипте. Если переменной не будет, скрипт немедленно завершиться.

set -x

Параметр -x очень полезен при отладке. С помощью него bash печатает в стандартный вывод все команды перед их исполнением.

К счастью, можно изменить поведение оболочки используя встроенные функции, в частности set. С ее помощью, можно значительно повысить безопасность.

9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

ОТВЕТ: 
Наиболее часто встречаются процессы с STAT равным S, Ss и Ssl (прерываемый сон), ожидающие дальнейшей команды/сигналов.
Приложил скрин.

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
