__________________________________________________________________________
Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения"
__________________________________________________________________________

Задача 1
__________________________________________________________________________
Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

ОТВЕТ:


1) Полная (аппаратная) виртуализация. Гипервизоры первого типа работают на аппаратном уровне без необходимости установки какой-либо ОС на хост. Они сами
являются ОС.
2) Паравиртуализация. Гипервизорам второго типа необходима ОС для доступа монитора виртуальных машин (гипервизора) к аппаратным ресурсам хоста.
2) Виртуализация уровня операционной системы. Виртуализация уровня ОС позволяет запускать изолированные и безопасные ВМ на одном хосте, но не позволяет запускать ОС с ядрами, отличными от типа ядра хостовой ОС.
Основное отличие виртуализации на уровне ОС в том, что виртуализируется не компьютер и не операционная система, а пользовательское окружение ОС. Эти экземпляры пользовательского окружения, называемые также контейнерами, полностью идентичны основному серверу и используют общее ядро ОС.



__________________________________________________________________________
Задача 2
__________________________________________________________________________
Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

физические сервера,
паравиртуализация,
виртуализация уровня ОС.
Условия использования:

Высоконагруженная база данных, чувствительная к отказу.
Различные web-приложения.
Windows системы для использования бухгалтерским отделом.
Системы, выполняющие высокопроизводительные расчеты на GPU.
Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

ОТВЕТ:

Высоконагруженная база данных, чувствительная к отказу
Для условия использования Высоконагруженная база данных, чувствительная к отказу выбрал физические сервера. Особенно, когда требуется высокая производительность, в этом случае происходит более высокий отклик, сокращение точки отказа в виде гипервизора хостовой машины. 
Для различных web-приложения -Виртуализация уровня ОС
Для такого решения требуется меньше ресурсов, также выше скорость масштабирования при необходимости расширения. Требуется меньше затрат на администрирование и нету жестких требований к аппаратным ресурсам.

Windows системы для использования Бухгалтерским отделом - Паравиртуализация 
Для бухгалтерии важно делать бэкап чуть ли не на «каждом шагу», важно клонирование всей виртуальной машины, также расширение ресурсов на уровне виртуальной машины и им всегда мало ресурсов для их «прожорливых» программ. Как раз такой вариант у меня на работе в администрации города. 
       

Системы, выполняющие высокопроизводительные расчеты на GPU - Физические сервера 
В данном случае требуется быстрый доступ к аппаратным ресурсам для быстрых расчетов. 

__________________________________________________________________________
Задача 3
__________________________________________________________________________

Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:

100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

ОТВЕТ:

Сценарий 1: VMWare. Стоит такой на работе. Вариант VMware vSphere 7.0. Решения от VMWare являются наиболее сбалансированными, имеют достаточно обширный функционал, который позволит закрыть вопросы по сопровождению и администрированию среды на 100 VM Windows, Linux.

Сценарий 2: KVM. Решение на базе KVM позволит управлять гостевыми ОС, как Windows, так и Linux, без особых потерь в производительности, что обеспечивает решение поставленной задачи. 

Сценарий 3: Использовать open source проект  XEN PV. Решение позволит поднять инфраструктуру на базе Windows OS

Сценарий 4: Virtual Box совместно с Vagrant. Позволяет создавать и конфигурировать легковесные, повторяемые и переносимые окружения для разработки. Первая задача любого разработчика на новом проекте - развернуть окружение. Как правило, она сводится к следующим шагам: Клонировать репозиторий с проектом, Поставить необходимые пакеты для работы (например, библиотеку для xml), Установить дополнительные программы, такие как базу данных, Правильно настроить конфигурационные параметры.

__________________________________________________________________________
Задача 4
__________________________________________________________________________
Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.

ОТВЕТ:
Гетерогенные виртуальные среды также могут быть неэффективны и сложны в управлении. Многие компании решили реализовывать пошаговую стратегию виртуализации ИТ-инфраструктуры, постепенно внедряя решения от различных производителей. Такой подход подразумевает, что определенные типы устройств выполняют соответственные задачи, и при этом компания зависит от продукции не одного вендора. Учитывая тот факт, что для управления таким «лоскутным одеялом» требуется целый набор специалистов различного профиля, данный способ виртуализации инфраструктуры является дорогостоящим и неэффективным в долгосрочной перспективе, поскольку, по мере развития технологий, компании стремятся обеспечить легкость в управлении, совместимость различных систем и масштабируемость инфраструктуры.
Разнородность директорий хранения данных и произвольное расположение виртуальных машин в инфраструктуре значительно усложняет задачу управления огромными объемами динамических данных. Эта проблема особенно ощутима, когда ИТ-отделу компании необходимо обеспечить оперативную миграцию данных и виртуальных машин между виртуальными и физическими средами в рамках физической ИТ-инфраструктуры предприятия.


например, я работаю в администрации города сисадмином и у нас Гетерогенные виртуальные среды, нас заставляют везде переходить на linux, но и от windows еще не можем отказаться, аналогичная проблема и с железкой: есть 2 машины supermicro, и хранилище synology а новый купить не можем, санкции. В итоге: телефония и helpdesk работают на lunix на отдельной физической машине, а домен и outlook на второй. И приходится их администрировать по отдельности, что неудобно. Из плюсов, что если телефония или helpdesk "падает", то домен остается "живым" и наоборот. 



