---
title : "Diameter"
description : ""
weight : 3

---

Для работы с протоколом DIAMETER реализованы три шага взаимодействия(в рамках сценария):

- Отправка запроса.

&emsp;&emsp;&emsp;&nbsp;Формирование и отправка запроса на базе предоставленного json-описания(указанных в виде структуры AVP-полей).

- Получение ответа.

&emsp;&emsp;&emsp;&nbsp;Получение ответа по предоставленному идентификатору (Username и тд, в зависимости от сообщения), и сверка полученного значений AVP-полей с указанными/ожидаемыми.

- Обработка входящего запроса.

&emsp;&emsp;&emsp;&nbsp;Обработка поступившего запроса(от удалённой стороны), формирование ответа по json-описанию AVP-полей.

> Для шагов отправки и обработки входящего запроса время ожидания - 10 секунд

### Структура json описания сообщений
Структура сообщений кодогенерируется на базе xml словаря. При описании в сценариях структуры запроса/ответа можно указать лишь доступные для данного сообщения AVP поля. Неиспользуемые или с некорректным наименованием будут проигнорированны.В случае если для обязательного AVP не будет указано значение: будет использоваться дефолтное в зависимости от типа данных(int-0,string-'' и тп.)

При указании структуры ожидаемых сообщений можно упустить ряд опциональных AVP, которые не критичны для вашего сценария.
В таком случае, не будет осуществляться проверка их значений.

### Наименование json тэгов

В кодогенерацию json-тэгов и структур DIAMETER сообщений на базе xml-словаря была заложена функция унификации наименований. Суть которой заключается в том, что из исходного AVP-наименования убираются дефисы, внё зависимости от сокращений каждое слово в отдельности приводится к TitleCase (без исключений для артиклей,сокращений и т.д.)

На примере AVP “Homogeneous-Support-of-IMS-Voice-Over-PS-Sessions” Поле структуры сообщения будет иметь имя “HomogeneousSupportOfImsVoiceOverPsSessions”,а json-тэг “homogeneousSupportOfImsVoiceOverPsSessions”

### 1. Отправка запроса

Регулярное выражение шага сценария - `send (\w+\.[A-Z]{2}R)(| via \w+):$`.

Данный шаг выполняет отправку (send) запроса. Регулярное выражение `(\w+\.[A-Z]{2}R)` представляет собой шаблон "interfaceName.XXR", где часть до точки - это название интерфейса, а часть "XX" - тип отправляемого запроса. Например, s6a.ULR

Часть "via" является опциональной и указывает подключение, в которое происходит отправка запроса. По умолчанию используется подключение, указанное в конфигурационном файле как "default_connection".

В теле запроса описываются лишь поддерживаемые запросом AVP (Attribute–Value Pairs).

Пример использования с структурой json-тела:

```
    When send s6a.ULR:
    """
    {
        "authSessionState":1,
        "originHost":"node.autotest.protei.ru",
        "originRealm":"autotest.protei.ru",
        "destinationHost":"node.autotest.protei.ru",
        "destinationRealm":"autotest.protei.ru",
        "userName":"123450000000001",
        "ratType":1004,
        "visitedPlmnId":10101,
        "ulrFlags":2
    }
    """
```

### 2. Получение ответа

Регулярное выражение шага сценария - `receive (\w+\.[A-Z]{2}A)(| via \w+):$`.

Данный шаг выполняет получение (receive) ответа на ранее(в одном из предыдущих шагов сценария) отправленный запрос. Регулярное выражение `(\w+\.[A-Z]{2}A)` представляет собой шаблон "interfaceName.XXA", где часть до точки - это название интерфейса, а часть после - тип ожидаемого ответа.

Структура ответа содержит следующие ключи:
- message [ обязательный ](содержит список AVP(полей структуры), наличие которых проверяется в ответе)
- null [ опциональный ] (проверка на отсутствие значений AVP в полученном сообщении)
- notNull [ опциональный ] (проверка наличия указанных значений AVP в полученном сообщение, т.е. опциональное AVP было получено, но проверка конкретного значения не интересна или не возможна )



Пример использования с структурой json-тела:

```
    Then receive s6a.AIA:
    """
    {
      "message":
      {
          "resultCode":2001,
          "authSessionState":1,
          "originHost":"hss.autotest.protei.ru",
          "originRealm":"autotest.protei.ru"
      },
      "notNull":[
          "AuthenticationInfo.EUtranVector.Rand",
          "AuthenticationInfo.EUtranVector.Xres",
          "AuthenticationInfo.EUtranVector.Autn",
          "AuthenticationInfo.EUtranVector.Kasme"
      ],
      "null":["ExperimentalResult"]
    }
    """
```      


Как говорилось ранее, наименование AVP полей в проверках null/notNull отличается от используемой в "message":
  - первая буква - заглавная
  - вложенность указывается через точку

Рассмотрим пример сообщения со следующими AVP:
```
{
  "authSessionState":1,
  "originHost":"srcHost",
  "originRealm":"srcRealm",
  "destinationHost":"dstHost",
  "destinationRealm":"dstRealm",
  "userName":"userID",
  "ratType":1004,
  "ulrFlags":2,
  "roaming-sgsn-info":{
    "vplmn": "10101"
  }
}
```

В "message" можно указать лишь:
```
"message":
{
  "userName":"userID",
  "ratType":1004,
  "originHost":"srcHost",
  "originRealm":"srcRealm",
  "roaming-sgsn-info":{
    "vplmn": "10101"
  }
}
```

Использование вышеуказанных полей в "notNull":
```
"notNull": [
    "UserName",
    "RatType",
    "OriginHost",
    "OriginRealm",
    "RoamingSgsnInfo.Vplmn"
]
```


### 3. Обработка входящего запроса

Регулярное выражение шага - `handle (\w+\.[A-Z]{2}R)(| via \w+):$`.

Данный шаг выполняет обработку (handle) входящего запроса. Регулярное выражение `(\w+\.[A-Z]{2}R)` представляет собой шаблон "interfaceName.XXR", где часть до точки - это название интерфейса, а часть после - тип ожидаемого запроса.

Структура ответа содержит следующие ключи:
- receive [ обязательный ] (содержит ключи "message", "null" и "notNull", описание которых представлено в шаге "Получение ответа")
- send [ обязательный ] (список AVP ответа на обрабатываемый запрос)


Пример использования с структурой json-тела:
```
  And handle s6a.CLR:
  """
  {
      "receive":
      {
        "message":
        {
            "originHost":"hss.autotest.protei.ru",
            "originRealm":"autotest.protei.ru",
            "destinationHost": "mme1.autotest.protei.ru",
            "destinationRealm":"autotest.protei.ru",
            "authSessionState":1,
            "userName":"123450000000008",
            "cancellationType":0
        }
      },
      "send":
        {
        "resultCode":2001,
        "authSessionState":1,
        "originHost":"mme1.autotest.protei.ru",
        "originRealm":"autotest.protei.ru"
        }
      }
  """
```

[Актуальный список поддерживаемых сообщений go-diameter библиотекой](https://git.protei.ru/MobileDevelop/go-diameter/-/blob/master/diam/application/supported/applications.go).


## Примеры использования

### 1. Отправка запроса Update Location (ULR)

Сценарий будет состоять из двух шагов:

Step 1: Отправить ULR-запрос

Step 2: Получить ULA-ответ


![Diameter Update Location](/featureTester/images/diam_expl_1.png)

Назовем Feature - `Изменение местоположения МС`, Scenario - `Успешное обновление информации о местоположении абонента`. Под названием укажем краткое описание выполняемых шагов.

Файл ULRexample.feature будет содержать следующее:

```
Feature: Изменение местоположения МС

Scenario: Успешное обновление информации о местоположении абонента
  Отправляем ULR
  Получаем ULA с успешным resultCode (2001)
      
  When send s6a.ULR:
  """
  {
    "authSessionState":1,
    "originHost":"mme1.autotest.protei.ru",
    "originRealm":"autotest.protei.ru",
    "destinationHost":"hss.autotest.protei.ru",
    "destinationRealm":"autotest.protei.ru",
    "userName":"123450000000001",
    "ratType":1004,
    "visitedPlmnId":10101
  }
  """
  
  Then receive s6a.ULA:
  """
    {
    "message":
      {
      "resultCode":2001,
      "authSessionState":1,
      "originHost":"hss.autotest.protei.ru",
      "originRealm":"autotest.protei.ru"
    }
  }
  """
```


### 2. Получение ошибки при отправке ULR-запроса для IMSI несуществующего абонента

В данном сценарии на отправляемый запрос Update Location, ожидаем получить ответ с ошибкой отсутствия данных об абоненте, а также проверить отсутствие успешного добавления (ключа "ResultCode"), данных об абоненте (ключ "SubscriptionData")
Сценарий следует разбить следующие шаги:

Step 1: Отправить ULR-запрос с IMSI несуществующего пользователя

Step 2: Получить ULA-ответ с ошибкой 5001 (DIAMETER_ERROR_USER_UNKNOWN)

![Diameter Error Update Location](/featureTester/images/diam_expl_2.png)

Назовём Scenario - `Получение ошибки об отсутствии данных о пользователе при ULR`. Под названием укажем краткое описание выполняемых шагов.

Сценарий в feature-файле будет иметь следующее описание:

```
Scenario: Получение ошибки об отсутствии данных о пользователе при ULR
  Отправляем ULR c несуществующим IMSI
  Получаем ULA с experimentalResultCode = 5001
      
    When send s6a.ULR:
      """
      {
        "authSessionState":1,
        "originHost":"mme1.autotest.protei.ru",
        "originRealm":"autotest.protei.ru",
        "destinationHost":"hss.autotest.protei.ru",
        "destinationRealm":"autotest.protei.ru",
        "userName":"123450000000001",
        "visitedPlmnId":10101,
        "ulrFlags":2
      }
      """
    
    Then receive s6a.ULA:
    """
     {
      "message":
       {
        "authSessionState":1,
        "originHost":"hss.autotest.protei.ru",
        "originRealm":"autotest.protei.ru",
        "experimentalResult":{
          "vendorId":10415,
          "experimentalResultCode":5001
        }
      },
      "null":["ResultCode","SubscriptionData"]
    }
    """
```

### 3. Отправка CLR на старый MME при получении запроса ULR

В данном сценарии осуществляется отправка двух запросов ULR с разных MME (originHost). Первый ULR необходим для первичной регистрации, второй - для изменения зоны обслуживания(MME). После второго запроса на старый MME должен поступить запрос CancelLocation(CL), в котором должен находится адрес старого MME. Ответив на CLR,ожидаем ответ ULA на последний запрос ULR. Для обработки запроса конкретным MME будем указывать название подключения (MME1 или MME2) используя функционал "via".

Сценария состоит из следующих шагов:

Step 1: Отправить ULR со старого MME (mme1.autotest.protei.ru)

Step 2: Получить ULA на старый MME

Step 3: Отправить ULR с нового MME (mme2.autotest.protei.ru)

Step 4: Обработать CLR на старом MME

Step 5: Получить ULA на новый MME


![Diameter Update Location](/featureTester/images/diam_expl_3.png)

Назовем Scenario - `Получение запроса CancelLocation на старый MME при регистрации на новом`. Под названием укажем краткое описание выполняемых шагов.

Сценарий в feature-файле будет иметь следующее описание:

```
Scenario: Получение запроса CancelLocation на старый MME при регистрации на новом
  Отправляем ULR со старого MME (mme1.autotest.protei.ru)
  Получаем ULA на старый MME
  Отправляем ULR с нового MME (mme2.autotest.protei.ru)
  Получаем CLR-запрос на старый MME
  Получаем ULA на новый MME
      
  When send s6a.ULR via MME1:
  """
  {
    "authSessionState":1,
    "originHost":"mme1.autotest.protei.ru",
    "originRealm":"autotest.protei.ru",
    "destinationHost":"hss.autotest.protei.ru",
    "destinationRealm":"autotest.protei.ru",
    "userName":"123450000000001",
    "ratType":1004,
    "visitedPlmnId":10101,
    "ulrFlags":2
  }
  """
  Then receive s6a.ULA via MME1:
  """
  {
    "message":
      {
      "resultCode":2001,
      "authSessionState":1,
      "originHost":"hss.autotest.protei.ru",
      "originRealm":"autotest.protei.ru",
      "destinationHost": "mme1.autotest.protei.ru"
    }
  }
  """

  And send s6a.ULR via MME2:
  """
  {
    "authSessionState":1,
    "originHost":"mme2.autotest.protei.ru",
    "originRealm":"autotest.protei.ru",
    "destinationHost":"hss.autotest.protei.ru",
    "destinationRealm":"autotest.protei.ru",
    "userName":"123450000000001",
    "ratType":1004,
    "visitedPlmnId":10109,
    "ulrFlags":2
  }
  """

  And handle s6a.CLR via MME1:
  """
  {
    "receive": {
      "message": {
        "originHost":"hss.autotest.protei.ru",
        "originRealm":"autotest.protei.ru",
        "destinationHost": "mme1.autotest.protei.ru",
        "destinationRealm":"autotest.protei.ru",
        "authSessionState":1,
        "userName":"123450000000001",
        "cancellationType":0
      }
    },

    "send": {
      "resultCode":2001,
      "authSessionState":1,
      "originHost":"mme1.autotest.protei.ru",
      "originRealm":"autotest.protei.ru"
    }
  }
  """

  And receive s6a.ULA via MME2:
  """
  {
    "message": {
      "resultCode":2001,
      "authSessionState":1,
      "originHost":"hss.autotest.protei.ru",
      "originRealm":"autotest.protei.ru",
      "destinationHost": "mme2.autotest.protei.ru"
    }
  }
  """
  
```

Другие примеры можно посмотреть [здесь](https://git.protei.ru/MobileDevelop/functional-tests/-/blob/master/hss/features/spec/TS103.261-2_S6a/UpdateLocation.feature).
