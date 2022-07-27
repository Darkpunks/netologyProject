__________________________________________________________________________
Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"
__________________________________________________________________________

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP:

telnet route-views.routeviews.org

Username: rviews

show ip route x.x.x.x/32

show bgp x.x.x.x/32

ОТВЕТ: 

route-views>show ip route 95.220.24.101/32

Routing entry for 95.220.0.0/16

  Known via "bgp 6447", distance 20, metric 0
  
  Tag 3303, type external
  
  Last update from 217.192.89.50 7w0d ago
  
  Routing Descriptor Blocks:
  
  * 217.192.89.50, from 217.192.89.50, 7w0d ago
  * 
      Route metric is 0, traffic share count is 1
      
      AS Hops 2
      
      Route tag 3303
      
      MPLS label: none
      

Вторая, часть: 

route-views>show bgp 95.220.24.101/32

% Network not in table

Но получилось без: Лучший путь по 21 маршруту. доступно всего 24 маршрута. Вот его часть.

route-views>show bgp 95.220.24.101

BGP routing table entry for 95.220.0.0/16, version 1002848477

Paths: (24 available, best #21, table default)

  Not advertised to any peer
  
  Refresh Epoch 1
  
  20912 3257 174 12714
  
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
    
      Origin IGP, localpref 100, valid, external
      
      Community: 3257:8070 3257:30155 3257:50001 3257:53900 3257:53902 20912:65004
      
      path 7FE0C09CC2E0 RPKI State not found
      
      rx pathid: 0, tx pathid: 0
      
  Refresh Epoch 1
  
  19214 174 12714
  
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
    
      Origin IGP, localpref 100, valid, external
      
      Community: 174:21101 174:22005
      
      path 7FE021777C88 RPKI State not found
      
      rx pathid: 0, tx pathid: 0
      
  Refresh Epoch 1
  
  1351 6939 12714
  
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
    
      Origin IGP, localpref 100, valid, external
      
      path 7FE125DFE780 RPKI State not found
      
      rx pathid: 0, tx pathid: 0
      
  Refresh Epoch 1
  
  3267 31133 12714
  
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
    
      Origin IGP, metric 0, localpref 100, valid, external
      
      path 7FE15D41C3C8 RPKI State not found
      
      rx pathid: 0, tx pathid: 0
      
  Refresh Epoch 1
  
  20130 6939 12714
  
    140.192.8.16 from 140.192.8.16 (140.192.8.16)

Подводя итог: Лучший путь по 21 маршруту. доступно всего 24 маршрута


2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

ОТВЕТ:
 создаю интерфейс: 
загрузка модуля: 

[ivan@localhost ~]$ sudo modprobe -v dummy numdummies=1

insmod /lib/modules/3.10.0-1160.71.1.el7.x86_64/kernel/drivers/net/dummy.ko.xz numdummies=1

[ivan@localhost ~]$ lsmod | grep dummy

dummy                  12960  0

[ivan@localhost ~]$ ip a | grep dummy

4: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN group default qlen 1000

Присваиваем ip адрес интерфейса:

[ivan@localhost ~]$ sudo ip addr add 10.10.10.10/24 dev dummy0

Запускаю интерфейс: 

[ivan@localhost ~]$ sudo ip link set dummy0 up

добавляю маршруты: 

[ivan@localhost ~]$ sudo ip route add 10.10.9.0/24 dev dummy0

[ivan@localhost ~]$ sudo ip route add 10.10.8.0/24 dev dummy0

[ivan@localhost ~]$ sudo ip route add 10.10.7.0/24 dev dummy0

[ivan@localhost ~]$ sudo ip route add 10.10.6.0/24 dev dummy0


Проверяем: 
[ivan@localhost ~]$ ip r

default via 192.168.5.1 dev eth0 proto dhcp metric 100

10.10.6.0/24 dev dummy0 scope link

10.10.7.0/24 dev dummy0 scope link

10.10.8.0/24 dev dummy0 scope link

10.10.9.0/24 dev dummy0 scope link

10.10.10.0/24 dev dummy0 proto kernel scope link src 10.10.10.10

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

ОТВЕТ: открытые порты:

[ivan@localhost ~]$ sudo netstat -lpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name

tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1154/sshd

tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1385/master

tcp6       0      0 :::9100                 :::*                    LISTEN      681/node_exporter

tcp6       0      0 :::22                   :::*                    LISTEN      1154/sshd

tcp6       0      0 ::1:25                  :::*                    LISTEN      1385/master

 





Например 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1154/sshd

протокол tcp, порт 22, использует sshd (ssh сервер)

tcp6       0      0 :::9100                 :::*                    LISTEN      681/node_exporter


Протокол tcp (ipv6) порт 9100 использует node_exporter

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

ОТВЕТ:
[ivan@localhost ~]$ sudo netstat -lpn 
udp        0      0 127.0.0.1:323           0.0.0.0:*                           704/chronyd

udp        0      0 0.0.0.0:68              0.0.0.0:*                           961/dhclient

udp6       0      0 ::1:323                 :::*                                704/chronyd


например,

udp        0      0 127.0.0.1:323           0.0.0.0:*                           704/chronyd


Протокол udp  порт 323 слушается только локальный интерфейс 127.0.0.1 - chronyd



udp        0      0 0.0.0.0:68              0.0.0.0:*                           961/dhclient

Протокол udp  порт 68 - использует dhclient.

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

ОТВЕТ: схема на картинке

