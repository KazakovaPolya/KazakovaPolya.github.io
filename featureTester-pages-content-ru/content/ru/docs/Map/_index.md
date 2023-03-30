---
title : "MAP"
description : ""
weight : 3

---
Для работы с МАР используется SigtranTester как модуль. Взаимодействие с ним происходит по средствам [SigtranTester API](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/http/)

![Взаимодействие через SigTester](/featureTester/images/sigtester.png)

>При необходимости расширения числа поддерживаемых сообщений/входящих индикаций следует создавать YT-задачу в проекте [Mobile_SigTester](https://youtrack.protei.ru/issues?q=%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82:%20Mobile_SigTester)

Для работы с MAP реализованы три шага взаимодействия(в рамках сценария):
- Отправка МАР-запроса 

&emsp;&emsp;&emsp;&nbsp;Формирование запроса по описанию.
- Получение ответа. 

&emsp;&emsp;&emsp;&nbsp;Ожидание ответа по предоставленному идентификатору(IMSI,MSISDN,IMEI), и сверка полученных значений MAP-полей с ожидаемыми.
- Обработка входящей индикаций запроса.

&emsp;&emsp;&emsp;&nbsp;Обработка входящего запроса:сверка и формирование ответа на базе указанных MAP-значений.


### 1. Отправка запроса

Регулярное выражение шага - **`send map.(\w+):$`**.

Данный шаг выполняет отправку (send) MAP-запроса без ожидания ответа на него. Регулярное выражение `map.(\w+)` представляет собой шаблон "map.RequestLogic", где часть до точки - это используемый протокол (map), а часть после - тип отправляемого запроса (UpdateLocation, InsertSubscriberData, ...). `$` в выражении обозначает описание шага.

В описании шага в json-формате указываются значения уровней SCCP, MAP, TCAP.

Параметры сообщений SCCP, которые используются подсистемой MAP для обеспечения роуминга, включают данные об адресе вызываемой/вызывающей стороны:
- [CdPA (Called Party Address)](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/http/#sccp_pa)
- [CgPA (Calling Party Address)](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/http/#sccp_pa) 

Пример использования со структурой json-тела:

```
When send map.UpdateLocation:
"""
{
    "CgPA":{
        "Address":"799988777333",
        "TT":1,
        "RI":0,
        "NP":1,
        "SSN":8
    },
    "CdPA":{
        "Address":"799988777333",
        "TT":1,
        "RI":0,
        "NP":1,
        "SSN":8
    },
    "IMSI":"123450000000001",
    "VLR":"799988777333",
    "MSC":"799988777333",
    "OpCode": 2
}
"""
```

### 2. Получение ответа

Регулярное выражение шага - **`receive map.(\w+):$`**.

Данный шаг обрабатывает ответ на ранее отправленный map-запрос.
Регулярное выражение `map.(\w+)` представляет собой шаблон "map.RequestLogic", где часть до точки - это используемый протокол (map), а часть после - тип запроса, на который ожидается ответ (UpadateLocation, InsertSubscriberData, ...). `$` в выражении обозначает описание шага.

В описание шага используются следующие ключи:
- "response" [обязательный] - содержит ожидаемую json-структуру [MAP-значений](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/http/#%D0%BE%D1%82%D0%BF%D1%80%D0%B0%D0%B2%D0%BA%D0%B0-%D1%81%D0%BE%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%B8%D0%B9-%D0%B2-sigtran)
- "tcap" - содержит ожидаемую [json-структуру значений уровня TCAP](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/httpold/#tcap) со следующими значениями:
  - Status [обязательный] - статус выполнения запроса
  - ErrorCode [обязательный] - код ошибки
  - MessageType [опциональный] - тип сообщения
  - AC [опциональный] - Причина аборта
  - OTID [опциональный] - Orig TCAP ID
  - DTID [опциональный] - Dest TCAP ID
  - OpCode [опциональный] - значение кода выполняемой операции MAP
  - SequenceInfo [опциональный] - Последний ли компонент

- "sccp" [опциональный] - содержит ожидаемую [json-структуру значений уровня SCCP](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/httpold/#sccp) (CgPA, CdPA)


Пример использования с структурой json-тела:

```
    Then receive map.UpdateLocation:
    """
    {
        "response":{
            "HLR_Number":1
        },
        "tcap":{
            "Status":0,
            "ErrorCode":0,
            "MessageType":"Continue"
        }
    }
    """
```

### 3. Обработка входящего запроса

Регулярное выражение шага - **`handle map.(\w+):$`**.

Данный шаг выполняет обработку (handle) входящего map-запроса. Регулярное выражение `map.(\w+)` представляет собой шаблон "map.RequestLogic", где часть до точки - это используемый протокол (map), а часть после - тип запроса, на который ожидается ответ (UpadateLocation, InsertSubscriberData, ...). `$` в выражении обозначает описание шага.


В описание шага в json формате используются ключи:
- "receive" [обязательный] - содержит ожидаемую json-структуру
- "send" [опциональный] - содержит ожидаемую json-структуру, аналогичную структуре шага "Отправка запроса"

Поле "receive" содержит обязательный ключ "message", содержащий [следующие поля](https://mobiledevelop.git.protei.ru/SigTester/docs/common/desc/interface/httpold/#pdu):
- MAP [обязательный] (Проверяемые MAP-значения, при этом запрос обязательно должен содержать идентификатор пользователя (IMSI/MSISDN/IMEI)). 
- SCCP [опциональный] (информация о CdPA и CgPA).
- TCAP [опциональный] (информация уровня TCAP)

Если поле "send" не указано, то по умолчанию отправляются значения:

```json
{
  "EndType": 0,
  "ErrorCode": 0
}
```

Пример использования с структурой json-тела:

```
And handle map.InsertSubscriberData:
"""
{
    "receive":{
        "message": {
            "MAP":{
                "MSISDN":"94321000000001",
                "NetworkAccessMode":"packetAndCircuit",
                "SubscriberStatus":"serviceGranted"
                },
            "SCCP":{
                "CgPA":{
                    "Address":"799988777333",
                    "TT":1,
                    "RI":0,
                    "NP":1,
                    "SSN":8
                },
                "CdPA":{
                    "Address":"799988777333",
                    "TT":1,
                    "RI":0,
                    "NP":1,
                    "SSN":8
                }
            },
            "TCAP":{
                "AC": "",
                "OTID": "00000004",
                "DTID": "ffffffff",
                "MessageType": "Continue",
                "OpCode": 7,
                "SequenceInfo": "LastComponent"
            }
        }
    },
    "send":{
        "EndType": 0,
        "ODB_GeneralData": 15,
        "RegionalSubscriptionResponse":1
    }
}
"""
```

[Актуальный список поддерживаемых логик](https://git.protei.ru/MobileDevelop/go-models/-/blob/master/api/sigtranTester/endpoints.go).

Необходимо отметить, что проверка значений осуществляется только по указанным в json-теле полям.

## Примеры использования FT

#### 1. Проверка функционала UpdateLocation(далее UL)

Рассмотрим изменение местоположения пользователя по запросу UL.Для этого следует отправить запрос UL на HLR, и обработать ответ.

Сценарий можно разбить на следующие шаги:

Step 1: Отправить map.UpdateLocation с IMSI пользователя

Step 2. Получить ответ на запрос map.UpdateLocation


![Обновление данных о местоположении](/featureTester/images/map_expl_1.png)

Назовем Feature - `Обновление местоположения абонента`, Scenario - `Успешное изменение местоположения абонента`. Под названием укажем краткое описание выполняемых шагов.

Файл feature будет содержать следующее:

```
Feature: Обновление местоположения абонента

Scenario: Успешное изменение местоположения абонента
  Отправляем запрос map.UpdateLocation
  Получаем ответ map.UpdateLocation

  When send map.UpdateLocation:
    """
    {
        "CgPA":{"Address":"799988777333", "SSN":7},
        "CdPA":{"Address":"799988777333", "SSN":6},
        "IMSI":"123450000002601",
        "VLR":"799988777333",
        "MSC":"799988777333"
    }
    """

    Then  receive map.UpdateLocation:
    """
    {
	  "response":{
	    "HLR_Number":{"Number":"1"}
	  }
	}
    """
```

#### 2. Регистрация абонента (UpdateLocation + InsertSubscriberData)

Рассмотрим процедуру регистрации абонента с получением данных о нём в ISD. Для этого: отправим запрос UpdateLocation на HLR,и обработаем от HLR запрос InsertSubscriberData с данными пользователя, и получим ответ на ранее отправленный запрос UpdateLocation.

Для реализации данного сценария следует выполнить следующие шаги:

Step 1: Отправить map.UpdateLocation

Step 2. Обработать входящий запрос map.InsertSubscriberData и ответить на него

Step 3. Получить ответ на запрос map.UpdateLocation


![Регистрация абонента](/featureTester/images/map_expl_2.png)

Назовем Feature - `Регистрация абонента`, Scenario - `Успешная регистрация абонента`. Под названием укажем краткое описание выполняемых шагов.

Файл feature будет содержать следующее:

```
Feature: Регистрация абонента

Scenario: Успешная регистрация абонента
  Отправляем запрос map.UpdateLocation
  Получаем запрос map.InsertSubscriberData и отправляем ответ
  Получаем ответ на запрос map.UpdateLocation

  When send map.UpdateLocation:
    """
    {
        "CgPA":{"Address":"799988777333", "SSN":7},
        "CdPA":{"Address":"799988777333", "SSN":6},
        "IMSI":"123450000002601",
        "VLR":"799988777333",
        "MSC":"799988777333"
    }
    """

    Then handle map.InsertSubscriberData:
    """
    {
       "receive":{
        "message":{
            "MAP":{
              "InsertSubscriberData":{
                "MSISDN":{
                 "Number":"94321000002601"
                 },
                "NetworkAccessMode":"packetAndCircuit",
                "SubscriberStatus":"serviceGranted"
              }
            }
          }
        }
    }
    """

  And  receive map.UpdateLocation:
  """
  {
    "response":{
      "HLR_Number":{"Number":"1"}
    }
  }
  """

```

Другие примеры можно посмотреть [здесь](https://git.protei.ru/MobileDevelop/functional-tests/-/blob/master/hss/features/spec/TS29.002/UpdateLocation.feature).
