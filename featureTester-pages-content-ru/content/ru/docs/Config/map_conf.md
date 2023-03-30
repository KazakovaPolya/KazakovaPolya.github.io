---
title : "MAP Configuration"
description : "Настройка MAP-подключения"
weight : 4

---

Для работы с МАР используется как модуль SigtranTester. Взаимодействие с ним происходит по средствам [SigtranTester API](https://mobiledevelop.git.protei.ru/SigTester/docs/)
В файле `conf.json` описываются подключения к SigTester API.

В ключе "sigtester_url" указывается клиентское подключение до SigTesterAPI,через который FT отправляет запросы на формирования МАР сообщений
В поле "indicator_address" указывается серверное подключение, то есть адрес, на который будет SigTester отправлять [индикации](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/httpold/#%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82) о пришедших запросах.

```
  "map": {
    "sigtester_url": "http://192.168.110.156:8080/",
    "indicator_address":"0.0.0.0:9991"
  }

```
Конфигурация Sigtester:
 - в `http.cfg` указывается адрес http-сервера на который будут поступать входящие от FT запросы и http-клиент, через который будут осуществляться отправка индикаций
 - `json.cfg` указать необходимое количество логик(отличное от нуля)
Для проверки получения сообщения используется *функционал индикаций*,  для активации отправки индикации для определенной логики необходимо в конфигурационном файле логики указать эндпоинт и ID http-клиента в секции `[IncomingEvent]`

[Полное описание http-конфигурации Sigtester](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/http/)
