# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1

В этом задании вы потренируетесь в:
- установке elasticsearch
- первоначальном конфигурировании elastcisearch
- запуске elasticsearch в docker

Используя докер образ [centos:7](https://hub.docker.com/_/centos) как базовый и 
[документацию по установке и запуску Elastcisearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html):

- составьте Dockerfile-манифест для elasticsearch
- соберите docker-образ и сделайте `push` в ваш docker.io репозиторий
- запустите контейнер из получившегося образа и выполните запрос пути `/` c хост-машины

Требования к `elasticsearch.yml`:
- данные `path` должны сохраняться в `/var/lib`
- имя ноды должно быть `netology_test`

В ответе приведите:
- текст Dockerfile манифеста
- ссылку на образ в репозитории dockerhub
- ответ `elasticsearch` на запрос пути `/` в json виде

Подсказки:
- возможно вам понадобится установка пакета perl-Digest-SHA для корректной работы пакета shasum
- при сетевых проблемах внимательно изучите кластерные и сетевые настройки в elasticsearch.yml
- при некоторых проблемах вам поможет docker директива ulimit
- elasticsearch в логах обычно описывает проблему и пути ее решения

Далее мы будем работать с данным экземпляром elasticsearch.

## Ответ

- текст Dockerfile манифеста
```commandline
FROM centos:7
RUN yum update -y && \
yum install wget  perl-Digest-SHA -y
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.2-linux-x86_64.tar.gz; \
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.2-linux-x86_64.tar.gz.sha512; \
shasum -a 512 -c elasticsearch-8.2.2-linux-x86_64.tar.gz.sha512
RUN tar -xzf elasticsearch-8.2.2-linux-x86_64.tar.gz
RUN groupadd elasticsearch; \
useradd elasticsearch -g elasticsearch; \
chown -R elasticsearch:elasticsearch /elasticsearch-8.2.2; \
mkdir /var/lib/data /var/lib/logs; \
chown elasticsearch:elasticsearch /var/lib/data /var/lib/logs
ENV ES_HOME=/elasticsearch-8.2.2
COPY elasticsearch.yml /elasticsearch-8.2.2/config/
WORKDIR /elasticsearch-8.2.2
USER elasticsearch
CMD [ "/elasticsearch-8.2.2/bin/elasticsearch" ]
```
- ссылку на образ в репозитории dockerhub
```commandline
https://hub.docker.com/repository/docker/raegent/elasticsearch
```
- ответ `elasticsearch` на запрос пути `/` в json виде
```commandline
{
  "name" : "netology_test",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "pcIEJ6-6SLaVeE_7a7x6tg",
  "version" : {
    "number" : "8.2.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "9876968ef3c745186b94fdabd4483e01499224ef",
    "build_date" : "2022-05-25T15:47:06.259735307Z",
    "build_snapshot" : false,
    "lucene_version" : "9.1.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}  
```
## Задача 2

В этом задании вы научитесь:
- создавать и удалять индексы
- изучать состояние кластера
- обосновывать причину деградации доступности данных

Ознакомтесь с [документацией](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html) 
и добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |

Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.

Получите состояние кластера `elasticsearch`, используя API.

Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

Удалите все индексы.

**Важно**

При проектировании кластера elasticsearch нужно корректно рассчитывать количество реплик и шард,
иначе возможна потеря данных индексов, вплоть до полной, при деградации системы.

## Ответ

добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:
```commandline
curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d '
> {
> "settings": {
> "index": {
> "number_of_replicas": 0,
> "number_of_shards": 1
> }
> }
> }'

curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d '
> {
> "settings": {
> "index": {
> "number_of_replicas": 1,
> "number_of_shards": 2
> }
> }
> }'

curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d '
> {
> "settings": {
> "index": {
> "number_of_replicas": 2,
> "number_of_shards": 4
> }
> }
> }'
```
Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.
```commandline
curl http://localhost:9200/_cat/indices
yellow open ind-2 3PRZFcAYQjKkNeYEiYxOAw 2 1 0 0 450b 450b
yellow open ind-3 0JeZPWVWRwi8hHjgy06NIg 4 2 0 0 900b 900b
green  open ind-1 cgYDQTOtTDmV98lkDg-zKA 1 0 0 0 225b 225b
```
Получите состояние кластера `elasticsearch`, используя API.
```commandline
curl -XGET 'localhost:9200/_cluster/health?level=indices&pretty'
{
  "cluster_name" : "elasticsearch",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 8,
  "active_shards" : 8,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 44.44444444444444,
  "indices" : {
    ".geoip_databases" : {
      "status" : "green",
      "number_of_shards" : 1,
      "number_of_replicas" : 0,
      "active_primary_shards" : 1,
      "active_shards" : 1,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 0
    },
    "ind-1" : {
      "status" : "green",
      "number_of_shards" : 1,
      "number_of_replicas" : 0,
      "active_primary_shards" : 1,
      "active_shards" : 1,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 0
    },
    "ind-3" : {
      "status" : "yellow",
      "number_of_shards" : 4,
      "number_of_replicas" : 2,
      "active_primary_shards" : 4,
      "active_shards" : 4,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 8
    },
    "ind-2" : {
      "status" : "yellow",
      "number_of_shards" : 2,
      "number_of_replicas" : 1,
      "active_primary_shards" : 2,
      "active_shards" : 2,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 2
    }
  }
}
```
Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

- Потому что кластер переживает что у него всего одна нода на которой располагаются шарды. И понимает что в случае падения ноды мы потеряем доступ ко всему.


Удалите все индексы.
```commandline
curl -X DELETE http://localhost:9200/ind-1
curl -X DELETE http://localhost:9200/ind-2
curl -X DELETE http://localhost:9200/ind-3
```
## Задача 3

В данном задании вы научитесь:
- создавать бэкапы данных
- восстанавливать индексы из бэкапов

Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.

Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.

[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.

Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.

[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.

Подсказки:
- возможно вам понадобится доработать `elasticsearch.yml` в части директивы `path.repo` и перезапустить `elasticsearch`

---

## Ответ

Пришлось переделать Dockerfile: 
```commandline
mkdir /var/lib/data /var/lib/logs /elasticsearch-8.2.2/snapshots;
```
и elasticsearch.yml:
```commandline
path.repo: /elasticsearch-8.2.2/snapshots
```

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.
```commandline
curl -X PUT localhost:9200/_snapshot/netology_backup -H 'Content-Type: application/json' -d'
{
"type": "fs",
"settings": {
"location": "snapshot_repository"
}
}'
```
Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.
```commandline
curl -X PUT localhost:9200/test -H 'Content-Type: application/json' -d '
{
"settings": {
"index": {
"number_of_replicas": 0,
"number_of_shards": 1
}
}
}'
```
[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.
```commandline
curl -X PUT "localhost:9200/_snapshot/netology_backup/love_snapshot?wait_for_completion=true&pretty"
{
  "snapshot" : {
    "snapshot" : "love_snapshot",
    "uuid" : "TheRhRE6WrvO55Yj4Ug",
    "repository" : "netology_backup",
    "version_id" : 8020099,
    "version" : "8.2.2",
    "indices" : [
      ".geoip_databases",
      "test"
    ],
    "data_streams" : [ ],
    "include_global_state" : true,
    "state" : "SUCCESS",
    "start_time" : "2022-05-25T16:17:21.259735307Z",
    "start_time_in_millis" : 1652194580917,
    "end_time" : "2022-05-25T16:17:23.259735307Z",
    "end_time_in_millis" : 1652194582321,
    "duration_in_millis" : 1404,
    "failures" : [ ],
    "shards" : {
      "total" : 2,
      "failed" : 0,
      "successful" : 2
    },
    "feature_states" : [
      {
        "feature_name" : "geoip",
        "indices" : [
          ".geoip_databases"
        ]
      }
    ]
  }
}
```
**Приведите в ответе** список файлов в директории со `snapshot`ами.
```commandline
docker exec -ti elastic bash
[elasticsearch@b68f7b853ceb elasticsearch-8.2.2]$ ls -la ./snapshots/snapshot_repository/
total 44
drwxr-xr-x 3 elasticsearch elasticsearch  4096 Jun 11 05:56 .
drwxr-xr-x 1 elasticsearch elasticsearch  4096 Jun 11 05:48 ..
-rw-r--r-- 1 elasticsearch elasticsearch   844 Jun 11 05:56 index-0
-rw-r--r-- 1 elasticsearch elasticsearch     8 Jun 11 05:56 index.latest
drwxr-xr-x 4 elasticsearch elasticsearch  4096 Jun 11 05:56 indices
-rw-r--r-- 1 elasticsearch elasticsearch 18230 Jun 11 05:56 meta-TheRhRE6WrvO55Yj4Ug.dat
-rw-r--r-- 1 elasticsearch elasticsearch   353 Jun 11 05:56 snap-TheRhRE6WrvO55Yj4Ug.dat
```
Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.
```commandline
curl -X DELETE localhost:9200/test

curl -X PUT localhost:9200/test-2 -H 'Content-Type: application/json' -d '
{
"settings": {
"index": {
"number_of_replicas": 0,
"number_of_shards": 1
}
}
}'
```
[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 
```commandline
curl -X POST localhost:9200/_snapshot/netology_backup/love_snapshot/_restore
```
**Приведите в ответе** запрос к API восстановления и итоговый список индексов.
```commandline
curl http://localhost:9200/_cat/indices
green open test-2 PGrTkrWNT9-e8amowRKddQ 1 0 0 0 225b 225b
green open test   LopxcCaWSE2vcfnWrui8OA 1 0 0 0 225b 225b
```