# Домашнее задание к занятию 5. «Оркестрация кластером Docker контейнеров на примере Docker Swarm» - Балдин

## Задача 1

Дайте письменые ответы на вопросы:

- В чём отличие режимов работы сервисов в Docker Swarm-кластере: replication и global?
```
В режиме global приложение (сервис) разворачивается с одной репликой на каждом узле кластера. А в режиме replication сервис масштабируется прямым указанием кол-ва реплик. При этом кол-во реплик может быть больше, чем всего нод. В этом случае на каких-то нодах запустится болше одной реплики одного и того же сервиса.
```
- Какой алгоритм выбора лидера используется в Docker Swarm-кластере?
```
Используется Raft - алгоритм поддержания консенсуса. В кластере должно быть не меньше 3-х менеджеров. При этом если лидер становится недоступен, то остальные ноды переходят в статус кандидата. Кандидат отправляет остальным запрос на голосование и большинством выбирается лидером. 
```
- Что такое Overlay Network?
```
Это виртуальная сеть кластера docker swarm
```
## Задача 2

Создайте ваш первый Docker Swarm-кластер в Яндекс Облаке.

Чтобы получить зачёт, предоставьте скриншот из терминала (консоли) с выводом команды:
```
docker node ls
```
## Решение:

```console
[centos@node01 ~]$ sudo docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
xck0ax7lr4kfkfktj4q5pj42t *   node01.netology.yc   Ready     Active         Leader           24.0.2
y5fyyb5nh6dceezhjlrr1lov0     node02.netology.yc   Ready     Active         Reachable        24.0.2
ok2sfadabfqwvk1y9ub9snmog     node03.netology.yc   Ready     Active         Reachable        24.0.2
owkd043jr1bbwhyd6gwpm74z6     node04.netology.yc   Ready     Active                          24.0.2
u64odclxgkd2wlhm5nqyokb85     node05.netology.yc   Ready     Active                          24.0.2
ltv7p18y6ommj96wa425sfme3     node06.netology.yc   Ready     Active                          24.0.2
```

## Задача 3

Создайте ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Чтобы получить зачёт, предоставьте скриншот из терминала (консоли), с выводом команды:
```
docker service ls
```
## Решение:

```console
[centos@node01 ~]$ sudo docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
zfs72vxdn7s6   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0    
aqgtl9rp76n8   swarm_monitoring_caddy              replicated   1/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
zlnfx4ez153o   swarm_monitoring_cadvisor           global       6/6        google/cadvisor:latest                         
tqmgtxmmfscz   swarm_monitoring_dockerd-exporter   global       6/6        stefanprodan/caddy:latest                      
s8qnjmbicvhd   swarm_monitoring_grafana            replicated   1/1        stefanprodan/swarmprom-grafana:5.3.4           
rwuc3urn7r2h   swarm_monitoring_node-exporter      global       6/6        stefanprodan/swarmprom-node-exporter:v0.16.0   
7t088ooyyhk5   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0       
hvd5g2sgcmn7   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0  
```

## Задача 4 (*)

Выполните на лидере Docker Swarm-кластера команду, указанную ниже, и дайте письменное описание её функционала — что она делает и зачем нужна:
```
# см.документацию: https://docs.docker.com/engine/swarm/swarm_manager_locking/
docker swarm update --autolock=true
```

## Решение:

```console
[centos@node01 ~]$ sudo docker swarm update --autolock=true
Swarm updated.
To unlock a swarm manager after it restarts, run the `docker swarm unlock`
command and provide the following key:

    SWMKEY-1-MTIJhaCpfMa9kE2F8dilDvQ8vomRVyI/MFTHcPJeW1s

Please remember to store this key in a password manager, since without it you
will not be able to restart the manager.
```
```
Команда запускает механизм защиты кластера: так после перезапуска менеджера для его разблокировки требуется ввод указанного в выводе команды ключа. Это определенная защита от несанкционированного доступа.
```
