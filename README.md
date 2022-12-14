# Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."

Бывает, что

* общедоступная документация по терраформ ресурсам не всегда достоверна,
* в документации не хватает каких-нибудь правил валидации или неточно описаны параметры,
* понадобиться использовать провайдер без официальной документации,
* может возникнуть необходимость написать свой провайдер для системы используемой в ваших проектах.

## Задача 1.

Давайте потренируемся читать исходный код AWS провайдера, который можно склонировать от сюда: [https://github.com/hashicorp/terraform-provider-aws.git](https://github.com/hashicorp/terraform-provider-aws.git). Просто найдите нужные ресурсы в исходном коде и ответы на вопросы станут понятны.

(1) Найдите, где перечислены все доступные `resource` и `data_source`, приложите ссылку на эти строки в коде на гитхабе.

*Ответ:*

`resource` - https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/provider.go#L411

`data_source` - https://github.com/hashicorp/terraform-provider-aws/blob/8e4d8a3f3f781b83f96217c2275f541c893fec5a/aws/provider.go#L169

(2) Для создания очереди сообщений SQS используется ресурс `aws_sqs_queue` у которого есть параметр `name`.

* С каким другим параметром конфликтует `name`? Приложите строчку кода, в которой это указано.
  *Ответ:*

```
ConflictsWith: []string{"name_prefix"},
```

* Какая максимальная длина имени?

*Ответ:*

80  символов максимальная длинна имени

```
func validateSQSQueueName(v interface{}, k string) (ws []string, errors []error) {
	value := v.(string)
	if len(value) > 80 {
		errors = append(errors, fmt.Errorf("%q cannot be longer than 80 characters", k))
	}
```

* Какому регулярному выражению должно подчиняться имя?

*Ответ:*

```
if !regexp.MustCompile(`^[0-9A-Za-z-_]+(\.fifo)?$`).MatchString(value) {
		errors = append(errors, fmt.Errorf("only alphanumeric characters and hyphens allowed in %q", k))
	}
```
