# Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."

Бывает, что 
* общедоступная документация по терраформ ресурсам не всегда достоверна,
* в документации не хватает каких-нибудь правил валидации или неточно описаны параметры,
* понадобиться использовать провайдер без официальной документации,
* может возникнуть необходимость написать свой провайдер для системы используемой в ваших проектах.   

## Задача 1. 
Давайте потренируемся читать исходный код AWS провайдера, который можно склонировать от сюда: 
[https://github.com/hashicorp/terraform-provider-aws.git](https://github.com/hashicorp/terraform-provider-aws.git).
Просто найдите нужные ресурсы в исходном коде и ответы на вопросы станут понятны.  

## Ответ
resource
```commandline
https://github.com/hashicorp/terraform-provider-aws/blob/29a44d1f45cad3a890553c5fa2145bae0de46210/internal/provider/provider.go#L417
```
data_source
```commandline
https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/provider.go#L169
```

1. Найдите, где перечислены все доступные `resource` и `data_source`, приложите ссылку на эти строки в коде на 
гитхабе.   
1. Для создания очереди сообщений SQS используется ресурс `aws_sqs_queue` у которого есть параметр `name`. 
    * С каким другим параметром конфликтует `name`? Приложите строчку кода, в которой это указано.
    * Какая максимальная длина имени? 
    * Какому регулярному выражению должно подчиняться имя? 

## Ответ
С каким другим параметром конфликтует `name`? Приложите строчку кода, в которой это указано.
```commandline
ConflictsWith: []string{"name_prefix"}
https://github.com/hashicorp/terraform-provider-aws/blob/ad0cf62929dd5700d00db0222c5da14212d17266/internal/service/sqs/queue.go#L87 
```
Какая максимальная длина имени?
```commandline
80 и 75 для fifo
```
Какому регулярному выражению должно подчиняться имя?
```commandline
re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,80}$`)
и для fifo
re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,75}\.fifo$`)
```