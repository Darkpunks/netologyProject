### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-03-yaml/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис
  
  
  ОТВЕТ: 
```
   { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : "7175" 
            },
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
    }
```

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
import json, socket, sys, yaml

HISTORY_FILE = 'history'


def main():

  if len(sys.argv) < 2:
    print("No hostname specified")
    exit()

  history = load_history(HISTORY_FILE)

  for hostname in sys.argv[1:]:
    process_host(hostname, history)

  save_history(HISTORY_FILE, history)


def load_history(filename):
  
  try:
    with open(filename+".yaml") as f:
      return yaml.safe_load(f)

  except FileNotFoundError:
    print('History bot found, creating new history')
    return {}


def save_history(file, history):

  with open(file+".yaml", 'w') as f:
    yaml.dump(history, f)

  with open(file+".json", 'w') as f:
    json.dump(history, f)

def process_host(hostname, known_names):

  last_ip = False

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


if __name__ == "__main__":
  main()
```

### Вывод скрипта при запуске при тестировании:
```
py .\test2.py drive.google.ru google.com  mail.google.com
drive.google.ru - 87.250.250.242
google.com - 74.125.131.138
[ERROR] mail.google.ru IP mismatch: 217.69.139.202 94.100.180.201
mail.google.com - 209.85.233.18
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{"drive.google.com": "173.194.221.194", "google.com": "142.250.184.238", "mail.google.com": "142.250.187.101"}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
drive.google.com: 142.250.186.174
google.com: 64.233.165.100
mail.google.com: 64.233.165.18

```

