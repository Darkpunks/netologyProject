__________________________________________________________________________
 3.9. Элементы безопасности информационных систем
__________________________________________________________________________
1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

Сделал: скрин
2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

Сделал: скрин
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
ОТВЕТ:


4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

ОТВЕТ:
Проверял на сайте ok.ru
часть вывода

Apple ATS 9 iOS 9            TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 256 bit ECDH (P-256)

 Java 6u45                    No connection
 
 Java 7u25                    TLSv1.0 ECDHE-RSA-AES128-SHA, 256 bit ECDH (P-256)
 
 Java 8u161                   TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 256 bit ECDH (P-256)
 
 Java 11.0.2 (OpenJDK)        TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 256 bit ECDH (P-256)
 
 Java 12.0.1 (OpenJDK)        TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 256 bit ECDH (P-256)
 
 OpenSSL 1.0.2e               TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 256 bit ECDH (P-256)
 
 OpenSSL 1.1.0l (Debian)      TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 253 bit ECDH (X25519)
 
 OpenSSL 1.1.1d (Debian)      TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 253 bit ECDH (X25519)
 
 Thunderbird (68.3)           TLSv1.2 ECDHE-RSA-AES128-GCM-SHA256, 253 bit ECDH (X25519)

 Done 2022-07-25 19:15:21 [ 212s] -->> 217.20.155.13:443 (ok.ru) <<--

------------------------------------------------------------------------------------
Done testing now all IP addresses (on port 443): 5.61.23.11 217.20.147.1 217.20.155.13


не понравился только 1 пункт 

 TLS 1      offered (deprecated)
 
 TLS 1.1    offered (deprecated)

По идее устаревшие протоколы TLS



5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

ОТВЕТ:
[ivan@localhost ~]$ ssh ivan@192.168.5.108
The authenticity of host '192.168.5.108 (192.168.5.108)' can't be established.
ECDSA key fingerprint is SHA256:0j2Ig8JVTy+LnmtkFwIgnRihu/Dekjnt6IvVMeSM5s8.
ECDSA key fingerprint is MD5:56:e1:74:71:2a:51:87:5e:c2:ba:87:2c:91:47:9a:34.



[ivan@localhost ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ivan/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ivan/.ssh/id_rsa.
Your public key has been saved in /home/ivan/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:GKm7h40NA+2HylTC9XcTxbWu0rQi6KDsl+ozi1YCnXY ivan@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|           o...  |
|    .  .  . .  . |
| o + .o    .  .  |
|. B E..o. o  .   |
|.. *....S. .. .  |
| ...=...   o o   |
| oo.oX. . o +    |
| +=.=++  . o     |
|ooB*...          |
+----[SHA256]-----+

Скопируем ID , пишет какой ключ [RSA 2048] генерирует. мы также можем его поменять. 

[ivan@localhost ~]$ ssh-copy-id ivan@192.168.5.108
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ivan/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ivan@192.168.5.108's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ivan@192.168.5.108'"
and check to make sure that only the key(s) you wanted were added.



6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

ОТВЕТ:
[ivan@localhost ~]$ ls .ssh

id_rsa  id_rsa.pub  known_hosts

[ivan@localhost ~]$ mv .ssh/id_rsa .ssh/id_piv

[ivan@localhost ~]$ mv .ssh/id_rsa.pub .ssh/id_piv.pub

теперь при попытке подлючения просит пароль: 

[ivan@localhost ~]$ ssh ivan@192.168.5.108

ivan@192.168.5.108's password:


б) создаем файл конфига

touch ~/.ssh/config

chmod 600 ~/.ssh/config

Вносим в данные конфига

Host pivhost

    HostName 192.168.5.108
    User ivan
    Port 22
    IdentityFile ~/.ssh/id_piv

теперь подключается по имени сервера: 

[ivan@localhost ~]$ ssh pivhost

Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)

 * Documentation:  https://help.ubuntu.com

 * Management:     https://landscape.canonical.com

 * Support:        https://ubuntu.com/advantage

  System information as of Mon Jul 25 19:36:33 UTC 2022

  System load:  0.04              Processes:               129
  
  Usage of /:   4.5% of 38.70GB   Users logged in:         2
  
  Memory usage: 28%               IPv4 address for enp0s3: 192.168.5.108
  
  Swap usage:   0%

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

1 update can be applied immediately.
To see these additional updates run: apt list --upgradable


Last login: Mon Jul 25 19:10:45 2022 from 192.168.5.157


7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.

ОТВЕТ: 100 пакетов: 

ivan@ubuntu-focal:~$ sudo tcpdump -c 100 -w dump.pcap 
Приложил рсар файл
