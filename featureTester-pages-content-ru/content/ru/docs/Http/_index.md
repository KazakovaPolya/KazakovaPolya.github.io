---
title : "HTTP"
description : ""
weight : 3

---

Для работы с HTTP реализованы четыре шага взаимодействия(в рамках сценария):

- Отправка синхронного запроса.

&emsp;&emsp;&emsp;&nbsp;Отправка запроса на базе предоставленного тела и заголовков с анализом полученного ответа

- Отправка асинхронного запроса.

&emsp;&emsp;&emsp;&nbsp;Отправка запроса без ожидания ответа(без блокировки логики), проверку результата следует выполнять с помощью шага - *Получение асинхронного ответа*

- Получение асинхронного ответа.

&emsp;&emsp;&emsp;&nbsp;Ожидание ответа и сверка с ожидаемыми значениями, указанными в теле шага

- Обработка входящего запроса.

&emsp;&emsp;&emsp;&nbsp;Обработка запроса: сравнение с ожидаемыми значениями и отправка ответа в соответствии c указанными в теле полями

Поддерживается:
- отправка и получение запросов HTTP2.0
- дополнительная проверка наличия/отсутствия ключей/тегов;
- работа с json/XML
- работа с [multipart/related](multipart)

Для работы с XML следует в заголовке Content-Type указать значение "`application/xml`". Само тело указывается как строковое значение ключа body.
Пример:

```json
{
    "endpoint":"/6",
    "request": {
        "headers": {
            "Content-Type":"application/xml"
        },
        "body": "<?xml version=\"1.0\" ?><Sh-Data>\n  <Extension>\n <TADSinformation>\n <IMSVoiceOverPSSessionSupport>1</IMSVoiceOverPSSessionSupport> \n </TADSinformation>\n</Extension>\n</Sh-Data>"
    },
    "response": {
        "body": "<?xml version=\"1.0\" ?><Sh-Data>\n  <Extension>\n <TADSinformation>\n <IMSVoiceOverPSSessionSupport>1</IMSVoiceOverPSSessionSupport> \n </TADSinformation>\n</Extension>\n</Sh-Data>"
    }
}
```

### 1. Отправка синхронного запроса

Регулярное выражение шага - `^send http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE)(| via \w+):$`.

Данный шаг выполняет отправку (send) http запроса. 
Регулярное выражение `http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE)` представляет собой шаблон "protocol.RequestType", где указывается метод отправляемого запроса (GET, POST, ...). `$` в выражении обозначает описание/тело шага.

В теле шага имеются следующие ключи:
- endpoint (адрес эндпоинта. Запрос отправляется на url из conf.json + endpoint)
- request  (описание запроса)
- response (описание ответа)

Поле "request" содержит следующие ключи:
- headers (http-заголовки, такие как "Allow", "Content-Type", ...). В ключе "Content-Type" указывается тип передаваемых данных ("application/json" или "application/xml"). 
- body (может содержать в себе как json-структуру, так и xml-структуру (передается как строка)).
- multipart (содержит json-структуру для отправки [составных сообщений](multipart)).
- query (содержит query-параметры следующего формата):

```json
{
  "query": {
    "Param1": [
      "value1",
      "value2"
    ],
    "requester-nf-type": ["SCP"],
    "target-nf-type": ["SMF"],
    "target-plmn-list":["[{\"mcc\":\"400\",\"mnc\":\"02\"}]"]
  }
}
```
В качестве тела отправляемого запроса поле "request" должно содержать один из ключей: "body" или "multipart".


Поле "response" содержит следующие ключи:
- "body" [опциональный] - содержит ожидаемую json/xml-структуру
- "headers" [опциональный] - http-заголовки аналогично "request"
- "status_code" [опциональный] - http-статус код выполнения запроса

Пример использования с структурой json-тела:

```json
When send http.POST:
"""
{
    "endpoint": "/ProfileService/GetProfile",
    "request": {
        "body":{
            "imsi": "123450000000001",
            "roaming-sgsn-info": 1
        },
        "headers":{
            "Allow":"POST,GET"
        }
    },
    "response": {
        "body":{
            "status": "OK",
            "imsi": "123450000000001",
            "roaming-sgsn-info":{
                "vplmn": "10101",
                "dmMmeHost": "mme1.autotest.protei.ru",
                "dmMmeRealm": "autotest.protei.ru"
            }
        },
        "headers":{
            "Allow":"GET"
        },
        "status_code":200
    }
}
```

### 2. Отправка асинхронного запроса

Регулярное выражение шага - `async send http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE)(| via \w+):`.

Данный шаг выполняет неблокирующую отправку (async send) http запроса (без ожидания ответа на него). 
Регулярное выражение `http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE)` представляет собой шаблон "protocol.RequestType", где указывается метод отправляемого запроса (GET, POST, ...). `$` в выражении обозначает описание/тело шага.

В теле шага указываются следующие ключи:
- endpoint [обязательный] - адрес эндпоинта, на который посылается запрос
- request  [обязательный] - тело запроса

Поле "request" содержит следующие ключи:
- headers [опционально] - http-заголовки, такие как "Allow", "Content-Type", .... В ключе "Content-Type" указывается тип передаваемых данных ("application/json" или "application/xml"). 
- body [обязательный] - может содержать в себе как json-структуру, так и xml-структуру (передается как строка).
- multipart [обязательный (если не указан ключ "body")] - содержит json-структуру для отправки [составных сообщений](multipart).
- query [опциональный] - содержит query-параметры следующего формата

```json
{
  "query": {
    "Param1": [
      "value1",
      "value2"
    ],
    "requester-nf-type": ["SCP"],
    "target-nf-type": ["SMF"],
    "target-plmn-list":["[{\"mcc\":\"400\",\"mnc\":\"02\"}]"]
  }
}
```

Пример использования с структурой json-тела:

```json
When async send http.POST via client2:
"""
{
    "endpoint": "/SubscriberService/GetUserLocation",
    "request": {
        "body":{
            "imsi":"123450000001703",
            "epsUserState":true,
            "epsLocationInformation": true,
            "epsCurrentLocation": true
        },
        "headers":{}
    }
}
"""
```

### 3. Получение асинхронного ответа

Регулярное выражение шага - `receive async http response(| via \w+):$`.

Данный шаг обрабатывает ответ на ранее отправленный асинхронный http-запрос. `$` в регулярном выражении обозначает описание/тело шага.

В теле шага указываются следующие ключи:
- "body" [опциональный] - содержит ожидаемую json/xml-структуру
- "headers" [опциональный] - http-заголовки аналогично
- "status_code" [опциональный] - http-статус код выполнения запроса

Пример использования с структурой json-тела:

```json
And receive async http response:
"""
{
    "body":{
        "status":"NOT_AVAILABLE",
        "errorMessage":"MME returned result-code 5012"
    },
    "headers":{
        "Allow":"GET"
    },
    "status_code":200
}
"""
```

### 4. Получение входящего запроса
Регулярное выражение шага - `handle http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE) request(| via \w+):$`.

Данный шаг выполняет обработку (handle) входящего http-запроса. Регулярное выражение `http.(GET|POST|PATCH|PUT|DELETE|HEAD|OPTIONS|CONNECT|TRACE)` представляет собой шаблон "protocolVersion.RequestType", где часть до точки - это версия используемого http-протокола, а часть после - тип отправляемого запроса (GET, POST, ...). `$` в выражении обозначает описание/тело шага.

В теле шага используются следующие ключи:
- endpoint [обязательный] - адрес, на который посылаются запрос
- receive [обязательный] - тело получаемого запроса
- send [опциональный] - тело отправляемого ответа

По умолчанию тип запроса json.

Поле "receive" содержит следующие ключи:
- headers (http-заголовки, такие как "Allow", "Content-Type", ...). В ключе "Content-Type" указывается тип передаваемых данных ("application/json" или "application/xml"). 
- body (может содержать в себе как json-структуру, так и xml-структуру (передается как строка)).
- query (содержит query-параметры следующего формата)

```json
{
  "query": {
    "Param1": [
      "value1",
      "value2"
    ],
    "requester-nf-type": ["SCP"],
    "target-nf-type": ["SMF"],
    "target-plmn-list":["[{\"mcc\":\"400\",\"mnc\":\"02\"}]"]
  }
}
```

Поле "send" содержит следующие ключи:
- "body" [опциональный] - содержит ожидаемую json/xml-структуру
- "headers" [опциональный] - http-заголовки аналогично 
- "status_code" [опциональный] - http-статус код выполнения запроса

Если часть "send" не задана, то по умолчанию будет отправлен ответ, содержащий `"status_code": 200`.

Пример использования с структурой json-тела:

```json
When handle http.POST request:
"""
{
    "endpoint": "/users",
    "receive":{
        "body":{
            "status":"error231"
        },
        "headers":{
            "Content-Type":"application/json"
        },
        "query":{
            "Param1":[
                "value1",
                "value2"
            ],
            "param2":[
                "value3"
            ]
        }
    },
    "send":{
        "body":{
            "response":"good"
        },
        "status_code": 200,
        "headers":{
            "Server":"featureTester"
        }
    }
}
"""
```
____
### Дополнительные проверки отсутствия/наличия тега в структуре

Для осуществления данной проверки добавлены ключи `null`, `not_null`.
Списком указываются имена ключей как список строк. Вложенность ключей указывается через точку.

```json
{
  "null": ["some_key"]
}
```

Пример 1. JSON-структура

```json
{
    "a":1,
    "b":{
        "c":2
    }
}
```

Проверка отсутствия ключа "а" в сценарии:

```json
{
"response": {
      "null": ["a"],
      "body":{
        "status": "OK",
        "imsi": "123450000002603",
        "roaming_not_allowed": 1
      }
    }
}
```

Проверка наличия ключа "c" в сценарии:

```json
{
"response": {
      "not_null": ["b.c"],
      "body":{
        "status": "OK",
        "imsi": "123450000002603",
        "roaming_not_allowed": 1
      }
    }
}
```

Пример 2. XML-структура

```xml
<getRegistrationResp>
    <subscriber>
        <imsi>100000014002201</imsi>
        <msisdn>10001402201</msisdn>
    </subscriber>
</getRegistrationResp>
```

Проверка отсутствия тега "subscriber" в сценарии:

```json
{
  "response": {
    "null": [
      "getRegistrationResp.subscriber"
    ],
    "body": "<getRegistrationResp> <subscriber> <imsi>100000014002201</imsi> <msisdn>10001402201</msisdn> </subscriber> </getRegistrationResp>"
  }
}
```

Проверка наличия тега "imsi" в сценарии:

```json
{
  "response": {
      "not_null": ["getRegistrationResp.subscriber.imsi"],
      "body": "<getRegistrationResp> <subscriber> <imsi>100000014002201</imsi> <msisdn>10001402201</msisdn> </subscriber> </getRegistrationResp>"
    }
}
```


## Примеры использования 

### 1. Авторизация пользователя с неправильным паролем

Отправим синхронный POST-запрос с некорректной информацией о логине и пароле пользователя для авторизации на сервисе. В качестве ответа должны получить ошибку "Неверный логин или пароль".

Таким образом, для реализации данного сценария следует выполнить следующие шаги:

Step 1: Отправить синхронный POST-запрос с указанием логина и пароля и получить ответ


![Авторизация с неверным паролем](/featureTester/images/http_expl_3.png)


Назовем Feature - `Авторизация пользователя`, Scenario - `Ошибка при вводе некорректных данных авторизации`. Под названием укажем краткое описание выполняемых шагов.

Файл feature будет содержать следующее:

```
Feature: Авторизация пользователя

Scenario: Ошибка при вводе некорректных данных авторизации
  Отправляем POST-запрос /login с неверными параметрами "login" и "password" и получаем ответ c ошибкой

  When send http.POST:
  """
  {
    "endpoint": "/login",
    "request": {
      "body": {
        "login": "user",
        "password": "qwerty"
      }
    },
    "response": {
      "body": {
        "error": "Неверный логин или пароль"
      }
    },
    "status_code":401
  }
  """

```

### 2. Получение информации о погоде 22.02.2022 из приложения `Weather`

Cценарий следует разбить на следующие шаг(и):

Step 1: Отправить GET-запрос с указанием нужного дня и получить ответ

Соответственно, в теле запроса будет содержатся запрашиваемый день, в теле ответа - температура в этот день.

![Получение информации о погоде](/featureTester/images/http_expl_1.png)

Назовем Feature - `Получение информации о погоде`, Scenario - `Получение информации о погоде в конкретный день`. Под названием укажем краткое описание выполняемых шагов.

Файл feature будет содержать следующее описание:

```
Feature: Получение информации о погоде

Scenario: Получение информации о погоде в конкретный день
  Отправляем GET-запрос GetWeather с параметром "day": "02/22/2022"
  Получаем ответ с информацией о температуре в этот день

  When send http.GET:
  """
  {
    "endpoint": "/GetWeather",
    "request": {
      "body": {
        "day": "02/22/2022"
      }
    },
    "response": {
      "body": {
        "day": "02/22/2022",
        "temperature": "-2"
      }
    }
  }
  """
```

### 3. Обработка входящего запроса 

Предположим, что наш запрос GetWeather будет перенаправляться приложением `Weather` обратно на FeatureTester. Таким образом, нужно обработать перенаправленный запрос и ответить на него. Затем нужно получить перенаправленный приложением ответ.

Таким образом, для реализации данного сценария следует выполнить следующие шаги:

Step 1: Отправить асинхронный GET-запрос с указанием дня

Step 2: Обработать входящий запрос и ответить на него 

Step 3: Получить асинхронный ответ на GET-запрос


![Получение информации о погоде](/featureTester/images/http_expl_2.png)


Назовем Feature - `Получение информации о погоде`, Scenario - `Обработка запроса о получении информации о погоде в конкретный день`. Под названием укажем краткое описание выполняемых шагов.

Файл feature будет содержать следующее:

```
Feature: Получение информации о погоде

Scenario: Получение информации о погоде в конкретный день и время
  Отправляем GET-запрос GetWeather с параметром "day": "02/22/2022"
  Обрабатываем перенаправленный запрос и отправляем ответ "temperature":"-2"
  Получаем ответ с информацией о температуре в конкретный день

  When async send http.GET:
  """
  {
    "endpoint": "/GetWeather",
    "request": {
      "body": {
        "day": "02/22/2022"
      }
    }
  }
  """

  Then handle http request:
  """
  {
      "endpoint": "/GetWeather",
      "send":{
          "body":{
              "day": "02/22/2022"
          },
          "headers":{
              "Content-Type":"application/json"
          },
      },
      "receive":{
          "body":{
              "day": "02/22/2022",
              "temperature": "-2"
          },
          "status_code": 200,
      }
  }
  """

  And receive async http response:
  """
  {
    "body":{
        "day": "02/22/2022",
        "temperature": "-2"
    },
    "status_code":200
  }
  """

```

