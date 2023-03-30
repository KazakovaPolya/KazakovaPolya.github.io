---
title : "HTTP Configuration"
description : "Настройка HTTP-подключения"
weight : 4

---

Настраиваются http-подключения в секции "http" файла `conf.json`.

В ключе "url" указывается соответствующее клиентские подключения. 
Поле "server" настраиваются серверные части для обработки входящих запросов. Является опциональным и настраивается по необходимости.

```
{
"http": {
    "nrf": {
      "nrf_1": {
        "address": "http://127.0.0.1:8981",
        "endpoint": "/nrf",
        "timeout": 10,
        "register": {
          "headers": {
            "Allow": "PUT"
          },
          "body": "BODY HELLO"
        },
        "heartbeat": {
          "body": "BODY HELLO"
        }
      }
    },
    "clients":{
      "default":"default",
      "list":{
        "default":{
          "url":":8989",
          "version":2,
          "nrf":"nrf_1",
          "tls":{
            "insecure_skip_verify": true
          }
        }
      }
    },
    "servers":{
      "default": "server1",
      "list": {
        "server1": {
          "version": 2,
          "address":":8981",
          "tls":{
            "cert": "internal/feature/api/cert.pem",
            "key": "internal/feature/api/key.pem"
          }
        },
        "server2": {
          "version": 2,
          "address":":9991"
        }
      }
    }
}
```

Описание http конфигураций для тестируемых приложений:
 - написанных на C++ [http.cfg](https://mobiledevelop.git.protei.ru/Protei_HTTP/docs/config/http/).
 - [Java-приложений](https://mobiledevelop.git.protei.ru/Protei_HLR_HSS/docs/api/config/).
