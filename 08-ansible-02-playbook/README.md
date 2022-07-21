# Домашнее задание к занятию "08.02 Работа с Playbook"

Playbook устанавливает elasticsearch и kibana на сервере. 

Все переменные указаны в group_var, отдельно для каждой группы. 
Используемый список тэгов: 
  * java
  * elastic
  * kibana

Чтобы запустить playbook надо:
  * отредактировать в соответствии файл inventory/prod.yml
  * в директории playbook/files положить jdk архив
  * запустить playbook можно так: 
```commandline
ansible-playbook -i inventory/prod.yml site.yml
```
Рекомендуется запустить первый раз с параметрами -D -C, чтобы увидеть все изменения
```commandline
ansible-playbook -i inventory/prod.yml site.yml -D -C
```