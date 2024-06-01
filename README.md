# 11-04 Стасенко Григорий Домашнее задание к занятию «» 11-04

## Задание 1. Установка RabbitMQ
Используя Vagrant или VirtualBox, создайте виртуальную машину и установите RabbitMQ. Добавьте management plug-in и зайдите в веб-интерфейс.

Итогом выполнения домашнего задания будет приложенный скриншот веб-интерфейса RabbitMQ.

---
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/e1c33a8c-08a4-4cf6-a52f-504ed659df2c)

---

## Задание 2. Отправка и получение сообщений
Используя приложенные скрипты, проведите тестовую отправку и получение сообщения. Для отправки сообщений необходимо запустить скрипт producer.py.

Для работы скриптов вам необходимо установить Python версии 3 и библиотеку Pika. Также в скриптах нужно указать IP-адрес машины, на которой запущен RabbitMQ, заменив localhost на нужный IP.

$ pip install pika
Зайдите в веб-интерфейс, найдите очередь под названием hello и сделайте скриншот. После чего запустите второй скрипт consumer.py и сделайте скриншот результата выполнения скрипта

В качестве решения домашнего задания приложите оба скриншота, сделанных на этапе выполнения.

Для закрепления материала можете попробовать модифицировать скрипты, чтобы поменять название очереди и отправляемое сообщение.

---
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/9cacd559-8bf6-45ec-8435-887613b19213)
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/edfda63b-8b3f-4e94-8c7d-6bd054bbca55)

---


## Задание 3. Подготовка HA кластера
Используя Vagrant или VirtualBox, создайте вторую виртуальную машину и установите RabbitMQ. Добавьте в файл hosts название и IP-адрес каждой машины, чтобы машины могли видеть друг друга по имени.

Пример содержимого hosts файла:

$ cat /etc/hosts
192.168.0.10 rmq01
192.168.0.11 rmq02
После этого ваши машины могут пинговаться по имени.

Затем объедините две машины в кластер и создайте политику ha-all на все очереди.

В качестве решения домашнего задания приложите скриншоты из веб-интерфейса с информацией о доступных нодах в кластере и включённой политикой.

Также приложите вывод команды с двух нод:

$ rabbitmqctl cluster_status
Для закрепления материала снова запустите скрипт producer.py и приложите скриншот выполнения команды на каждой из нод:

$ rabbitmqadmin get queue='hello'
После чего попробуйте отключить одну из нод, желательно ту, к которой подключались из скрипта, затем поправьте параметры подключения в скрипте consumer.py на вторую ноду и запустите его.

Приложите скриншот результата работы второго скрипта.


---
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/baac94c3-f87e-4d1d-a2f1-386f277f0855)
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/b0cef132-a536-4982-8648-6f9c853dd81f)
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/9bff3c8f-9b7b-4dc3-8484-a803a38b1194)
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/26a6e3a7-a490-43b6-bbe0-a243fb56cbcc)

Ноды поднимались в Docker, порты проброшены только у первой ноды. При отключении первой потеряется и вторая.
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/3bda6ae9-3696-4056-83a6-fbe2900cbdd3)

![image](https://github.com/Nightnek/HW_11_04/assets/127677631/09831e58-02c9-43a9-b87a-66c33f9cc15e)
![image](https://github.com/Nightnek/HW_11_04/assets/127677631/59eeb1bf-4046-4724-a89f-b4c4faca5b99)


````
# rabbitmqctl cluster_status
Cluster status of node rabbit@rabbit1 ...
Basics

Cluster name: rabbit@rabbit1
Total CPU cores available cluster-wide: 16

Disk Nodes

rabbit@rabbit1
rabbit@rabbit2

Running Nodes

rabbit@rabbit1
rabbit@rabbit2

Versions

rabbit@rabbit1: RabbitMQ 3.12.14 on Erlang 25.3.2.12
rabbit@rabbit2: RabbitMQ 3.12.14 on Erlang 25.3.2.12

CPU Cores

Node: rabbit@rabbit1, available CPU cores: 8
Node: rabbit@rabbit2, available CPU cores: 8

Maintenance status

Node: rabbit@rabbit1, status: not under maintenance
Node: rabbit@rabbit2, status: not under maintenance

Alarms

(none)

Network Partitions

(none)

Listeners

Node: rabbit@rabbit1, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit1, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit1, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit1, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit@rabbit2, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit2, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit2, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit2, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

Feature flags

Flag: classic_mirrored_queue_version, state: enabled
Flag: classic_queue_type_delivery_support, state: enabled
Flag: direct_exchange_routing_v2, state: enabled
Flag: drop_unroutable_metric, state: enabled
Flag: empty_basic_get_metric, state: enabled
Flag: feature_flags_v2, state: enabled
Flag: implicit_default_bindings, state: enabled
Flag: listener_records_in_ets, state: enabled
Flag: maintenance_mode_status, state: enabled
Flag: quorum_queue, state: enabled
Flag: restart_streams, state: enabled
Flag: stream_queue, state: enabled
Flag: stream_sac_coordinator_unblock_group, state: enabled
Flag: stream_single_active_consumer, state: enabled
Flag: tracking_records_in_ets, state: enabled
Flag: user_limits, state: enabled
Flag: virtual_host_metadata, state: enabled
#

# rabbitmqctl cluster_status
Cluster status of node rabbit@rabbit2 ...
Basics

Cluster name: rabbit@rabbit2
Total CPU cores available cluster-wide: 16

Disk Nodes

rabbit@rabbit1
rabbit@rabbit2

Running Nodes

rabbit@rabbit1
rabbit@rabbit2

Versions

rabbit@rabbit1: RabbitMQ 3.12.14 on Erlang 25.3.2.12
rabbit@rabbit2: RabbitMQ 3.12.14 on Erlang 25.3.2.12

CPU Cores

Node: rabbit@rabbit1, available CPU cores: 8
Node: rabbit@rabbit2, available CPU cores: 8

Maintenance status

Node: rabbit@rabbit1, status: not under maintenance
Node: rabbit@rabbit2, status: not under maintenance

Alarms

(none)

Network Partitions

(none)

Listeners

Node: rabbit@rabbit1, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit1, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit1, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit1, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit@rabbit2, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit2, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit2, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit2, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

Feature flags

Flag: classic_mirrored_queue_version, state: enabled
Flag: classic_queue_type_delivery_support, state: enabled
Flag: direct_exchange_routing_v2, state: enabled
Flag: drop_unroutable_metric, state: enabled
Flag: empty_basic_get_metric, state: enabled
Flag: feature_flags_v2, state: enabled
Flag: implicit_default_bindings, state: enabled
Flag: listener_records_in_ets, state: enabled
Flag: maintenance_mode_status, state: enabled
Flag: quorum_queue, state: enabled
Flag: restart_streams, state: enabled
Flag: stream_queue, state: enabled
Flag: stream_sac_coordinator_unblock_group, state: enabled
Flag: stream_single_active_consumer, state: enabled
Flag: tracking_records_in_ets, state: enabled
Flag: user_limits, state: enabled
Flag: virtual_host_metadata, state: enabled
# 
````
---

