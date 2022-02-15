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




РАБОТА В ТЕРМИНАЛЕ (лекция 1)

1. Установите средство виртуализации Oracle VirtualBox. 
2. 
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
  
7    Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.

Сделано

 8. Ознакомиться с разделами man bash, почитать о настройках самого bash: какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?

1. HISTFILESIZE – сколько всего может сохраниться строк в файле истории, 

по команде получилось 1155

2. HISTSIZE – количество команд для сохранения, 

по команде получилось 1164
       
       что делает директива ignoreboth в bash?

•ignoreboth – это сокращение от значений «ignorespace» и «ignoredups». 

• ignorespace – строки, начинающиеся с пробела, не будут сохранены в истории.

• ignoredups – строки, соответствующие предыдущей записи истории, не будут сохранены. Другими словами, дубликаты игнорируются.
  
  9  В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

{} - зарезервированные слова.

Применяется, когда надо ограничивать тело функции, циклах, операторах.

В командах выполняет подстановку элементов из списка, если упрощенно то цикличное выполнение команд с подстановкой например mkdir ./DIRPIV_{1..1000} - создаст каталоги с именами DIRPIV_1, DIRPIV_2 и т.д. до DIRPIV_1000

строка в баш 343
  
  10  С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

touch {000001..100000}.txt – все создалось

При команде touch {000001..300000}.txt пишет Argument list too long

При попытке выявить максимум (getconf ARG_MAX). Выдал число 2097152

  11  В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]
  
Конструкция проверяет сам -d /tmp и меняет ее статус (0 или 1), а также проверяет наличие каталога /tmp

 12   Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:
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

13.    Чем отличается планирование команд с помощью batch и at?
14.    
batch - запускает команды, когда уровни загрузки системы позволяют это делать

at  - запускает команды в заданное время.

 14   Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.
 
Сделано. Командой vagrant suspend

