# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Возникнет ошибка, (не соответствие типы переменных, нельзя сложить строку и число, питон так не умеет)  |
| Как получить для переменной `c` значение 12?  | надо добавить кавычки‘1’   |
| Как получить для переменной `c` значение 3?  | надо убрать из  переменной b (‘2’), кавычки и оставить просто число |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```
import os

DIR = '~/netology/sysadm-homeworks'

bash_command = [f"cd {DIR} 2>&1", "pwd", "git status --porcelain 2>&1"]

result_os = os.popen(' && '.join(bash_command)).read().split('\n')

#first string is our basepath
project_dir = result_os.pop(0)

#if cd command rerutns something like "/bin/sh: line 0: cd: " assuming no such dir
if project_dir.find('/bin/sh: line 0: cd:') != -1: 
  print('No such dir')
  exit(2)

#if git command rerutns something like "fatal: Not a git repository ..." assuming not a repository
if result_os[0].find('fatal:') != -1: 
  print('Not a repo')
  exit(2)

#remove tailing empty string
if result_os[-1] == '':
    result_os.pop()

def resolve_status(status):
    status_mapping = {
      'M': 'modified',
      'A': 'added',
      'D': 'deleted',
      'R': 'renamed',
      'C': 'copied',
      'U': 'updated',
      '??': 'untracked'
  }
    try:
        return status_mapping[status]
    except KeyError:
        return status


for f in result_os:
    status = resolve_status(f[0:2].strip())
    path = os.path.join(project_dir, f[3:len(f)])

    print(f'{status}: {path}')

```

### Вывод скрипта при запуске при тестировании:
```
Он не должен принимать никакие параметры и просто искать в папке
Например, 
Если удалить папку или файлы то он найдет их тоже 
[root@localhost ivan]# python3 test.py /home/ivan/sysadm-homeworks/
deleted: /home/ivan/sysadm-homeworks/01-intro-01/README.md
 клонировал репозиторий и изменил только один файл и он вывел его. 
[root@localhost ivan]# python3 test.py /home/ivan/sysadm-homeworks/
untracked: /home/ivan/sysadm-homeworks/Readme.md                                                                                                          
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозитори                                                                                         й в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```
import os, sys

DIR = '~/netology/sysadm-homeworks'

try:
    DIR = sys.argv[1]
except IndexError:
    pass

bash_command = [f"cd {DIR} 2>&1", "pwd", "git status --porcelain 2>&1"]

result_os = os.popen(' && '.join(bash_command)).read().split('\n')

#first string is our basepath
project_dir = result_os.pop(0)

#if cd command rerutns something like "/bin/sh: line 0: cd: " assuming no such dir
if project_dir.find('/bin/sh: line 0: cd:') != -1: 
  print('No such dir')
  exit(2)

#if git command rerutns something like "fatal: Not a git repository ..." assuming not a repository
if result_os[0].find('fatal:') != -1: 
  print('Not a repo')
  exit(2)

#remove tailing empty string
if result_os[-1] == '':
    result_os.pop()

def resolve_status(status):
    status_mapping = {
      'M': 'modified',
      'A': 'added',
      'D': 'deleted',
      'R': 'renamed',
      'C': 'copied',
      'U': 'updated',
      '??': 'untracked'
  }
    try:
        return status_mapping[status]
    except KeyError:
        return status


for f in result_os:
    status = resolve_status(f[0:2].strip())
    path = os.path.join(project_dir, f[3:len(f)])

    print(f'{status}: {path}') 

```

### Вывод скрипта при запуске при тестировании:
```
Вывод похож на вторую задачу только принимает путь в параметр. 
Например, клонировал репозиторий и изменил только один файл и он вывел его. 
[root@localhost ivan]# python3 test.py /home/ivan/sysadm-homeworks/
untracked: /home/ivan/sysadm-homeworks/Readme.md

Если удалить папку или файлы то он найдет их тоже 
[root@localhost ivan]# python3 test.py /home/ivan/sysadm-homeworks/
deleted: /home/ivan/sysadm-homeworks/01-intro-01/README.md
deleted: /home/ivan/sysadm-homeworks/01-intro-01/img/bash.png
deleted: /home/ivan/sysadm-homeworks/01-intro-01/img/jsonnet.png
deleted: /home/ivan/sysadm-homeworks/01-intro-01/img/markdown.png
deleted: /home/ivan/sysadm-homeworks/01-intro-01/img/terraform.png
deleted: /home/ivan/sysadm-homeworks/01-intro-01/img/yaml.png
deleted: /home/ivan/sysadm-homeworks/01-intro-01/netology.jsonnet
deleted: /home/ivan/sysadm-homeworks/01-intro-01/netology.md
deleted: /home/ivan/sysadm-homeworks/01-intro-01/netology.sh
deleted: /home/ivan/sysadm-homeworks/01-intro-01/netology.tf
deleted: /home/ivan/sysadm-homeworks/01-intro-01/netology.yaml
untracked: /home/ivan/sysadm-homeworks/Readme.md
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```
import socket, sys, json

hostname = ''
HISTORY_FILE = 'history.json'

try:
  hostname = sys.argv[1]
except IndexError:
  print("No hostname specified")
  exit()

known_names = {}
last_ip = False

try:
  with open(HISTORY_FILE) as f:
    known_names = json.load(f)
except FileNotFoundError:
  print('History bot found, creating new history')

try:
  last_ip = known_names[hostname]

except KeyError:
  pass

try:
    ip = socket.gethostbyname(hostname)
except socket.gaierror:
    print('Host resolve failed')
    exit()
    
if last_ip and last_ip != ip:
  print(f'[ERROR] {hostname} IP mismatch: {last_ip} {ip}')
else:
  print(f'{hostname} - {ip}')


known_names[hostname] = ip

with open(HISTORY_FILE, 'w') as f:
  json.dump(known_names, f)

```

### Вывод скрипта при запуске при тестировании:
```
если не указывать хост то будет выдавать
 No hostname specified


[root@localhost 04-script-02-py]# python3 n4.py google.ru
google.ru - 216.58.211.3
[root@localhost 04-script-02-py]# python3 n4.py google.ru
[ERROR] google.ru IP mismatch: 216.58.211.3 209.85.233.94
[root@localhost 04-script-02-py]# python3 n4.py google.ru
[ERROR] google.ru IP mismatch: 209.85.233.94 216.58.211.3
[root@localhost 04-script-02-py]# python3 n4.py google.ru
[ERROR] google.ru IP mismatch: 173.194.73.94 142.250.74.35

И на каждый Сервис будет даваться разный  ipшник, аналогично будет и с 
drive.google.com 
mail.google.com


