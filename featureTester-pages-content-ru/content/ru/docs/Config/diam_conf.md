---
title : "Diameter Configuration"
description : "Настройка Diameter-подключения"
weight : 1

---

Настройка клиентских диаметровых подключений осуществляется в файле `conf.json` по json-ключу "diam".
Возможна настройка нескольких подключений в поле "connections" с присвоением уникального имени. Это позволяет указывать в сценариях используемые подключения по имени. В шагах сценариев имя подключения указывается через "via <connectionName>", если не указано - используется то, что указано в "default_connection".

```
{
  "diam":{
    "default_connection":"name",
    "connections":[...]
  }
}

```

Поле "connections" содержит список подключений следующего вида:

```
"connectionName":  {                // название подключения
    "transport": "",
    "server_addr": "",
    "origin_host": "",
    "origin_realm": "",
    "destination_host": "",
    "destination_realm": "",
    "host_ip": "",
    "watchdog_interval": X,
    "local_addrs": "" 
}
```
Где:
* "transport" - тип серверного подключения. Возможные значения "sctp","tcp4",""(tcp)
* "server_addr" -  адрес DIAMETER-сервера к которому будет подключаться клиент
* "origin_host" - Origin-Host AVP [More in specification section 6.3](https://tools.ietf.org/rfc/rfc6733.txt)
* "origin_realm" - Origin-Realm AVP [More in specification section 6.4](https://tools.ietf.org/rfc/rfc6733.txt)
* "destination_host" - Destination-Host AVP [More in specification section 6.5](https://tools.ietf.org/rfc/rfc6733.txt)
* "destination_realm" - Destination-Realm AVP [More in specification section 6.6](https://tools.ietf.org/rfc/rfc6733.txt)
* "host_ip" - Host-IP-Address AVP [More in specification section  5.3.5](https://tools.ietf.org/rfc/rfc6733.txt). Default "127.0.0.1"
* "local_addrs" - адрес используемый для включения|отключения SCTP multihouming функционала


Пример настройки одного подключения: 
```json
{
  "diam": {
    "default_connection": "default",
    "connections": {
      "default": {
        "transport":"sctp",
        "server_addr": "192.168.110.156:3868",
        "origin_host": "node.autotest.protei.ru",
        "origin_realm": "autotest.protei.ru",
        "destination_host": "hss.protei.ru",
        "destination_realm": "protei.ru",
        "host_ip": "192.168.110.156",
        "watchdog_interval": 5,
        "local_addrs": "172.31.0.1:0"
      }
    }
  }
}
```

Пример настройки двух подключений:

```json
"diam":{
    "default_connection": "connection_1",
    "connections": {
      "connection_1": {
        "server_addr": "192.168.110.156:3878",
        "transport": "sctp",
        "origin_host": "node.autotest.protei.ru",
        "origin_realm": "autotest.protei.ru",
        "destination_host": "mi.protei.ru",
        "destination_realm": "protei.ru",
        "host_ip": "192.168.110.156",
        "local_addrs": "172.34.0.1:0"
      },
      "connection_2": {
        "server_addr": "192.168.110.156:3879",
        "transport": "sctp",
        "origin_host": "OCS.test.ru",
        "origin_realm": "test.ru",
        "destination_host": "mi.protei.ru",
        "destination_realm": "protei.ru",
        "host_ip": "192.168.110.156",
        "local_addrs": "172.34.0.1:0"
      }
    }
}

```
