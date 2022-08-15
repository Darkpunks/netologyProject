__________________________________________________________________________
Домашнее задание к занятию "6.5. Elasticsearch "
__________________________________________________________________________
1 задание
__________________________________________________________________________

текст Dockerfile манифеста
```
# syntax = docker/dockerfile:1.4

FROM elasticsearch:7.17.5

RUN <<ECSCFG cat >> /usr/share/elasticsearch/config/elasticsearch.yml
node.name: netology_test
path:
  data: /var/lib
ECSCFG
```

Собирал командой 
```
[ivan@localhost elasticsearch]$ DOCKER_BUILDKIT=1 docker build -t darkpunks/elasticsearch-netology:7.17.5 .
```

ссылку на образ в репозитории dockerhub
```
https://hub.docker.com/r/darkpunks/elasticsearch-netology
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.1.jpg">

ответ elasticsearch на запрос пути / в json виде
```
[ivan@localhost elasticsearch]$ curl 127.0.0.1:9200/
{
  "name" : "netology_test",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "KA8lBeWpRHKe4_xqXZB5Cw",
  "version" : {
    "number" : "7.17.5",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "8d61b4f7ddf931f219e3745f295ed2bbc50c8e84",
    "build_date" : "2022-06-23T21:57:28.736740635Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.1-2.jpg">

__________________________________________________________________________
Задание 2 
__________________________________________________________________________
```
curl -X PUT 127.0.0.1:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
curl -X PUT 127.0.0.1:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
curl -X PUT 127.0.0.1:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'  
```
Список индексов:
```
[ivan@localhost elasticsearch]$ curl -X GET 'http://127.0.0.1:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases 4RvWxsg6QPm0XO-admXwng   1   0         40            0     37.9mb         37.9mb
green  open   ind-1            3IF3_47qQIiy8mRt7kruvQ   1   0          0            0       226b           226b
yellow open   ind-3            Qqr3CFgOS_CepSrsio2BMA   4   2          0            0       904b           904b
yellow open   ind-2            ML8uOdIjQTC2WBlOGoNNyg   2   1          0            0       452b           452b
```


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.2.jpg">





Почему часть индексов и кластер находится в состоянии yellow?


yellow 2 и 3 , у них должна быть 1 и 2 реплики соответсвенно, а у нас всего 1 node в кластере, негде реплики поднимать. 

```
curl -X DELETE 'http://127.0.0.1:9200/ind-1?pretty' 
curl -X DELETE 'http://127.0.0.1:9200/ind-2?pretty' 
curl -X DELETE 'http://127.0.0.1:9200/ind-3?pretty' 

[ivan@localhost elasticsearch]$ curl -X GET 'http://127.0.0.1:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases 4RvWxsg6QPm0XO-admXwng   1   0         40            0     37.9mb         37.9mb
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.2-1.jpg">

__________________________________________________________________________
Задание 3
__________________________________________________________________________
  
```
[ivan@localhost elasticsearch]$ curl -XPOST 127.0.0.1:9200/_snapshot/netology_backup?pretty -H 'Content-Type: application/json' -d'{"type": "fs", "settings": { "location":"/var/lib/snapshots" }}'
{
  "acknowledged" : true
}



[ivan@localhost elasticsearch]$ curl 'http://127.0.0.1:9200/_snapshot/netology_backup?pretty'
{
  "netology_backup" : {
    "type" : "fs",
    "settings" : {
      "location" : "/var/lib/snapshots"
    }
  }
}
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.3.jpg">

Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.

```
[ivan@localhost elasticsearch]$ curl -X PUT 127.0.0.1:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}

[ivan@localhost elasticsearch]$ curl -X GET 'http://127.0.0.1:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases 4RvWxsg6QPm0XO-admXwng   1   0         40            0     37.9mb         37.9mb
green  open   test             r1t2L7yXQr-A7dECE-oejA   1   0          0            0       226b           226b
```

Создайте snapshot состояния кластера elasticsearch.

```
[ivan@localhost elasticsearch]$ curl -X PUT 127.0.0.1:9200/_snapshot/netology_backup/elasticsearch?wait_for_completion=true
{"snapshot":{"snapshot":"elasticsearch","uuid":"VJAoFRMtRCeFVf6zjnQtHA","repository":"netology_backup","version_id":7170599,"version":"7.17.5","indices":[".geoip_databases","test"],"data_streams":[],"include_global_state":true,"state":"SUCCESS","start_time":"2022-08-12T23:28:15.414Z","start_time_in_millis":1660346895414,"end_time":"2022-08-12T23:28:16.421Z","end_time_in_millis":1660346896421,"duration_in_millis":1007,"failures":[],"shards":{"total":2,"failed":0,"successful":2},"feature_states":[{"feature_name":"geoip","indices":[".geoip_databases"]}]}}
```

Приведите в ответе список файлов в директории со snapshotами.
```
root@elasticsearch:/usr/share/elasticsearch# ls /var/lib/snapshots
index-0  index.latest  indices  meta-VJAoFRMtRCeFVf6zjnQtHA.dat  snap-VJAoFRMtRCeFVf6zjnQtHA.dat
```
Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.
```
root@elasticsearch:/usr/share/elasticsearch# curl -X DELETE 'http://127.0.0.1:9200/test?pretty'
{
  "acknowledged" : true
}


root@elasticsearch:/usr/share/elasticsearch# curl -X PUT 127.0.0.1:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test-2"
}
```

список индексов: 
```
root@elasticsearch:/usr/share/elasticsearch# curl -X GET 'http://127.0.0.1:9200/_cat/indices?v'
health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases 4RvWxsg6QPm0XO-admXwng   1   0         40            0     37.9mb         37.9mb
green  open   test-2           V1rLprX1T2m7soTz2JzNug   1   0          0            0       226b           226b
```

Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.
```
root@elasticsearch:/usr/share/elasticsearch# curl -X POST 127.0.0.1:9200/_snapshot/netology_backup/elasticsearch/_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'
{
  "accepted" : true
}
```
список индексов: 
```
root@elasticsearch:/usr/share/elasticsearch# curl -X GET 'http://127.0.0.1:9200/_cat/indices?v'                          health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .geoip_databases eHYHbbkIT9C1fOExuFkiRw   1   0         40            0     37.9mb         37.9mb
green  open   test-2           V1rLprX1T2m7soTz2JzNug   1   0          0            0       226b           226b
green  open   test             rapgGDGpQj63kglXFLsdEQ   1   0          0            0       226b           226b
```



<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/6.5/6.5.3-1.jpg">

