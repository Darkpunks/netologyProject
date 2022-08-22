__________________________________________________________________________
Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."
__________________________________________________________________________

Бывает, что общедоступная документация по терраформ ресурсам не всегда достоверна,
в документации не хватает каких-нибудь правил валидации или неточно описаны параметры,
понадобиться использовать провайдер без официальной документации,
может возникнуть необходимость написать свой провайдер для системы используемой в ваших проектах.

__________________________________________________________________________
Задача 1.
__________________________________________________________________________
Давайте потренируемся читать исходный код AWS провайдера, который можно склонировать от сюда: https://github.com/hashicorp/terraform-provider-aws.git. 

ОТВЕТ: делаем командой: git clone https://github.com/hashicorp/terraform-provider-aws.git

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.6/7.6.jpg">

Просто найдите нужные ресурсы в исходном коде и ответы на вопросы станут понятны: 


1. Найдите, где перечислены все доступные resource и data_source, приложите ссылку на эти строки в коде на гитхабе.

ОТВЕТ: 

[resource](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/provider.go#L411)

[data_source](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/provider.go#L169) 



2. Для создания очереди сообщений SQS используется ресурс aws_sqs_queue у которого есть параметр name.

2а. С каким другим параметром конфликтует name? Приложите строчку кода, в которой это указано.

ОТВЕТ:
ConflictsWith: []string{"name_prefix"} 

[конфликт_name](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/resource_aws_sqs_queue.go#L56)


2б. Какая максимальная длина имени?

ОТВЕТ: [не_более_80 символов](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/validators.go#L1038)


2в. Какому регулярному выражению должно подчиняться имя?

ОТВЕТ: 
[0-9A-Za-z-_] и доп ограничения Fifo - содежатся в: 
[func_validateSQSNonFifoQueueName_func_validateSQSFifoQueueName](https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/validators.go#L1041)
