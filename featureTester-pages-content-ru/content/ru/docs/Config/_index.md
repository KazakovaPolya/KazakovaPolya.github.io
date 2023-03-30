---
title : "Конфигурация"
description : ""
weight : 3

---
В файле `conf.json` осуществляется настройка подключений, которые featureTester будет пытаться установить.

Есть возможность настроить следующие подключения:
- [DIAMETER](diam_conf)
- SMPP
- [HTTP](http_conf)
- [MAP](map_conf)

> Стоит отметить, что в файле должны присутствовать только нужные подключения, так как FeatureTester(FT) пытается установить все указанные в конфигурационном файле соединения. То есть если какое-то из указанных подключений будет не настроено и соединение не установится, FT завершит работу с ошибкой.
Предварительная инициализация всех соединений, связана с отсутствием возможности предварительного прочтения всех Gherkin-сценариев, и для возможности комбинирования с использованием шагов разных протоколов.

Пример настройки `conf.json` со всеми доступными подключениями.
```json
config/conf.json
{
  "diam":{
    "default_connection": "default",
    "connections": {
      "default": {
        "server_addr": "sorokin_v:3868",
        "transport": "sctp",
        "origin_host": "node.autotest.protei.ru",
        "origin_realm": "autotest.protei.ru",
        "destination_host": "hss.protei.ru",
        "destination_realm": "protei.ru",
        "host_ip": "192.168.110.156",
        "watchdog_interval": 5
      },
      "second": {
        "transport": "sctp",
        "server_addr": "127.0.0.1:3868",
        "origin_host": "dra_server.host",
        "origin_realm": "dra_server.realm",
        "destination_host": "hss.protei.ru",
        "destination_realm": "protei.ru",
        "product_name": "go-diameter",
        "host_ip": "192.168.110.156"
      }
    }
  },
  "map": {
    "sigtester_url": "http://127.0.0.1:8080/",
    "indicator_address": "0.0.0.0:9191",
    "timeout": 10
  },
  "http": {
    "url": "http://sorokin_v:9090/",
    "server": {
      "address": "0.0.0.0:8989"
    }
  },
  "smpp": {
    "default_connection": "default",
    "connections": {
      "default": {
        "address": ":2775",
        "user": "admin",
        "password": "passwd",
        "resp_timeout": 30,
        "enquire_link": 60
      }
    }
  }
}
```
